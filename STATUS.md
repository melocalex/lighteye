# Status dashboard

Generated at **2026-11-16T15:48:21.589Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12516 |
| Streams discovered (unique URLs) | 18689 |
| Duplicate URLs collapsed | 11502 |
| Channels with no metadata in the source | 2092 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11033 | 2325 |
| UNSTABLE | 195 | 87 |
| OFFLINE | 4271 | 1171 |
| TIMEOUT | 330 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 31 | 21 |
| UNSUPPORTED | 624 | 143 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8376 |
| UNSTABLE | 141 |
| OFFLINE | 0 |
| TIMEOUT | 198 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3728 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7662 |
| Of those, status WORKING | 7544 |
| Of those, unstable | 118 |
| Brazilian channels | 294 |
| International channels | 7368 |
| With an alternative stream kept | 2359 |
| With guide coverage | 937 |
| With **verified** Portuguese audio | 53 |
| With **verified** subtitles | 1263 |
| With unreliable subtitle detection (HLS) | 7499 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4301 |
| LATAM | 1282 |
| US | 1253 |
| BR | 294 |
| ES | 203 |
| UK | 155 |
| INT | 133 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2735 |
| Notícias | 776 |
| Cultura | 694 |
| Entretenimento | 693 |
| Música | 602 |
| Filmes | 500 |
| Variedades | 380 |
| Infantil | 316 |
| Séries | 315 |
| Esportes | 314 |
| Documentários | 155 |
| Educação | 117 |
| Animação | 65 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7662 |
| Channels with a guide | 937 |
| Measured coverage | 12.23% |
| Programmes in the guide | 38320 |
| Horizon | 72 h |
| Channels without a guide | 6725 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 937 |
| Programmes | 30050 |
| Window | 24 h |
| Total size | 6751.1 KB |
| Largest file | 72.8 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 209 | 195 | 9608 | - |
| meuguia.tv | grabber | yes | 33 | 18 | 359 | - |
| meo.pt | grabber | yes | 52 | 52 | 1664 | - |
| nostv.pt | grabber | yes | 35 | 22 | 450 | - |
| gatotv.com | grabber | yes | 130 | 125 | 4468 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12218 | - |
| kanya-us | published | yes | n/a | 336 | 9553 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7662 | 7662 | 100 |
| `playlist-br.m3u` | Brazil | 294 | 294 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 349 | 349 | 32 |
| `playlist-movies.m3u` | Movies | 502 | 502 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 362 | 362 | 14 |
| `playlist-sports.m3u` | Sports | 316 | 316 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7368 | 7368 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12443 | 18595 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3728 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 937 of 7662 channels in the main playlist (6725 without a guide). Measured, with no rounding up.
- 8385 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2092 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
