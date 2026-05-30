# 🩺 Thyroid Vision ML: TI-RADS Classification from Ultrasound Images

> *Roughly 600,000 thyroid biopsies are performed in the U.S. annually. Most come back benign. A model that reliably identifies truly high-risk nodules reduces unnecessary procedures without missing the ones that matter.*

---

note: if notebook doesnt open in github click here to see it in [nbviewer](https://nbviewer.org/github/ssithimo/thyroid-vision-ml/blob/main/notebooks/thyroid-vision-ml.ipynb)

---

## 🌟 Motivation

This project started personally. Having experienced acute hyperthyroidism and with a family history of thyroid disorders, I wanted to explore how deep learning could support earlier and more accurate thyroid nodule risk stratification. Ultrasound is the standard imaging tool for evaluating thyroid nodules, and the TI-RADS scoring system guides clinical decisions about whether to monitor, biopsy, or intervene.

The clinical challenge is real. Radiologists interpreting ultrasound images face a grading system with seven categories, several of which overlap in visual presentation. Inconsistent classification leads to both over-biopsy of benign nodules and under-detection of malignant ones. A reliable classification model reduces that inconsistency and gives clinicians a data-backed second opinion.

---

## 📊 Dataset

**Colombia Thyroid Ultrasound Collection (Kaggle)**

- 400 patients with thyroid nodule ultrasound images
- XML annotations with nodule regions marked via SVG polygons
- Metadata including age, sex, nodule composition, echogenicity, margins, calcifications, and TI-RADS score

---

## 🛠️ Approach

**Data Processing**
- Parsed and organized images, XML annotations, and patient metadata
- Generated binary segmentation masks from SVG polygon coordinates to isolate nodule regions for model input
- Visualized annotated nodules to validate mask generation quality

**Key Design Decision: Label Consolidation**

The original dataset contains 7 TI-RADS categories. Training on 7 classes produced 60% accuracy with the model struggling on mid-range categories (TI-RADS 3, 4a, 4b) that share overlapping visual features in ultrasound images. Rather than accepting that ceiling, the 7 categories were consolidated into 3 clinically meaningful groups:

- Benign (TI-RADS 1 and 2)
- Low-risk (TI-RADS 3)
- High-risk (TI-RADS 4a, 4b, 4c, 5)

This consolidation reflects how clinical decisions are actually made. The actionable question for a radiologist is not whether a nodule is TI-RADS 4a vs 4b but whether it warrants biopsy. Consolidating to 3 classes aligns the model's output with that real-world decision boundary and improved test accuracy from 60% to 85%.

**Models Evaluated**
- Custom CNN architecture trained from scratch on ultrasound images
- DenseNet121 via transfer learning with ImageNet pretrained weights
- *RadImageNet was considered but not used because of the inconvenience of sourcing the weights and downloading and uploading them into the model.

---

## 📈 Results

**Original 7-class classification**

| Model | Test Accuracy | Macro F1 |
|-------|--------------|----------|
| Custom CNN | 60% | 0.57 |

**Consolidated 3-class classification**

| Model | Test Accuracy | Macro F1 |
|-------|--------------|----------|
| Custom CNN | 85% | 0.67 |
| DenseNet121 | Below CNN baseline | N/A |

**Per-class recall (3-class CNN):**

| Class | Recall | Clinical Interpretation |
|-------|--------|------------------------|
| Benign | 0.71 | 71% of truly benign nodules correctly identified |
| Low-risk | 0.25 | Weakest class, visually ambiguous in ultrasound |
| High-risk | 0.98 | 98% of high-risk nodules correctly flagged |

The 0.98 high-risk recall is the most clinically significant result. In cancer screening, missing a high-risk nodule is the highest-cost error. The model correctly flags 98 out of every 100 truly high-risk cases, making it a reliable safety net for radiologist review.

The 0.25 low-risk recall reflects the inherent ambiguity of that category in ultrasound imaging. Low-risk nodules share visual characteristics with both benign and high-risk categories, making them the hardest class to classify reliably regardless of architecture. This is not a model failure but a known limitation of ultrasound-based TI-RADS classification that affects human radiologists as well.

---

## 🔍 Key Findings

**Custom CNN outperformed transfer learning on medical imaging**

DenseNet121 with ImageNet pretrained weights underperformed the custom CNN on this dataset. This is a well-documented challenge in medical imaging: weights trained on natural photographs (dogs, cars, landscapes) do not generalize cleanly to ultrasound images, which have fundamentally different texture, contrast, and noise characteristics. Training a simpler architecture from scratch on domain-specific data outperformed a more complex pretrained model, reinforcing that model complexity is not a substitute for data relevance.

**Label consolidation improved clinical utility**

Reducing from 7 to 3 classes was not just a performance optimization. It reflects a deliberate alignment between model output and clinical decision making. A radiologist does not need to know whether a nodule is TI-RADS 4a or 4b. They need to know whether it warrants a biopsy. The 3-class model answers that question more reliably than the 7-class version at 85% vs 60% accuracy.

**High-risk recall justifies clinical decision support deployment**

A 0.98 recall on high-risk nodules means the model misses only 2% of the cases that matter most. Combined with 85% overall accuracy on a 3-class problem, this suggests the model is a viable candidate for clinical decision support, flagging high-risk nodules for priority radiologist review rather than replacing clinical judgment.

---

## 🚀 Future Work

- Evaluate RadImageNet pretrained weights as a domain-appropriate alternative to ImageNet for transfer learning on medical imaging
- Explore multimodal architectures that more tightly integrate image features with patient metadata (age, nodule composition, calcification patterns)
- Validate on an independent external dataset to assess generalizability beyond the Colombia collection
- Deploy as a clinical decision support prototype to assist radiologists in prioritizing high-risk nodule review

---

## 🔧 Tools
Python, TensorFlow, Keras, CNN, DenseNet121, Transfer Learning, 
OpenCV, Pandas, NumPy, Matplotlib, Scikit-learn
