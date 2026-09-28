# RELIEF // PARAMETRIC LINE FIELD

A fictional relief map drawn entirely in lines. A seeded fractal terrain is eroded by
actual droplet simulation, then rendered as pure polyline geometry — contours, stacked
profiles, wire mesh, flow lines or hachures — with elevation nodes you place by hand.

Single-file HTML, no build step, no dependencies. Open `index.html` in a browser.

## Line modes

- **contour** — marching-squares isolines, with index contours, bathymetry and a weighted coastline
- **profile** — stacked ridgeline profiles with hidden-line occlusion, optional cross wires and a
  cut strata block, either flat on a shear or turned on a real camera
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
  horizon the sections use. *square cells* spaces the rungs to match the rows on screen,
  and *triangles* runs a diagonal through every cell so the ground reads as a triangulated
  surface rather than a grid.
- **plate shape** — width, convergence of the far rows, how the depth is eased, plus a
  turn and a zoom that decide which ground the sections are cut through and at what angle.
- **strata** cuts the front of the plate away and draws the ground under it as beds.
  Each bed carries the shape of the surface above it, flatter the deeper it lies, with a
  fold of its own; *flattening* slides between beds of constant thickness and beds that
  level off onto the floor. Shear the plate to swing a flank into view.
- **hatching** rules the beds through. *hatch angle* leans the strokes off the vertical and
  the style decides what they are — ruled, crossed, stippled, broken or herringbone — with
  *hatch every* choosing which beds carry it.

## The block as a solid

**3d block** takes the plate off the shear and hangs it on a camera: a yaw, a tilt and a lens.
**shift+drag** the sheet turns it, **shift+wheel** pulls the camera in, and the sliders do the
same thing by hand. A shift+click that never moves still plants a basin, so the one gesture
serves both.

With a camera on it there is no fixed front, so nothing can be hidden by a horizon buffer any
more. Instead the ground, the walls, the wires and the cover are cut into pieces, each carrying
how far off it is, and the whole lot is painted back to front:

- **opaque body** fills the ribbon between one row and the next with the page itself. That is
  what stops the far flank, the beds of the back wall and the underside showing through the
  ground standing in front of them. Turn it off and the block goes back to being a wireframe.
- **opaque faces** does the same for the walls of the cut. *body tone* lifts both fills off the
  page toward the dark end of the ramp, so at 0 the fill is pure occlusion and nothing but the
  lines show.
- **block plan** cuts the same stack to a disc instead of a rectangle. Every row becomes a
  chord and the two flanks together walk the wall the whole way round — a core rather than a
  block. On a camera the footprint is round; flat on the sheet it foreshortens into the ellipse
  an oblique view should give.
- the walls are cut into chunks across as well as along, the more the block has been swung
  round, so a wall seen nearly edge-on still interleaves correctly with the ground over it.
- everything that lies on a piece of ground — its rungs, the cover standing on it, the rows
  along its edges — is filed at exactly that piece's depth, so it is painted straight after
  the fill it stands on and never underneath it. The hatching is likewise laid out on one
  lattice belonging to the whole wall and only then cut to the chunk, so a wall split into
  sixty pieces is ruled exactly as densely as one drawn whole.

## Taking the plan view off the page

Contour, flow and hachure are drawn flat and can then be moved somewhere else. Both
projections turn on the same camera as the block, so **shift+drag** orbits them and
**shift+wheel** pulls the camera in.

- **globe** wraps the sheet round a sphere and draws the near half, cutting every line at
  the limb. *wrap* decides how much of the world the sheet covers, *relief bulge* lifts the
  land off the surface, and the **graticule** draws meridians and parallels on the sphere
  itself rather than on the ground, so the net stays clean whatever the relief does. With
  the sea filled, the water is the disc of the planet with the land cut back out of it.
- **relief** lays the same drawing on the ground it describes. The ground is rasterised
  once into a small depth buffer and every point of the drawing is asked whether anything
  stands between it and the camera — so a contour behind a ridge goes behind the ridge, and
  a line nothing hides stays whole. Sorting cannot answer that one: a contour wanders all
  over the plate, and cutting it small enough to sort makes tens of thousands of pieces and
  is still only approximately right. With **opaque ground** on, the ribbons under the drawing
  are painted in, and a ribbon under the waterline takes the sea's colour, which is what
  makes a drowned coast read without a separate sea to cut out.

## Line work

