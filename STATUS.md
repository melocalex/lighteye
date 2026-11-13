# Status dashboard

Generated at **2026-11-13T14:43:26.839Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12488 |
| Streams discovered (unique URLs) | 18622 |
| Duplicate URLs collapsed | 11464 |
| Channels with no metadata in the source | 2092 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11033 | 2325 |
| UNSTABLE | 184 | 86 |
| OFFLINE | 4227 | 1174 |
| TIMEOUT | 333 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 608 | 143 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8374 |
| UNSTABLE | 96 |
| OFFLINE | 0 |
| TIMEOUT | 203 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3741 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7615 |
| Of those, status WORKING | 7542 |
| Of those, unstable | 73 |
| Brazilian channels | 297 |
| International channels | 7318 |
| With an alternative stream kept | 2347 |
| With guide coverage | 939 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 1260 |
| With unreliable subtitle detection (HLS) | 7475 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4271 |
| LATAM | 1274 |
| US | 1245 |
| BR | 297 |
| ES | 200 |
| UK | 155 |
| INT | 131 |
| PT | 42 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2726 |
| Notícias | 775 |
| Cultura | 689 |
| Entretenimento | 688 |
| Música | 602 |
| Filmes | 492 |
| Variedades | 380 |
| Séries | 316 |
| Infantil | 308 |
| Esportes | 303 |
| Documentários | 155 |
| Educação | 116 |
| Animação | 65 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7615 |
| Channels with a guide | 939 |
| Measured coverage | 12.33% |
| Programmes in the guide | 38832 |
| Horizon | 72 h |
| Channels without a guide | 6676 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 939 |
| Programmes | 30176 |
| Window | 24 h |
| Total size | 6805.1 KB |
| Largest file | 71.3 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 211 | 197 | 9733 | - |
| meuguia.tv | grabber | yes | 33 | 18 | 224 | - |
| meo.pt | grabber | yes | 53 | 53 | 1802 | - |
| nostv.pt | grabber | yes | 36 | 29 | 516 | - |
| gatotv.com | grabber | yes | 128 | 123 | 4660 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12274 | - |
| kanya-us | published | yes | n/a | 337 | 9623 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7615 | 7615 | 100 |
| `playlist-br.m3u` | Brazil | 297 | 297 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 353 | 353 | 32 |
| `playlist-movies.m3u` | Movies | 494 | 494 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 362 | 362 | 14 |
| `playlist-sports.m3u` | Sports | 305 | 305 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7318 | 7318 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12414 | 18526 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3741 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 939 of 7615 channels in the main playlist (6676 without a guide). Measured, with no rounding up.
- 8331 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2092 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
