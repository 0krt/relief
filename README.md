# RELIEF // PARAMETRIC LINE FIELD

A fictional relief map drawn entirely in lines. A seeded fractal terrain is eroded by
actual droplet simulation, then rendered as pure polyline geometry — contours, stacked
profiles, wire mesh, flow lines or hachures — with elevation nodes you place by hand.

Single-file HTML, no build step, no dependencies. Open `index.html` in a browser.

## Line modes

- **contour** — marching-squares isolines, with index contours, bathymetry and a weighted coastline
- **profile** — stacked ridgeline profiles with hidden-line occlusion, optional cross wires and a cut strata block
- **flow** — streamlines following the downhill gradient; rotate the comb angle toward 90° to run them across the slope instead
- **hachure** — Lehmann's rule: strokes point downhill and get heavier as the ground steepens

## Ground

Fractal noise (fbm / ridged / billow) with domain warping, a radial landmass mask and a
regional tilt. Droplet erosion carries sediment downhill and drops it where the slope
flattens — the asymmetry is what cuts branching valleys that pure noise never has.

## The profile plate

Every point of the stack — the sections, the mesh rungs, the beds of the block — is
placed by one projection, so the whole thing stays welded together however it is moved.

- **cross wires** turns the stack into a square wire mesh, hidden by the same floating
  horizon the sections use. *square cells* spaces the rungs to match the rows on screen.
- **plate shape** — width, convergence of the far rows, how the depth is eased, plus a
  turn and a zoom that decide which ground the sections are cut through and at what angle.
- **strata** cuts the front of the plate away and draws the ground under it as beds.
  Each bed carries the shape of the surface above it, flatter the deeper it lies, with a
  fold of its own; *flattening* slides between beds of constant thickness and beds that
  level off onto the floor. Shear the plate to swing a flank into view.

## Rivers, paths and the sea

Plan views only — these live in map space.

- **fill sea** floods the page and cuts the land back out of it, so a lake inside an
  island comes out as water again. Real geometry, not a raster.
- **rivers** come out of the flow accumulation: the hollows the erosion left are flooded
  first so nothing drains into a dead end, then every cell sends its water to its lowest
  neighbour and the channels appear where enough of it has collected. A reach that runs
  into a filled hollow stops at the shore — that is a lake, not a channel.
- **paths** are least-cost routes between a shoreline and a summit, or between two far
  shores. The cost of a step is its length times a penalty growing with the square of the
  grade, so turning *grade aversion* up breaks the route into switchbacks on its own.

## The pen

Two separate things push a line off its ideal path: the tremor of the hand, which runs
along the stroke and belongs to the line, and the drift of arm and paper, which belongs
to the place on the sheet and so is shared by every line crossing it. *hand drift* mixes
between them — it is the second one that makes neighbouring contours bend together
instead of each wandering off alone.

## Controls

Click the plate to drop an elevation node, shift+click for a basin. Drag a node to move it,
drag its ring to resize, wheel over it to change height, alt+click to delete.

| key | action |
| --- | --- |
| `R` | roll seed |
| `H` | toggle handles |
| `C` | clear nodes |
| `1`–`4` | line mode |
| `M` | wire mesh |
| `S` | fill sea |
| `D` | drift |
| `space` | re-render |
| `del` | delete selected node |
| `esc` | deselect |

## Export

The map is a set of polylines, not an image, so the SVG export is real geometry — grouped
by stroke weight, dashed where the paths are dashed, the sea a single even-odd path,
ready to plot, cut or engrave. PNG export is also available. The JSON carries every
parameter and node, so a map you like comes back exactly.
