# Fraud and Credit Risk 
This repository contains **synthetic datasets** and Jupyter notebooks designed to simulate the joint modeling of **fraud detection** and **credit risk (PD, IRB/IRK)**.  
It is intended for **educational and demonstration purposes only**, showing how to keep **fraud** separate from **default risk** while combining them in a **policy triage framework** with economics.  

The two channels modeled are:  
- **Fraud** ($\hat p_F$) — probability an application is fraudulent  
- **Credit PD** ($\hat p_D$) — probability of default given non-fraud  

Together with **loan amount (EAD)** and **LGD**, these allow expected losses and portfolio P&L to be simulated transparently.  

---

## 1. Fraud Modeling  

Notebook: `notebooks/1a_fraud_model.ipynb`  

Key features:  
- Synthetic fraud dataset (imbalanced, grouped by customer)  
- Logistic Regression baseline with calibration (isotonic CV)  
- Diagnostics: ROC AUC, PR-AUC, prevalence alignment  
- Calibration metrics: Brier, Expected Calibration Error (ECE), Max Calibration Error (MCE)  

---

## 2. PD Modeling  

Notebook: `notebooks/1b_pd_model.ipynb`  

Key features:  
- Synthetic PD dataset, conditional on $F=0$  
- Logistic Regression baseline with monotonicity-friendly design  
- Isotonic CV calibration and evaluation  
- Metrics: ROC AUC, PR-AUC, Brier, ECE/MCE  
- Reliability diagrams with quantile binning  

---

## 3. Policy Simulator  

Notebook: `notebooks/2_policy_simulator.ipynb`  

Core logic:  
1. **Triage rules:** BLOCK, REVIEW, or DIRECT-to-PD  
2. **PD gating:** approve if $\hat p_D \leq \tau_{PD}$  
3. **Expected economics:** revenue, credit EL, fraud EL, capital, costs  

Key features:  
- Grid search over fraud percentiles & PD cutoffs  
- Selection of thresholds under approval/review constraints  
- Validation summary: approval %, review %, block %, P&L breakdown  

---

## 4. Results 

- Approval ≈ **45.9%**  
- Review ≈ **1.9%**  
- Block ≈ **3.0%**  
- P&L ≈ **+1.77M**  

Pareto band observed near $t_{block} \approx Q_{0.98}(\hat p_F)$ and $\tau_{PD} \in [0.08, 0.12]$, enabling explicit trade-offs between **volume** and **fraud EL**.  

---

## 5. Roadmap  

- Out-of-time (OOT) validation & PSI drift monitoring  
- Fairness slice reporting (channel, age bands)  
- Threshold optimizer (Bayesian search)  
- Persist artifacts (models, thresholds, predictions)  
- Scenario stress tests (fraud spikes, macro shocks)  

---

## Disclaimer  

All datasets in this repository are **synthetically generated** for demonstration purposes only.  
They do **not** represent any real financial institution’s data and are **not suitable for production use**.  

The aim is to illustrate the methodology: **fraud modeling, PD calibration, policy economics, and trade-off analysis** in a transparent and reproducible way.  
