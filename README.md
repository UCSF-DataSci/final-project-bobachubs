# EEG Motor Imagery Classification: Evaluating ICA-Based Artifact Removal

**Author:** Sarah Li - DataSci 223

## Overview
This project evaluates whether Independent Component Analysis (ICA) preprocessing improves classification accuracy for EEG motor imagery tasks using the PhysioNet EEGMMIDB dataset.

## Repository Contents
```
├── README.md                   # This file
├── Project.ipynb               # Complete analysis pipeline
├── requirements.txt            # Python dependencies
├── FinalReport.pdf             # Detailed project report
└── FinalPresentation.pptx/     # Presenation Explanation
```

## Dataset
- **Source**: PhysioNet EEG Motor Movement/Imagery Database (EEGMMIDB)
- **Subjects**: 4 (subset for computational efficiency)
- **Classes**: Rest, Left fist, Right fist, Feet movement imagery
- **Channels**: 64 EEG electrodes (10-20 montage)
- **Sample Rate**: 160 Hz

## Methods
- **Preprocessing**: ICA artifact removal using automated ICLabel
- **Feature Extraction**: Time/frequency domain, Hjorth parameters, CSP
- **Models**: Random Forest, MLP, EEGNet (domain-specific CNN)
- **Evaluation**: Classification accuracy on raw vs. ICA-cleaned data

## Key Results
| Model | Raw EEG | ICA Cleaned |
|-------|---------|-------------|
| Random Forest | 62.4% | 60.9% |
| MLP | 66.1% | 66.7% |
| **EEGNet** | **70.7%** | 68.4% |

**Finding**: ICA preprocessing did not consistently improve classification performance.

## How to Run

### Prerequisites
```bash
pip install numpy pandas matplotlib seaborn scikit-learn scipy torch mne mne-icalabel
```

### Dataset Setup
1. Download EEGMMIDB from: https://physionet.org/content/eegmmidb/1.0.0/
2. Extract to `data/physionet.org/files/eegmmidb/1.0.0/`

### Execution
```bash
Project.ipynb
```

## Output
- Confusion matrices for each model/data combination
- Training curves and performance comparisons
- Classification accuracy metrics
- Console logs with hyperparameter optimization results

## Issues Overcome

### 1. Channel Name Standardization
**Problem**: Inconsistent EEG channel naming across files
```python
def clean_ch_name(ch):
    ch = ch.strip('.')
    if ch.startswith('Fc'): ch = 'FC' + ch[2:]
    # ... additional mappings
```

### 2. Class Imbalance
**Problem**: Majority class (Rest/Baseline Task) dominated dataset
**Solutions**: 
- Stratified train-test split
- Class-weighted Random Forest
- Focused analysis on per-class metrics

### 3. Poorly Annotated Data
**Problem**: Some of the data produced corrupt task annotations and only had 1
**Solutions**: Skip them


## Citations
- **Dataset**: Goldberger et al. (2000). PhysioBank, PhysioToolkit, and PhysioNet. *Circulation*, 101(23), e215–e220.
- **EEGNet**: Lawhern et al. (2018). EEGNet: a compact convolutional neural network for EEG-based brain–computer interfaces. *Journal of Neural Engineering*, 15(5), 056013.

## Notes
- Complete methodology, results, and discussion available in `FinalProject.pdf`
- Code includes implementations adapted from published methods
- ICA processing uses MNE-Python with automated component labeling