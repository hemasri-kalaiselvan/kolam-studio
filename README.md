# Kolam Generator

> A small web app that draws kolam patterns on a grid of dots (pulli). 
 
**Tech:** HTML, JavaScript, CSS
**Tools:** GitHub
**AI Tools:** Claude

## About

A small web app that draws kolam patterns on a grid of dots (pulli). Open `index.html` in any browser — no build step, no dependencies.
 
## Modes

### Sikku Kolam (default)
Interlacing loops with knot-like turns. Every dot is wrapped in its own closed loop. Each edge between two neighbouring dots gets one shared random on/off bit. Where an edge is on, both neighbouring loops grow a pointed tip that meets at the edge, so the lines cross and interlace. The bits are mirrored left–right (and top–bottom when the layout allows), so every pattern is symmetric, and because neighbours share the same bit they can never disagree about a shared edge.

**Dot layouts** decide which dots exist. Size is the number of dots in the widest row.
- **Square** – n × n dots.
- **Diamond** – rows of 1, 3, 5 … n … 5, 3, 1 dots (for example the "7 to 1 dots" diamond).
- **Ascending** – rows of 1, 3, 5 … n dots (a triangle pointing up).
- **Descending** – rows of n … 5, 3, 1 dots (a triangle pointing down).
- **Cross** – a plus-shaped layout.

Diamond, Ascending and Descending need an odd size, so an even size is rounded up.

### Hridaya Kolam
A radial, single-stroke kolam. It follows the published algorithm from Chakraborty & Manna, *Extending Hridaya Kolam to Even-Ordered Dot Patterns* ([arXiv:2507.02874](https://arxiv.org/abs/2507.02874)): a modular-arithmetic sequence (a₀ = m, aₖ = (k·n) mod m) is repeated over n arms and plotted in polar coordinates. When gcd(m, n) = 1 the line closes into one unbroken loop, so the app only offers arm counts coprime to the number of dots per arm.

## How many variations?
For Sikku (square layout), the number of patterns at size N is 2 raised to the number of independent edge bits: about 4.3 billion at size 8, about 4.7 × 10²¹ at size 12, and about 3.4 × 10³⁸ at size 16. Hridaya has 94 valid (m, n) combinations × 3 connection styles.

## Planned
- Kambi Kolam (straight-stroke style) — an earlier attempt was removed because it was not correct.
- More dot layouts.

## Run it
- Locally: open `index.html`.
- On GitHub Pages: push `index.html` and `README.md`, then Settings → Pages → deploy from the main branch.

## Credits
- Inspired by [zen-kolam](https://github.com/Crazzygamerr/zen-kolam). The tile idea (each dot in a loop, with tips toward connected sides) was learned from studying that project. No code or data from it is used here; the curves and generator are written from scratch.
- Hridaya Kolam: Chakraborty & Manna, arXiv:2507.02874.

## Licence
Not chosen yet — add a `LICENSE` file before sharing widely.
