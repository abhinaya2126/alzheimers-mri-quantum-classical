# Hybrid Quantum-Classical vs Classical Deep Learning for Four-Class Alzheimer's MRI Classification

A comparative deep-learning study of classical ResNet18 and hybrid quantum-classical classifiers for four-class Alzheimer's MRI image classification.

## Project overview

This project classifies 2-D structural MRI images into four dataset-provided categories:

- `NonDemented`
- `VeryMildDemented`
- `MildDemented`
- `ModerateDemented`

Both model families use an ImageNet-pretrained ResNet18. The classical models fine-tune the encoder and classification head. The hybrid models use frozen 512-dimensional ResNet18 features, project them to eight values, encode those values into an 8-qubit variational quantum circuit (VQC), and classify the measured outputs. Hybrid C additionally uses a parallel classical branch.

> **Research-use notice:** This repository is an academic image-classification experiment, not a clinical diagnostic tool. Predictions must not be used to diagnose Alzheimer's disease or guide treatment.

## Repository structure

```text
alzheimers-mri-quantum-classical/
├── notebooks/
│   ├── classical_resnet18.ipynb
│   └── hybrid_quantum_classical.ipynb
├── reports/
│   └── final_project_report.pdf
├── README.md
├── RESULTS.md
├── DATASET.md
├── requirements.txt
└── .gitignore
```

## Dataset

The project uses the **Augmented Alzheimer MRI Dataset** by `uraninjo` on Kaggle:

https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset

The dataset contains 2-D MRI JPEG images in two configurations:

| Class | Original folder | Augmented folder |
|---|---:|---:|
| NonDemented | 3,200 | 9,600 |
| VeryMildDemented | 2,240 | 8,960 |
| MildDemented | 896 | 8,960 |
| ModerateDemented | 64 | 6,464 |
| **Total** | **6,400** | **33,984** |

The augmented folder is derived from the original images using image transformations; it should not be interpreted as 33,984 independent MRI scans. See [DATASET.md](DATASET.md) for download and folder details.

**The dataset is not included in this repository.** Download it from Kaggle and follow its terms of use. Do not commit the dataset, patient data, or other restricted material to GitHub.

## Methodology

1. Load the Kaggle image folders and map the four class labels.
2. Resize images to `224 × 224`, convert to three channels, and normalise using ImageNet statistics.
3. Fine-tune an ImageNet-pretrained ResNet18 for the classical baseline.
4. Remove the ResNet18 classification layer and extract 512-dimensional image features.
5. Project the features to eight rotation angles.
6. Encode the angles into an 8-qubit circuit using angle embedding.
7. Apply two `StronglyEntanglingLayers` and measure Pauli-Z expectation values.
8. Train a classical output head; Hybrid C also fuses a parallel classical branch.
9. Evaluate using accuracy, precision, recall, F1-score, confusion matrices and computational measurements.

### Main technologies

- Python
- PyTorch and Torchvision
- PennyLane
- scikit-learn
- NumPy, Pandas and Matplotlib
- Jupyter Notebook / Kaggle

## Setup and execution

### Option 1: Run on Kaggle (recommended)

The notebooks were developed in a Kaggle environment and use Kaggle input paths.

1. Open the dataset page: https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset
2. Sign in to Kaggle and accept the dataset's terms, if prompted.
3. Create a Kaggle Notebook and add the dataset using **Add Input**.
4. Upload or import the relevant notebook from this repository.
5. Confirm the dataset path in the notebook. The hybrid notebook uses:

   ```python
   DATA_PATH = "/kaggle/input/datasets/uraninjo/augmented-alzheimer-mri-dataset"
   ```

   If Kaggle mounts the dataset under a different directory, update this path to match the folder shown in the notebook's Input panel.
6. Select a GPU accelerator for ResNet18 training when available. The quantum circuit is simulated using PennyLane.
7. Run the notebook cells in order.

### Option 2: Run locally

Use Python 3.10 or 3.11 in a virtual environment. Install the dependencies:

```bash
python -m venv .venv
```

Activate it:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS / Linux
source .venv/bin/activate
```

Install packages:

```bash
pip install -r requirements.txt
```

Install and launch Jupyter:

```bash
jupyter notebook
```

Open a notebook under `notebooks/`. Update its dataset path to the location where you extracted the Kaggle dataset. The notebooks contain environment-specific paths and may need minor path adjustments outside Kaggle.

For local GPU training, install a PyTorch build compatible with your CUDA version by following the official PyTorch installation selector: https://pytorch.org/get-started/locally/

## Results

The final project report records the following test accuracies:

| Model | Dataset/configuration | Test accuracy |
|---|---|---:|
| Classical Model 1 – ResNet18 | Augmented dataset | **94.69%** |
| Classical Model 2 – ResNet18 | Original dataset | **96.78%** |
| Hybrid Quantum Model 1 – circuit-only head | Configuration B, 600 test images | **85.83%** |
| Hybrid C – quantum + classical branches | Configuration B, 600 test images | **88.33%** |

Additional reported results:

- Hybrid C macro-F1: **0.8821**
- Hybrid Quantum Model 1: **515/600** correct predictions
- Hybrid C: **530/600** correct predictions
- Hybrid Quantum Model 1 trainable head parameters: **4,188**
- ResNet18 total parameters in the recorded comparison: **11,178,564**
- Hybrid circuit-only head recorded inference time: **10.78 ms/image** on CPU using cached features.
- ResNet18 recorded inference time: **3.21 ms/image** on GPU.

The original-data classical model has the highest reported accuracy. These results come from different data configurations/evaluation stages and should be interpreted descriptively, not as a paired head-to-head statistical test. The report also identifies further controls needed, including a matched group-aware evaluation and a quantum-branch ablation. See [RESULTS.md](RESULTS.md) for details.

## Reproducibility notes and limitations

- Earlier development splits use seed `42`; Hybrid C training uses seed `123`.
- The updated classical evaluation uses `StratifiedGroupKFold` to reduce leakage between samples with the same assigned group identifier.
- The hybrid results reported in the project report are from earlier Configuration B experiments. They have not been presented as results from the updated group-aware classical folds.
- The quantum circuit is evaluated with a noiseless state-vector simulator and exact expectation values; this is not a demonstration of quantum-hardware speedup.
- Reproduction requires the dataset, the relevant group identifiers/fold assignments, and compatible software versions. The notebooks preserve the experimental workflow, but paths and environment setup may need adjustment.

## Authors

- Reddi Abhinaya Sai
- Rayankula Nidhi

Project report submitted to Amrita Vishwa Vidyapeetham, Amrita School of Computing, Chennai.

## Citation

If you use this project, cite the project report and the original Kaggle dataset. Dataset reference:

`uraninjo, "Augmented Alzheimer MRI Dataset," Kaggle.`  
https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset
