# Introduction to Test Statistics

## What is Test Statistics?
Test statistics are numerical values derived from sample data during a hypothesis test. They are used to determine whether to reject the null hypothesis (H₀) in favor of the alternative hypothesis (Hₐ).

## Why is Test Statistics Important?
* **In Data Science:** Validates models and interprets trends.
* **In AI Research:** Evaluates model performance.
* **In Bioinformatics:** Analyzes gene expression and biological data.

## Types of Testing
1. **Hypothesis Testing:** Framework to decide between H₀ and Hₐ.
2. **Parametric vs. Non-Parametric Tests:**
   * Parametric tests assume data follows specific distributions (e.g., normal).
   * Non-parametric tests make no such assumptions.

## Hypothesis Testing

### Key Concepts
Null Hypothesis (H₀) and Alternative Hypothesis (Hₐ)
* **H₀:** No effect or difference exists.
* **Hₐ:** Effect or difference exists.

Significance Level (α)
The probability of rejecting H₀ when it is true. Common levels: 0.05, 0.01.

p-value
The probability of observing the test results under H₀.
* If p-value ≤ α, reject H₀.

### Steps in Hypothesis Testing
1. Define hypotheses H₀ and Hₐ.
2. Choose an appropriate test and check assumptions.
3. Calculate the test statistic.
4. Compare the test statistic or p-value against critical values.
5. Conclude based on α.

### Types of Errors
* **Type I Error (α):** Rejecting H₀ when it is true.
* **Type II Error (β):** Failing to reject H₀ when Hₐ is true (the probability of failing to reject H₀ when Hₐ is true).

### Interpreting p-values and Statistical Significance
* A p-value less than the chosen significance level (α) indicates strong evidence against H₀, suggesting that the observed difference is statistically significant.
* A p-value greater than α suggests that there is not enough evidence to reject H₀, and the observed difference may be due to chance.

## Parametric Tests

### t-tests

#### One-Sample t-test
Tests whether the sample mean differs from a known value.

**Formula:** t = (xˉ - μ) / (s / √n)

Where:
* xˉ: Sample mean
* μ: Hypothesized population mean
* s: Sample standard deviation
* n: Sample size

**Assumptions:**
* Independence of observations
* Normality of the data or a large sample size (n > 30)

**Python Example:**

```python
from scipy.stats import ttest_1samp
import numpy as np

data = [1.1, 2.2, 3.1, 4.3, 5.5]
t_stat, p_value = ttest_1samp(data, popmean=3.0)
print(f"T-statistic: {t_stat}, P-value: {p_value}")
```

**R Example:**

```r
data <- c(1.1, 2.2, 3.1, 4.3, 5.5)
t.test(data, mu=3.0)
```

#### Independent Two-Sample t-test
Compares means of two independent groups.

**Assumptions:**
* Independence of observations
* Normality of the data or a large sample size (n > 30) for each group
* Equal variances between the two groups

#### Paired t-test
Compares means of paired data.

## Non-Parametric Tests

### Mann-Whitney U Test
Used to compare two independent samples.

**Python Example:**

```python
from scipy.stats import mannwhitneyu

group1 = [1, 2, 3, 4, 5]
group2 = [6, 7, 8, 9, 10]
u_stat, p_value = mannwhitneyu(group1, group2)
print(f"U-statistic: {u_stat}, P-value: {p_value}")
```

**R Example:**

```r
group1 <- c(1, 2, 3, 4, 5)
group2 <- c(6, 7, 8, 9, 10)
wilcox.test(group1, group2)
```

### Chi-Square Tests

#### Test for Independence
Checks if two categorical variables are independent.

**Python Example:**

```python
import numpy as np
from scipy.stats import chi2_contingency

data = np.array([[10, 20, 30], [6, 9, 17]])
chi2, p, dof, expected = chi2_contingency(data)
print(f"Chi2: {chi2}, P-value: {p}")
```

**R Example:**

```r
data <- matrix(c(10, 20, 30, 6, 9, 17), nrow=2)
chisq.test(data)
```

## Correlation Tests

### Pearson Correlation
Measures linear relationship between two variables.

**Interpretation of the correlation coefficient (r):**
* r = 1: Perfect positive linear relationship
* r = -1: Perfect negative linear relationship
* r = 0: No linear relationship

**Python Example:**

```python
from scipy.stats import pearsonr

x = [1, 2, 3, 4, 5]
y = [5, 6, 7, 8, 7]
r, p_value = pearsonr(x, y)
print(f"Correlation: {r}, P-value: {p_value}")
```

**R Example:**

```r
x <- c(1, 2, 3, 4, 5)
y <- c(5, 6, 7, 8, 7)
cor.test(x, y)
```

## Projects and Assignments
1. **Gene Expression Analysis (Bioinformatics):** Use ANOVA to identify significant differences in gene expression across conditions.
2. **A/B Testing (Data Science):** Evaluate the effectiveness of a website feature using t-tests.
3. **Model Validation (AI Research):** Use resampling methods to validate a machine learning model's performance.
