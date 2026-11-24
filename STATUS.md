# Status dashboard

Generated at **2026-11-24T17:48:45.870Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12858 |
| Streams discovered (unique URLs) | 19263 |
| Duplicate URLs collapsed | 11473 |
| Channels with no metadata in the source | 2433 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11080 | 2310 |
| UNSTABLE | 410 | 87 |
| OFFLINE | 4282 | 1141 |
| TIMEOUT | 314 | 202 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 32 | 20 |
| UNSUPPORTED | 733 | 140 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8443 |
| UNSTABLE | 254 |
| OFFLINE | 0 |
| TIMEOUT | 193 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3897 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 988 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7709 |
| Of those, status WORKING | 7478 |
| Of those, unstable | 231 |
| Brazilian channels | 294 |
| International channels | 7415 |
| With an alternative stream kept | 2369 |
| With guide coverage | 932 |
| With **verified** Portuguese audio | 56 |
| With **verified** subtitles | 1229 |
| With unreliable subtitle detection (HLS) | 7559 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4301 |
| LATAM | 1319 |
| US | 1259 |
| BR | 294 |
| ES | 207 |
| UK | 159 |
| INT | 129 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2823 |
| Notícias | 802 |
| Cultura | 705 |
| Entretenimento | 671 |
| Música | 609 |
| Filmes | 485 |
| Variedades | 377 |
| Infantil | 312 |
| Séries | 299 |
| Esportes | 297 |
| Documentários | 156 |
| Educação | 118 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7709 |
| Channels with a guide | 932 |
| Measured coverage | 12.09% |
| Programmes in the guide | 29574 |
| Horizon | 72 h |
| Channels without a guide | 6777 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 932 |
| Programmes | 24823 |
| Window | 24 h |
| Total size | 5751.4 KB |
| Largest file | 90.6 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 209 | 194 | 9324 | - |
| meuguia.tv | grabber | yes | 33 | 19 | 276 | - |
| meo.pt | grabber | yes | 52 | 52 | 1627 | - |
| nostv.pt | grabber | yes | 37 | 23 | 464 | - |
| gatotv.com | grabber | yes | 128 | 109 | 3353 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12179 | - |
| kanya-us | published | yes | n/a | 326 | 2351 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7709 | 7709 | 100 |
| `playlist-br.m3u` | Brazil | 294 | 294 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 350 | 350 | 33 |
| `playlist-movies.m3u` | Movies | 487 | 487 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 341 | 341 | 14 |
| `playlist-sports.m3u` | Sports | 299 | 299 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7415 | 7415 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12787 | 19169 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3897 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 932 of 7709 channels in the main playlist (6777 without a guide). Measured, with no rounding up.
- 8789 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2433 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
