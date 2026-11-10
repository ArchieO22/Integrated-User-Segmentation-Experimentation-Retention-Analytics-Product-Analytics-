# User Segmentation, Experimentation & Retention Analytics
## FitTrack Product Analytics Integration (Final Capstone Project)

**Prepared by:** Archana Ogirala (Data Analyst)  
**Environment:** Google Colab / Python 3.8+ & R 4.0+  
**Project Domain:** Product Analytics, User Segmentation, A/B Testing & Survival Modeling  

---

## 📌 Executive Project Overview

FitTrack is a digital fitness tracking and wellness coaching mobile application scaling rapidly from **100,000 to 500,000 Monthly Active Users (MAU)**. Ahead of an upcoming **Series B fundraising round** and a **$2.0M annual marketing budget reallocation**, leadership required a unified, data-driven analytics engine to evaluate customer acquisition quality, behavioral engagement, and subscription retention.

This project delivers a full-stack product analytics solution combining:
1. **Unsupervised Machine Learning (RFM K-Means Clustering)** to segment users into 4 actionable personas.
2. **Experimental Bias Evaluation & Power Analysis** to detect novelty spikes and enforce sample size rigor in A/B testing.
3. **Cohort Retention Analysis** comparing N-day vs. rolling retention decay across 5 acquisition channels.
4. **Kaplan-Meier Survival Analysis & Log-Rank Statistical Testing** in R to map subscription lifespans and quantify customer churn risks.
5. **Strategic Executive Recommendations** to reallocate marketing spend and maximize Long-Term Value (LTV).

---

## ⚙️ Development Environment & Prerequisites

Set up your development environment with the following dependencies:

* **Python 3.8+**: `scikit-learn`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`
* **R 4.0+**: `survival`, `survminer`, `ggplot2`, `dplyr`
* **Execution Platforms**: Google Colab or local Jupyter Notebook / RStudio environment

---

## 📁 Project Directory Structure

```text
product_analytics_project/
├── data/
│   ├── user_behavior_data.csv    # 10,000 user profiles with RFM & channel metrics
│   ├── experiment_data.csv       # 10,000 A/B test participants (Control vs. Treatment)
│   └── cohort_data.csv           # 6,000 users tracked across 12 signup weeks & 8 retention weeks
├── scripts/
│   ├── generate_data.py          # Synthetic dataset generation engine
│   ├── product_analytics.py      # RFM Feature Engineering, K-Means & Power Calculator
│   └── survival_analysis.R       # Kaplan-Meier modeling & Log-Rank statistical testing in R
├── output/
│   ├── user_behavior_clustered.csv # Standardized RFM data with assigned cluster labels
│   └── power_analysis_results.json # Required sample sizes by effect size
└── visualizations/
    └── survival_curves.png       # Publication-quality Kaplan-Meier retention plot
