# fixes 0.2.1 (May 11, 2025)

## Minor Improvements
- Added a warning when lead/lag dummy variable names (e.g., `lead1`, `lag0`) already exist in the dataset to prevent accidental overwriting.
- Added a warning when filtered data (based on `lead_range`, `lag_range`, and `interval`) has fewer than 10 rows, helping users identify overly narrow estimation windows.
- Improved handling of the `treatment` variable: it is now coerced to logical using `as.logical()` to support both binary numeric (`0/1`) and logical (`TRUE/FALSE`) formats.
- Fixed internal bug in model formula construction:
  - Previously, fixed effects specified via the `fe` argument (e.g., `~ id + year`) were combined using `model_formula | fe_text`, which caused evaluation errors during tests.
  - Now, the full model formula is safely constructed as a string and parsed with `as.formula()` to ensure compatibility with `fixest::feols()`.

## Compatibility
- No breaking changes. This is a backward-compatible patch release with internal robustness improvements and enhanced error handling.

---

# fixes 0.2.0 (March 29, 2025)

## Major Features
- **Support for covariates in `run_es()`**:
  - Covariates must now be specified as a **one-sided formula** (e.g., `~ x1 + x2`).
- **Fixed Effects and Clustering Interface Updated**:
  - `fe` and `cluster` arguments must now be specified using a **one-sided formula** (e.g., `~ id + year`).
  - Character vector input for `cluster` is still accepted.
  - Improved internal handling and validation of fixed effects and clustering variables.
- Improved error messages for invalid or missing variable names.

# fixes 0.1.0 (March 17, 2025)

## Major Changes
- `fe_var` argument now supports additive notation (`firm_id + year`) instead of character vectors.
- Improved `plot_es()` efficiency and documentation.

## Minor Improvements
- Fixed cluster variable handling to correctly reference column names.
- Updated test cases to support new `fe` notation.
- Improved package documentation.

# fixes 0.0.2 (Enhancements & Fixes)

This version introduced several enhancements and refinements to improve usability and maintainability.

## Improvements
- Refactored variable name handling:
  - `outcome_var`, `treated_var`, and `time_var` are now processed using `rlang::ensym()` for better robustness.
  - `fe_var` and `cluster_var` handling improved for more reliable column referencing.
- More informative **error messages** when variables are missing in the dataset.
- Enhanced **baseline term handling** in regression models to prevent incorrect factor levels.
- Improved `plot_es()` function:
  - Added validation checks to ensure required columns (`relative_time`, `estimate`, etc.) are present.
  - Adjusted confidence interval calculations to avoid missing values in error bars.

## Fixes
- Addressed an issue where `baseline` handling could lead to incorrect sorting of lead/lag terms.
- Resolved a minor inconsistency in fixed effects variable name parsing.

# fixes 0.0.1 (Initial Release)

This is the first release of the `fixes` package, providing tools for estimating and visualizing event study models with fixed effects.

## Features
- **`run_es()`**: A function to estimate event study models using `fixest::feols()`, generating lead and lag variables automatically.
  - Supports **fixed effects** (`fe_var` as character vector).
  - Allows **clustered standard errors** via `cluster_var`.
  - Handles **time scaling** through the `interval` argument.
- **`plot_es()`**: A function to visualize event study results with ggplot2.
  - Supports **ribbon-style confidence intervals** (`type = "ribbon"`, default).
  - Allows **error bar visualization** (`type = "errorbar"`).
  - Customizable plot elements including colors, line styles, and reference lines.

## Initial Implementation
- **Fixed effects regression model** using `fixest::feols()`.
- **Automated creation of lead/lag dummy variables** based on treatment timing.
- **Baseline period exclusion** to avoid multicollinearity.
- **Support for custom time intervals (`interval` argument)**.

## Limitations in 0.0.1
- Fixed effects must be specified as a **character vector (`c("firm_id", "year")`)**.
- Clustered standard errors require variable names as **character strings (`"state_id"`)**.
- No direct support for additive notation in `fe_var`.
