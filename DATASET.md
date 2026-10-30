# Dataset information

## Source

**Augmented Alzheimer MRI Dataset** by `uraninjo` on Kaggle:

https://www.kaggle.com/datasets/uraninjo/augmented-alzheimer-mri-dataset

The project report describes 2-D structural MRI slices stored as JPEG images in two folders, each with four class subfolders.

## Class distribution

| Class | Original | Augmented |
|---|---:|---:|
| NonDemented | 3,200 | 9,600 |
| VeryMildDemented | 2,240 | 8,960 |
| MildDemented | 896 | 8,960 |
| ModerateDemented | 64 | 6,464 |
| **Total** | **6,400** | **33,984** |

The augmented images are derived from the original images through image transformations. Augmentation increases the number of files but does not create independent source MRI scans.

## Expected folder layout

```text
augmented-alzheimer-mri-dataset/
├── OriginalDataset/
│   ├── MildDemented/
│   ├── ModerateDemented/
│   ├── NonDemented/
│   └── VeryMildDemented/
└── AugmentedAlzheimerDataset/
    ├── MildDemented/
    ├── ModerateDemented/
    ├── NonDemented/
    └── VeryMildDemented/
```

Use the actual folder names and mount path provided by Kaggle; update the `DATA_PATH`/`PATH` variables in the notebooks accordingly.

## Preprocessing described in the project

- Resize each image to `224 × 224`.
- Convert to three channels.
- Normalise using ImageNet mean and standard deviation.
- Apply random horizontal flips during training.
- Some configurations additionally use small rotations and brightness/contrast jitter.
- Validation and test transforms are deterministic in the described pipeline.

## Download and responsible use

Download the dataset directly from Kaggle after reviewing its current licence, terms, and any access requirements. This repository intentionally does not redistribute the images. Do not upload private or restricted medical data to a public repository.
