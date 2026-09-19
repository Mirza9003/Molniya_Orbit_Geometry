# Molniya Orbit Geometry and the Sun–Earth L1 Point

A compact, self-contained orbital-mechanics workshop that derives the full
geometry of a **Molniya orbit** from first principles, visualizes its motion,
and then locates the **Sun–Earth L1 Lagrange point**. Built as a teaching and
reference notebook for anyone working with satellite geometry in remote
sensing and Earth observation.

![Molniya orbit with hourly satellite positions](assets/molniya_orbit.png)

<p align="center"><em>The satellite lingers near apogee (right) and races
through perigee (left) — the "apogee dwell" that makes highly elliptical
orbits useful over high latitudes.</em></p>

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![NumPy](https://img.shields.io/badge/numpy-informational)
![SciPy](https://img.shields.io/badge/scipy-informational)
![Matplotlib](https://img.shields.io/badge/matplotlib-informational)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

---

## Why this exists

Highly elliptical orbits such as the Molniya orbit trade a circular ground
track for something more valuable at high latitudes: a satellite that stays
over one hemisphere for most of its period. That behavior is not magic — it
falls straight out of Kepler's laws. This notebook makes the chain from a
two-number orbit specification (`e`, `T`) to full geometry, speed, timing, and
visualization explicit and reusable, and then reuses the same two-body
machinery to find a Lagrange point.

## What's inside

The notebook works through five stages, each with derivation, code, and a
result:

1. **Orbit geometry** — semi-major and semi-minor axes, perigee and apogee, from
   Kepler's third law.
2. **Orbital speed** — the transverse component from Kepler's second law
   (`V = 2S/rT`) *and* the total speed from the vis-viva equation, plotted
   together to show exactly where and why they differ.
3. **Time of flight** — travel time from perigee via the true → eccentric →
   mean anomaly chain and Kepler's equation.
4. **Orbit visualization** — Newton's-method inversion of Kepler's equation to
   place the satellite at each hour, with Earth drawn to scale at the focus.
5. **The Sun–Earth L1 point** — a first-order Hill-radius estimate plus a
   numerical solution of the full collinear balance.

## Key results

| Quantity | Value |
| --- | --- |
| Semi-major axis `a` | 26,610 km |
| Semi-minor axis `b` | 18,411 km |
| Perigee / apogee altitude | ~1,030 km / ~39,450 km |
| Speed at perigee / apogee | 9.63 km/s / 1.55 km/s |
| Time past ν = 90° | ~2/3 of the 12 h period (apogee dwell) |
| Sun–Earth L1 distance | ~1.49 million km |

<p align="center">
  <img src="assets/orbital_speed.png" width="620"
       alt="Total vs. transverse orbital speed against true anomaly">
</p>

## Getting started

**Run it in the browser (no install):**

Open the notebook in Google Colab — replace the path with your repository:

```
https://colab.research.google.com/github/<username>/<repo>/blob/main/molniya_orbit_workshop.ipynb
```

**Run it locally:**

```bash
git clone https://github.com/<username>/<repo>.git
cd <repo>
pip install numpy scipy matplotlib jupyter
jupyter notebook molniya_orbit_workshop.ipynb
```

The notebook has no data dependencies and runs top to bottom in a few seconds.

## Repository structure

```
.
├── molniya_orbit_workshop.ipynb   # the workshop (derivations, code, figures)
├── assets/
│   ├── molniya_orbit.png          # orbit visualization (README preview)
│   └── orbital_speed.png          # speed-vs-anomaly figure (README preview)
└── README.md
```

## Reusing the code

The routines are written for general eccentricity, period, and gravitational
parameter, so they work for any bound two-body orbit — not just this one:

| Function | Purpose |
| --- | --- |
| `semi_major_axis(T, mu)` | Kepler's third law |
| `semi_minor_axis(a, e)` | ellipse geometry |
| `radius_of_true_anomaly(nu, a, e)` | orbit equation `r(ν)` |
| `total_speed(r, a, mu)` | vis-viva speed |
| `transverse_speed(r, a, b, T)` | Kepler-second-law speed |
| `time_of_flight(nu, e, T)` | time since perigee |
| `solve_kepler(M, e)` | invert Kepler's equation (Newton) |

## Notes on the physics

- The transverse speed `2S/rT` and the vis-viva total speed agree **only at the
  apses**, where the radial velocity vanishes. Away from perigee and apogee the
  total speed is larger; the notebook plots both so the distinction is explicit
  rather than assumed.
- The first-order L1 estimate `R ≈ r (M_E / 3M_S)^(1/3)` lands within ~0.3% of
  the numerically solved value used operationally by missions such as SOHO and
  DSCOVR.

## References

- H. D. Curtis, *Orbital Mechanics for Engineering Students* — Kepler's laws,
  the vis-viva equation, and Kepler's-equation solvers.
- NASA SOHO and DSCOVR mission pages — Sun–Earth L1 operations.

## License

Released under the MIT License. See `LICENSE` (add one when you publish).

## Author

Mirza Md Tasnim Mukarram
University of Iowa
