# Status dashboard

Generated at **2026-11-01T13:26:21.879Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12371 |
| Streams discovered (unique URLs) | 18434 |
| Duplicate URLs collapsed | 11388 |
| Channels with no metadata in the source | 2033 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 5974 | 2383 |
| UNSTABLE | 79 | 86 |
| OFFLINE | 2335 | 1241 |
| TIMEOUT | 177 | 204 |
| GEO_BLOCKED | 7 | 0 |
| AUTH_REQUIRED | 19 | 21 |
| UNSUPPORTED | 306 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 4876 |
| UNSTABLE | 53 |
| OFFLINE | 0 |
| TIMEOUT | 140 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 7227 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 4074 |
| Of those, status WORKING | 4044 |
| Of those, unstable | 30 |
| Brazilian channels | 296 |
| International channels | 3778 |
| With an alternative stream kept | 1285 |
| With guide coverage | 500 |
| With **verified** Portuguese audio | 53 |
| With **verified** subtitles | 637 |
| With unreliable subtitle detection (HLS) | 4006 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 2169 |
| US | 683 |
| LATAM | 646 |
| BR | 296 |
| ES | 106 |
| UK | 79 |
| INT | 52 |
| PT | 43 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 1259 |
| Cultura | 476 |
| Notícias | 459 |
| Entretenimento | 437 |
| Música | 287 |
| Filmes | 272 |
| Variedades | 216 |
| Séries | 171 |
| Esportes | 162 |
| Infantil | 158 |
| Educação | 77 |
| Documentários | 74 |
| Animação | 26 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 4074 |
| Channels with a guide | 500 |
| Measured coverage | 12.27% |
| Programmes in the guide | 22526 |
| Horizon | 72 h |
| Channels without a guide | 3574 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 500 |
| Programmes | 15981 |
| Window | 24 h |
| Total size | 3714.7 KB |
| Largest file | 43 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 158 | 149 | 7615 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 42 | 42 | 1411 | - |
| nostv.pt | grabber | yes | 28 | 21 | 377 | - |
| gatotv.com | grabber | yes | 69 | 60 | 1799 | - |
| pluto.tv | grabber | yes | 197 | 188 | 6084 | - |
| kanya-us | published | yes | n/a | 183 | 5240 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 4074 | 4074 | 94 |
| `playlist-br.m3u` | Brazil | 296 | 296 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 342 | 342 | 26 |
| `playlist-movies.m3u` | Movies | 274 | 274 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 184 | 184 | 11 |
| `playlist-sports.m3u` | Sports | 164 | 164 | 8 |
| `playlist-international.m3u` | International (everything outside Brazil) | 3778 | 3778 | 81 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12296 | 18337 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 7227 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 500 of 4074 channels in the main playlist (3574 without a guide). Measured, with no rounding up.
- 4860 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2033 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
