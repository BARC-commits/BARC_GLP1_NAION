# BARC: Bayesian Assessment of Research Causality

[![PyMC](https://img.shields.io/badge/PyMC-5.0+-blue.svg)](https://github.com/pymc-devs/pymc)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

A hierarchical Bayesian model for pharmacovigilance signal assessment that integrates multiple evidence streams to quantify drug-adverse event relationships.

## Overview

BARC addresses a fundamental challenge in pharmacovigilance: how to systematically integrate heterogeneous evidence sources to assess potential drug safety signals. The model synthesizes:

- **Randomized Controlled Trial Data** (Poisson likelihood)
- **Case Report Time Series** (Negative Binomial with temporal effects)
- **Pharmacovigilance Databases** (Normal with bias correction)
- **Mechanistic Studies** (Normal with between-study heterogeneity)

## Key Innovations

- **Full time-series modeling** of case reports (not aggregated to means)
- **Non-centered parameterization** for efficient MCMC sampling
- **Proper probability computation** from posterior samples (no approximations)
- **Skewness-aware metrics** for excess risk and Number Needed to Harm (NNH)
- **Comprehensive convergence diagnostics** with manual trace plots for stability

## Statistical Features

- Hierarchical structure with latent pathway parameter (θ)
- Prior calibration based on clinical relevance
- Time-varying effects via Gaussian random walks
- Reporting bias modeling for spontaneous reports
- Decision-relevant derived quantities:
  - Incidence Rate Ratio (IRR)
  - Excess absolute risk (per 100,000 patient-years)
  - Number Needed to Harm (NNH) with uncertainty intervals
  - Probability of clinically meaningful harm

## Example Applications

This framework is particularly suited for:
- Post-marketing drug safety surveillance
- Signal detection and quantification
- Regulatory decision support
- Meta-analysis of heterogeneous evidence
- Benefit-risk assessment

## Tutorial Notebook

The included tutorial demonstrates the complete workflow:
1. Simulated data generation with realistic properties
2. Model specification with calibrated priors
3. MCMC sampling with convergence diagnostics
4. Posterior analysis with proper probability computation
5. Sensitivity analysis and model extensions
6. Plain language interpretation of results

## Requirements

- Python 3.8+
- PyMC 5.0+
- ArviZ
- NumPy, Pandas
- Matplotlib, Seaborn

## Citation

If you use BARC in your research, please cite:
Tan Aik Kah(2025). BARC: Hierarchical Bayesian Evidence Integration for Rare Safety Signals: A Reproducible Tutorial Using Glucagon-Like Peptide-1 (GLP1) Receptor Agonist/Non-Arteritic Ischemic Optic Neuropathy (NAION) Data. Retrieved from 
https://github.com/BARC-commits/BARC_GLP1_NAION
