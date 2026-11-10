# Status dashboard

Generated at **2026-11-10T15:07:37.457Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12482 |
| Streams discovered (unique URLs) | 18615 |
| Duplicate URLs collapsed | 11470 |
| Channels with no metadata in the source | 2096 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 10554 | 2372 |
| UNSTABLE | 118 | 86 |
| OFFLINE | 4152 | 1217 |
| TIMEOUT | 296 | 204 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 548 | 143 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8044 |
| UNSTABLE | 75 |
| OFFLINE | 0 |
| TIMEOUT | 198 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 4091 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7264 |
| Of those, status WORKING | 7212 |
| Of those, unstable | 52 |
| Brazilian channels | 297 |
| International channels | 6967 |
| With an alternative stream kept | 2240 |
| With guide coverage | 909 |
| With **verified** Portuguese audio | 59 |
| With **verified** subtitles | 1222 |
| With unreliable subtitle detection (HLS) | 7134 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4070 |
| LATAM | 1201 |
| US | 1179 |
| BR | 297 |
| ES | 195 |
| UK | 153 |
| INT | 128 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2566 |
| Notícias | 723 |
| Cultura | 674 |
| Entretenimento | 663 |
| Música | 570 |
| Filmes | 479 |
| Variedades | 364 |
| Séries | 306 |
| Infantil | 298 |
| Esportes | 292 |
| Documentários | 156 |
| Educação | 109 |
| Animação | 64 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7264 |
| Channels with a guide | 909 |
| Measured coverage | 12.51% |
| Programmes in the guide | 37590 |
| Horizon | 72 h |
| Channels without a guide | 6355 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 909 |
| Programmes | 29211 |
| Window | 24 h |
| Total size | 6630.7 KB |
| Largest file | 45.5 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 204 | 190 | 9162 | - |
| meuguia.tv | grabber | yes | 34 | 22 | 400 | - |
| meo.pt | grabber | yes | 49 | 49 | 1605 | - |
| nostv.pt | grabber | yes | 33 | 19 | 414 | - |
| gatotv.com | grabber | yes | 119 | 116 | 4456 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12339 | - |
| kanya-us | published | yes | n/a | 323 | 9214 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7264 | 7264 | 100 |
| `playlist-br.m3u` | Brazil | 297 | 297 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 350 | 350 | 31 |
| `playlist-movies.m3u` | Movies | 481 | 481 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 352 | 352 | 14 |
| `playlist-sports.m3u` | Sports | 294 | 294 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 6967 | 6967 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12408 | 18519 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 4091 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 909 of 7264 channels in the main playlist (6355 without a guide). Measured, with no rounding up.
- 7991 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2096 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
