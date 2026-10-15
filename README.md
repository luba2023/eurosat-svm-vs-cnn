# SVM vs CNN for Agricultural Land-Cover Classification (EuroSAT)

Code and results for the paper *"A Comparative Evaluation of SVM and CNN for Agricultural Land-Cover Classification Using EuroSAT Satellite Imagery"* (L. Ndlovu, National University of Science and Technology, Zimbabwe — SCS 5103 Pattern Recognition and Image Processing).

## What this study does
Compares two ways of classifying 64 × 64 Sentinel-2 RGB image patches into five vegetation-related land-cover classes (AnnualCrop, PermanentCrop, Pasture, HerbaceousVegetation, Forest):

- **SVM** (RBF kernel) on 96-value colour-histogram features, tuned by grid search on a validation set.
- **Small CNN** (3 convolutional blocks, 93,893 parameters) trained from scratch, in three stages: Run A (40-epoch cap), Run B (100-epoch cap) and a pre-planned six-setting tuning search.

Data: 600 images per class (3,000 total), stratified 70/15/15 train/validation/test split. Main split seed 42; repeated on seeds 1, 2 and 3.

## Main results (test set)

| Model | Accuracy | Macro F1 |
|---|---|---|
| SVM, colour histogram | 0.8489 | 0.8495 |
| CNN Run A | 0.8156 | 0.8173 |
| CNN Run B | 0.8222 | 0.8253 |
| Tuned CNN | 0.8556 | 0.8581 |

Across four splits (tuned models): SVM macro F1 0.8414 (SD 0.0143), tuned CNN 0.8644 (SD 0.0048). The CNN was higher on all four splits, but the difference was not statistically confirmed (McNemar p = 0.784 on the main split; sign test p = 0.125). The SVM's full pipeline took about 6 s on a CPU, compared with about 5 minutes for the CNN search on a GPU.

## Repository contents
- `notebooks/01_original_experiment.ipynb` — data selection, splits, SVM, CNN Runs A and B, repeated splits
- `notebooks/02_cnn_fair_tuning.ipynb` — fair CNN tuning search and per-split comparison
- `results/` — saved outputs: split file, configuration, tuning plan, metrics, training histories and figures
- `requirements.txt` — Python packages

## How to reproduce
1. Open a notebook in [Google Colab](https://colab.research.google.com) (File → Upload notebook) and set Runtime → Change runtime type → T4 GPU.
2. Run the cells in order. The notebook downloads EuroSAT RGB from Zenodo automatically and saves outputs to Google Drive.
3. Random seeds are fixed (42, 1, 2, 3). GPU training can still vary slightly between runs.

## Data
EuroSAT RGB, version 2 — P. Helber, B. Bischke, A. Dengel and D. Borth. Zenodo, DOI [10.5281/zenodo.7711810](https://doi.org/10.5281/zenodo.7711810), MIT licence. The dataset itself is not included in this repository.

## Citation
If you use this code, please cite the paper above and the EuroSAT dataset:
P. Helber, B. Bischke, A. Dengel, and D. Borth, "EuroSAT: A novel dataset and deep learning benchmark for land use and land cover classification," *IEEE JSTARS*, vol. 12, no. 7, pp. 2217–2226, 2019.
