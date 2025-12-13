# X-Ray-Pneumonia-Screening-with-Deep-Learning-and-Trustworthy-Interpretability-Mechanisms


This repository presents a comprehensive deep learning study for **pneumonia detection from pediatric chest X-ray images**, with a strong emphasis on **model interpretability, shortcut learning detection, and clinical trustworthiness**.  
Rather than focusing solely on numerical performance, this project systematically evaluates *how* and *why* models make decisions, using explainable AI (XAI) techniques and controlled causal experiments.

---

## Project Overview

Deep learning models have demonstrated strong performance in chest X-ray–based pneumonia classification. However, in medical imaging, high accuracy alone is insufficient: models may rely on **spurious correlations or dataset artifacts** rather than genuine pathological features, leading to unreliable or unsafe predictions in real-world clinical settings.

This project addresses this challenge by:
- Training high-performing CNN models for pneumonia classification
- Auditing model behavior using **interpretability methods**
- Identifying and validating **shortcut learning**
- Comparing architectures to understand differences in reasoning behavior

---

## Objectives

- Classify chest X-ray images into **Normal** and **Pneumonia**
- Achieve strong performance using clinically relevant metrics (AUROC, Recall, F1-score)
- Apply **GradCAM++ and RISE** to visualize and analyze model attention
- Detect and causally validate shortcut learning via controlled perturbation experiments
- Compare a DenseNet-based interpretability-focused model against ResNet baselines
- Provide actionable insights for building more trustworthy medical AI systems

---

## Dataset

- **Source:** Pediatric Chest X-Ray Dataset (Kermany et al., *Cell*, 2018)
- **Patients:** Children aged 1–5 years
- **Imaging view:** Anterior–Posterior (AP)
- **Classes:** Normal, Pneumonia
- **Total images:** 5,863
- **Annotations:** Verified by multiple expert physicians

Dataset link:  
https://data.mendeley.com/datasets/rscbjbr9sj/2

All images were screened for quality before model training.

---

## Models and Training

### DenseNet (Primary Model)

- Transfer learning using a pretrained DenseNet architecture
- Input resolution: 224 × 224
- Loss: Binary Cross-Entropy with Logits
- Optimizer: Adam
- Regularization: Data augmentation and early stopping
- Evaluation: Held-out test set only

The DenseNet model was intentionally configured to prioritize **high sensitivity**, reflecting clinical screening requirements where missed pneumonia cases are more harmful than false positives.

### ResNet Baselines

- ResNet-34 and ResNet-50 architectures
- Identical preprocessing, data splits, and evaluation protocol
- Used to contextualize performance and interpretability behavior

---

## Quantitative Performance Summary

| Model | ROC-AUC | Accuracy | F1-score | Sensitivity | Specificity |
|------|--------:|---------:|---------:|------------:|------------:|
| DenseNet (raw threshold) | 0.9737 | 0.8125 | 0.8693 | 0.9974 | 0.5043 |
| DenseNet (tuned threshold) | 0.9737 | 0.9247 | — | 0.9590 | 0.8675 |
| ResNet-34 | 0.9692 | 0.8526 | 0.8940 | ~0.99 | ~0.615 |
| ResNet-50 | 0.9731 | 0.9183 | — | 0.9128 | 0.9274 |

These results demonstrate that multiple architectures achieve strong AUROC, but differ substantially in error profiles and decision behavior.

---

## Interpretability and Explainable AI

To analyze model reasoning, two complementary interpretability techniques were applied:

### GradCAM++
- Gradient-based method
- Produces class-discriminative localization maps
- Efficient and suitable for rapid inspection

### RISE
- Perturbation-based and model-agnostic
- Measures causal importance of image regions
- More computationally expensive but often more faithful

Explainability quality was further assessed using **faithfulness** and **sparsity** metrics across GradCAM, RISE, and Ablation-based methods.

---

## Key Finding: Shortcut Learning

Despite strong numerical performance, interpretability analysis revealed that models sometimes relied on **non-pathological visual cues**, including:
- Radiographic text markers (e.g., orientation labels)
- Image borders
- Clavicles and scapulae
- Mediastinal and vertebral regions

These features are not causally related to pneumonia, indicating **shortcut learning**.

---

## Causal Validation Experiments

To confirm shortcut dependence, controlled interventions were performed:

- **Marker Injection:** Artificially adding radiographic markers to normal images caused statistically significant increases in pneumonia prediction confidence.
- **Marker Removal:** Removing such markers from pneumonia images had negligible effect on predictions.

These experiments provide causal evidence that models partially relied on dataset artifacts while still leveraging true pathological signals when present.

---

## Post-Audit Results

After mitigating shortcut cues and retraining:
- Saliency maps shifted toward anatomically meaningful lung regions
- Reliance on text markers and borders decreased
- Alignment between predictions and clinical reasoning improved

This demonstrates that **interpretability-driven auditing can meaningfully improve model reliability** without sacrificing discriminative performance.

---

## DenseNet vs ResNet Comparison

- DenseNet favored high sensitivity and benefited more from interpretability-driven refinement
- ResNet models showed stronger raw specificity and balanced accuracy
- Shortcut learning was observed in both architectures, indicating a dataset-driven issue
- Architecture choice alone was insufficient to ensure trustworthy behavior

The DenseNet interpretability pipeline is highlighted as the primary contribution due to its combination of performance, explainability, and bias mitigation.

---

## Repository Structure

