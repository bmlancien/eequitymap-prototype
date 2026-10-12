# EEquityMap

A prototype for exploring **consensus-based siting of wind and solar (PV)
expansion** across multiple, independently selectable planning principles
(e.g. proximity to consumption, population, infrastructure, economic
efficiency).

Instead of scoring sites against a single weighted criterion, EEquityMap
computes a **consensus score per grid cell as the minimum across all
selected principles** — a cell only counts as "consensus" if *every* chosen
principle recommends it. This surfaces where expansion is broadly
defensible versus where it depends on trade-offs between competing goals,
and identifies the specific principle that is "blocking" consensus in any
given area.

Core views include a consensus map, a "what's limiting?" map (which
principle is the bottleneck per cell), per-principle comparison maps,
principle overlap/agreement analysis, and comparisons of the consensus
surface against official designated areas or other reference distributions.

## Status

Early-stage prototype. Interface and terminology are still being iterated
on — see [`EEquityMap_SoT.md`](EEquityMap_SoT.md) for the current source of
truth on design decisions, open questions, and terminology.

## Contents

- `EEquityMap.dc.html` — the prototype (no build step required)
- `support.js` — runtime that renders the prototype
- `vendor/` — React, ReactDOM and Babel, served locally (nothing is loaded
  from third-party servers)
- `fonts/` — IBM Plex Sans and IBM Plex Mono with their licence
- `gemeinden/`, `landkreise.topo.json` — map geometry
- `impressum.html`, `datenschutz.html` — legal pages (drafts with placeholders)
- `eequitymap-logo.png` — logo used in the header
- `EEquityMap_SoT.md` — source-of-truth notes on design decisions and
  open issues
- `screenshots/` — UI screenshots from prototype sessions
- `uploads/` — reference captures used during design iteration

## Running it

The prototype is a set of static files with no build tooling. Serve the
folder with any static file server and open `EEquityMap.dc.html` in a modern
browser, for example:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000/EEquityMap.dc.html`. Opening the file
straight from disk does not work reliably, because the map data is loaded
with `fetch`.

## License

MIT — see [`LICENSE`](LICENSE).
