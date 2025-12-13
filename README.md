# X-Ray-Pneumonia-Screening-with-Deep-Learning-and-Trustworthy-Interpretability-Mechanisms


This repository presents a research-oriented deep learning framework for detecting pneumonia from pediatric chest X-ray images, with a strong emphasis on model interpretability, bias detection, and clinical trustworthiness rather than accuracy alone.  
The project systematically demonstrates how high-performing convolutional neural networks (CNNs) can rely on spurious correlations and shortcut learning, and how Explainable AI (XAI) techniques can be used to uncover and mitigate such behavior.

---

## Project Motivation

Deep learning models have shown impressive performance in medical imaging tasks such as pneumonia detection from chest X-rays. However, high numerical accuracy does not guarantee clinically meaningful reasoning. In high-stakes medical domains, models may exploit dataset artifacts (such as text markers, image borders, or bony structures) rather than true pathological features. This phenomenon is commonly referred to as shortcut learning or the “Clever Hans” effect.

This project was designed to go beyond standard performance evaluation and address the critical question:

Is the model making the correct prediction for clinically valid reasons?

---

## Objectives

- Train a CNN-based model to classify chest X-rays as Normal or Pneumonia  
- Evaluate performance using clinically appropriate metrics (AUROC, recall, F1-score)  
- Apply interpretability methods (GradCAM++ and RISE) to analyze model reasoning  
- Identify and causally validate shortcut learning through controlled experiments  
- Mitigate spurious correlations and re-evaluate model behavior post-audit  

---

## Dataset

- Source: Pediatric Chest X-Ray Dataset (Kermany et al., Cell, 2018)  
- Patients: Children aged 1–5 years  
- Views: Anterior–Posterior (AP)  
- Classes: Normal, Pneumonia  
- Total images: 5,863  
- Annotations: Verified by multiple expert physicians  

Dataset link:  
https://data.mendeley.com/datasets/rscbjbr9sj/2  

---

## Model Architecture and Training

- Backbone: DenseNet (transfer learning from ImageNet)  
- Input resolution: 224 × 224  
- Loss function: Binary Cross-Entropy with Logits  
- Optimizer: Adam  
- Regularization: Data augmentation and early stopping  
- Evaluation protocol: Held-out test set only (no data leakage)  

The trained model achieved high sensitivity and strong AUROC, resulting in a very low false-negative rate suitable for screening scenarios.

---

## Performance Summary

| Metric    | Value |
|----------|-------|
| Accuracy | ~80.8% |
| Precision | ~0.77 |
| Recall | ~0.997 |
| F1-score | ~0.87 |
| AUROC | High (threshold-independent separation) |

While these results appear strong numerically, they motivated deeper interpretability-driven evaluation.

---

## Interpretability and Explainable AI

Two complementary interpretability techniques were used:

### GradCAM++

- Gradient-based method  
- Produces class-discriminative localization maps  
- Efficient and suitable for rapid inspection  

### RISE

- Perturbation-based and model-agnostic  
- Measures causal importance of image regions  
- Computationally expensive but more faithful  

Agreement between these methods was used to validate explanations and reduce the risk of misleading saliency interpretations.

---

## Key Finding: Shortcut Learning

Interpretability analysis revealed that the model occasionally relied on non-pathological visual cues, including:

- Radiographic text markers (e.g., orientation labels)  
- Image borders  
- Clavicles and scapulae  
- Mediastinal regions and vertebral bodies  

These cues are not causally related to pneumonia, yet influenced high-confidence predictions, providing clear evidence of shortcut learning.

---

## Causal Validation Experiments

To confirm shortcut dependence:

- Artificial radiographic markers were injected into normal chest X-rays, resulting in statistically significant increases in predicted pneumonia probability  
- Removing these markers from pneumonia images produced negligible changes, confirming dominance of genuine pathological signal  

These experiments establish causal evidence of spurious feature reliance rather than simple correlation.

---

## Post-Audit Results

After mitigating shortcut cues and retraining the model:

- Model attention shifted toward anatomically meaningful lung regions  
- Reliance on borders and text artifacts was reduced  
- Improved alignment between predictions and clinical reasoning was observed  

Post-audit GradCAM++, RISE, and ablation-based explanations showed consistent and clinically plausible attention patterns.

---

## Error Analysis

High-confidence errors were explicitly analyzed:

- No high-confidence false negatives were observed  
- Errors were predominantly false positives  
- Remaining errors reflected anatomically plausible confusions, such as bone density being mistaken for pulmonary consolidation or prominent bronchovascular markings being misinterpreted as infiltrates  

This highlights the intrinsic challenges of two-dimensional projection radiography, particularly in pediatric imaging.

---

## Why This Project Matters

- Moves beyond accuracy-centric evaluation  
- Demonstrates real-world risks of biased medical AI systems  
- Uses causal experiments rather than visualization alone  
- Emphasizes trust, safety, and interpretability  
- Research-focused and clinically motivated  

---

## Future Work

- Lung segmentation prior to classification  
- Evaluation on multi-center datasets  
- Model calibration and uncertainty estimation  
- Attention-based architectures  
- Extension to multi-class pneumonia subtypes  

---

## References

- Kermany et al., Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning, Cell (2018)  
- Selvaraju et al., Grad-CAM: Visual Explanations from Deep Networks, ICCV (2017)  
- Petsiuk et al., RISE: Randomized Input Sampling for Explanation, BMVC (2018)  

---

## Author

Undergraduate researcher with interests in medical computer vision, explainable AI, and robust machine learning systems.

---

Note: This project prioritizes interpretability and clinical reliability over leaderboard-style performance metrics.

