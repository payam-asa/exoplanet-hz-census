# Rocky Planets in the Habitable Zone — a dated census of the NASA Exoplanet Archive

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)

Code and data for the research note *Rocky Planets in the Habitable Zone: A Dated,
Reproducible Census of the NASA Exoplanet Archive with Radius-Provenance Flags*
(Asadinia et al., in prep., *Research Notes of the AAS*).

The notebook downloads the NASA Exoplanet Archive Planetary Systems Composite
Parameters table (`pscomppars`), selects rocky planets in the habitable zone (HZ),
ranks them by the Earth Similarity Index (ESI), and flags whether each planet's
radius was **measured by transit** or **estimated** by the archive from a
mass–radius relation. All results in this release come from the archive
download of **22 November 2026**.

## Contents

| File | Description |
|---|---|
| `Exoplanet_P1_P2.ipynb` | Analysis notebook (Parts A–C reproduce this release; Part D is a separate, unpublished project and its outputs have been cleared) |
| `pscomppars_2026-11-22.csv` | Archive snapshot used for all results (6,473 confirmed planets, 21 columns) |
| `P2_rocky_HZ_ranked_2026-11-22.csv` | The 32 rocky HZ planets, ranked by ESI, with radius-source and controversy flags |
| `P2_HZ_diagram_2026-11-22.png` | Figure 1 of the note: insolation vs stellar temperature with HZ boundaries (300 dpi) |
| `requirements.txt` | Python packages |
| `CITATION.cff` | Citation metadata |
| `LICENSE` | MIT licence (code) |

## Method

| Step | Definition |
|---|---|
| Insolation | S = L / a² (Earth units; `st_lum` is log₁₀ L/L☉, `pl_orbsmax` in AU) |
| Equilibrium temperature | T_eq = 278.6 K · S^¼ · (1 − A)^¼, with Bond albedo A = 0.3 |
| Habitable zone | Kopparapu et al. (2014): conservative = runaway → maximum greenhouse; optimistic = recent Venus → early Mars. Valid for 2600 ≤ T_eff ≤ 7200 K; planets outside that range are flagged (`teff_in_range = False`) |
| Rocky | R ≤ 1.6 R⊕ (Rogers 2015) |
| ESI | 1 − √(½[((R−1)/(R+1))² + ((S−1)/(S+1))²]) (radius + insolation form; Schulze-Makuch et al. 2011) |
| Radius source | `tran_flag = 1` → "measured (transit)"; otherwise "estimated (archive M-R)" (Chen & Kipping 2017 relation, applied by the archive) |

Sanity checks in the notebook: Earth gives S = 1, T_eq = 254.8 K and ESI = 1; the
Sun's HZ is 0.95–1.68 AU.

## Main results (archive download 22 Nov 2026)

- 6,473 planets → 5,873 with S and T_eff → 1,215 rocky → **32 rocky HZ planets**
  (20 conservative, 12 optimistic only).
- **14 of the 32 (44%) have estimated, not measured, radii**; 18 are transit-measured.
- Of 276 HZ planets with a radius, 32 are rocky and 137 are giants; 133 of those
  giants are radial-velocity detections (detection bias, not an occurrence rate).

## Reproducing the results

Tested with Python 3.7.3 in a local Jupyter notebook.

```bash
git clone https://github.com/payam-asa/exoplanet-hz-census.git
cd exoplanet-hz-census
pip install -r requirements.txt
jupyter notebook Exoplanet_P1_P2.ipynb
```

Part A downloads a **new** copy of the archive, and the archive changes over time.
To reproduce the numbers above exactly, use the snapshot in this repository instead:
replace the `df_raw = tap_query(...)` line in Part A with

```python
df_raw = pd.read_csv("pscomppars_2026-11-22.csv")
```

then run Parts A–C. Output files are named with the run date.

## Data source and acknowledgement

This research has made use of the NASA Exoplanet Archive, which is operated by the
California Institute of Technology, under contract with the National Aeronautics
and Space Administration under the Exoplanet Exploration Program
(doi:[10.26133/NEA13](https://doi.org/10.26133/NEA13)).

## How to cite

Please cite the research note (once published) and this archive:

> Asadinia, P. 2026, *Rocky planets in the habitable zone: code and data* (v1.0),
> Zenodo, doi:10.5281/zenodo.XXXXXXX

GitHub's "Cite this repository" button uses `CITATION.cff`.

## Licence

- **Code** (the notebook): MIT — see `LICENSE`.
- **Derived data and figure** (`P2_*` files): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- **Archive snapshot** (`pscomppars_2026-11-22.csv`): NASA Exoplanet Archive data,
  redistributed unchanged; please acknowledge the archive as above.

## Author

Payam Asadinia — Learn Satit Pattana School, Bangkok, Thailand ·
ORCID [0009-0002-3953-6104](https://orcid.org/0009-0002-3953-6104).
Student contributors are credited in the research note.

## References

- Chen, J., & Kipping, D. 2017, ApJ, 834, 17
- Kopparapu, R. K., et al. 2014, ApJL, 787, L29
- Rogers, L. A. 2015, ApJ, 801, 41
- Schulze-Makuch, D., et al. 2011, Astrobiology, 11, 1041
