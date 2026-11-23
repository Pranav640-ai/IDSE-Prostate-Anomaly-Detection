# IDSE: Prostate Tissue Anomaly Detection

This project tests a two-stage method for finding unfamiliar tissue patterns in prostate histology images. It uses the SICAPv2 dataset, with NC, G3 and G4 as known classes and G5 held out for evaluation.

Stage 1 measures nuclear and tissue-structure features from 256 × 256 patches. The measurements are compared with known-grade reference distributions using Mahalanobis distance. Patch scores are averaged for each slide to decide which slides need further checking.

Stage 2 uses frozen Phikon embeddings and cosine-distance 5-nearest-neighbour search against separate known-grade reference banks. A patch is marked only when its slide passes Stage 1 and its Phikon score exceeds the calibration threshold. ResNet18, texture features and score fusion were also evaluated for comparison.

## Files

- `notebooks/IDSE.ipynb`: main experiment and saved outputs
- `results/recorded_metrics.csv`: results from the full experiment
- `results/README.md`: how to interpret the saved results
- `requirements.txt`: Python dependencies

## Running

Use Kaggle with SICAPv2 attached as an input dataset. The code expects the original image directory and the `partition/Test/Train.xlsx` and `partition/Test/Test.xlsx` label files. Run the notebook cells in order, with a GPU enabled for image embeddings. Model weights for `owkin/phikon` and pretrained ResNet18 must be available.

The notebook contains results saved from the original experiment. Those saved outputs are not a separate verification of reproducibility in a fresh environment.

## Reported results

Stage 1 reached 0.9833 AUROC on 29 eligible test slides, correctly classifying 26. Phikon reached 0.7961 within-slide AUROC on the six mixed slides used for that analysis. The complete Stage 1 + Phikon system reached 0.8816 end-to-end AUROC across 1,404 test patches and detected 26 of 179 G5 patches at the chosen threshold.

These are experiment results, not clinical validation. The system highlights patches for review; it does not identify exact tumour boundaries.

## References

- [SICAPv2 dataset](https://data.mendeley.com/datasets/9xxm58dvs3/2)
- [Phikon model](https://huggingface.co/owkin/phikon)
