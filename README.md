# BambuDome - Reproducibility Package

Code, structural models, and benchmark result datasets for the study:  
**"Structural Optimization and The Decarbonization Paradox of Large-Span Bamboo Space Domes"**

---

## Contents

| Folder / File | Description |
|---|---|
| `03_Code_Execution/` | Direct stiffness finite element solver (`fem_bridge.py`, `truss_model.py`), parametric dome generators (Geodesic 16 m, Kiewitt 32 m, Ribbed 40 m in `dome_types.py`), *Guadua angustifolia* culm catalogue (`bamboo_catalog.py`), ISO 22156:2021 & IStructE (2025) ASD limit-state verification routines (`iso22156_checks.py`), multi-bolt joint shear checks (`joint_bolt_check.py`), metaheuristic optimization algorithms (`metaheuristic_optimizers.py`; ABC, ACO, DO), steel reference benchmark (`steel_reference.py`), post-optimization LCA & sensitivity analyzers, and statistical testing scripts. |
| `04_Results/campaign_final/` | Production campaign summary (`summary.csv`: 270 bamboo runs = 3 domes × 3 algorithms × 30 independent seeds 1001–1030), steel reference summary (`steel_reference_aisc37_summary.csv`: 120 runs), and optimal profile definitions (`global_best_*.json`). |
| `04_Results/tables/` | Full set of publication tables in structured CSV format: optimization statistics (`T1`, `T_stats_*`), optimal cross-sections (`T2`), limit state utilizations (`T4`), material comparisons (`T5`), component breakdowns (`T7`), LCA sensitivities (`T8`, `T_lca_*`), price scenarios (`T9`), joint bolt requirements (`T10`), hyper-parameter tuning summary (`T_tuning_study_summary.csv`), and raw convergence trajectories (`T_convergence_curves_history.csv`). |
| `04_Results/figures/` | High-resolution manuscript figures (`F1` to `F5`), flowchart, graphical abstract, and structural plan/elevation renderings (`figures/domes/`). |
| `04_Results/SAP2000_models/` | Independent benchmark validation models (`.sdb` binary and `.s2k` text files) for the optimum designs of all three dome configurations. |
| `04_Results/PRICE_SCENARIOS.csv`, `PRICE_SOURCES_IN_CO.csv`, `TUNING_STUDY_runs.csv` | Empirical price scenario matrices, Colombian commercial culm pricing sources, and algorithm tuning logs. |

---

## Requirements and Installation

- Python 3.10 or later
- Install dependencies:
  ```bash
  pip install -r requirements.txt
  ```
- *Note:* CSI SAP2000 (v20+) is required only if executing the external OAPI verification scripts (`sap2000_bridge.py`, `validate_sap.py`). All core structural analyses and metaheuristic sizing optimization routines run natively via the included Python finite element solver.

---

## Quick Verification Checks

To verify the installation and the structural models, execute from `03_Code_Execution/`:

```bash
cd 03_Code_Execution
python test_units.py       # Unit tests for FEA solver, ISO 22156 checks, and material catalogue
python test_fast_eval.py   # Vectorized evaluation pipeline test
python render_domes.py     # Generate 3D perspective and elevation views of optimal domes
```

---

## Re-Running Optimization and Post-Processing

All optimization campaigns and post-processing analyses can be executed directly from `03_Code_Execution/`:

```bash
cd 03_Code_Execution

# 1. Non-parametric statistical tests (Wilcoxon rank-sum / Holm adjustment & Friedman ranks):
python statistical_analysis.py --results-dir ../04_Results/campaign_final

# 2. Cradle-to-gate (A1-A3) LCA sensitivity analysis and component carbon breakdown:
python lca_sensitivity_analysis.py

# 3. Parametric material cost and price scenario evaluations:
python price_scenarios.py

# 4. Generate all manuscript summary tables and figure assets:
python results_report.py
```

---

## Methodological Notes

- **Embodied Carbon Factors:** All carbon metrics in `04_Results/campaign_final/summary.csv`, `global_best_*.json`, and `04_Results/tables/T5`–`T8` are evaluated using declared Environmental Product Declaration (EPD) cradle-to-gate factors:
  - *Guadua* bamboo culm: 0.243 kg CO2e/kg (INBAR Technical Report No. 35 + UK DESNZ/DEFRA 2023 transport)
  - Structural steel plate / hub: 1.130 kg CO2e/kg (Bauforumstahl EPD)
  - Hollow structural steel tube: 1.701 kg CO2e/kg (Welded Tube of Canada EPD)
  - Non-shrink cementitious grout: 0.310 kg CO2e/kg (SikaGrout-312 HP EPD)
- **Feasibility Verification:** As demonstrated in `bamboo_354_feasibility.py`, the coarse 354-member benchmark configuration is structurally infeasible under natural bamboo capacity and geometric constraints, validating the frequency-2 densification framework adopted in this research.
- **Raw Evaluation Records:** Individual per-iteration convergence histories for all 270 production runs (~135 MB) are synthesized into `04_Results/tables/T_convergence_curves_history.csv` for direct, lightweight inspection without requiring high-performance computing storage.

---

## License

This reproducibility package is licensed under the MIT License. See `LICENSE` for details.
