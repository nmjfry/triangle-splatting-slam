# Triangle Splatting SLAM — Project Page

![Teaser](teaser_readme.png)

Static project page for *Triangle Splatting SLAM* (Fry, Dexheimer, Mazur, Kelly, Davison — Imperial College London).

A single static HTML site, ready to deploy to GitHub Pages with no build step.

## Deploy to GitHub Pages

1. Push this folder to a GitHub repository, e.g. `triangle-splatting-slam`.
2. **Settings → Pages**, set source to `main` / `/ (root)`.
3. Page goes live at `https://<your-username>.github.io/triangle-splatting-slam/`.

For a personal landing page (`<username>.github.io`), name the repo `<username>.github.io` and the page lives at the root URL.

The `.nojekyll` file in the root is included so GitHub Pages serves files as-is, with no Jekyll processing.

## Files

| File                      | Purpose                                                                       |
| ------------------------- | ----------------------------------------------------------------------------- |
| `index.html`              | Main page with the Delaunay-triangulated title.                               |
| `index-serif.html`        | Alternate version with a plain sans-serif title (Distill-style).              |
| `assets/method.webp/.jpg` | System diagram figure. WebP for modern browsers, JPEG fallback.               |
| `assets/method.pdf`       | Vector source of the system diagram (kept for camera-ready use).              |
| `assets/teaser.webp/.jpg` | Teaser image at the top of the Abstract.                                      |

Pick one of the two HTML files as your `index.html`. To switch between them, just rename:

```bash
# Use the Delaunay-triangulated title (currently the default)
mv index.html index-delaunay.html
mv index-serif.html index.html
```

## Replacing placeholders

Open the chosen `index.html` and search for these to swap real content in:

- `href="#"` on the four hero buttons → real URLs (paper, code, demo, BibTeX). The fourth button is an in-page anchor to `#cite` so leave that one.
- `assets/teaser.mp4` → drop your teaser video at this path. The placeholder `<div>` inside `.teaser-frame` should be replaced with `<video src="assets/teaser.mp4" autoplay loop muted playsinline poster="assets/teaser.jpg"></video>`.
- The `id="video"` section's placeholder → YouTube embed or `<video>` tag for the long walkthrough video.
- The six `.tile` placeholders inside `#results` → real images, e.g. `<img src="assets/render_rgb.jpg" alt="…">` inside each tile.
- The `@article{...}` block in `#cite` → final BibTeX once the paper has a venue and arXiv ID.

## Design notes

- **Fonts:** Instrument Serif (display in `index.html`'s body), IBM Plex Sans (sans body + section headings), JetBrains Mono (small UI labels, BibTeX), and Archivo Black (only used in `index.html` to rasterise the title before triangulation — the actual rendering is SVG).
- **Colour:** white paper, dark ink, with pink (`#EC4899`) and blue (`#3B82F6`) accents drawn from the surface-normal renderings in the paper. Variables live at `:root` in the CSS for easy retuning.
- **Hero mesh band** (top of the page): generated client-side. Each vertex carries its own pair of oscillators with random frequency/phase/amplitude, so vertices wander quasi-independently. The band regenerates on viewport resize.
- **Delaunay title** (`index.html` only): the title is rasterised on a hidden canvas, sample points are drawn from the boundary and a jittered interior grid, then [`delaunator`](https://github.com/mapbox/delaunator) (loaded from a CDN) computes a real 2D Delaunay triangulation. Triangles whose centroid lands outside a letter are discarded. Falls back to plain serif text if the CDN ever fails.

No build step, no JS framework, no tracking, no analytics.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```
