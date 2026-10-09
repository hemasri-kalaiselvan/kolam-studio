# Kolam Studio

> A web app that draws traditional South Indian kolam patterns on a grid of dots (pulli), in Sikku, Padi, Shape, and Hridaya styles.

**Tech:** HTML, CSS, JavaScript
**Tools:** GitHub
**AI Tools:** Claude

## About

- A small web app that draws kolam patterns on a grid of dots (pulli). Open `index.html` in any browser — no build step, no dependencies.
- Four modes: **Sikku Kolam** (interlacing loops), **Padi Kolam** (stencil-style bands and lotus motifs), **Shape Kolam** (line and leaf patterns drawn over a visible dot layout), and **Hridaya Kolam** (a radial, single-stroke kolam).
- A tutorial-style animation: the dots appear one at a time, row by row, then every line is drawn in turn, one stroke after another, so viewers can follow how the kolam is drawn. Choose Slow, Normal or Fast, and press **Replay animation** to run it again.
- Multiple dot layouts and styles per mode, and a one-click download of the finished pattern as an image.

### Sikku Kolam (default)

- Interlacing loops with knot-like turns. Every dot is wrapped in its own closed loop. Each edge between two neighbouring dots gets one shared random on/off bit. Where an edge is on, both neighbouring loops grow a pointed tip that meets at the edge, so the lines cross and interlace.
- The bits are mirrored left–right (and top–bottom when the layout allows), so every pattern is symmetric, and because neighbours share the same bit they can never disagree about a shared edge.
- **Dot layouts** decide which dots exist. Size is the number of dots in the widest row.
  - **Square** – n × n dots.
  - **Diamond** – rows of 1, 3, 5 … n … 5, 3, 1 dots (for example the "7 to 1 dots" diamond).
  - **Ascending** – rows of 1, 3, 5 … n dots (a triangle pointing up).
  - **Descending** – rows of n … 5, 3, 1 dots (a triangle pointing down).
  - **Cross** – a plus-shaped layout.
- Diamond, Ascending and Descending need an odd size, so an even size is rounded up.

### Padi Kolam

- Stencil-style kolam, as on the square padi boards: white lines on a blue board, no corner dots. Every pattern is generated from a random seed, so **New pattern** always gives a different one, and toggling the animation redraws the same pattern. Each pattern is scaled to fill the board.
- **Styles** (the menu, or "Surprise me" for a random one):
  - **Frame & loops** – a curved square frame of parallel bands, paired concentric loops on each side, lotus buds, fans or curls at the corners.
  - **Pinwheel** – four bands that each end in a spiral hook.
  - **Lotus corners** – straight bands, a diamond "eye", lotus fans at the corners and a small crown on each side.
  - **Triangle arms** – a frame with stacked triangles on each side and a lotus bud at each corner.
  - **Interlaced rosette** – 4 to 8 loops, each woven over and under its neighbours, with lotus buds or dots at the tips.
  - **Lattice star** – a 4, 6 or 8-sided frame of straight bands (sometimes a second one turned half a step, which makes a star), with net-like lattice petals.
  - **Centre motif** (all styles) – 8-point star, rings, four-petal flower, lotus or double spiral.

### Shape Kolam

- Pulli kolam: a lattice of dots in a fixed arrangement, with the pattern drawn over it. White lines on black; the dots are small and amber with a thin dark rim, so they stay visible without crowding the lines.
- **Dot layout**: Diamond (rows of 1, 3, 5 … n … 5, 3, 1), Square, Hexagon (staggered rows) or Star (six-pointed star). **Size** sets how many dots.
- **Pattern style**: Flowers (leaf-shaped petals and scallops), Stars (star polygons and spokes), Diamonds, or "Surprise me". **New pattern** gives a different one each time.
- Every line starts and ends exactly on a dot, and every dot is touched or enclosed by the pattern.
- **Show dots** switches the dots on or off.

### Hridaya Kolam

