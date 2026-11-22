# Status dashboard

Generated at **2026-11-22T14:42:15.780Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12818 |
| Streams discovered (unique URLs) | 19389 |
| Duplicate URLs collapsed | 11512 |
| Channels with no metadata in the source | 2378 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11198 | 2318 |
| UNSTABLE | 352 | 87 |
| OFFLINE | 4324 | 1154 |
| TIMEOUT | 320 | 201 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 31 | 21 |
| UNSUPPORTED | 737 | 142 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8401 |
| UNSTABLE | 218 |
| OFFLINE | 0 |
| TIMEOUT | 197 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3930 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 920 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7699 |
| Of those, status WORKING | 7504 |
| Of those, unstable | 195 |
| Brazilian channels | 291 |
| International channels | 7408 |
| With an alternative stream kept | 2392 |
| With guide coverage | 939 |
| With **verified** Portuguese audio | 55 |
| With **verified** subtitles | 1230 |
| With unreliable subtitle detection (HLS) | 7550 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4310 |
| LATAM | 1307 |
| US | 1262 |
| BR | 291 |
| ES | 201 |
| UK | 158 |
| INT | 130 |
| PT | 40 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2783 |
| Notícias | 785 |
| Cultura | 702 |
| Entretenimento | 683 |
| Música | 601 |
| Filmes | 503 |
| Variedades | 381 |
| Infantil | 318 |
| Esportes | 307 |
| Séries | 298 |
| Documentários | 164 |
| Educação | 119 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7699 |
| Channels with a guide | 939 |
| Measured coverage | 12.2% |
| Programmes in the guide | 38821 |
| Horizon | 72 h |
| Channels without a guide | 6760 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 939 |
| Programmes | 29472 |
| Window | 24 h |
| Total size | 6634.7 KB |
| Largest file | 72.8 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 211 | 196 | 9277 | - |
| meuguia.tv | grabber | yes | 33 | 19 | 419 | - |
| meo.pt | grabber | yes | 51 | 51 | 1719 | - |
| nostv.pt | grabber | yes | 37 | 26 | 645 | - |
| gatotv.com | grabber | yes | 131 | 127 | 4743 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12139 | - |
| kanya-us | published | yes | n/a | 341 | 9879 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7699 | 7699 | 100 |
| `playlist-br.m3u` | Brazil | 291 | 291 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 346 | 346 | 33 |
| `playlist-movies.m3u` | Movies | 505 | 505 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 341 | 341 | 14 |
| `playlist-sports.m3u` | Sports | 309 | 309 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7408 | 7408 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12746 | 19294 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3930 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 939 of 7699 channels in the main playlist (6760 without a guide). Measured, with no rounding up.
- 8672 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2378 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
