# Rocky Planets in the Habitable Zone — a dated census of the NASA Exoplanet Archive

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24442222.svg)](https://doi.org/10.5281/zenodo.24442222)

Code and data for two research notes in *Research Notes of the AAS*:

1. *Rocky Planets in the Habitable Zone: A Dated, Reproducible Census of the NASA
   Exoplanet Archive with Radius-Provenance Flags* (Asadinia et al., submitted) —
   notebook `Exoplanet_P1_P2.ipynb`, Parts A–C.
2. *How Often Does a Planet's Habitable-Zone Status Depend on the Paper?
   Solution-to-Solution Scatter in the NASA Exoplanet Archive* (Asadinia et al., in prep.) —
   notebook `Exoplanet_P2_PS_scatter_v2.ipynb`, Part E (see below).

For note 1, `Exoplanet_P1_P2.ipynb` downloads the NASA Exoplanet Archive Planetary Systems Composite
Parameters table (`pscomppars`), selects rocky planets in the habitable zone (HZ),
ranks them by the Earth Similarity Index (ESI), and flags whether each planet's
radius was **measured by transit** or **estimated** by the archive from a
mass–radius relation. All note 1 results come from the archive download of
**22 November 2026**.

## Contents

| File | Description |
|---|---|
| `Exoplanet_P1_P2.ipynb` | Analysis notebook (Parts A–C reproduce this release; Part D is a separate, unpublished project and its outputs have been cleared) |
| `pscomppars_2026-11-22.csv` | Archive snapshot used for all results (6,473 confirmed planets, 21 columns) |
| `P2_rocky_HZ_ranked_2026-11-22.csv` | The 32 rocky HZ planets, ranked by ESI, with radius-source and controversy flags |
| `P2_HZ_diagram_2026-11-22.png` | Figure 1 of note 1: insolation vs stellar temperature with HZ boundaries (300 dpi) |
| `Exoplanet_P2_PS_scatter_v2.ipynb` | Part E notebook (note 2): classifies every published solution separately |
| `ps_2026-11-23.csv` | Archive snapshot of the Planetary Systems (`ps`) table (40,428 solutions, 6,473 planets) |
| `pscomppars_reflinks_2026-11-24.csv` | Source references of a, L★ and T_eff in `pscomppars` (snapshot of 24 Nov 2026) |
| `P2E_ps_solutions_candidates_2026-11-23.csv` | Every published solution of the 43 candidate planets, with S, HZ class, radius and rocky flag |
| `P2E_ps_scatter_summary_2026-11-23.csv` | One row per candidate planet: number of solutions, label changes, S and R ranges |
| `P2E_ps_solutions_candidates_2026-11-23_allsoltypes.csv`, `P2E_ps_scatter_summary_2026-11-23_allsoltypes.csv` | Same, sensitivity run including Kepler/TESS pipeline and candidate solutions |
| `P2E_HZ_literature_scatter_2026-11-23.png` | Figure 1 of note 2 (300 dpi) |
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

## Part E — solution-to-solution scatter (note 2)

`Exoplanet_P2_PS_scatter_v2.ipynb` uses the Planetary Systems (`ps`) table, which keeps
one row per planet per published solution, instead of the composite table.

| Step | Definition |
|---|---|
| Solutions | Only `soltype = "Published Confirmed"` (20,788 of 40,428); `USE_ALL_SOLTYPES = True` gives the sensitivity run |
| Insolation | S = L / a²; if `st_lum` is blank, L = R★² (T_eff / 5772 K)⁴; if `pl_orbsmax` is blank, a from Kepler's third law; archive `pl_insol` as last resort (method stored in `S_src`) |
| HZ class | Kopparapu et al. (2014), evaluated at each solution's own T_eff, clipped to 2600–7200 K |
| Rocky | R ≤ 1.6 R⊕; without a radius but with M ≤ 3 M⊕, R = M^0.279 (Chen & Kipping 2017), flagged `estimated (C&K17)` |
| Candidates | The 32 rocky HZ planets of note 1 plus every planet that is rocky and in the HZ in at least one published solution (43) |
| Testable | At least two solutions with an HZ class |

Main results (`ps` download of 23 Nov 2026):

- **16 of the 30 testable planets** change HZ class or rocky flag between papers;
  median S_max / S_min = 1.39.
- Of the 32 planets of note 1, 19 are testable and **5 change label**
  (K2-72 e, Kepler-1229 b, Kepler-442 b, Kepler-296 e, K2-3 d).
- In `pscomppars`, **8 of the 32** combine a, L★ and T_eff from two or more sources.
- **Kepler-1512 b** is in the HZ only in the composite table (S = 1.08); no published
  solution places it there.
- Sensitivity run (all 40,428 solutions): 26 of 39 testable planets change label;
  median S_max / S_min = 1.56.

Part E loads the frozen snapshots `ps_2026-11-23.csv` and
`pscomppars_reflinks_2026-11-24.csv` when they are in the same folder, so running it
reproduces these numbers exactly. It also needs `P2_rocky_HZ_ranked_2026-11-22.csv`.

## Reproducing the results

Tested with Python 3.7.3 in a local Jupyter notebook.

```bash
git clone https://github.com/payam-asa/exoplanet-hz-census.git
cd exoplanet-hz-census
pip install -r requirements.txt
jupyter notebook Exoplanet_P1_P2.ipynb            # note 1
jupyter notebook Exoplanet_P2_PS_scatter_v2.ipynb  # note 2
```

For note 1, Part A downloads a **new** copy of the archive, and the archive changes over time.
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
(Planetary Systems: doi:[10.26133/NEA12](https://doi.org/10.26133/NEA12);
Planetary Systems Composite Parameters: doi:[10.26133/NEA13](https://doi.org/10.26133/NEA13)).

## How to cite

Please cite the research notes (once published) and this archive:

> Asadinia, P. 2026, *Rocky planets in the habitable zone: code and data* (v1.0),
> Zenodo, doi:10.5281/zenodo.24442222

GitHub's "Cite this repository" button uses `CITATION.cff`.

## Licence

- **Code** (both notebooks): MIT — see `LICENSE`.
- **Derived data and figures** (`P2_*` and `P2E_*` files): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- **Archive snapshots** (`pscomppars_2026-11-22.csv`, `ps_2026-11-23.csv`,
  `pscomppars_reflinks_2026-11-24.csv`): NASA Exoplanet Archive data,
  redistributed unchanged; please acknowledge the archive as above.

## Author

Payam Asadinia 
ORCID [0009-0002-3953-6104](https://orcid.org/0009-0002-3953-6104).
Student contributors are credited in the research note.

## References

- Chen, J., & Kipping, D. 2017, ApJ, 834, 17
- Kopparapu, R. K., et al. 2014, ApJL, 787, L29
- Rogers, L. A. 2015, ApJ, 801, 41
- Schulze-Makuch, D., et al. 2011, Astrobiology, 11, 1041
