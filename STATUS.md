# Status dashboard

Generated at **2026-11-05T14:44:25.867Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12368 |
| Streams discovered (unique URLs) | 18458 |
| Duplicate URLs collapsed | 11403 |
| Channels with no metadata in the source | 2031 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 8079 | 2381 |
| UNSTABLE | 80 | 86 |
| OFFLINE | 3024 | 1236 |
| TIMEOUT | 231 | 204 |
| GEO_BLOCKED | 13 | 0 |
| AUTH_REQUIRED | 24 | 21 |
| UNSUPPORTED | 406 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 6318 |
| UNSTABLE | 68 |
| OFFLINE | 0 |
| TIMEOUT | 172 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 5734 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 5531 |
| Of those, status WORKING | 5486 |
| Of those, unstable | 45 |
| Brazilian channels | 297 |
| International channels | 5234 |
| With an alternative stream kept | 1719 |
| With guide coverage | 776 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 995 |
| With unreliable subtitle detection (HLS) | 5430 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 3051 |
| US | 927 |
| LATAM | 853 |
| BR | 297 |
| ES | 158 |
| UK | 110 |
| INT | 94 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 1789 |
| Notícias | 613 |
| Cultura | 555 |
| Entretenimento | 544 |
| Música | 419 |
| Filmes | 399 |
| Variedades | 282 |
| Séries | 246 |
| Infantil | 226 |
| Esportes | 205 |
| Documentários | 126 |
| Educação | 93 |
| Animação | 34 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 5531 |
| Channels with a guide | 776 |
| Measured coverage | 14.03% |
| Programmes in the guide | 32107 |
| Horizon | 72 h |
| Channels without a guide | 4755 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 776 |
| Programmes | 25111 |
| Window | 24 h |
| Total size | 5906.9 KB |
| Largest file | 48 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 169 | 158 | 8296 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 46 | 46 | 1490 | - |
| nostv.pt | grabber | yes | 31 | 20 | 396 | - |
| gatotv.com | grabber | yes | 81 | 78 | 2741 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12172 | - |
| kanya-us | published | yes | n/a | 254 | 7012 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 5531 | 5531 | 99 |
| `playlist-br.m3u` | Brazil | 297 | 297 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 342 | 342 | 27 |
| `playlist-movies.m3u` | Movies | 401 | 401 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 277 | 277 | 13 |
| `playlist-sports.m3u` | Sports | 207 | 207 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 5234 | 5234 | 86 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12292 | 18360 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 5734 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 776 of 5531 channels in the main playlist (4755 without a guide). Measured, with no rounding up.
- 6286 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2031 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
