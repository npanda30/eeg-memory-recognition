# EEG Predictors of Visual Memory Recognition Timing

**Neha Panda · UNC Chapel Hill · Neuroscience & Statistics**

---

## Overview

Can a machine learning model predict whether a person will recognize a movie clip — and how quickly — based on their brain activity alone?

This project uses EEG (electroencephalography) data from 27 adult participants to classify three memory recognition states:

- **NotRecognised** — the participant failed to recognize the clip
- **RecognisedFirstWatch** — the participant recognized the clip but did not immediately recall its source film
- **RememberedFirstWatch** — the participant immediately recalled having seen the source film before

A Random Forest classifier was trained on EEG-derived features including bandpower (alpha, beta, theta, delta), functional connectivity, and participant demographics, achieving **71% Leave-One-Subject-Out (LOSO) cross-validation accuracy**.

---

## Key Findings

- All four EEG frequency bands (alpha, beta, theta, delta) showed statistically significant differences across the three memory states (Kruskal-Wallis, p < 0.05)
- LOSO accuracy of 71% is well above the 33% chance baseline for a 3-class problem
- The classifier was heavily biased toward the majority class (NotRecognised) due to class imbalance — an honest limitation discussed in the paper
- Functional connectivity patterns (Pearson correlation across 64 channels) provided a rich but high-dimensional feature space, requiring PCA to reduce to 50 components retaining 95% of variance

---

## Methods

| Step | Tool/Method |
|---|---|
| Data loading | MNE-BIDS |
| EEG preprocessing | Baseline correction, high-pass filter (1 Hz Butterworth), average reference |
| Feature extraction | Bandpower via Welch PSD; functional connectivity via Pearson correlation |
| Dimensionality reduction | PCA (50 components, 95% variance retained) |
| Classification | Random Forest (200 estimators, balanced class weights) |
| Validation | Leave-One-Subject-Out (LOSO) cross-validation |
| Statistics | Shapiro-Wilk, Levene's, Kruskal-Wallis |

---

## Dataset

Data sourced from [OpenNeuro ds006142](https://openneuro.org/datasets/ds006142/versions/1.0.2) (Essex EEG Movie Memory dataset, Matran-Fernandez & Halder, 2025). 27 participants, BioSemi ActiveTwo system, 64 electrodes, 2048 Hz sampling rate.

---

## Repository Structure

```
├── FinalProject_Code.ipynb   # Full analysis pipeline
└── README.md
```

> **Note:** The raw EEG data files are not included due to size. Download the dataset from [OpenNeuro](https://openneuro.org/datasets/ds006142/versions/1.0.2) and place subject folders in the same directory as the notebook before running.

---

## How to Run

1. Clone this repository
2. Download the dataset from OpenNeuro (link above)
3. Install dependencies:
```bash
pip install mne mne-bids numpy pandas scipy scikit-learn matplotlib seaborn
```
4. Open `FinalProject_Code.ipynb` and run cells in order

---

## Limitations

- Strong class imbalance (NotRecognised >> other classes) limits minority-class recall
- Small sample size (n=27) constrains generalizability
- Pearson correlation captures only linear connectivity; frequency-specific metrics (e.g., phase-locking value) may capture richer signal
- PCA compresses variance but does not guarantee preservation of task-relevant features

---

## Future Directions

- Larger, balanced dataset with richer demographic metadata
- Deep learning approaches (e.g., EEGNet) for raw signal classification
- Continuous regression model to predict the precise timing of recognition
- Frequency-specific connectivity metrics (coherence, phase-locking value)
- ICA-based artifact rejection for cleaner signal

---

## References

Matran-Fernandez, A., & Halder, S. (2025). Essex EEG Movie Memory dataset. OpenNeuro. doi:10.18112/openneuro.ds006142.v1.0.2
