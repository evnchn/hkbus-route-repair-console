# HK Bus route-geometry repair console

An interactive console for comparing candidate repairs to defective bus-route polylines in
the Hong Kong bus data behind [hkbus.app](https://hkbus.app) /
[hk-independent-bus-eta](https://github.com/hkbus/hk-independent-bus-eta).

The government waypoint source (CSDI / Transport Department, mirrored via `route-waypoints`)
sometimes draws a spurious loop or detour on a route line. This tool shows, for each affected
route, what each candidate repair does to the geometry — next to a decision matrix (resulting
length, lines of code, where the fix runs, risk) so the trade-off is legible rather than buried
in a diff.

**Live:** pick a route from the left rail; toggle the candidate lines; read the cards on the right.

## The candidates
- **CSDI (raw)** — the source line, drawn as-is (the baseline / the defect).
- **#272 chord** — the shipped minimal client-side guard that collapses spurious *terminal* loops
  ([hkbus/hk-independent-bus-eta#272](https://github.com/hkbus/hk-independent-bus-eta/pull/272)).
- **pfaedle** — re-deriving the whole shape from the stop sequence + OpenStreetMap
  ([`ad-freiburg/pfaedle`](https://github.com/ad-freiburg/pfaedle) map-matching), which would run
  in the data pipeline rather than the client.

## Oracle overlay (official KMB / CTB lines)
The official operator geometry is **not bundled in this page**. Load it with the *“Load oracle
file”* control — the file is read **entirely in your browser and never uploaded**. It overlays the
official line (cyan) as the arbiter: a shorter repaired line is not automatically a correct one.

The oracle file is a GeoJSON `FeatureCollection`; each `Feature` is one route’s `LineString`
(`[lon,lat]`, WGS84) with `properties.tag` matching a route in this console.

## Data & attribution
- **Stops & raw waypoints:** Transport Department / CSDI, via data.gov.hk open data.
- **pfaedle shapes:** map-matched against **© OpenStreetMap contributors** data (ODbL 1.0) using
  `pfaedle`; the derived shapes are therefore ODbL.
- **Basemap tiles:** © OpenStreetMap contributors.

Built as a maintainer decision aid + a talk demo. Not affiliated with the Transport Department,
KMB, or Citybus.
