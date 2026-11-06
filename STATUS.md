# Status dashboard

Generated at **2026-11-06T14:54:55.974Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12433 |
| Streams discovered (unique URLs) | 18456 |
| Duplicate URLs collapsed | 11404 |
| Channels with no metadata in the source | 2096 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 8243 | 2377 |
| UNSTABLE | 83 | 86 |
| OFFLINE | 3293 | 1235 |
| TIMEOUT | 237 | 204 |
| GEO_BLOCKED | 16 | 0 |
| AUTH_REQUIRED | 23 | 21 |
| UNSUPPORTED | 411 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 6423 |
| UNSTABLE | 67 |
| OFFLINE | 0 |
| TIMEOUT | 172 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 5695 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 5635 |
| Of those, status WORKING | 5591 |
| Of those, unstable | 44 |
| Brazilian channels | 298 |
| International channels | 5337 |
| With an alternative stream kept | 1756 |
| With guide coverage | 777 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 1007 |
| With unreliable subtitle detection (HLS) | 5531 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 3129 |
| US | 936 |
| LATAM | 864 |
| BR | 298 |
| ES | 158 |
| UK | 114 |
| INT | 95 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 1836 |
| Notícias | 625 |
| Cultura | 575 |
| Entretenimento | 550 |
| Música | 427 |
| Filmes | 402 |
| Variedades | 284 |
| Séries | 247 |
| Infantil | 228 |
| Esportes | 205 |
| Documentários | 127 |
| Educação | 95 |
| Animação | 34 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 5635 |
| Channels with a guide | 777 |
| Measured coverage | 13.79% |
| Programmes in the guide | 31452 |
| Horizon | 72 h |
| Channels without a guide | 4858 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 777 |
| Programmes | 24981 |
| Window | 24 h |
| Total size | 5850.3 KB |
| Largest file | 47.8 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 169 | 158 | 7765 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 46 | 46 | 1483 | - |
| nostv.pt | grabber | yes | 31 | 23 | 440 | - |
| gatotv.com | grabber | yes | 81 | 78 | 2614 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12123 | - |
| kanya-us | published | yes | n/a | 255 | 7027 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 5635 | 5635 | 99 |
| `playlist-br.m3u` | Brazil | 298 | 298 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 343 | 343 | 27 |
| `playlist-movies.m3u` | Movies | 404 | 404 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 278 | 278 | 13 |
| `playlist-sports.m3u` | Sports | 207 | 207 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 5337 | 5337 | 86 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12357 | 18358 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 5695 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 777 of 5635 channels in the main playlist (4858 without a guide). Measured, with no rounding up.
- 6387 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2096 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
