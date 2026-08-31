# Numerical Study of Inflationary Cosmology

A numerical framework for evolving single-field inflationary backgrounds and computing
the primordial **scalar and tensor power spectra** by solving the Mukhanov–Sasaki
equation, applied across eight inflationary potentials and compared against slow-roll
analytic expectations.

Full write-up: [`docs/report.pdf`](docs/report.pdf). A from-scratch theory primer
covering everything from the FLRW metric to the tensor power spectrum is in
[`docs/inflation_primer.pdf`](docs/inflation_primer.pdf).

## Shared numerical benchmark

Every notebook in this repo uses the same convention, so results are directly comparable
across potentials:

| Setting | Value |
|---|---|
| Total e-folds of inflation, $N_{\rm end}$ | 65 |
| Pivot mode, $k_*$ | 0.05 Mpc$^{-1}$ |
| Pivot-scale amplitude, $\mathcal{P}_\mathcal{R}(k_*)$ | $2.05\times10^{-9}$ |
| e-folds before end at horizon crossing, $N_*$ | 50 (fixed by construction) |
| Sub-horizon initial-condition depth, $k/aH$ | 100 (`Nic_cond`) |
| ODE absolute tolerance | $10^{-13}$ |

$\phi_i$ is root-found on the exact background ODE (not a slow-roll estimate) to hit
$N_{\rm end}=65$ exactly; the potential's overall scale is then tuned to match the pivot
amplitude.

## Repository structure

```
potentials/          Tensor notebooks (scalar + tensor spectra) — one per potential,
                      each independently validated against its own slow-roll benchmark
parameter_sweeps/     Sweeps over a potential family's free parameter(s), used to study
                      how (n_s, r) moves as the parameter varies
comparison/           Cross-potential overlay: all eight potentials run through the
                      same pipeline side by side; source of the classification results
docs/                 Report (PDF + LaTeX source) and the theory primer
legacy/               Early/superseded material, kept for reference
```

| Potential | `potentials/` | `parameter_sweeps/` |
|---|---|---|
| Quadratic, $V=\tfrac12 m^2\phi^2$ | `quadratic_tensor.ipynb` | via `monomial_sweep.ipynb` ($p=2$) |
| Quartic, $V=\tfrac14\lambda\phi^4$ | `quartic_tensor.ipynb` | via `monomial_sweep.ipynb` ($p=4$) |
| Hilltop | `hilltop_tensor.ipynb` | `hilltop_sweep.ipynb` |
| Starobinsky | `starobinsky_tensor.ipynb` | — |
| Natural inflation | `natural_tensor.ipynb` | `natural_sweep.ipynb` |
| Symmetry breaking | — | `symmetry_breaking_sweep.ipynb` |
| Quadratic + quartic mixture | — | `quad_quartic_mix_sweep.ipynb` |
| T-model ($\alpha$-attractor) | — | `t_model_sweep.ipynb` |

## Summary of results

Validated on quadratic inflation against closed-form slow-roll expectations
($n_s,r$ agree with the analytic values to $\lesssim0.5\%$ and $\lesssim3\%$
respectively), then extended to seven further potentials with both scalar and tensor
spectra. The single-field consistency relation $r=-8n_t$ was checked across all eight
potentials and found to hold at the several-percent level expected from
next-to-leading-order slow-roll corrections, with the largest violations for the
hilltop-type potentials (largest $|\eta_v/\varepsilon_v|$ at the pivot).

Parameter sweeps across each potential family reveal a pattern in how $(n_s,r)$ moves:
parameters that rescale $\phi$ at fixed potential shape (**scale-type**) trace smooth,
exponential-looking tracks; parameters that change the functional form of $V$
(**shape-type**) trace either an exact linear relation (monomial power, provable
analytically) or non-monotonic behaviour (the quadratic+quartic mixing ratio). Full
discussion in `docs/report.pdf`, Section 5.

## Planned next step

The solver code (background ODE, Mukhanov–Sasaki integration, calibration routine) is
currently duplicated inline across notebooks. Factoring this into a shared module that
each notebook imports is planned as the next cleanup pass.

## References

D. Baumann, *Cosmology*, lecture notes — primary theoretical reference throughout.
