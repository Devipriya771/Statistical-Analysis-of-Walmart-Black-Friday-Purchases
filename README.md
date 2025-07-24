# Statistical-Analysis-of-Walmart-Black-Friday-Purchases


> *Exploring how customer demographics drive spending on one of the busiest retail days of the year, using non‑parametric bootstrapping to quantify uncertainty*

---

## 📑 Project Overview

This repository contains an exploratory and inferential analysis of the **Black Friday** purchases made at Walmart. Using the public Kaggle *BlackFriday* dataset (550 k transactions) we address questions such as:

* Do women spend differently from men?
* Does marital status or age predict purchase amount?
* Which product categories dominate sales?

The companion Jupyter notebook walks through data preparation, exploratory visualisation and a set of statistical techniques—most notably **non‑parametric bootstrapping**—that quantify uncertainty around key comparisons.

## 📂 Dataset

| Field                        | Type      | Notes                                |
| ---------------------------- | --------- | ------------------------------------ |
| `User_ID`                    | int → str | Anonymised customer id               |
| `Product_ID`                 | obj       | SKU code                             |
| `Gender`                     | obj       | `F`, `M`                             |
| `Age`                        | obj       | 7 ordinal bands (`0‑17`, `18‑25`, …) |
| `Occupation`                 | int       | Encoded occupation categories        |
| `City_Category`              | obj       | `A`, `B`, `C`                        |
| `Stay_In_Current_City_Years` | obj       | `0`, `1`, `2`, `3`, `4+`             |
| `Marital_Status`             | int       | 0 = unmarried, 1 = married           |
| `Product_Category`           | int → str | 20 high‑level product groups         |
| `Purchase`                   | int       | Purchase amount (INR)                |

## 🔧 Pre‑processing Pipeline

1. **Type casting** – numeric IDs to strings for categorical treatment.
2. **Integrity checks** – confirmed *no* missing values or duplicates.
3. **Outlier mitigation** – clipped `Purchase` to its 5ᵗʰ–95ᵗʰ percentiles to stabilise mean estimates.

## 🔍 Exploratory Data Analysis

* **Univariate**: `describe()`, histograms, pie / donut charts.
* **Bivariate**: stacked bar charts & heat‑maps linking product categories with age, gender and city.
* **Boxplots**: scanned `Age`, `Occupation`, `Stay_In_Current_City_Years`, `Purchase` for extreme values.

## 📊 Statistical Methods

### 1 · Descriptive statistics

Classical measures of central tendency and dispersion summarise numeric features, while frequency tables reveal dominant categorical levels.

### 2 · Outlier detection & treatment

Visual inspection via seaborn **boxplots** → mild skew and heavy tails in `Purchase`. To prevent leverage on the mean, extreme values beyond the 5 / 95 percentiles were **winsorised**.

### 3 · Bootstrapping for confidence intervals *(main method)*

A custom helper (`bootstrapclt`, `bootstrapclt90`, `bootstrapclt99`) draws *1‑000* resamples and applies the **Central Limit Theorem** to derive normal‑approximation intervals for the mean. This approach is distribution‑free and robust to skew.

CI’s are computed for:

* **Gender** differences (M vs F) — sample sizes 300, 3 000, 30 000 & full set.
* **Marital status** (Married vs Unmarried).
* **Age bands** (7 levels).

Analyses compare **width**, **overlap** and the impact of **confidence level** (90 / 95 / 99 %).

### 4 · Cross‑tabulations & conditional probability matrices

`pd.crosstab` + row‑wise normalisation estimate
$P(\text{Age band}\,|\,\text{Product})$ and $P(\text{Gender}\,|\,\text{Product})$ to spot demographic affinities.

### 5 · Correlation analysis

Ordinal categories (`Age`, `StayYears`) mapped to mid‑point integers → Pearson coeffs visualised with a **heat‑map** (all $|r|<0.15$ → weak linear relations).

### 6 · Sampling demonstrations

Density plots of **bootstrap means** show convergence towards normality as *n* grows, reinforcing the CLT.

## ✨ Key Findings

* **Gender gap**: Men outspend women by ≈ ₹ 690 on average; 95 % CI’s do **not** overlap ⇒ statistically meaningful.
* **Age effect**: Spending climbs from 0‑17 through 51‑55, then plateaus; 51‑55 has the highest mean.
* **Marital status**: CI’s for married vs unmarried overlap heavily → no meaningful difference.
* **Product mix**: Categories **5, 1, 8** account for > 73 % of orders.