- A radial, single-stroke kolam. It follows the published algorithm from Chakraborty & Manna, *Extending Hridaya Kolam to Even-Ordered Dot Patterns* ([arXiv:2507.02874](https://arxiv.org/abs/2507.02874)): a modular-arithmetic sequence (a₀ = m, aₖ = (k·n) mod m) is repeated over n arms and plotted in polar coordinates.
- When gcd(m, n) = 1 the line closes into one unbroken loop, so the app only offers arm counts coprime to the number of dots per arm.

## How AI Helped

- **Claude** — design direction, the pattern-generation code, geometry and debugging, and deployment.
- Designed the dot-lattice Shape Kolam: dot layouts on square and triangular lattices, and ring-based patterns whose every line starts and ends on a dot.
- Worked through the weaving problem: making bands and rosette loops pass convincingly over and under each other with a single drawing pass, so every crossing alternates all the way round.
- Implemented the published Hridaya Kolam algorithm from the referenced paper, including the coprime-arm constraint that keeps the single stroke closed.

## Credits

- Sikku inspired by [zen-kolam](https://github.com/Crazzygamerr/zen-kolam). The tile idea (each dot in a loop, with tips toward connected sides) was learned from studying that project. No code or data from it is used here; the curves and the Kolam Studio code are written from scratch.
- Hridaya Kolam: Chakraborty & Manna, [arXiv:2507.02874](https://arxiv.org/abs/2507.02874).

---

## Tech notes

- **No build, no dependencies.** A single `index.html` — open it locally, or deploy on GitHub Pages (push `index.html` and `README.md`, then Settings → Pages → deploy from the main branch).
- **All drawing is written from scratch** — no code, data or images from any other project.
  - **Curves as points.** Every line is a list of points along a curve. Bands and petals use quadratic and cubic Bézier curves; loops use a polar formula.
  - **Bands.** A band is one centre curve copied sideways at equal steps, which makes the parallel strands. Where two bands cross, the strands make small lattice cells.
  - **Spirals and curls.** A "turtle" walks forward while its turning rate increases, which tightens the path into a spiral. Pinwheel hooks and scroll curls are made this way.
  - **Loops.** A rosette loop is two edges at +angle and −angle from the same radial line. The angle rises and falls as sin^0.7, so both ends are rounded and the edges pinch together. Neighbouring loops cross twice.
  - **Symmetry.** One quarter (or one N-th) of the design is drawn and rotated to make the rest.
  - **Randomness.** A seeded random-number generator picks sizes, strand counts, curvature and ornaments, so a seed always redraws the same pattern.
  - **Weaving.** Each band is drawn with a thick halo in the board colour behind it. The halo hides whatever lies beneath, so the band passes over it. Drawing order decides over and under. A ring of bands cannot alternate with a single order, so the start of the first band is drawn again on top, and the weave then alternates all the way round. In the rosette each loop edge is split at its widest point into an inner and an outer half; the "under" halves are drawn first and the "over" halves after, which makes every crossing alternate.
  - **Opaque petals.** Petals are filled with the board colour so they cover the lines beneath them.
  - **Lattice petals.** A net is made from nested petal outlines plus straight rungs across them.
  - **Fitting.** Each finished pattern is measured and scaled to fill the board, with the line weight adjusted to stay constant.
  - **Download.** The image keeps the halos and fills, so the weave is preserved.
  - **Tutorial animation.** After each drawing, `sequenceAnimation` gives every dot a pop-in delay (sorted row by row, left to right), then gives every line its own delay so the strokes are drawn one after another. A stroke's duration is its length divided by the board width times a speed rate, so long lines take longer; a cap scales everything down if a pattern would run too long. Padi halos and petal fills appear just before their lines, so the weave builds up in the right order. Slow, Normal and Fast change the rates, and **Replay animation** redraws the same pattern from the start. The downloaded image has none of the animation state.
  - **Shape Kolam dot layouts.** `dotLattice` makes the dot set for each layout. Square and Diamond use integer points. Hexagon and Star use a triangular lattice (x = q + r/2, y = r·√3/2); the Star keeps the points inside either of two overlapping triangles.
  - **Shape Kolam patterns.** The pattern is made from rings of dots round a centre dot: 8 dots on the square lattice, 6 on the triangular one. On a ring of radius k, an element is one of: a polygon through neighbouring ring dots, a polygon skipping one dot (a diamond, or the two triangles of a hexagram), a star polygon, spokes from the centre dot, leaf-shaped petals from the centre dot to each ring dot, or scallops (arcs bulging outward between neighbouring ring dots). Up to three rings (k from 1 to 5) round the centre each get a random element; long star and spoke lines are used only on the two smallest rings, so the drawing stays open.
  - **Shape Kolam filling.** Every dot still untouched, nearest the centre first, gets a small ring element of one chosen type, repeated at its rotated positions, so the whole layout is covered evenly. Dots on the edge have no complete ring, so each gets a leaf pointing to its neighbour nearest the centre, which makes a petal fringe. An element is skipped if any of its dots is missing from the layout, and the finished drawing is scaled to fill the board.
- **How many variations.** For Sikku (square layout), the number of patterns at size N is 2 raised to the number of independent edge bits: about 4.3 billion at size 8, about 4.7 × 10²¹ at size 12, and about 3.4 × 10³⁸ at size 16. Hridaya has 94 valid (m, n) combinations × 3 connection styles.
- **Known limits (Padi).** The patterns imitate the look of traditional padi kolam but are not copies of traditional designs. Not yet included: the conch (shankh) centre, dotted grid cells, and one-line petal drawings with veins.
- **Known limits (Shape).** Birds, animals and lotus shapes are not included. They need to be designed dot by dot for each layout.
- **Planned.**
  - Kambi Kolam (straight-stroke style) — an earlier attempt was removed because it was not correct.
  - More dot layouts.
  - Padi Kolam: veined lotus petals, scroll-and-flower pinwheels and the conch centre.
  - Shape Kolam: birds, animals and lotus designed dot by dot.
- **Licence.** Not chosen yet — add a `LICENSE` file before sharing widely.
