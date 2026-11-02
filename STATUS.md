# Status dashboard

Generated at **2026-11-02T16:10:00.593Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12377 |
| Streams discovered (unique URLs) | 18451 |
| Duplicate URLs collapsed | 11399 |
| Channels with no metadata in the source | 2033 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 6286 | 2383 |
| UNSTABLE | 77 | 86 |
| OFFLINE | 2441 | 1238 |
| TIMEOUT | 199 | 204 |
| GEO_BLOCKED | 9 | 0 |
| AUTH_REQUIRED | 22 | 21 |
| UNSUPPORTED | 319 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 5044 |
| UNSTABLE | 55 |
| OFFLINE | 0 |
| TIMEOUT | 160 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 7041 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 4244 |
| Of those, status WORKING | 4212 |
| Of those, unstable | 32 |
| Brazilian channels | 297 |
| International channels | 3947 |
| With an alternative stream kept | 1363 |
| With guide coverage | 521 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 659 |
| With unreliable subtitle detection (HLS) | 4169 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 2283 |
| US | 716 |
| LATAM | 663 |
| BR | 297 |
| ES | 107 |
| UK | 80 |
| INT | 54 |
| PT | 44 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 1299 |
| Notícias | 517 |
| Cultura | 483 |
| Entretenimento | 445 |
| Música | 291 |
| Filmes | 283 |
| Variedades | 220 |
| Séries | 179 |
| Esportes | 169 |
| Infantil | 158 |
| Documentários | 91 |
| Educação | 81 |
| Animação | 28 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 4244 |
| Channels with a guide | 521 |
| Measured coverage | 12.28% |
| Programmes in the guide | 23289 |
| Horizon | 72 h |
| Channels without a guide | 3723 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 521 |
| Programmes | 17699 |
| Window | 24 h |
| Total size | 4007 KB |
| Largest file | 47.7 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 161 | 151 | 7386 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 43 | 43 | 1387 | - |
| nostv.pt | grabber | yes | 30 | 20 | 477 | - |
| gatotv.com | grabber | yes | 71 | 70 | 2492 | - |
| pluto.tv | grabber | yes | 201 | 192 | 6285 | - |
| kanya-us | published | yes | n/a | 198 | 5262 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 4244 | 4244 | 94 |
| `playlist-br.m3u` | Brazil | 297 | 297 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 344 | 344 | 26 |
| `playlist-movies.m3u` | Movies | 285 | 285 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 193 | 193 | 11 |
| `playlist-sports.m3u` | Sports | 171 | 171 | 8 |
| `playlist-international.m3u` | International (everything outside Brazil) | 3947 | 3947 | 81 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12300 | 18352 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 7041 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 521 of 4244 channels in the main playlist (3723 without a guide). Measured, with no rounding up.
- 5022 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2033 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
