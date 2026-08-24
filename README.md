# RELIEF // PARAMETRIC LINE FIELD

A fictional relief map drawn entirely in lines. A seeded fractal terrain is eroded by
actual droplet simulation, then rendered as pure polyline geometry — contours, stacked
profiles, flow lines or hachures — with elevation nodes you place by hand.

Single-file HTML, no build step, no dependencies. Open `index.html` in a browser.

## Line modes

- **contour** — marching-squares isolines, with index contours, bathymetry and a weighted coastline
- **profile** — stacked ridgeline profiles with hidden-line occlusion
- **flow** — streamlines following the downhill gradient; rotate the comb angle toward 90° to run them across the slope instead
- **hachure** — Lehmann's rule: strokes point downhill and get heavier as the ground steepens

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
by stroke weight, ready to plot, cut or engrave. PNG export is also available. The JSON
carries every parameter and node, so a map you like comes back exactly.
