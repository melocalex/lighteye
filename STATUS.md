# Status dashboard

Generated at **2026-11-18T15:45:43.731Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12561 |
| Streams discovered (unique URLs) | 18765 |
| Duplicate URLs collapsed | 11557 |
| Channels with no metadata in the source | 2091 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11044 | 2321 |
| UNSTABLE | 274 | 87 |
| OFFLINE | 4253 | 1164 |
| TIMEOUT | 327 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 31 | 21 |
| UNSUPPORTED | 630 | 142 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8393 |
| UNSTABLE | 181 |
| OFFLINE | 0 |
| TIMEOUT | 198 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3715 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7719 |
| Of those, status WORKING | 7561 |
| Of those, unstable | 158 |
| Brazilian channels | 295 |
| International channels | 7424 |
| With an alternative stream kept | 2383 |
| With guide coverage | 944 |
| With **verified** Portuguese audio | 53 |
| With **verified** subtitles | 1269 |
| With unreliable subtitle detection (HLS) | 7552 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4341 |
| LATAM | 1286 |
| US | 1255 |
| BR | 295 |
| ES | 207 |
| UK | 160 |
| INT | 134 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2764 |
| Notícias | 780 |
| Entretenimento | 697 |
| Cultura | 697 |
| Música | 601 |
| Filmes | 502 |
| Variedades | 382 |
| Infantil | 321 |
| Esportes | 318 |
| Séries | 315 |
| Documentários | 157 |
| Educação | 119 |
| Animação | 66 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7719 |
| Channels with a guide | 944 |
| Measured coverage | 12.23% |
| Programmes in the guide | 37829 |
| Horizon | 72 h |
| Channels without a guide | 6775 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 944 |
| Programmes | 29824 |
| Window | 24 h |
| Total size | 6859.6 KB |
| Largest file | 80.5 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 211 | 196 | 9855 | - |
| meuguia.tv | grabber | yes | 33 | 19 | 280 | - |
| meo.pt | grabber | yes | 53 | 53 | 1754 | - |
| nostv.pt | grabber | yes | 38 | 22 | 481 | - |
| gatotv.com | grabber | yes | 131 | 111 | 3650 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12325 | - |
| kanya-us | published | yes | n/a | 338 | 9484 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7719 | 7719 | 100 |
| `playlist-br.m3u` | Brazil | 295 | 295 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 350 | 350 | 32 |
| `playlist-movies.m3u` | Movies | 504 | 504 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 363 | 363 | 14 |
| `playlist-sports.m3u` | Sports | 320 | 320 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7424 | 7424 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12487 | 18669 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3715 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 944 of 7719 channels in the main playlist (6775 without a guide). Measured, with no rounding up.
- 8487 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2091 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
