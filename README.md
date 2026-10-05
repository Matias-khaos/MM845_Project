# Learning the eventual law of a parametric Frobenius number

MM845 — Tópicos de Geometria III (IMECC–UNICAMP). Matias Zimmermann.

##  Reproduce the central results

Run the whole notebook headless. This writes an executed copy and fills `results/`:

```bash
jupyter nbconvert --to notebook --execute cloud.ipynb \
        --output cloud_executed.ipynb --ExecutePreprocessor.timeout=-1
```

Alternatively, open it interactively (`jupyter lab cloud.ipynb`, then *Run All*), or upload it to Google Colab,
where every dependency is preinstalled.

The full run takes about 13 minutes on a Colab CPU. The Set Transformer of §11.2 takes about 7 of those minutes; set
`RUN_SET_TRANSFORMER = False` in that cell to skip it. Everything else in the report is unaffected.

### Where each result of the report is produced

| Report | Notebook section | What the notebook prints / saves |
|---|---|---|
| §3 exact oracle and its checks | §2, §3, §4.1 | `all checks passed ... {'Sylvester': 167, 'Roberts': 684, 'brute force x 4 moduli': 200, 'integer-heap Dijkstra': 72}`; `3998 exact labels`; `results/frobenius_family.csv` |
| §4.1 (H1): period, onset, law (Eq. 3, Appendix A) | §5.2–§5.5 | `periods admitting an exact validation fit: [27, 54]`, `q_hat=27`, `T_disc=14`, `exact discovery residual: 0`, the 27 components `P_r(t)` |
| Fig. 1(b), Fig. 1(c) | §5.2, §7.2 | `results/fig4_model_selection.png`, `results/fig7_data_efficiency.png` (`N* = 160`) |
| §4.2 (H2–H3): Table 1, Fig. 2 | §6, §7 | the model table, `H2 exact final-test extrapolation: PASS`, the FAR stress test, `results/fig6_model_ladder.png` |
| §4.3 calibration and replication | §8.3, §8.5, Appendices A–B of the notebook | `q = 2 ... q = 9 (Corollary 1.4 predicts 9)`; families B/C/D with `q = 5, 19, 30`; the 60-family survey; the cubic law with period 12 |
| §4.4 Proposition 2, Theorem 3, Fig. 3(a) | §11.1b–§11.1c | `first unbroken candidate tail = 21`; `t*(mu_hat*w_hat-1) on 21..2000: ... range [26.478,28.925]`; `c2 = 1/(t^2 w_t) ... 60/60`; `results/fig10_flatness.png` |
| §4.5 (H4): Table 2 | §11.2 | the printed table of the nine geometric models; `results/fig11_geometry_models.png` |
| §4.6 (H5), Fig. 3(b) | §11.3 | `DBSCAN ... clusters=27, noise=18`, ARI `1.000`, permutation test, probe `100.0%`; `results/fig14_stacking_torus.png` |

### Determinism

* All exact computations are deterministic: labels, Model 1 and the rational law, the replications and surveys, the
  lattice-width checks, and the clustering and probes. The non-neural cells were re-run outside Colab (Python 3.13,
  NumPy 2.5, SciPy 1.18, scikit-learn 1.9) and printed exactly the same output as the original run.
* The neural models are seeded (global seed `20261001`, plus per-model seeds). Their numbers can still differ in
  the last digits across PyTorch versions and hardware.
* Original run: Google Colab, Python 3.13.15, NumPy 2.1.3, PyTorch 2.11.0+cpu.

Uploading the `report/` folder to Overleaf also works. The main text is five pages; the references and
appendices follow.
