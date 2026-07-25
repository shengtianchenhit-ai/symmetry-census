# A Symmetry Census of Zero-Group-Velocity Resonances in Anisotropic Plates

Numerical code, reference figures, and manuscript sources for the study
*A symmetry census of zero-group-velocity resonances in anisotropic plates*
(long-form title: *A taxonomy of zero-group-velocity points in anisotropic
plates: valley census, symmetry pinning, and catastrophe channels*).

## Scientific overview

In an anisotropic plate the zero-group-velocity (ZGV) frequency depends on the
in-plane propagation direction. The directional ZGV frequency, called here the
*valley function* `Omega_0(theta)`, is real-analytic and pi-periodic, every
in-plane mirror axis pins one of its critical points, and its minima and maxima
balance on the circle. These facts fix the generic number of zero-flux
directions for each symmetry class and restrict every change of that number to
three catastrophe channels.

The code implements the certified bordered-Newton valley sweep behind these
results: a warm-started Newton iteration on the bordered system that carries the
ZGV point together with its Jordan chain, so that every located point is
accompanied by an exceptional-point certificate at the `1e-11` level. Included
examples are isotropic aluminium, cubic (001) silicon, a transversely isotropic
T700 unidirectional composite, a synthetic monoclinic plate, and a full phase
diagram of silicon under orthorhombic distortion.

## Contents

- `code/lamb_core.py` — isotropic Rayleigh–Lamb layer: the branch-free entire
  dispersion function, high-precision ZGV location, and the aluminium reference
  material.
- `code/aniso_core.py` — sagittal (two-component) orthotropic pencil, Chebyshev
  collocation, bordered-Newton ZGV solve, and the Puiseux-exponent fit.
- `code/run00_seed.py` — writes `out/results.json`, the seed file: the aluminium
  `S1S2` point from the entire dispersion function, and the Si [100] point with
  its EP certificate. Run this first.
- `code/p5_core.py` — fully coupled three-component anisotropic pencil, stiffness
  rotation, cubic-tensor construction, and the multi-parameter ZGV solve.
- `code/p2_core.py` — Clenshaw–Curtis quadrature and auxiliary pencil routines.
- `code/pubstyle.py` — shared publication figure style (vector PDF, embedded
  Type-42 fonts, 3.4 in single column / 7.0 in double column).
- `code/run12_cfrp.py`, `code/run13_t700.py` — composite-plate examples.
- `code/run14_taxonomy.py` — the main computation: valley sweeps across the
  symmetry classes, the census, and the orthorhombic-distortion phase diagram.
- `code/figs_t1.py`, `code/figs_t1_extra.py`, `code/figs_t1_extra2.py` —
  publication figure generation.
- `code/figutils/` — PDF post-processing used on the final figures; see below.
- `figures/` — the reference figure PDFs as used in the manuscript.
- `manuscript/` — LaTeX sources for the long-form article and its supplementary
  material, and under `manuscript/apl/` the letter-format version and its
  supplementary material.
- `outputs/` — ignored by git; generated results and plots are written here.

## Reproduction status

The workflow runs end to end and reproduces the published values. On a laptop
the whole chain takes a few minutes; `run14_taxonomy.py`, the main computation,
takes about 20 seconds.

| Quantity                                   | Reproduced | Manuscript |
|--------------------------------------------|-----------|------------|
| Aluminium `S1S2`: `Omega_0`, `xi_0`         | 2.890028961, 0.790958549 | same |
| Si [100]: `Omega_0`, `xi_0`, `f*d`          | 2.17409363, 0.89736995, 4.0458 MHz*mm | same |
| Isotropic valley flatness (critical circle) | 2.76e-13  | 2.8e-13    |
| Cubic Si census / pinning error             | 8 waves, 0.00 deg | 8 waves, <0.005 deg |
| T700 census                                 | 4 waves   | 4 waves    |
| Monoclinic critical angles / drift          | 2.4, 31.4, 80.4, 135.6 deg; 7.6 deg | same |
| Pitchfork merger `eta_c` / scaling exponent | 0.3444 / 0.528 | 0.344 +/- 0.004 / 0.528 |

## Requirements

- Python 3.9 or newer.
- The packages in `requirements.txt`.
- `pymupdf` is needed only for `code/figutils/`, and is listed separately in
  `requirements-figutils.txt`.

## Installation

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Usage

All scripts use paths relative to the repository root, so run them from there.

```bash
python check_env.py     # confirm the checkout is complete
make reproduce          # run00 -> run12 -> run13 -> run14, into out/
make figures            # regenerate the figure PDFs into out/sub/
make clean              # remove out/ and caches
```

Without `make`, run in this order from the repository root:

```bash
python3 code/run00_seed.py       # -> out/results.json      (required first)
python3 code/run12_cfrp.py       # -> out/results_12.json
python3 code/run13_t700.py       # -> out/results_13.json   (needed by run14)
python3 code/run14_taxonomy.py   # -> out/results_14.json   main computation
python3 code/figs_t1.py          # -> out/sub/fig_t1_1..3.pdf
python3 code/figs_t1_extra.py    # -> out/sub/fig_t1_4..5.pdf
python3 code/figs_t1_extra2.py   # -> out/sub/fig_t1_6..7.pdf
```

## Conventions

Nondimensionalization is by the half-thickness, the reference shear speed, and
the density, so that `h = c_T = rho = 1`. Frequencies are reported as
`Omega = omega h / c_T` and wavenumbers as `xi = k h`. Dimensional
frequency-thickness products use `c_T = sqrt(C44 / rho)` and `d = 2h`.

Reference values for the aluminium `S1S2` point, certified against a 60-digit
Newton iteration on the entire dispersion function:

```
Omega_0 = 2.890028961      xi_0 = 0.790958549      f*d = 2.8518 MHz*mm
```

## Related work

The exceptional-point structure of a single ZGV point, on which the smoothness
of the valley function rests, is developed in
[`zgv-EP2s`](https://github.com/shengtianchenhit-ai/zgv-EP2s).

## License

MIT. See `LICENSE`.
