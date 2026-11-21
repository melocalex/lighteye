# Status dashboard

Generated at **2026-11-21T14:46:54.976Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12842 |
| Streams discovered (unique URLs) | 19420 |
| Duplicate URLs collapsed | 11567 |
| Channels with no metadata in the source | 2350 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11290 | 2318 |
| UNSTABLE | 322 | 87 |
| OFFLINE | 4308 | 1160 |
| TIMEOUT | 321 | 201 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 740 | 142 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8453 |
| UNSTABLE | 202 |
| OFFLINE | 0 |
| TIMEOUT | 193 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3920 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 914 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7741 |
| Of those, status WORKING | 7562 |
| Of those, unstable | 179 |
| Brazilian channels | 291 |
| International channels | 7450 |
| With an alternative stream kept | 2411 |
| With guide coverage | 940 |
| With **verified** Portuguese audio | 55 |
| With **verified** subtitles | 1224 |
| With unreliable subtitle detection (HLS) | 7590 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4342 |
| LATAM | 1307 |
| US | 1269 |
| BR | 291 |
| ES | 202 |
| UK | 157 |
| INT | 133 |
| PT | 40 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2789 |
| Notícias | 787 |
| Cultura | 704 |
| Entretenimento | 691 |
| Música | 608 |
| Filmes | 508 |
| Variedades | 383 |
| Infantil | 321 |
| Esportes | 314 |
| Séries | 298 |
| Documentários | 164 |
| Educação | 119 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7741 |
| Channels with a guide | 940 |
| Measured coverage | 12.14% |
| Programmes in the guide | 38097 |
| Horizon | 72 h |
| Channels without a guide | 6801 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 940 |
| Programmes | 29568 |
| Window | 24 h |
| Total size | 6685.9 KB |
| Largest file | 73.3 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 211 | 196 | 9550 | - |
| meuguia.tv | grabber | yes | 33 | 15 | 291 | - |
| meo.pt | grabber | yes | 52 | 52 | 1718 | - |
| nostv.pt | grabber | yes | 38 | 27 | 583 | - |
| gatotv.com | grabber | yes | 131 | 127 | 4267 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12086 | - |
| kanya-us | published | yes | n/a | 341 | 9602 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7741 | 7741 | 100 |
| `playlist-br.m3u` | Brazil | 291 | 291 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 346 | 346 | 33 |
| `playlist-movies.m3u` | Movies | 510 | 510 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 341 | 341 | 14 |
| `playlist-sports.m3u` | Sports | 316 | 316 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7450 | 7450 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12768 | 19323 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3920 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 940 of 7741 channels in the main playlist (6801 without a guide). Measured, with no rounding up.
- 8644 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2350 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
