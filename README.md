# Coastal Ocean Flux Toolkit

A Python toolkit I developed for quantitative analysis of coastal and open-ocean
observations: air–sea flux estimation, TEOS-10 seawater thermodynamics, and
error-propagation methods for in-situ measurements. The code grew out of my own
work analyzing field datasets (Monterey Bay air–sea exchange and Palau tidal-flow
drag) and is organized as a reusable set of methods rather than one-off scripts.

## What's inside

- **Air–sea flux estimation** — bulk-formula heat, momentum, and gas exchange
  calculations, with a Monterey Bay case study comparing flux parameterizations
  across forcing conditions.
- **Seawater thermodynamics (TEOS-10)** — density, conservative vs. in-situ
  temperature, and absolute vs. practical salinity relationships built on the
  GSW-Python implementation, including water-mass mixing and cabbeling effects.
- **Drag-coefficient (C_D) uncertainty analysis** — a finite-difference momentum
  method for estimating bottom drag from paired pressure sensors in tidal flow,
  with full error propagation to recommend optimal sensor geometry.
- **Supporting utilities** — synthetic data generation, `.mat` ingestion, and
  reproducible figure pipelines.

## Methods

### Air–sea flux
`air_sea_flux_monterey.py`, `monterey_airsea_exploration.py`, and the
`airsea_*` modules estimate turbulent and radiative fluxes from bulk variables
and compare parameterization choices against observed forcing.

### Seawater density and thermodynamics
`explore_density_relationships.py` and `gsw_examples.py` characterize how
density responds to pressure, temperature, and salinity, and produce T–S
diagrams and water-mass mixing diagnostics from TEOS-10.

### Drag-coefficient uncertainty
`cd_uncertainty_analysis.py` estimates the quadratic drag coefficient from
two pressure sensors using a centered finite-difference momentum balance:

$$C_D = -g\,h_{i+1}\,\frac{\xi_{i+2}-\xi_i}{x_{i+2}-x_i}\,\frac{1}{U_{i+1}\,|U_{i+1}|}$$

where $\xi$ are surface elevations, $x$ sensor positions, $h$ depth, and $U$ the
depth-averaged velocity. Discretizing at the actual sensor locations (rather than
assuming a continuous gradient) is what makes the estimate match the experimental
setup. Propagating measurement uncertainties — surface elevation (±1 mm), position
(±1 cm), depth (±1 cm), and velocity (±5 cm/s) — over sensor separations of
0.5–5 m and velocities of 0.1–1.5 m/s yields a practical recommendation: a
~2 m sensor separation and velocities above 0.5 m/s minimize the relative error
on C_D. Outputs include `cd_uncertainty_contour.png` and
`cd_uncertainty_vs_separation.png`.

## Installation

```bash
git clone https://github.com/PeterMastnak/coastal-ocean-flux-toolkit.git
cd coastal-ocean-flux-toolkit
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Usage

Each module runs independently and writes figures/summaries to the working
directory, e.g.:

```bash
python explore_density_relationships.py
python air_sea_flux_monterey.py
python cd_uncertainty_analysis.py
```

## Requirements

Python 3.7+, with `gsw-python`, `numpy`, `scipy`, `matplotlib`, `netCDF4`, and
`cmocean`. See `requirements.txt` for pinned versions.

## License

Released under the MIT License — see [LICENSE](LICENSE).
