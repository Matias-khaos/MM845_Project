# Learning the eventual law of a parametric Frobenius number

MM845 — Tópicos de Geometria III (IMECC–UNICAMP)  
Matias Zimmermann

## Set up a virtual environment

Python 3.11–3.13 is recommended.

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### Windows PowerShell

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

If PowerShell blocks activation, either change the execution policy for the current process or run the environment's Python directly, e.g. `.\.venv\Scripts\python.exe -m pip install -r requirements.txt`.

## Reproduce the central results

From the repository root, execute the complete notebook headlessly:

```bash
jupyter nbconvert --to notebook --execute MM845_frobenius_project.ipynb \
  --output MM845_frobenius_project_executed.ipynb \
  --ExecutePreprocessor.timeout=-1
```

On Windows PowerShell, the same command may be entered on one line:

```powershell
jupyter nbconvert --to notebook --execute MM845_frobenius_project.ipynb --output MM845_frobenius_project_executed.ipynb --ExecutePreprocessor.timeout=-1
```

The run creates `results/` and writes all CSV/figure outputs used for the computational checks.

The committed notebook also contains reference outputs from the verified executed run. Re-execution is still the authoritative reproducibility check. The corrected 60-family flatness figure cell is intentionally left without a stored image because the earlier rendered output carried the obsolete “30 families” title; **Run All** regenerates the corrected figure.

Alternatively, open the notebook interactively:

```bash
jupyter lab MM845_frobenius_project.ipynb
```

then choose **Run All**. The notebook can also be uploaded to Google Colab.

### Optional speed-up

The Set Transformer in §11.2 is the slowest optional neural baseline. For a faster run, set

```python
RUN_SET_TRANSFORMER = False
```

in §11.2. This leaves the exact arithmetic, structured-law recovery, affine-family survey, quadratic-generator experiment, flatness/width calculations, and torus/DBSCAN results unchanged. Keep it `True` to reproduce the Set Transformer row of the geometry-model table.

## Central checks to expect

A successful full run reproduces the following report-level results.

| Report result | Notebook section | Expected output |
|---|---|---|
| Exact Frobenius oracle and 3,998 labels | §2, §4 | `all checks passed ...`; `3998 exact labels` |
| Candidate period and onset | §5.2–§5.5 | validation periods `[27, 54]`; `q_hat=27`; `T_disc=14`; exact discovery residual `0` |
| Final arithmetic extrapolation | §6–§7 | theorem-informed model: `100%` exact on `t=2001..4000`; FAR checks pass through `t=100000` |
| Data efficiency | §7.2 | first successful budget `N*=160` |
| Controlled replications | §8.3, §8.5 | periods `2`, `9`, and the three additional-family periods `5`, `19`, `30` |
| 60-family affine survey | Appendix A | `q divides D: 60/60`; proposed sharper period rule `59/60`; common leading coefficient `60/60`; simple coefficient formula `56/60` |
| Quadratic-generator experiment | Appendix B | selected degree `3`, period `12`, inferred onset `28`; exact far checks at `400, 600, 1000, 2000`; leading coefficient `5/6` |
| Project-family exact width | §11.1b–§11.1c | first unbroken exact-width tail `21`; analytic tail for all `t>=91` |
| Positive-affine flatness theorem | §11.1d | proof draft matching Appendix B of the report: `1 <= mu_hat*w_hat <= 1 + C/t` |
| Width explains leading coefficient | §11.1d / affine survey | `c2 = 1/(t^2 w_t)` within `0.2%` for `60/60` surveyed affine families |
| Geometry-to-radius experiment | §11.2 | `150 fixed-order families, 3600 lattices`; split `2520/528/552`; best test median relative error about `2.38%` |
| Blind stacking-period recovery | §11.3 | DBSCAN on `S`: `27` clusters, `18` noise points; post-reveal ARI `1.000`; chronological probe `100%` |

## Report-to-notebook map

The current report is organized as follows:

- **§3 Data and experimental protocol** ↔ notebook §§2–4.
- **§4.1 Arithmetic recovery and neural comparison** ↔ notebook §§5–7.
- **§4.2 Geometry suggested by the data** ↔ notebook §11.1b–§11.1d.
- **§4.3 Period as a geometric stacking phase** ↔ notebook §11.3.
- **§4.4 Robustness and exploratory extensions** ↔ notebook §8.3, §8.5, Appendix A, Appendix B, and §11.2.
- **Report Appendix A** ↔ exact rational law reconstructed in notebook §5.5.
- **Report Appendix B** ↔ notebook §11.1d.
- **Report Appendices C–F** ↔ notebook Appendix A, Appendix B, §11.3, and §11.2.

## Determinism and numerical reproducibility

The exact arithmetic portions are deterministic: Frobenius labels, structured Model 1, rational reconstruction, controlled replications, the 60-family survey, exact width checks, and the clustering/probe protocol once its fixed seed is set.

The neural models are seeded with global seed `20261001` plus fixed per-model seeds. Floating-point losses may vary slightly across PyTorch/BLAS versions and hardware, but the qualitative comparisons in the report should remain the same.

The reference executed run used Google Colab with Python 3.13.15, NumPy 2.1.3, and PyTorch 2.11.0+cpu. A later non-neural verification run was also performed outside Colab with newer NumPy/SciPy/scikit-learn versions and reproduced the deterministic outputs.
