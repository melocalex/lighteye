# Status dashboard

Generated at **2026-11-14T13:36:02.938Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12495 |
| Streams discovered (unique URLs) | 18631 |
| Duplicate URLs collapsed | 11472 |
| Channels with no metadata in the source | 2092 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11017 | 2325 |
| UNSTABLE | 178 | 86 |
| OFFLINE | 4256 | 1174 |
| TIMEOUT | 327 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 616 | 143 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8376 |
| UNSTABLE | 100 |
| OFFLINE | 0 |
| TIMEOUT | 201 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3744 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7621 |
| Of those, status WORKING | 7544 |
| Of those, unstable | 77 |
| Brazilian channels | 294 |
| International channels | 7327 |
| With an alternative stream kept | 2350 |
| With guide coverage | 937 |
| With **verified** Portuguese audio | 53 |
| With **verified** subtitles | 1260 |
| With unreliable subtitle detection (HLS) | 7481 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4277 |
| LATAM | 1277 |
| US | 1246 |
| BR | 294 |
| ES | 200 |
| UK | 155 |
| INT | 131 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2731 |
| Notícias | 775 |
| Entretenimento | 689 |
| Cultura | 689 |
| Música | 602 |
| Filmes | 492 |
| Variedades | 379 |
| Séries | 316 |
| Infantil | 308 |
| Esportes | 304 |
| Documentários | 155 |
| Educação | 116 |
| Animação | 65 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7621 |
| Channels with a guide | 937 |
| Measured coverage | 12.29% |
| Programmes in the guide | 38682 |
| Horizon | 72 h |
| Channels without a guide | 6684 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 937 |
| Programmes | 29591 |
| Window | 24 h |
| Total size | 6699 KB |
| Largest file | 68.2 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 210 | 196 | 9749 | - |
| meuguia.tv | grabber | yes | 33 | 18 | 323 | - |
| meo.pt | grabber | yes | 52 | 52 | 1745 | - |
| nostv.pt | grabber | yes | 35 | 26 | 522 | - |
| gatotv.com | grabber | yes | 128 | 124 | 4421 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12253 | - |
| kanya-us | published | yes | n/a | 337 | 9669 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7621 | 7621 | 100 |
| `playlist-br.m3u` | Brazil | 294 | 294 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 349 | 349 | 32 |
| `playlist-movies.m3u` | Movies | 494 | 494 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 362 | 362 | 14 |
| `playlist-sports.m3u` | Sports | 306 | 306 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7327 | 7327 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12421 | 18535 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3744 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 937 of 7621 channels in the main playlist (6684 without a guide). Measured, with no rounding up.
- 8343 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2092 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
