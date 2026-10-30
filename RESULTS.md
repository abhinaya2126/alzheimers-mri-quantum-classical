# Experimental results

This file summarises the final comparison reported in `reports/final_project_report.pdf`.

## Final comparison

| Model | Data configuration | Test samples | Accuracy | Macro-F1 |
|---|---|---:|---:|---:|
| Classical Model 1 (ResNet18) | Augmented | Not reported in final comparison table | 94.69% | Not reported |
| Classical Model 2 (ResNet18) | Original | Not reported in final comparison table | 96.78% | Not reported |
| Hybrid Quantum Model 1 (8-qubit circuit-only head) | B | 600 | 85.83% | 0.86 |
| Hybrid C (quantum + parallel classical branch) | B | 600 | 88.33% | 0.8821 |

Hybrid Quantum Model 1 correctly classified 515 of 600 test images. Hybrid C correctly classified 530 of 600 test images. The report gives a nominal 95% Wilson interval of 82.8–88.4% for Hybrid Quantum Model 1 and 85.5–90.7% for Hybrid C.

## Hybrid C per-class F1

| Class | F1 |
|---|---:|
| MildDemented | 0.91 |
| ModerateDemented | 0.98 |
| NonDemented | 0.84 |
| VeryMildDemented | 0.79 |
| **Macro-F1** | **0.8821** |

VeryMildDemented is the weakest class for both hybrid variants in the report. The report recommends focusing future work on the NonDemented / VeryMildDemented boundary.

## Recorded model cost

| Measurement | Classical ResNet18 | Hybrid circuit head |
|---|---:|---:|
| Total parameters | 11,178,564 | 4,188 trainable head parameters |
| Trainable parameters | 10,495,492 | 4,188 |
| Recorded inference time | 3.21 ms/image | 10.78 ms/image |

Important: the hybrid model uses a frozen ResNet18 feature encoder as well as its 4,188-parameter trainable head. The parameter and inference figures are not a like-for-like end-to-end deployment comparison: the classical timing is GPU image inference, while the hybrid timing is CPU inference on cached features.

## Interpretation

- The original-data classical model has the highest reported accuracy (96.78%).
- The augmented-data classical model reports 94.69%.
- Hybrid C improves on the circuit-only hybrid in the reported Configuration B experiment (88.33% vs 85.83%).
- The augmented classical score is 6.36 percentage points above Hybrid C and 8.86 points above the circuit-only hybrid, but these results span different evaluation stages and should not be treated as a paired statistical comparison.
- The variational circuit is trainable as part of the classifier, but this study does not show an accuracy advantage over the classical models or a quantum-hardware speedup.

## Evaluation limitations and next steps

The report recommends:
1. Remove the quantum branch from Hybrid C and evaluate on the same splits and seeds (ablation).
2. Apply the same group-aware folds to both classical and hybrid model families.
3. Split by source group before augmentation and augment training data only.
4. Compare both families under matched original-data conditions.
5. Cross-fit features so validation examples are not represented by a backbone trained on those examples.
6. Repeat experiments across multiple seeds and use paired statistical tests.

These caveats are important when interpreting the results.
