# **Requests**
All requests of this project can be found in the **[Titanic Hypothesis Testing Scenarios](#https://github.com/ahmedgalaaali/Titanic-Data-Hypothesis-Testing/blob/main/Titanic%20Hypothesis%20Testing%20Scenarios.pdf)** file in the repo.

Also, see the notebook published on [my Kaggle account](#https://www.kaggle.com/code/ahmedgalaaali/hypothesis-testing)

# **Steps Applied in the Notebook**
- Environment Preparation & Importing Data
- Exploratory Data Analysis:
    - Data overview & Statistical summary
    - Null detection & 2 dataframes combination
    - Test of normal distribution (QQ Plot and empirical rule test)
    - Outliers detection and removal
- Hypothesis Testing:
    - Two sample t-test
    - One sample t-test
    - One sample proportion z-test
    - Two sample proportion z-test

# **Results**

## **Two sample t-test**

### **Request 1**

$H_0: \bar{x}_s = \bar{x}_n$

$H_a: \bar{x}_s \not= \bar{x}_n$

$\alpha = 0.05$

### **Interpretation**

1- **Two-tailed test:**

$p (0.055) > \alpha (0.05)$

**Failed to reject** the null hypothesis at 5% significance level, there's no statistically significantly enough evidence to say that that there was a difference in average age between survivors and non-survivors.

---

## **One sample t-test**

### **Request 2**

$H_0: \mu = 30$

$H_a: \mu \not= 30$

$\alpha = 0.05$

### **Interpretation**

**Two-tailed test:**

$p (0.56) > \alpha (0.05)$

**Failed to reject** the null hypothesis, there's no enough statistically significant evidence to prove that the average age was not 30.

---

## **One Sample Proportion z-test**

### **Request 3**

### **First Method: Using the mathematical equation**

$H_0: P_0 = 0.65$

$H_0: P_0 \neq 0.65$

$\alpha = 0.05$

$\hat{P} = \frac{x}{n}$

$Z_s = \frac{\hat{P} - P_0}{\sqrt{\frac{P_0 \cdot Q_0}{n}}}$

### **Alternative Method: Using `proportions_ztest` from `statsmodels.stats.proportion`**

```python
from statsmodels.stats.proportion import proportions_ztest

z_stats4, p_value4 = proportions_ztest(count=len(fc_survivors_ages),
                                        value=0.65,
                                        prop_var=0.65,
                                        nobs=len(fc_ages),
                                        alternative="two-sided")

print("Historical Proportion = 0.65")
print(f"z statistic = {round(abs(z_stats4), 2)}")
print(f"z critical  = {round(z_crit, 2)}")
print(f"p-value     = {round(p_value4, 3)}")
```

```bash
Historical Proportion = 0.65
z statistic = 1.91
z critical  = 1.96
p-value     = 0.056
```

### **Interpretation**

$p(0.056) > \alpha(0.05)$

$Z_s(1.91) < Z_c(1.96)$

At the 0.05 significance level, we **fail to reject** the null hypothesis. The data does not provide sufficient statistical evidence to conclude that the survival rate of first-class passengers on the Titanic was different from 65%. In other words, based on our sample, the observed survival proportion is consistent with the historical claim of 65%.

---

## **Two Sample Proportion z-test**

### **Request 4**

### **First Method: Using the mathematical equation**

$H_0: \hat{p}_m = \hat{p}_f$

$H_a: \hat{p}_m \neq \hat{p}_f$

$\alpha = 0.05$

$p_0 = \frac{x_m + x_f}{n_m + n_f}$

$Z_s = \frac{p_m - p_f}{\sqrt{p_0 \cdot q_0 (\frac{1}{n_m} + \frac{1}{n_f})}}$


### **Alternative Method: Using `proportions_ztest` from `statsmodels.stats.proportion`**

```python
z_stats3, p_value3 = proportions_ztest(count=[male_sur,female_sur],
                                        nobs=[male,female],
                                        alternative="two-sided")

print(f"Proportion of survived male   = {round(p_m, 2)}")
print(f"Proportion of survived female = {round(p_f, 2)}")
print(f"z statistic = {round(abs(z_stats3), 2)}")
print(f"z critical  = {round(z_crit2, 2)}")
print(f"p-value     = {p_value3}")
```

```bash
Proportion of survived male   = 0.13
Proportion of survived female = 0.5
z statistic = 14.62
z critical  = 1.96
p-value     = 2.172720641443993e-48
```

### **Interpretation**

$p-value < \alpha(0.05)$

$Z_s(14.62) > Z_c(1.96)$

There's a **very strong** statistical evidence that there was a difference in survival rate between men and women from the titanic disaster. In fact, the survival rate of women was massive compared to men's, so that we **reject** the null hypothesis.