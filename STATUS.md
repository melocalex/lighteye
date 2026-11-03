# Status dashboard

Generated at **2026-11-03T14:16:48.721Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12373 |
| Streams discovered (unique URLs) | 18457 |
| Duplicate URLs collapsed | 11403 |
| Channels with no metadata in the source | 2034 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 7160 | 2383 |
| UNSTABLE | 91 | 86 |
| OFFLINE | 2679 | 1238 |
| TIMEOUT | 211 | 204 |
| GEO_BLOCKED | 9 | 0 |
| AUTH_REQUIRED | 26 | 21 |
| UNSUPPORTED | 358 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 5680 |
| UNSTABLE | 68 |
| OFFLINE | 0 |
| TIMEOUT | 166 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 6382 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 4893 |
| Of those, status WORKING | 4848 |
| Of those, unstable | 45 |
| Brazilian channels | 298 |
| International channels | 4595 |
| With an alternative stream kept | 1528 |
| With guide coverage | 722 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 909 |
| With unreliable subtitle detection (HLS) | 4810 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 2643 |
| US | 868 |
| LATAM | 720 |
| BR | 298 |
| ES | 134 |
| UK | 103 |
| INT | 83 |
| PT | 44 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 1480 |
| Notícias | 555 |
| Cultura | 525 |
| Entretenimento | 500 |
| Filmes | 371 |
| Música | 331 |
| Variedades | 257 |
| Séries | 235 |
| Infantil | 209 |
| Esportes | 191 |
| Documentários | 117 |
| Educação | 90 |
| Animação | 32 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 4893 |
| Channels with a guide | 722 |
| Measured coverage | 14.76% |
| Programmes in the guide | 30655 |
| Horizon | 72 h |
| Channels without a guide | 4171 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 722 |
| Programmes | 23361 |
| Window | 24 h |
| Total size | 5493.1 KB |
| Largest file | 46.6 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 166 | 155 | 7970 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 44 | 44 | 1569 | - |
| nostv.pt | grabber | yes | 31 | 23 | 458 | - |
| gatotv.com | grabber | yes | 78 | 76 | 2829 | - |
| pluto.tv | grabber | yes | 382 | 370 | 11541 | - |
| kanya-us | published | yes | n/a | 236 | 6288 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 4893 | 4893 | 97 |
| `playlist-br.m3u` | Brazil | 298 | 298 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 345 | 345 | 26 |
| `playlist-movies.m3u` | Movies | 373 | 373 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 260 | 260 | 12 |
| `playlist-sports.m3u` | Sports | 193 | 193 | 8 |
| `playlist-international.m3u` | International (everything outside Brazil) | 4595 | 4595 | 84 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12296 | 18358 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 6382 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 722 of 4893 channels in the main playlist (4171 without a guide). Measured, with no rounding up.
- 5662 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2034 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
