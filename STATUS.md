# Status dashboard

Generated at **2026-11-04T14:35:45.602Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12365 |
| Streams discovered (unique URLs) | 18454 |
| Duplicate URLs collapsed | 11399 |
| Channels with no metadata in the source | 2031 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 7616 | 2381 |
| UNSTABLE | 91 | 86 |
| OFFLINE | 2832 | 1236 |
| TIMEOUT | 220 | 204 |
| GEO_BLOCKED | 9 | 0 |
| AUTH_REQUIRED | 22 | 21 |
| UNSUPPORTED | 386 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 6030 |
| UNSTABLE | 65 |
| OFFLINE | 0 |
| TIMEOUT | 169 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 6025 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 5240 |
| Of those, status WORKING | 5198 |
| Of those, unstable | 42 |
| Brazilian channels | 298 |
| International channels | 4942 |
| With an alternative stream kept | 1617 |
| With guide coverage | 766 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 961 |
| With unreliable subtitle detection (HLS) | 5145 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 2857 |
| US | 898 |
| LATAM | 800 |
| BR | 298 |
| ES | 144 |
| UK | 110 |
| INT | 90 |
| PT | 43 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 1624 |
| Notícias | 583 |
| Cultura | 546 |
| Entretenimento | 520 |
| Música | 393 |
| Filmes | 389 |
| Variedades | 274 |
| Séries | 244 |
| Infantil | 220 |
| Esportes | 199 |
| Documentários | 125 |
| Educação | 90 |
| Animação | 33 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 5240 |
| Channels with a guide | 766 |
| Measured coverage | 14.62% |
| Programmes in the guide | 31308 |
| Horizon | 72 h |
| Channels without a guide | 4474 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 766 |
| Programmes | 24646 |
| Window | 24 h |
| Total size | 5840.1 KB |
| Largest file | 48.6 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 167 | 156 | 8217 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 45 | 45 | 1547 | - |
| nostv.pt | grabber | yes | 31 | 20 | 451 | - |
| gatotv.com | grabber | yes | 78 | 64 | 1880 | - |
| pluto.tv | grabber | yes | 400 | 388 | 12381 | - |
| kanya-us | published | yes | n/a | 249 | 6832 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 5240 | 5240 | 97 |
| `playlist-br.m3u` | Brazil | 298 | 298 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 344 | 344 | 26 |
| `playlist-movies.m3u` | Movies | 391 | 391 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 273 | 273 | 12 |
| `playlist-sports.m3u` | Sports | 201 | 201 | 8 |
| `playlist-international.m3u` | International (everything outside Brazil) | 4942 | 4942 | 84 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12289 | 18356 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 6025 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 766 of 5240 channels in the main playlist (4474 without a guide). Measured, with no rounding up.
- 5998 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2031 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
