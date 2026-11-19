# Status dashboard

Generated at **2026-11-19T15:52:21.636Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12610 |
| Streams discovered (unique URLs) | 18882 |
| Duplicate URLs collapsed | 11613 |
| Channels with no metadata in the source | 2092 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11117 | 2320 |
| UNSTABLE | 278 | 87 |
| OFFLINE | 4282 | 1162 |
| TIMEOUT | 332 | 201 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 636 | 142 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8437 |
| UNSTABLE | 183 |
| OFFLINE | 0 |
| TIMEOUT | 200 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3716 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7765 |
| Of those, status WORKING | 7605 |
| Of those, unstable | 160 |
| Brazilian channels | 294 |
| International channels | 7471 |
| With an alternative stream kept | 2411 |
| With guide coverage | 941 |
| With **verified** Portuguese audio | 53 |
| With **verified** subtitles | 1270 |
| With unreliable subtitle detection (HLS) | 7599 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4364 |
| LATAM | 1309 |
| US | 1254 |
| BR | 294 |
| ES | 207 |
| UK | 159 |
| INT | 137 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2785 |
| Notícias | 784 |
| Cultura | 704 |
| Entretenimento | 702 |
| Música | 605 |
| Filmes | 501 |
| Variedades | 380 |
| Infantil | 323 |
| Esportes | 323 |
| Séries | 317 |
| Documentários | 157 |
| Educação | 118 |
| Animação | 66 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7765 |
| Channels with a guide | 941 |
| Measured coverage | 12.12% |
| Programmes in the guide | 37353 |
| Horizon | 72 h |
| Channels without a guide | 6824 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 941 |
| Programmes | 29314 |
| Window | 24 h |
| Total size | 6723.1 KB |
| Largest file | 77 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 211 | 196 | 9761 | - |
| meuguia.tv | grabber | yes | 33 | 15 | 162 | - |
| meo.pt | grabber | yes | 53 | 53 | 1749 | - |
| nostv.pt | grabber | yes | 38 | 25 | 527 | - |
| gatotv.com | grabber | yes | 130 | 111 | 3749 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12202 | - |
| kanya-us | published | yes | n/a | 336 | 9203 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7765 | 7765 | 100 |
| `playlist-br.m3u` | Brazil | 294 | 294 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 350 | 350 | 33 |
| `playlist-movies.m3u` | Movies | 503 | 503 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 365 | 365 | 14 |
| `playlist-sports.m3u` | Sports | 325 | 325 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7471 | 7471 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12536 | 18786 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3716 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 941 of 7765 channels in the main playlist (6824 without a guide). Measured, with no rounding up.
- 8549 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2092 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
