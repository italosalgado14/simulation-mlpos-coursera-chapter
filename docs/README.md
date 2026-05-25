# Problem Framing & Algorithm Design — GitHub Pages site

A small two-page static site:

1. **Problem Framing for ML & Computer Vision** — understanding the problem,
   deciding whether ML is the right approach, defining the model's goal and
   success metrics, with a section connecting it to computer vision problems
   (classification, object detection, segmentation, defect/anomaly detection).
   Adapted from Google's
   [ML Problem Framing](https://developers.google.com/machine-learning/problem-framing/problem-framing)
   guide.
2. **Algorithm Design Techniques** — brute force, decrease/divide/transform &
   conquer, greedy, dynamic programming, backtracking, branch & bound,
   randomized, and approximation algorithms, with inline SVG diagrams.

This folder is named `docs/` because **GitHub Pages can publish a site directly
from a `/docs` folder** — no build step, no Jekyll, no workflow needed.

## Files

| File | Purpose |
|---|---|
| `index.html` | Part 1 — Problem Framing. Landing page. Self-contained, inline CSS. |
| `algorithm-design.html` | Part 2 — Algorithm Design Techniques, with SVG diagrams. |
| `.nojekyll` | Tells GitHub Pages to serve files as-is (skip Jekyll processing). |
| `README.md` | This file. |

The two pages cross-link via a sticky nav bar at the top.

## View locally

```bash
cd docs
python -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

Because this is plain static HTML in a `/docs` folder, deployment is just a
settings toggle:

1. Make sure `docs/` lives at the **root of a GitHub repository**, then push it.
2. In the repo: **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Select your branch (e.g. `main`) and folder **`/docs`**. Save.
5. The site goes live at `https://<username>.github.io/<repository>/` within a
   minute or two. `index.html` is served automatically as the landing page.

> **Heads-up:** this `docs/` folder currently sits inside
> `simulation-mlops-coursera/`, which is **not** itself a git repository (the two
> Coursera labs next to it are separate repos). To use the steps above, this
> `docs/` folder must be at the root of whatever repo you push to GitHub. If you
> want, the folder can instead become its own repo, or move to the root of an
> existing one.
