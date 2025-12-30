# FIFA22 Player Market Value: Applied Regression

Modeling in-game FIFA22 player market value using multiple linear regression, with a focus on data cleaning, diagnostics, transformations, and interpretability.

## Project Overview
FIFA player “Value” is heavily right-skewed and influenced by a mix of technical skill attributes (0–100 ratings), physical traits (height/weight), and overall rating. This project builds an interpretable regression model that explains market value using player attributes and standard applied-regression workflow:
- data cleaning / preprocessing
- exploratory analysis
- model diagnostics + assumption checks
- response transformation (Box–Cox → log)
- multicollinearity assessment (VIF)
- variable selection (stepwise)
- inference (95% confidence intervals)

## Data
- Observations: **16,710** base player cards
- Features: **65** variables including overall rating, skill attributes (e.g., finishing, dribbling), physical measures, and a few categorical fields (e.g., preferred foot).
- Response: `Value` (string formatted like “€125.5M” / “€450K”), cleaned into a numeric euro value.

> Note: The dataset reflects a specific FIFA edition and base cards only (no promos/special cards), so results describe EA’s in-game valuation mechanics for this data snapshot.

## Methods
### 1) Cleaning & preprocessing
- Converted `Value`, `Height`, and `Weight` from strings with units/symbols into numeric values.
- Removed missing/non-finite/zero values in `Value` to ensure a valid regression response.

### 2) Baseline model + diagnostics
- Fit an initial multiple linear regression on raw `Value`.
- Residual and Q–Q diagnostics showed violations (non-normality + heteroscedasticity), motivating transformation.

### 3) Box–Cox transformation
- Box–Cox estimated λ near 0, supporting a **log transform** of `Value`.
- Created `Value_bc = log(Value)` and refit the model.

### 4) Multicollinearity + variable selection
- Computed **VIF** to quantify multicollinearity among player attributes.
- Used **stepwise selection (both directions)** to reduce redundancy and improve stability/interpretability.

### 5) Inference & interpretation
- Reported multiplicative effects on the original-value scale (because the response is log-transformed).
- Computed **95% confidence intervals** for coefficients in the final model.

## Key Results
- Log-transform dramatically improved fit and diagnostics.
- Final stepwise model explained **~95.8%** of variability in log(Value) (Adj. R² ≈ 0.958).
- Strongest relationships:
  - **Age**: negative association with value (holding other attributes constant).
  - **Overall rating**: strong positive association with value.
  - Several technical attributes contributed meaningful (often small but significant) effects in the final model.

## Repository Structure
- `Project.qmd` — Quarto source for the report (code + narrative)
- `STAT408 Final Project (6).pdf` — rendered final report
- (Optional) `data/` — place raw dataset here if you are allowed to share it
- (Optional) `figures/` — exported plots

## How to Run
### Option A: Render the Quarto report
1. Open the project in RStudio
2. Install packages (if needed)
3. Render:

```r
quarto::quarto_render("Project.qmd")
