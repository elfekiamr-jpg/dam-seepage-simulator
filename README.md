# Seepage Under Dam — Sheet Pile Cutoff Simulator

Interactive, single-file browser simulator of steady-state seepage beneath a dam founded on a spatially random, anisotropic soil. Drag sheet piles and the dam base directly on the plot, tune the hydraulic conductivity statistics, and watch the flow net, uplift pressure, and piping safety verdict update live.

**[Live demo →](https://elfekiamr-jpg.github.io/dam-seepage-simulator/)**

No build step, no dependencies, no install — it's one `index.html` file that runs entirely in the browser.

---

## What it does

- Solves the 2D steady-state groundwater flow equation, **∇·(K∇h) = 0**, on a finite-volume grid using a **Jacobi-preconditioned conjugate gradient** solver.
- Generates the hydraulic conductivity field as a **log-Gaussian random field** (Gaussian-correlated noise, exponentiated), with independently adjustable geometric mean, heterogeneity (σ ln K), correlation length, and **anisotropy ratio Ky/Kx**.
- Models up to **three sheet pile cutoffs**, each an exact impermeable barrier (zero horizontal conductance across its column) down to any penetration depth, positioned anywhere along the dam base — draggable directly on the plot, or set with sliders.
- Renders a live **flow net**: head-field heatmap, equipotential contours, and flux-weighted streamlines traced (both forward and backward) from a discharge-proportional control section, so lines fan out correctly instead of bunching in anisotropic fields.
- Draws an **uplift pressure diagram** along the dam base (heel → toe) and computes the resultant uplift force per unit length.
- Computes the **outlet-side exit gradient and vertical exit velocity distribution** downstream of the toe, with a critical gradient (i꜀ᵣ), a required safety factor, and a SAFE/UNSAFE verdict.
- Everything — dam base width, sheet pile position/depth — can be **dragged directly on the figure**, in addition to the sidebar sliders.

## Controls

| Section | What it adjusts |
|---|---|
| Domain | Foundation length/depth, dam base width, reservoir head (H₁), tailwater head (H₂) |
| Sheet pile cutoffs | Number of piles (0–3, including a no-cutoff baseline), position and penetration depth per pile — draggable on the plot |
| Hydraulic conductivity field | Geometric mean Kₓ, heterogeneity σ(ln K), correlation length, anisotropy ratio Ky/Kx, random seed |
| Outlet piping safety | Critical exit gradient i꜀ᵣ, required safety factor |

## Method notes

- **Solver:** matrix-free, symmetric positive-definite finite-volume system (harmonic-mean face conductances between cells); Dirichlet boundary contributions are moved to the RHS, and the remaining unknown-node system is solved with preconditioned CG rather than relaxation (SOR), which converges reliably even for strongly heterogeneous/anisotropic fields where SOR can stall.
- **Boundary conditions:** fixed head (H₁/H₂) at the upstream/downstream ground surface; no-flow at the dam footprint, domain sides, and impervious base.
- **Streamlines:** seeded by equal increments of discharge at a control section on the higher-head side of the dam, then traced both upstream and downstream from each seed — avoids the streamline bunching that naive evenly-spaced-in-x seeding produces once flow converges toward a cutoff.
- **Critical exit gradient:** i꜀ᵣ = (Gs − 1)/(1 + e); defaults to 1.0 (typical for loose saturated sand) but is a user input — set it from your own soil's specific gravity and void ratio if available.

## Disclaimer

This is a screening / teaching tool for exploring how cutoff geometry and soil variability affect seepage behavior — **not** a substitute for a calibrated, site-specific design model. Safety verdicts use a generic critical gradient unless you supply your own soil parameters.

## Running locally

Just open `index.html` in any modern browser. No server, no build, no dependencies.

