# EEG Predictors of Visual Memory Recognition Timing

**Neha Panda · UNC Chapel Hill · Neuroscience & Statistics**

---

## Overview

Can a machine learning model predict whether a person will recognize a movie clip (and how quickly) based on their electrical brain activity?

This project uses EEG (electroencephalography) data from 27 adult participants to classify three memory recognition states:

- **NotRecognised** — the participant failed to recognize the clip
- **RecognisedFirstWatch** — the participant recognized the clip but did not immediately recall its source film
- **RememberedFirstWatch** — the participant immediately recalled having seen the source film before

A Random Forest classifier was trained on EEG-derived features including bandpower (alpha, beta, theta, delta), functional connectivity, and participant demographics, achieving **71% Leave-One-Subject-Out (LOSO) cross-validation accuracy**.

---

## Key Findings:

- All four EEG frequency bands (alpha, beta, theta, delta) showed statistically significant differences across the three memory states (Kruskal-Wallis, p < 0.05)
- LOSO accuracy of 71% is well above the 33% chance baseline, and found to be significant by Welch's t-test (p=1.29e-13)
- The classifier was heavily biased toward the majority class (NotRecognised) due to class imbalance, which is a key limitation of this project as-is
- Functional connectivity patterns (Pearson correlation across 64 channels) provided a high-dimensional feature space, requiring PCA to reduce to 50 components while retaining 95% of variance

---

## Methods

1. EEG data were loaded using a MNE-BIDS pipeline with a helper script provided by Matran-Fernandez & Halder.
2. Preprocessing included applying a BioSemi 64-channel montage, average referencing, baseline correction, and a 1 Hz high-pass Butterworth filter. Data were epoched from 0 to 1.0 seconds following each event onset and downsampled x32.
3. Bandpower features (alpha, beta, theta, delta) were extracted for each epoch using Welch's power spectral density method.
4. Functional connectivity was computed as pairwise Pearson correlation across all 64 EEG channels, yielding 2,016 unique connectivity features per epoch. Demographic variables (age, sex, handedness) were merged from participant metadata.
5. PCA was applied after standard scaling, retaining 50 principal components that accounted for 95% of the total variance.
6. A Random Forest classifier (200 estimators and balanced class weights) was trained to predict one of three recognition labels per epoch.
7. Model performance is evaluated using Leave-One-Subject-Out (LOSO) cross-validation, where the model is trained on 26 subjects and tested on the held-out subject, repeated for all 27 subjects.
8. Normality and homogeneity of variance were assessed with Shapiro-Wilk and Levene's tests. Group differences in bandpower across recognition states were tested with Kruskal-Wallis. Classifier accuracy above chance was confirmed with Welch's t-test (p = 1.29e-13).

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
- Pearson correlation captures only linear connectivity; frequency-specific metrics could possibly capture richer signal
- PCA compresses variance but does not guarantee preservation of task-relevant features

---

## Future Directions

- Larger, balanced dataset with richer demographic metadata
- Deep learning approaches (e.g., EEGNet) for raw signal classification
- Continuous regression model to predict the precise timing of recognition
- Frequency-specific connectivity metrics (coherence, phase-locking value)
- ICA-based artifact rejection for cleaner signal

---

## Acknowledgements

The author thanks Dr. Rosie Dutt and Jonathan Leung for advice on experimental design, relevant pipelines & packages, and statistical analysis. Helper script courtesy of Matran-Fernandez & Halder (2025).

---

## References

Matran-Fernandez, A., & Halder, S. (2025). Essex EEG Movie Memory dataset. OpenNeuro. doi:10.18112/openneuro.ds006142.v1.0.2
