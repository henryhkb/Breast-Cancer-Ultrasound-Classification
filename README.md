# Breast Cancer Ultrasound Classification

Classifying **breast ultrasound images** as **normal**, **benign** or **malignant** using deep learning, with heat maps that show where the model is looking.

> **Status:** a learning and portfolio project, **just started**. The dataset has been chosen and the project plan is set; no model has been trained yet. Results will be added here as each stage is completed.

---

## Why this project

Breast cancer is the most commonly diagnosed cancer among women worldwide, and survival depends heavily on finding it early. In many low-resource settings, including much of Ghana, mammography (breast X-ray) is limited and many women are diagnosed late.

Ultrasound machines are cheaper, portable and far more widely available in district hospitals, but reading breast ultrasound well takes specialist training. This project explores whether a model can look at a breast ultrasound image, flag lesions that look suspicious, and **show its reasoning** with a heat map so a clinician can check it.

## The task

**Three-class image classification:** given a breast ultrasound image, predict:

| Class | Meaning |
|---|---|
| Normal (0) | No lesion visible |
| Benign (1) | A lesion that is not cancer (e.g. fibroadenoma, cyst) |
| Malignant (2) | A cancerous lesion |

Features that radiologists use to separate benign from malignant lesions (BI-RADS ultrasound lexicon):

| Feature | Usually benign | Usually malignant |
|---|---|---|
| Shape | Oval or round | Irregular |
| Margin | Smooth, well-defined | Spiculated (spiky) or indistinct |
| Orientation | Parallel to the skin (wider than tall) | Not parallel (taller than wide) |
| Posterior features | Enhancement or none | Shadowing |

## Data

This project uses the **Breast Ultrasound Images (BUSI)** dataset: 780 PNG images (average size about 500 × 500 pixels) from 600 women aged 25–75, collected in 2018 at Baheya Hospital, Cairo, Egypt, using LOGIQ E9 ultrasound systems. Each benign and malignant image comes with a **segmentation mask** (a black-and-white outline of the lesion) drawn by radiologists.

- Source: Al-Dhabyani et al. (2020), *Data in Brief* (see [References](#references)); also mirrored on Kaggle
- The dataset is **not included** in this repository. The notebooks download it into Google Colab.

### Class counts

| Class | Images | Share |
|---|---|---|
| Normal | 133 | 17% |
| Benign | 437 | 56% |
| Malignant | 210 | 27% |
| **Total** | **780** | |

The classes are **imbalanced**: benign images outnumber normal ones by more than 3 to 1. Evaluation will therefore report per-class sensitivity, specificity and macro-F1, not accuracy alone.

### Known data issues

A published review of BUSI (Pawłowska et al., 2023) found problems that this project will check for and handle:

- **Duplicate images**, including a few that appear in two different classes
- **Annotations burned into the images** (text, calliper measurement marks, Doppler overlays), which a model could learn instead of the tissue
- **About 70 images of the axilla** (armpit) rather than the breast
- **At least 19 images with a questionable label**

### Validation plan

BUSI has **no official train/test split and no patient IDs** (780 images from 600 women, so some women contribute more than one image). The plan is:

- Remove exact and near-duplicate images **before** splitting, so the same image never appears in both training and testing
- Hold out a **stratified test set** (same class proportions as the full data), used **once** at the very end
- Use **stratified 5-fold cross-validation** on the remaining images for model selection, reporting the mean ± standard deviation across folds

## Notebooks

| Notebook | Purpose | Status |
|---|---|---|
| `BUSI_01_Explore_Data.ipynb` | Download BUSI, inspect images and masks, count classes, find duplicates and burned-in annotations, build the splits | ⏳ Next |
| `BUSI_02_Train_Model.ipynb` | Fine-tune a pretrained CNN with stratified k-fold cross-validation and class weighting | 🔜 Planned |
| `BUSI_03_Explainability.ipynb` | Grad-CAM heat maps, error analysis, final test-set evaluation | 🔜 Planned |
| `BUSI_04_Segmentation.ipynb` | (Optional) Outline the lesion using the radiologist masks | 🔜 Planned |

Built and run in **Google Colab**, with data stored in Google Drive.

## Roadmap

- [x] Choose the dataset (BUSI) and define the task: Normal vs. Benign vs. Malignant
- [x] Learn breast ultrasound basics (echogenicity, BI-RADS shape, margin and orientation features)
- [ ] Download BUSI and explore it: class counts, image sizes, masks
- [ ] Find and remove duplicate images
- [ ] Check for burned-in annotations (text, calliper marks) and axilla images
- [ ] Build a stratified held-out test set and 5 cross-validation folds
- [ ] Simple baselines (always-Benign, plus a simple model)
- [ ] Train a CNN image classifier (ResNet-18, transfer learning) with 5-fold cross-validation
- [ ] Handle class imbalance (class-weighted loss)
- [ ] Grad-CAM heat maps, compared with the radiologist masks
- [ ] Error analysis of cross-validation mistakes
- [ ] Evaluate once on the held-out test set (per-class sensitivity, specificity, macro-F1)
- [ ] (Optional) Lesion segmentation with U-Net using the BUSI masks
- [ ] (Optional) Public web demo (Gradio on Hugging Face Spaces)

## Model

*To be added after training.* Planned starting point: ResNet-18 pretrained on ImageNet, final layer replaced with a 3-class output, class-weighted cross-entropy loss and light data augmentation.

## Results

*To be added.* Will include baselines, cross-validation scores (mean ± SD), the final test-set confusion matrix, per-class sensitivity and specificity, and example Grad-CAM heat maps.

## Limitations

- **Small dataset** (780 images) from **one hospital** and **one ultrasound manufacturer**; a model trained on it may not work on images from other machines or populations, including Ghanaian clinics.
- **No patient IDs**, so images from the same woman may end up in both training and test sets.
- Known label noise, duplicates and annotation artefacts (see [Known data issues](#known-data-issues)).
- Uses single still images; in clinical practice, radiologists scan in real time from many angles.
- Not validated clinically; see the disclaimer below.

## How to run

*To be added once the first notebook is complete.*

## Related work

- Al-Dhabyani et al. (2020) introduced BUSI and reported baseline CNN classification results.
- Pawłowska et al. (2023) documented BUSI's data-quality issues (duplicates, annotations, axilla images).

## Disclaimer

This is an educational research project. It is **not a medical device** and must not be used for clinical diagnosis or treatment decisions.

## References

This project uses the BUSI dataset:

> Al-Dhabyani, W., Gomaa, M., Khaled, H., & Fahmy, A. (2020). Dataset of breast ultrasound images. *Data in Brief*, 28, 104863. https://doi.org/10.1016/j.dib.2019.104863

Data-quality review of BUSI:

> Pawłowska, A., et al. (2023). Letter to the Editor. Re: "Dataset of breast ultrasound images by W. Al-Dhabyani, M. Gomaa, H. Khaled & A. Fahmy, Data in Brief, 2020, 28, 104863". *Data in Brief*, 48, 109247. https://doi.org/10.1016/j.dib.2023.109247

## Author

**Henry Kofi Brandoh**, MPhil candidate, Intelligent Computing Systems, Ashesi University
