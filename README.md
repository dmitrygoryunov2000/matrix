# Numerical Linear Algebra Experiments

This repository contains Jupyter notebooks that reproduce and extend parts of the poster **<File>Poster___numerical_linear_algebra (3).pdf</File>**.

The current focus is on:

1. **Merged Experiment 1 & 2**
   - General dense matrices
   - Symmetric positive definite (SPD) matrices
   - Combined Figure 2–style plot
   - Curve fitting for all cases

2. **Experiment 6 attempt**
   - SparseLU vs dense LU crossover
   - Linear interpolation logic for estimating the crossover point

---

## Repository contents

### 1. `merged_experiments_1_2_combined_figure_with_curve_fits.ipynb`

This notebook combines the dense and SPD performance studies into a single workflow.

#### What it covers
- **Dense matrices**
  - LU
  - Matrix inverse
  - QR
- **SPD matrices**
  - Cholesky
  - LU
  - Matrix inverse
  - QR

#### Outputs
- A combined **log-log performance plot** in the same overall style as the poster’s merged *Experiments 1 & 2* figure.
- Summary timing tables by matrix size.
- Relative comparisons between methods.
- **Curve fitting for every method case** using two models:
  - `t(n) = a * n^b`
  - `t(n) = c * n^3`

#### Curve fitting details
For each curve in the merged experiment, the notebook estimates:
- `a` and `b` in the power-law fit `a * n^b`
- `c` in the constrained cubic fit `c * n^3`

The notebook also produces:
- a coefficient table,
- formula strings for each fitted curve,
- an overlay plot showing measured timings together with both fitted models.

#### Notes
- The poster reports qualitative dense-matrix findings and an SPD timing table, but not the full raw dense timing table. This notebook therefore reproduces the **experiment structure and visual style**, rather than claiming exact raw poster timing values.
- The plotting code avoids multiline string syntax errors by using explicit `\n` inside title strings.

---

### 2. `experiment_6_sparseLU_denseLU_crossover.ipynb`

This notebook is an **attempt to reproduce the SparseLU vs dense LU crossover analysis** discussed in the poster.

#### What it covers
- Dense LU using `scipy.linalg.lu_factor` / `lu_solve`
- SparseLU using `scipy.sparse.linalg.splu(...).solve(...)`
- Sparse matrices generated with density scaling
  - `p = k / n`
  - default `k = 10`

#### Outputs
- Benchmark table for SparseLU and dense LU across multiple matrix sizes.
- Log-log timing comparison plot.
- Timing-difference plot:
  - `SparseLU time - Dense LU time`
- Automatic crossover detection based on a sign change in the timing difference.
- **Linear interpolation** for estimating the crossover point once a bracket is found.

#### Why it is called an “attempt”
The poster reports a crossover near `n* ~ 126.5` using interpolation between measurements at `n = 100` and `n = 200`.

This notebook includes the same style of interpolation logic, but the observed crossover can depend strongly on:
- sparse matrix construction,
- sparsity pattern,
- diagonal stabilization,
- ordering strategy,
- SciPy / BLAS / LAPACK / SuperLU backend,
- machine and runtime environment.

So the notebook should be treated as a **reproducible experimental framework**, not as a guarantee of matching the poster’s crossover value exactly on every system.

---

## Background from the poster

The original poster motivates the following findings:

- **Dense matrices:** LU is the fastest among the tested dense solvers.
- **SPD matrices:** Cholesky outperforms LU beyond small sizes and its relative advantage grows with dimension.
- **Sparse crossover experiment:** SparseLU is expected to overtake dense LU beyond a crossover size when density is scaled as `k/n`.

This repository packages those ideas into runnable notebooks and extends the merged dense/SPD analysis with explicit curve fitting.

---

## Suggested environment

Recommended Python environment:

```bash
python -m venv .venv
source .venv/bin/activate   # Linux / macOS
# .venv\Scripts\activate  # Windows
pip install numpy pandas matplotlib scipy jupyter
```

If you use Jupyter:

```bash
jupyter notebook
```

Then open the notebooks directly.

---

## Typical workflow

### Reproduce merged Experiment 1 & 2 with curve fits
Run:
- `merged_experiments_1_2_combined_figure_with_curve_fits.ipynb`

This will generate:
- the combined Figure 2–style plot,
- fitted coefficients for `a * n^b`,
- fitted coefficients for `c * n^3`,
- fitted overlay plots.

### Re-run the crossover study
Run:
- `experiment_6_sparseLU_denseLU_crossover.ipynb`

This will generate:
- SparseLU vs dense LU timings,
- crossover detection logic,
- interpolation-based estimate if a sign change is observed.

---

## Practical notes

- Benchmark timings are sensitive to hardware and software stack.
- Repeated runs may produce slightly different timings.
- For more stable results, increase the number of realizations / repeats in the notebooks.
- If Experiment 6 does not show a crossover on your machine, that does **not** necessarily contradict the poster; it may indicate that the sparse generator or runtime environment differs from the original setup.

---

## Future improvements

Potential next steps for this repository:
- refine the sparse generator to better match the original crossover experiment,
- save publication-ready PNG figures automatically,
- export timing tables to CSV,
- add notebook execution badges / CI validation,
- add a `requirements.txt` or `environment.yml`.

---

## Attribution

The work in this repository is based on the poster **<File>Poster___numerical_linear_algebra (3).pdf</File>** and the derived benchmark notebooks created around it.
