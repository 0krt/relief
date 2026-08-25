# RELIEF // PARAMETRIC LINE FIELD

A fictional relief map drawn entirely in lines. A seeded fractal terrain is eroded by
actual droplet simulation, then rendered as pure polyline geometry — contours, stacked
profiles, flow lines or hachures — with elevation nodes you place by hand. On top of the
linework sits an optional overlay: symbolic vegetation, a solid plate of sea, and the
gradient washes a printed sheet carries.

Single-file HTML, no build step, no dependencies. Open `index.html` in a browser.

## Line modes

- **contour** — marching-squares isolines, with index contours, bathymetry and a weighted coastline
- **profile** — stacked ridgeline profiles with hidden-line occlusion
- **flow** — streamlines following the downhill gradient; rotate the comb angle toward 90° to run them across the slope instead
- **hachure** — Lehmann's rule: strokes point downhill and get heavier as the ground steepens

## Overlay

- **vegetation** — the ground picks its own cover: marsh on the coastal flats, broadleaf low,
  conifer up to the treeline, then scrub and bare scree above it, thinning out on rock too steep
  to hold anything. a slow noise clumps the stands so they have an edge instead of an even
  sprinkle. the symbols are polylines like the rest of the plate, and in the profile stack they
  are projected onto their own row and dropped wherever a nearer ridge covers them.
- **gradient wash** — the one raster on the sheet, laid over or under the lines: *hypsometric
  tint* paints the palette ramp on the ground by height, *hillshade* rakes a sun of your own
  azimuth across the slopes, *aerial haze* thickens toward the far edge and *vignette* falls away
  at the rim.
- **solid sea** — everything under the waterline flooded with one opaque plate of the palette's
  water colour, clipped on the same crossings the coastline is drawn from, so the edge of the fill
  sits exactly under the coast. keep it under the lines and the bathymetry still reads over it;
  put it over them and the sea floor is buried. a plan-view layer — the profile stack has no water
  to fill.

## Ground

Fractal noise (fbm / ridged / billow) with domain warping, a radial landmass mask and a
regional tilt. Droplet erosion carries sediment downhill and drops it where the slope
flattens — the asymmetry is what cuts branching valleys that pure noise never has.

## Controls

Click the plate to drop an elevation node, shift+click for a basin. Drag a node to move it,
drag its ring to resize, wheel over it to change height, alt+click to delete.

| key | action |
| --- | --- |
| `R` | roll seed |
| `H` | toggle handles |
| `C` | clear nodes |
| `1`–`4` | line mode |
| `D` | drift |
| `space` | re-render |
| `del` | delete selected node |
| `esc` | deselect |

## Export

The map is a set of polylines, not an image, so the SVG export is real geometry — grouped
by stroke weight, ready to plot, cut or engrave. The vegetation symbols and the sea plate go
out the same way, as strokes and as one filled path; only the gradient wash has to ride along
as an embedded raster. PNG export is also available. The JSON carries every parameter and node,
so a map you like comes back exactly.
