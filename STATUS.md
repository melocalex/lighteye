# Status dashboard

Generated at **2026-11-08T13:25:59.175Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12492 |
| Streams discovered (unique URLs) | 18637 |
| Duplicate URLs collapsed | 11479 |
| Channels with no metadata in the source | 2096 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 9475 | 2372 |
| UNSTABLE | 88 | 86 |
| OFFLINE | 3719 | 1220 |
| TIMEOUT | 264 | 204 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 25 | 21 |
| UNSUPPORTED | 486 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 7288 |
| UNSTABLE | 69 |
| OFFLINE | 0 |
| TIMEOUT | 181 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 4878 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 6502 |
| Of those, status WORKING | 6456 |
| Of those, unstable | 46 |
| Brazilian channels | 297 |
| International channels | 6205 |
| With an alternative stream kept | 2020 |
| With guide coverage | 850 |
| With **verified** Portuguese audio | 58 |
| With **verified** subtitles | 1135 |
| With unreliable subtitle detection (HLS) | 6385 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 3641 |
| US | 1063 |
| LATAM | 1028 |
| BR | 297 |
| ES | 178 |
| UK | 137 |
| INT | 117 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2196 |
| Notícias | 667 |
| Cultura | 636 |
| Entretenimento | 622 |
| Música | 494 |
| Filmes | 456 |
| Variedades | 321 |
| Séries | 276 |
| Infantil | 270 |
| Esportes | 262 |
| Documentários | 139 |
| Educação | 100 |
| Animação | 63 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 6502 |
| Channels with a guide | 850 |
| Measured coverage | 13.07% |
| Programmes in the guide | 35905 |
| Horizon | 72 h |
| Channels without a guide | 5652 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 850 |
| Programmes | 26870 |
| Window | 24 h |
| Total size | 6262.5 KB |
| Largest file | 42.4 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 191 | 178 | 8883 | - |
| meuguia.tv | grabber | yes | 34 | 0 | 0 | - |
| meo.pt | grabber | yes | 48 | 48 | 1589 | - |
| nostv.pt | grabber | yes | 31 | 24 | 698 | - |
| gatotv.com | grabber | yes | 108 | 106 | 3912 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12423 | - |
| kanya-us | published | yes | n/a | 287 | 8400 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 6502 | 6502 | 100 |
| `playlist-br.m3u` | Brazil | 297 | 297 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 344 | 344 | 29 |
| `playlist-movies.m3u` | Movies | 458 | 458 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 319 | 319 | 14 |
| `playlist-sports.m3u` | Sports | 264 | 264 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 6205 | 6205 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12416 | 18539 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 4878 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 850 of 6502 channels in the main playlist (5652 without a guide). Measured, with no rounding up.
- 7242 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2096 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
