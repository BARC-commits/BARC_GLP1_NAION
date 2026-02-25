# BARC Tutorial: Bayesian Pharmacovigilance with PyMC

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![PyMC 5.27.0](https://img.shields.io/badge/PyMC-5.27.0-purple.svg)](https://github.com/pymc-devs/pymc)

## 📌 Overview

This repository contains a **complete, well-commented tutorial notebook** demonstrating how to implement a hierarchical Bayesian model for pharmacovigilance signal assessment using PyMC. The code integrates four evidence streams to quantify drug-adverse event relationships:

| Evidence Stream | Statistical Approach |
|-----------------|----------------------|
| Randomized Controlled Trials | Poisson likelihood |
| Case Report Time Series | Negative Binomial with temporal effects |
| Pharmacovigilance Databases | Normal with bias correction |
| Mechanistic Studies | Normal with between-study heterogeneity |

**This is a teaching tool, not a production-ready package.** It provides a working example that you can modify for your own analyses.

---

## ⚠️ Important Disclaimer

**This tutorial uses SIMULATED DATA patterned after the GLP-1/NAION controversy for demonstration only. It is NOT an analysis of the actual safety signal.** No conclusions about real drug safety should be drawn from the example output.

---

## 🎯 What This Is

- ✅ A fully documented, step-by-step PyMC implementation
- ✅ A working example that runs out-of-the-box
- ✅ Clear configuration section for user modification
- ✅ Best practices (non-centered parameterization, time-varying effects)
- ✅ Decision-relevant outputs with uncertainty quantification

## ❌ What This Is NOT

- ❌ Not a Python package (you cannot `pip install` it)
- ❌ Not production-ready code (no unit tests, limited error handling)
- ❌ Not validated on real data (uses simulated data)
- ❌ Not a substitute for domain expertise

---

## 🧠 The Model

### Hierarchical Structure

The model features a latent pathway parameter (θ) that connects all evidence streams:
- **θ > 0**: Harmful pathway activation
- **θ ≈ 0**: No effect
- **θ < 0**: Protective effect (biologically unlikely for this example)

### Key Statistical Features

| Feature | Implementation |
|---------|---------------|
| **Non-centered parameterization** | Improves MCMC sampling efficiency |
| **Time-varying effects** | Gaussian random walks for case reports |
| **Reporting bias correction** | Explicit bias term for pharmacovigilance data |
| **Between-study heterogeneity** | Hierarchical meta-analysis for mechanistic studies |

### Decision-Relevant Outputs

All quantities are computed directly from posterior samples (no approximations):

| Output | Interpretation |
|--------|----------------|
| Incidence Rate Ratio (IRR) | Relative risk on rate scale |
| Excess absolute risk | Additional cases per 100,000 patient-years |
| Number Needed to Harm (NNH) | Patient-years exposure for one additional event |
| P(excess > threshold) | Probability of clinically meaningful harm |
| Posterior probability of causation | Combines prior belief with evidence |

The code includes discussion of **skewness** (why mean and median differ for NNH) and proper probability computation.

---

## 📋 Requirements

The tutorial uses specific versions for reproducibility:

```text
pymc==5.27.0
arviz==0.22.0
numpy==2.0.2
pytensor==2.36.0
matplotlib==3.10.0
pandas==2.2.3
```

These are installed automatically when running the notebook in Kaggle or Google Colab.

---

## 🚀 Quick Start

### Option 1: Run in Kaggle (Recommended - 5 minutes)

1. Go to [Kaggle](https://kaggle.com) and sign in
2. Click "Create" → "New Notebook"
3. Delete all existing cells
4. Copy the entire code from [`barc_tutorial.py`](barc_tutorial.py) (or the notebook)
5. Paste into the first cell and run all cells (Shift+Enter)

### Option 2: Run in Google Colab

1. Go to [Google Colab](https://colab.research.google.com)
2. File → New Notebook
3. Copy-paste the entire code
4. Runtime → Run all

### Option 3: Run Locally

```bash
# Clone the repository
git clone https://github.com/BARC-commits/BARC_GLP1_NAION
cd BARC_GLP1_NAION

# Create a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the notebook
jupyter notebook barc_tutorial.ipynb
```

---

## 📖 Tutorial Structure

The notebook is organized into logical sections:

| Section | Description |
|---------|-------------|
| **1. Installation** | Sets up environment with pinned dependencies |
| **2. User Configuration** | **ALL TUNABLE PARAMETERS IN ONE PLACE** - Modify this for your data |
| **3. Data Validation** | Basic checks to catch common errors |
| **4. Model Specification** | Line-by-line PyMC implementation |
| **5. MCMC Sampling** | Runs No-U-Turn Sampler with convergence diagnostics |
| **6. Convergence Checks** | R-hat, ESS, divergences, trace plots |
| **7. Posterior Predictive Checks** | Validates model fit |
| **8. Results Analysis** | Computes all quantities from posterior samples |
| **9. Plain Language Interpretation** | Dynamic output matching your results |
| **10. Export** | Saves results to CSV and NetCDF |

---

## 🔧 Using With Your Own Data

### Step-by-Step Guide

1. **Make a copy** of the notebook for each new analysis
2. **Navigate to Section 2** (USER CONFIGURATION)
3. **Replace the example values** with your actual data:

```python
# Replace these with your values
GLP1_EVENTS = 8        # Your exposed group events
GLP1_PY = 144226        # Your exposed person-years
COMP_EVENTS = 4         # Your comparator events
COMP_PY = 132922        # Your comparator person-years

# Your case report time series (monthly counts)
CASE_COUNTS = [0, 1, 0, 0, ...]  # Your actual monthly data

# Your pharmacovigilance data
ROR_OBSERVED = 1.23     # Your Reporting Odds Ratio
ROR_SE = 0.35           # Your standard error

# Your mechanistic study effect sizes
MECH_EFFECTS = [0.3, 0.5, -0.1, ...]  # Your study-level data
```

4. **Adjust priors and multipliers** if needed (all commented)
5. **Run all cells** (Section 5 may take 1-2 minutes)
6. **Check convergence** in Section 6 (all R-hat < 1.01, no divergences)
7. **Interpret results** from Section 8-9

### Data Format Requirements

| Data Type | Required Format | Notes |
|-----------|-----------------|-------|
| RCT events | Positive integers | Can be zero |
| Person-years | Positive numbers | Can be fractional |
| Case reports | 1D list of integers | Monthly counts, any length |
| ROR | Positive number | >0 |
| ROR_SE | Positive number | Standard error of log(ROR) |
| Mechanistic effects | 1D list of floats | Any length ≥1 |

---

## 📊 Example Output

When run with the provided simulated data, you'll see output like:

```
IRR: 1.18 [0.87, 1.59]
P(IRR > 1): 84.2%
Baseline NAION rate: 3.5 [2.0, 5.7] per 100k PY
Excess rate (median): 0.56 per 100k PY
NNH (median): 180,000 patient-years
P(excess > 1/100k): 24.2%
```

**Note:** These numbers vary slightly between runs due to MCMC randomness, even with fixed seed. Your actual output will be similar but not identical.

---

## ⚠️ Limitations

| Limitation | Implication |
|------------|-------------|
| **Simulated data only** | Not validated on real pharmacovigilance data |
| **Basic input validation** | May fail ungracefully with unexpected inputs |
| **No unit tests** | Cannot guarantee correctness for all use cases |
| **Single model structure** | May need adaptation for different drug-event pairs |
| **No dose-response** | Models binary exposure only |
| **Conditional independence assumed** | Evidence streams may share unmeasured confounders |

For production use, consider:
- Adding comprehensive input validation
- Implementing unit tests
- Validating on historical drug-event pairs
- Extending model for dose-response or multiple drugs

---

## 📚 Citation

If you use this tutorial in your work, please cite:

```
Tan Aik Kah (2025). BARC Tutorial: Hierarchical Bayesian Evidence 
Integration for Pharmacovigilance. GitHub repository:
https://github.com/BARC-commits/BARC_GLP1_NAION
```

BibTeX:
```bibtex
@misc{tan2025barc,
  author = {Tan, Aik Kah},
  title = {BARC Tutorial: Hierarchical Bayesian Evidence Integration for Pharmacovigilance},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/BARC-commits/BARC_GLP1_NAION}
}
```

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

This is a tutorial repository, not an active software project. However:

- **Bug reports**: Open an issue with the error message and environment details
- **Clarifications**: Pull requests improving comments or documentation are welcome
- **Extensions**: Consider forking for your own modifications

Before contributing, please read the [limitations](#-limitations) section to understand the scope.

---

## 📬 Contact

Tan Aik Kah  
Clinique d'ophthalmologie, Normah Medical Specialist Centre  
Email: portwinestain@hotmail.com  

For questions about the methodology, please open an issue rather than emailing.

---

## 🙏 Acknowledgments

- The PyMC development team for their excellent probabilistic programming framework
- The ArviZ team for visualization and diagnostics tools
- The pharmacovigilance community for ongoing discussions on evidence synthesis

---

**Happy modeling!** 🎲
```
