# Status dashboard

Generated at **2026-11-12T15:17:34.004Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12484 |
| Streams discovered (unique URLs) | 18605 |
| Duplicate URLs collapsed | 11461 |
| Channels with no metadata in the source | 2092 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 10912 | 2329 |
| UNSTABLE | 162 | 86 |
| OFFLINE | 4171 | 1177 |
| TIMEOUT | 330 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 595 | 143 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8275 |
| UNSTABLE | 80 |
| OFFLINE | 0 |
| TIMEOUT | 204 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3851 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7500 |
| Of those, status WORKING | 7443 |
| Of those, unstable | 57 |
| Brazilian channels | 296 |
| International channels | 7204 |
| With an alternative stream kept | 2312 |
| With guide coverage | 932 |
| With **verified** Portuguese audio | 52 |
| With **verified** subtitles | 1255 |
| With unreliable subtitle detection (HLS) | 7364 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4186 |
| LATAM | 1253 |
| US | 1238 |
| BR | 296 |
| ES | 201 |
| UK | 155 |
| INT | 129 |
| PT | 42 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2682 |
| Notícias | 756 |
| Cultura | 685 |
| Entretenimento | 673 |
| Música | 595 |
| Filmes | 489 |
| Variedades | 373 |
| Séries | 315 |
| Infantil | 302 |
| Esportes | 296 |
| Documentários | 155 |
| Educação | 115 |
| Animação | 64 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7500 |
| Channels with a guide | 932 |
| Measured coverage | 12.43% |
| Programmes in the guide | 38131 |
| Horizon | 72 h |
| Channels without a guide | 6568 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 932 |
| Programmes | 29564 |
| Window | 24 h |
| Total size | 6726.3 KB |
| Largest file | 49.2 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 207 | 194 | 9314 | - |
| meuguia.tv | grabber | yes | 33 | 18 | 178 | - |
| meo.pt | grabber | yes | 51 | 51 | 1713 | - |
| nostv.pt | grabber | yes | 35 | 24 | 464 | - |
| gatotv.com | grabber | yes | 125 | 120 | 4611 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12497 | - |
| kanya-us | published | yes | n/a | 336 | 9354 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7500 | 7500 | 100 |
| `playlist-br.m3u` | Brazil | 296 | 296 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 352 | 352 | 32 |
| `playlist-movies.m3u` | Movies | 491 | 491 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 361 | 361 | 14 |
| `playlist-sports.m3u` | Sports | 298 | 298 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7204 | 7204 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12410 | 18509 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3851 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 932 of 7500 channels in the main playlist (6568 without a guide). Measured, with no rounding up.
- 8218 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2092 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
