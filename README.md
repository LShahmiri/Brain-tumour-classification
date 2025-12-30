# Brain Tumor Classification: MIGT vs Random

This repository compares two dataset partitioning strategies for brain tumor classification using an Xception-based CNN:

- Random split (baseline)
- Mutual Information Guided Training (MIGT)

## Dataset
- Classes: Brain Tumor, Healthy
- Image size: 224×224
- Split ratio: 50% Train / 10% Validation / 40% Test

## MIGT
MIGT uses mutual information to guide dataset splitting, ensuring balanced information distribution across training, validation, and test sets.

## Random
Random splitting assigns samples to each subset without considering image information content and serves as a baseline.

## Results

| Method | Test Accuracy | Test AUC |
|--------|---------------|----------|
| MIGT   | **97.61%**    | **0.9939** |
| Random| 97.57%        | 0.9936   |

## Conclusion
Although both methods achieve similar accuracy on this dataset, MIGT provides a more principled and stable dataset partitioning strategy, reducing sampling bias and improving training reliability.
