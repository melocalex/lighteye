# Status dashboard

Generated at **2026-11-26T17:00:42.493Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12884 |
| Streams discovered (unique URLs) | 19300 |
| Duplicate URLs collapsed | 11512 |
| Channels with no metadata in the source | 2421 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11047 | 2310 |
| UNSTABLE | 430 | 87 |
| OFFLINE | 4336 | 1141 |
| TIMEOUT | 324 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 31 | 20 |
| UNSUPPORTED | 732 | 140 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8441 |
| UNSTABLE | 269 |
| OFFLINE | 0 |
| TIMEOUT | 204 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3899 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 988 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7722 |
| Of those, status WORKING | 7476 |
| Of those, unstable | 246 |
| Brazilian channels | 292 |
| International channels | 7430 |
| With an alternative stream kept | 2374 |
| With guide coverage | 744 |
| With **verified** Portuguese audio | 56 |
| With **verified** subtitles | 1214 |
| With unreliable subtitle detection (HLS) | 7575 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4341 |
| LATAM | 1311 |
| US | 1249 |
| BR | 292 |
| ES | 201 |
| UK | 159 |
| INT | 128 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2817 |
| Notícias | 812 |
| Cultura | 702 |
| Entretenimento | 685 |
| Música | 599 |
| Filmes | 482 |
| Variedades | 379 |
| Infantil | 318 |
| Séries | 299 |
| Esportes | 299 |
| Documentários | 158 |
| Educação | 117 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7722 |
| Channels with a guide | 744 |
| Measured coverage | 9.63% |
| Programmes in the guide | 28534 |
| Horizon | 72 h |
| Channels without a guide | 6978 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 744 |
| Programmes | 23433 |
| Window | 24 h |
| Total size | 5305.8 KB |
| Largest file | 76.8 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 208 | 193 | 9469 | - |
| meuguia.tv | grabber | yes | 33 | 17 | 272 | - |
| meo.pt | grabber | yes | 50 | 50 | 1619 | - |
| nostv.pt | grabber | yes | 36 | 23 | 433 | - |
| gatotv.com | grabber | yes | 128 | 124 | 4256 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12161 | - |
| kanya-us | published | yes | n/a | 108 | 324 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7722 | 7722 | 100 |
| `playlist-br.m3u` | Brazil | 292 | 292 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 346 | 346 | 32 |
| `playlist-movies.m3u` | Movies | 484 | 484 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 341 | 341 | 14 |
| `playlist-sports.m3u` | Sports | 301 | 301 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7430 | 7430 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12813 | 19206 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3899 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 744 of 7722 channels in the main playlist (6978 without a guide). Measured, with no rounding up.
- 8864 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2421 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