The same geometry, broken into marks along its own arc length: **dots**, **dashes**, **crosses**,
or a **dither** that thins the marks out toward the light end of the ramp — the tone of a line
becoming the density of its stipple. Every mark is still real geometry and still exports; a
stippled contour costs one path, not four hundred.

## Colour

The palette is only the starting point. Every colour it carries — page, ink, sea, node accent and
the four stops of the ramp — has a picker under *Line style*; touching any of them turns the palette
into **custom**, seeded from whichever one was showing, and a custom sheet saves and loads with the
rest of the parameters.

- **ramp** swaps the palette's four stops for a false-colour scale: turbo, spectral, viridis,
  hypsometric, thermal, rainbow or ice. The strip under the menu shows the ramp exactly as it will
  be laid down.
- **colour bands** steps the ramp into that many flat classes; **ramp low / high** decide which
  part of the ground the scale is spread across, **ramp gamma** pushes the colour toward one end,
  and **invert ramp** runs it the other way.
- **false colour fill** paints the ground itself with the ramp, under the lines. On the flat sheet
  it is the grid, one pixel a cell; on the globe, the drape and the profile stack it is real
  polygons cut into runs of one colour, painted back to front with everything else, so it hides
  what stands behind it. *fill opacity* mixes it into the page. `F` toggles it.

## Characters

A limited character set standing in for the ink, beside the dots, dashes and dither of the line work.

- **along the lines** sets glyphs on the drawing at a fixed step of arc length. Which glyph is
  chosen by the tone of the line, so with the digits a contour reads out its own height; with
  **spell in order** the set is written along the line instead, and **follow line** turns each
  glyph with it.
- **character grid** renders the whole picture — lines, fills, sea — off screen and reads it back
  through a grid of cells. Each cell takes the character whose weight matches the ink under it and
  the colour of that ink.
- the sets run light to dark: digits, an ascii ramp, blocks, dots, a survey set of `3 4 ▲ ●`,
  squares, braille, binary, hex, or one of your own. `G` cycles the mode.

The SVG keeps the characters as text, so a plotter font or an editor can take them over.

## Elevation overlay

**elevation 0–9** scatters the height of the ground across the sheet as a digit. *range low* is 0
and *range high* is 9, the ground between them split evenly; with **from sea level** on the range
is measured over the land, so 0 is the coast. The digits sit on the ground in every view — the flat
sheet, the globe, the drape and the block — and are hidden by whatever stands in front of them. The
digit colour follows the ramp, the ink, or goes black or white against whatever is under it. `E`
toggles it.

## Vegetation

Symbols scattered where the ground, the slope and a moisture field all allow them: marsh on the
wet flats, broadleaf up to the *tree line*, conifer under it, scrub on what is too steep for
either, or one cover chosen by hand. *patchiness* clumps them into woods instead of spreading
them evenly. It rides on the plate as well as the map, so the 3d block comes up wooded, with the
ground in front painting over whatever stands behind it.

## MIDI

The cursor is the instrument. With a Web MIDI output picked, **height** plays the ground under
the pointer on the channel you set, and **coast** plays how far that point lies from the water
on the next channel up. The coast voice is rooted on a **C**: stand in the sea and it is a plain
C, walk inland and it climbs the scale. That distance is a real distance transform of the
coastline, not the height, so a low plain far from any shore still sings high. Both voices send
a continuous controller alongside the note — mod wheel for height, expression for the coast —
and everything is silenced the moment the pointer leaves the sheet.

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
| `B` | 3d block |
| `V` | vegetation |
| `X` | cycle line work |
| `P` | projection — sheet, globe, relief |
| `G` | characters — off, along lines, grid |
| `E` | elevation digits |
| `F` | false colour fill |
| `shift+drag` | turn the block, the globe or the drape |
| `shift+wheel` | camera zoom |
| `D` | drift |
| `space` | re-render |
| `del` | delete selected node |
| `esc` | deselect |

## Export

The map is a set of polylines, not an image, so the SVG export is real geometry — grouped
by stroke weight, dashed where the paths are dashed, the sea a single even-odd path,
ready to plot, cut or engrave. PNG export is also available. The JSON carries every
parameter and node, so a map you like comes back exactly.

Presets are the parameters without the sheet. **export preset** writes the current settings out
under the name you give them and adds them to the list; **import preset** reads back one preset,
an array of them, or a whole saved sheet, and they join the dropdown under *my presets* and stay
there between visits. **export all** writes everything you have imported or saved as one file.
Unknown keys are ignored on the way in, so a preset from an older sheet still loads.
