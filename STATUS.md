# Status dashboard

Generated at **2026-11-23T18:19:05.298Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12889 |
| Streams discovered (unique URLs) | 19385 |
| Duplicate URLs collapsed | 11505 |
| Channels with no metadata in the source | 2458 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11142 | 2318 |
| UNSTABLE | 372 | 87 |
| OFFLINE | 4351 | 1154 |
| TIMEOUT | 321 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 34 | 21 |
| UNSUPPORTED | 736 | 142 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8443 |
| UNSTABLE | 236 |
| OFFLINE | 0 |
| TIMEOUT | 197 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3941 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 995 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7684 |
| Of those, status WORKING | 7471 |
| Of those, unstable | 213 |
| Brazilian channels | 290 |
| International channels | 7394 |
| With an alternative stream kept | 2384 |
| With guide coverage | 934 |
| With **verified** Portuguese audio | 56 |
| With **verified** subtitles | 1233 |
| With unreliable subtitle detection (HLS) | 7535 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4287 |
| LATAM | 1318 |
| US | 1262 |
| BR | 290 |
| ES | 201 |
| UK | 159 |
| INT | 127 |
| PT | 40 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2804 |
| Notícias | 788 |
| Cultura | 706 |
| Entretenimento | 682 |
| Música | 599 |
| Filmes | 487 |
| Variedades | 375 |
| Infantil | 311 |
| Séries | 299 |
| Esportes | 299 |
| Documentários | 160 |
| Educação | 119 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7684 |
| Channels with a guide | 934 |
| Measured coverage | 12.16% |
| Programmes in the guide | 36641 |
| Horizon | 72 h |
| Channels without a guide | 6750 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 934 |
| Programmes | 29894 |
| Window | 24 h |
| Total size | 6740.2 KB |
| Largest file | 71.7 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 208 | 193 | 8924 | - |
| meuguia.tv | grabber | yes | 33 | 16 | 288 | - |
| meo.pt | grabber | yes | 51 | 51 | 1591 | - |
| nostv.pt | grabber | yes | 37 | 23 | 416 | - |
| gatotv.com | grabber | yes | 131 | 126 | 4188 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12448 | - |
| kanya-us | published | yes | n/a | 339 | 8786 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7684 | 7684 | 100 |
| `playlist-br.m3u` | Brazil | 290 | 290 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 345 | 345 | 33 |
| `playlist-movies.m3u` | Movies | 489 | 489 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 341 | 341 | 14 |
| `playlist-sports.m3u` | Sports | 301 | 301 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7394 | 7394 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12817 | 19290 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3941 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 934 of 7684 channels in the main playlist (6750 without a guide). Measured, with no rounding up.
- 8750 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2458 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
