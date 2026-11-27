# Status dashboard

Generated at **2026-11-27T16:47:26.702Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12928 |
| Streams discovered (unique URLs) | 19377 |
| Duplicate URLs collapsed | 11556 |
| Channels with no metadata in the source | 2421 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11105 | 2304 |
| UNSTABLE | 439 | 86 |
| OFFLINE | 4298 | 1139 |
| TIMEOUT | 330 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 32 | 20 |
| UNSUPPORTED | 767 | 140 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8477 |
| UNSTABLE | 268 |
| OFFLINE | 0 |
| TIMEOUT | 207 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3905 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 988 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7757 |
| Of those, status WORKING | 7512 |
| Of those, unstable | 245 |
| Brazilian channels | 294 |
| International channels | 7463 |
| With an alternative stream kept | 2398 |
| With guide coverage | 747 |
| With **verified** Portuguese audio | 57 |
| With **verified** subtitles | 1229 |
| With unreliable subtitle detection (HLS) | 7602 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4371 |
| LATAM | 1308 |
| US | 1251 |
| BR | 294 |
| ES | 201 |
| UK | 159 |
| INT | 132 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2817 |
| Notícias | 819 |
| Cultura | 701 |
| Entretenimento | 694 |
| Música | 602 |
| Filmes | 488 |
| Variedades | 380 |
| Infantil | 320 |
| Esportes | 302 |
| Séries | 299 |
| Documentários | 162 |
| Educação | 118 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7757 |
| Channels with a guide | 747 |
| Measured coverage | 9.63% |
| Programmes in the guide | 28651 |
| Horizon | 72 h |
| Channels without a guide | 7010 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 747 |
| Programmes | 23688 |
| Window | 24 h |
| Total size | 5388.2 KB |
| Largest file | 70.4 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 210 | 195 | 9294 | - |
| meuguia.tv | grabber | yes | 33 | 17 | 254 | - |
| meo.pt | grabber | yes | 51 | 51 | 1776 | - |
| nostv.pt | grabber | yes | 38 | 29 | 716 | - |
| gatotv.com | grabber | yes | 127 | 123 | 4160 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12235 | - |
| kanya-us | published | yes | n/a | 108 | 216 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7757 | 7757 | 100 |
| `playlist-br.m3u` | Brazil | 294 | 294 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 351 | 351 | 33 |
| `playlist-movies.m3u` | Movies | 490 | 490 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 341 | 341 | 14 |
| `playlist-sports.m3u` | Sports | 304 | 304 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7463 | 7463 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12857 | 19277 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3905 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 747 of 7757 channels in the main playlist (7010 without a guide). Measured, with no rounding up.
- 8895 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2421 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
