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

- `EEquityMap.html`, `EEquityMap.dc.html` — self-contained prototype builds
  (open directly in a browser, no build step required)
- `support.js` — supporting application logic
- `EEquityMap_SoT.md` — source-of-truth notes on design decisions and
  open issues
- `screenshots/` — UI screenshots from prototype sessions
- `uploads/` — reference captures used during design iteration

## Running it

These are static, bundled HTML files with no dependencies or build tooling.
Open `EEquityMap.dc.html` (or `EEquityMap.html`) directly in a modern
browser.

## License

MIT — see [`LICENSE`](LICENSE).