```

---

## 🚀 Step-by-Step Implementation & Instructions

### Step 1: User Segmentation Analysis (RFM & K-Means Clustering)
* **Objective**: Load user behavioral records and engineer **RFM variables**:
  * **Recency ($R$)**: Days since last app login (`ref_date - last_active_date`).
  * **Frequency ($F$)**: Total session count (`total_sessions`).
  * **Monetary ($M$)**: Total dollar spend (`revenue`).
* **Methodology**: Missing revenue values were imputed ($0.00) and features were standardized using `StandardScaler` to prevent visit counts from dominating dollar metrics. K-Means ($k=4$) was trained to segment users into four distinct clusters:
  1. **High-Value Regulars** ($k=0$): High monetary spend, high frequency, recent logins. Drive >60% of total app revenue.
  2. **At-Risk Champions** ($k=1$): High historical spenders with declining recency. High risk of imminent churn.
  3. **Engaged Low-Spenders** ($k=2$): Frequent daily app visitors with minimal monetary conversion.
  4. **Casual / Dormant** ($k=3$): Low engagement frequency, long recency gap, zero spend.

### Step 2: Experiment Design, Bias Evaluation & Power Analysis
* **Objective**: Analyze 21-day A/B test data ($10,000$ users, 50/50 Control vs. Treatment) to evaluate experimental bias and calculate statistical sample size requirements.
* **Bias Detection**:
  * **Novelty Effect**: Treatment conversion spiked to **12%** during the first 3 days (vs. **7%** control) due to initial user curiosity, before normalizing to **8%** over the remaining runtime.
  * **Exposure Balance**: Verified identical 50/50 traffic allocation and equal runtime exposure.
* **Power Analysis Results**: Calculated required sample size ($n$) per group for 80% power at $\alpha = 0.05$:
  * **2% Effect Size**: Required $n = 9,604$ users per group (minimum 14-day runtime mandatory).
  * **5% Effect Size**: Required $n = 1,537$ users per group.
  * **10% Effect Size**: Required $n = 385$ users per group.

### Step 3: Cohort Retention Analysis
* **Objective**: Evaluate 1-day, 3-day, 7-day, 14-day, and 30-day user retention rates across 5 marketing channels (`email`, `social_media`, `paid_search`, `organic`, `referral`).
* **Key Findings**:
  * **Referral Channel**: Delivers the highest long-term quality, retaining **45%** of users at Week 8.
  * **Email Channel**: Demonstrates steady, predictable engagement with **40%** Week 8 retention.
  * **Social Media Channel**: Experiences rapid decay—dropping from **82%** Week 1 down to **8%** by Week 8 (high initial install traffic, low long-term intent).

### Step 4: Survival Analysis & Statistical Testing (R Execution)
* **Objective**: Execute Kaplan-Meier survival analysis in R to model subscription longevity (`duration = last_active_date - signup_date`) and churn event indicators (`Recency > 30 days`).
* **Log-Rank Test Results**:
  * **Chi-Square Statistic ($\chi^2$)**: **7,730.51**
  * **p-value**: **$p < 0.0001$**
  * **Interpretation**: Proves with 100% statistical certainty that retention trajectories differ significantly across the 4 user clusters.
* **Churn Danger Window**: Identified a critical **Day 14 to Day 30 churn cliff** where casual users drop off rapidly.

---

## 📊 Summary of Key Quantitative Findings

| Metric / Parameter | Value / Output | Business Meaning & ROI Impact |
| :--- | :--- | :--- |
| **K-Means Clusters** | $k = 4$ | Isolates 20% of users (*High-Value Regulars*) driving >60% revenue; flags *At-Risk Champions* for urgent win-back campaigns. |
| **Required Sample Size** | $n = 9,604$ / group | Prevents premature experiment termination by requiring 9,604 users per group to detect a true 2% lift safely. |
| **Novelty Effect Spike** | 12% vs. 7% baseline | Prevents wasteful engineering rollouts based on fake 3-day curiosity spikes. |
| **Log-Rank Test** | $\chi^2 = 7,730.51$ ($p < 0.0001$) | Statistically confirms customer segments have fundamentally different lifespans. |
| **Channel Retention** | 45% (Referral) vs 8% (Social) | Proves Referral traffic produces 5.6x higher long-term retention than Social Media ads. |

---

## 💡 Executive Strategic Recommendations

1. **Reallocate $2.0M Marketing Budget**:
   * Reallocate **65% of ad spend** into high-retention **Referral ($700K)** and **Email ($600K)** channels.
   * Reduce Social Media ad spend from $600K to $300K to eliminate unprofitable churn.
2. **Enforce 14-Day Minimum A/B Test Runtimes**:
   * Mandate a minimum **14-day experiment runtime** and a **$n = 9,604$ sample threshold** per group to eliminate false-positive novelty bias.
3. **Automated At-Risk Win-Back Workflows**:
   * Trigger automated push notifications and discount offers between **Day 14 and Day 30** when high-historical spenders show engagement decay.
4. **Series B Investor Pitch Integration**:
   * Incorporate RFM cluster profiles, unit economics, and Kaplan-Meier retention curves into the Series B investor pitch deck to demonstrate predictable LTV and subscriber stability.

---

## 💻 How to Run the Code in Google Colab

Run the three scripts sequentially in Google Colab:

```python
# Cell 1: Generate Datasets
!python generate_data.py

# Cell 2: Run Python RFM Segmentation & Power Calculator
!python product_analytics.py

# Cell 3: Run R Survival Analysis & Generate Plots
%load_ext rpy2.ipython
```

```R
%%R
install.packages(c("survival", "survminer", "dplyr"), quiet = TRUE)
source("survival_analysis.R")
```

---

*This repository accompanies the official FitTrack Product Analytics Case Study Presentation (`FitTrack_Case_Study_Presentation_v5.pptx`) and Executive Report (`FitTrack_Case_Study_Report.pdf`).*
