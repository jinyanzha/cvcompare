## GenAI Tutorial

This project includes a tutorial describing how generative AI tools were
used during the development of this package.

-  [GenAI Tutorial](GenAI_Tutorial.md)


# cvstrategy

## Overview

`cvstrategy` is an R package designed to compare multiple cross-validation
strategies for L1-regularized logistic regression.  
The package provides a unified framework for evaluating predictive
performance, computational efficiency, and stability of different
cross-validation schemes.

Specifically, `cvstrategy` implements and compares four approaches:
basic cross-validation, warm-start cross-validation, adaptive
coarse-to-fine search, and the official `cv.glmnet` procedure.

- You can run examples in the `?compare_cv_methods` help page.
- The package outputs a summary table for easy comparison across methods.
- Visualization functions are provided for CV error curves and coefficient paths.

---

## Features

`cvstrategy` provides the following functionality:

- Compare four cross-validation strategies for L1-regularized logistic regression:
  - Basic CV (cold start)
  - Warm-start CV
  - Adaptive coarse-to-fine CV
  - Official `cv.glmnet`

- Automatically select the optimal regularization parameter (`lambda`)
  for each method.

- Report predictive performance metrics, including:
  - Cross-validated deviance
  - Classification accuracy
  - AUC

- Quantify model stability across folds using coefficient variability.

- Measure and report runtime for each cross-validation strategy.

- Provide plotting utilities for:
  - Cross-validation error curves
  - Coefficient paths across lambda values

---





## Installation

```r
# Install devtools if not already installed
install.packages("devtools")

# Install cvstrategy from GitHub
devtools::install_github("jinyanzha/cvstrategy")
```




## Usage

``` r
library(cvstrategy)
data(Sonar, package = "mlbench")

X <- as.matrix(Sonar[, 1:60])
y <- ifelse(Sonar$Class == "M", 1, 0)

tab <- compare_cv_methods(X, y, K = 5, seed = 1)
print(tab)

    method best_lambda best_cvm train_mse  accuracy       auc stability_sd_best stability_mean_sd runtime
1    basic  0.03359275 1.070146 0.1343958 0.8173077 0.9083310        0.09162666        1.33953828    0.19
2     warm  0.03359275 1.070146 0.1343958 0.8173077 0.9083310        0.09162666        1.33953828    0.19
3 adaptive  0.03248939 1.070109 0.1332396 0.8125000 0.9099099        0.09063466        0.08706317    0.11
4 official  0.03359275 1.070493 0.1343958 0.8173077 0.9083310        0.09162666        1.33953828    0.37

  
```

## Main Functions

### `compare_cv_methods()`

**Description**  
The core function of the package.  
It compares multiple cross-validation strategies for L1-regularized logistic
regression and returns a unified summary table.

**Purpose**

- Compare multiple cross-validation strategies in a single framework  
- Evaluate predictive performance, stability, and computational efficiency  
- Provide a reproducible benchmark for model selection  

**Key Inputs**

- `X`: Numeric design matrix of predictors (n × p)
- `y`: Binary response vector coded as 0/1
- `K`: Number of cross-validation folds
- `seed`: Random seed for reproducibility
- `lambda_path`: Optional regularization path
- `n_lambda_coarse`: Number of coarse grid points (adaptive CV)
- `n_lambda_fine`: Number of fine grid points (adaptive CV)
- `threshold`: Classification threshold for accuracy

**Output**

A `data.frame` summarizing results for each CV strategy, including:

- `best_lambda`
- `best_cvm` (minimum CV deviance)
- `train_mse`
- `accuracy`
- `auc`
- `stability_sd_best`
- `stability_mean_sd`
- `runtime`

This function serves as the main entry point of the package.

---

## Visualization Functions

### `plot_cv_compare()`

**Description**  
Plots cross-validation error curves for different CV strategies.

**Purpose**

- Visualize CV error behavior across the regularization path  
- Compare convergence patterns among methods  
- Complement numerical results with graphical diagnostics  

### `plot_path()`

**Description**  
Plots coefficient paths as a function of the regularization parameter `lambda`

**Purpose**

- Visualize coefficient shrinkage under L1 regularization
- Examine sparsity patterns across lambda values 
- Assess coefficient stability and interpretability

