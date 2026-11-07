# Status dashboard

Generated at **2026-11-07T13:06:53.289Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12458 |
| Streams discovered (unique URLs) | 18602 |
| Duplicate URLs collapsed | 11439 |
| Channels with no metadata in the source | 2096 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 9078 | 2374 |
| UNSTABLE | 86 | 86 |
| OFFLINE | 3560 | 1225 |
| TIMEOUT | 253 | 204 |
| GEO_BLOCKED | 16 | 0 |
| AUTH_REQUIRED | 23 | 21 |
| UNSUPPORTED | 441 | 144 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 6985 |
| UNSTABLE | 66 |
| OFFLINE | 0 |
| TIMEOUT | 175 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 5156 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 855 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 6196 |
| Of those, status WORKING | 6153 |
| Of those, unstable | 43 |
| Brazilian channels | 298 |
| International channels | 5898 |
| With an alternative stream kept | 1936 |
| With guide coverage | 808 |
| With **verified** Portuguese audio | 56 |
| With **verified** subtitles | 1101 |
| With unreliable subtitle detection (HLS) | 6080 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 3505 |
| US | 1020 |
| LATAM | 925 |
| BR | 298 |
| ES | 166 |
| UK | 128 |
| INT | 113 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2015 |
| Notícias | 658 |
| Cultura | 614 |
| Entretenimento | 601 |
| Música | 479 |
| Filmes | 451 |
| Variedades | 306 |
| Séries | 264 |
| Infantil | 263 |
| Esportes | 248 |
| Documentários | 135 |
| Educação | 99 |
| Animação | 63 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 6196 |
| Channels with a guide | 808 |
| Measured coverage | 13.04% |
| Programmes in the guide | 33270 |
| Horizon | 72 h |
| Channels without a guide | 5388 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 808 |
| Programmes | 25654 |
| Window | 24 h |
| Total size | 5977.1 KB |
| Largest file | 42.4 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 177 | 166 | 8248 | - |
| meuguia.tv | grabber | yes | 33 | 0 | 0 | - |
| meo.pt | grabber | yes | 47 | 47 | 1617 | - |
| nostv.pt | grabber | yes | 31 | 25 | 568 | - |
| gatotv.com | grabber | yes | 87 | 86 | 3201 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12056 | - |
| kanya-us | published | yes | n/a | 272 | 7580 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 6196 | 6196 | 100 |
| `playlist-br.m3u` | Brazil | 298 | 298 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 344 | 344 | 28 |
| `playlist-movies.m3u` | Movies | 453 | 453 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 307 | 307 | 14 |
| `playlist-sports.m3u` | Sports | 250 | 250 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 5898 | 5898 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12382 | 18504 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 5156 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 808 of 6196 channels in the main playlist (5388 without a guide). Measured, with no rounding up.
- 6936 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2096 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
