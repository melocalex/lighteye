# Status dashboard

Generated at **2026-11-20T15:33:26.457Z**.

## Discovery

| Metric | Value |
| --- | --- |
| Channels discovered | 12823 |
| Streams discovered (unique URLs) | 19335 |
| Duplicate URLs collapsed | 11681 |
| Channels with no metadata in the source | 2348 |

## Validation

A validation result depends on where the probe runs. The GitHub Actions runner sits in the United States and sees different availability than a machine in Brazil. **Numbers from different observation points do not add up.**

| Status | github-us | local-br |
| --- | --- | --- |
| WORKING | 11268 | 2318 |
| UNSTABLE | 304 | 87 |
| OFFLINE | 4261 | 1159 |
| TIMEOUT | 327 | 201 |
| GEO_BLOCKED | 19 | 0 |
| AUTH_REQUIRED | 30 | 21 |
| UNSUPPORTED | 737 | 142 |
| UNKNOWN | 0 | 0 |

| Observation point | Last run | Checked | Eligible | Fresh (skipped) |
| --- | --- | --- | --- | --- |

## Status per channel

A channel's status is the best among its streams. `UNKNOWN` dominates because each run checks at most `validation.maxChecksPerRun` streams, and a channel with any stream not yet checked counts as unknown, not as unavailable.

| Status | Channels |
| --- | --- |
| WORKING | 8453 |
| UNSTABLE | 197 |
| OFFLINE | 0 |
| TIMEOUT | 196 |
| GEO_BLOCKED | 0 |
| AUTH_REQUIRED | 0 |
| UNSUPPORTED | 0 |
| UNKNOWN | 3904 |

## Published channels

These are the channels that enter the validated playlists: eligible **and** carrying metadata in the source. A stream with no channel metadata is never published, so it is not counted here — there were 912 such streams in this run, and they appear only in `playlist-all.m3u`.

| Metric | Value |
| --- | --- |
| Channels published in the main playlist | 7738 |
| Of those, status WORKING | 7564 |
| Of those, unstable | 174 |
| Brazilian channels | 294 |
| International channels | 7444 |
| With an alternative stream kept | 2403 |
| With guide coverage | 943 |
| With **verified** Portuguese audio | 54 |
| With **verified** subtitles | 1222 |
| With unreliable subtitle detection (HLS) | 7574 |

### By country group

| Group | Published channels |
| --- | --- |
| OTHER | 4330 |
| LATAM | 1310 |
| US | 1271 |
| BR | 294 |
| ES | 203 |
| UK | 157 |
| INT | 132 |
| PT | 41 |

### By category

| Category | Published channels |
| --- | --- |
| Geral | 2789 |
| Notícias | 785 |
| Cultura | 703 |
| Entretenimento | 690 |
| Música | 610 |
| Filmes | 502 |
| Variedades | 381 |
| Infantil | 322 |
| Esportes | 319 |
| Séries | 299 |
| Documentários | 163 |
| Educação | 120 |
| Animação | 55 |

## Programme guide

| Metric | Value |
| --- | --- |
| Channels requested from the guide | 7738 |
| Channels with a guide | 943 |
| Measured coverage | 12.19% |
| Programmes in the guide | 31839 |
| Horizon | 72 h |
| Channels without a guide | 6795 |

### Per-channel slices

A short guide in JSON, one file per channel, for consumers that do not parse XMLTV. Times are **epoch seconds, UTC**. The index publishes the ready-made file name, so a consumer never has to reproduce any `tvg-id` transformation.

| Metric | Value |
| --- | --- |
| Files | 943 |
| Programmes | 26120 |
| Window | 24 h |
| Total size | 5951.5 KB |
| Largest file | 84.2 KB |
| Index | `https://raw.githubusercontent.com/melocalex/lighteye/main/docs/epg/index.json` |

iptv-org's guide coverage is partial by nature. The number above is the **measured** one, with no rounding up. The full list of channels without a guide is in `data/epg-coverage.json`.

### Guide sources

| Source | Kind | OK | Channels requested | Channels with a guide | Programmes | Note |
| --- | --- | --- | --- | --- | --- | --- |
| mi.tv | grabber | yes | 211 | 196 | 9660 | - |
| meuguia.tv | grabber | yes | 33 | 18 | 189 | - |
| meo.pt | grabber | yes | 53 | 53 | 1760 | - |
| nostv.pt | grabber | yes | 38 | 26 | 526 | - |
| gatotv.com | grabber | yes | 131 | 126 | 4533 | - |
| pluto.tv | grabber | yes | 400 | 387 | 12237 | - |
| kanya-us | published | yes | n/a | 329 | 2934 | - |
| kanya-ke | published | yes | n/a | 0 | 0 | - |

## Playlists

| File | Contents | Channels | Entries | Sections |
| --- | --- | --- | --- | --- |
| `playlist.m3u` | Main (all sections, validated) | 7738 | 7738 | 100 |
| `playlist-br.m3u` | Brazil | 294 | 294 | 13 |
| `playlist-pt.m3u` | Portuguese (PT + Portuguese-speaking) | 350 | 350 | 33 |
| `playlist-movies.m3u` | Movies | 504 | 504 | 10 |
| `playlist-series.m3u` | Series (TV shows) | 343 | 343 | 14 |
| `playlist-sports.m3u` | Sports | 321 | 321 | 9 |
| `playlist-international.m3u` | International (everything outside Brazil) | 7444 | 7444 | 87 |
| `playlist-all.m3u` | Everything, including unverified and unavailable | 12750 | 19239 | 101 |

## Known limitations

- Results from 2 observation points (local-br, github-us) are reported separately and **do not add up**: the same stream can be WORKING at one and GEO_BLOCKED at another.
- 3904 channels have never been checked. The per-run cap is `validation.maxChecksPerRun` = 1200; coverage grows each round.
- Measured guide coverage: 943 of 7738 channels in the main playlist (6795 without a guide). Measured, with no rounding up.
- 8606 channels are served over HLS. ffprobe does not open the subtitle variant playlist, so **absence of a subtitle track does not prove absence of subtitles** on those channels.
- No playlist performs automatic failover. Alternative streams are kept in `data/channels.json`, but switching streams is up to the player: the project neither implements nor has tested failover, so it does not claim one exists.
- Nothing here circumvents DRM, authentication, paywalls or geoblocking. `GEO_BLOCKED` is only a diagnostic label, based on HTTP 451 or explicit text in the response.
- 2348 streams have no channel metadata in the source. They stay out of the validated playlists and appear only in `playlist-all.m3u`, because classifying by channel name would be guessing.
