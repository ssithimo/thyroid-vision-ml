# 🩺 Thyroid Vision ML: Thyroid Nodule Ultrasound Analysis & TI-RADS Classification

**Deep learning-based thyroid nodule ultrasound analysis and TI-RADS classification using CNNs and patient metadata for clinically meaningful risk stratification.**

---

## 🌟 Motivation
Having personally experienced **acute hyperthyroidism** and with a family history of thyroid disorders, I wanted to explore how AI could aid **early detection** and **risk stratification** of thyroid nodules. Ultrasound imaging is the primary tool for assessing thyroid nodules, and TI-RADS scoring helps clinicians decide on follow-ups or interventions like biopsies.

---

## 📊 Dataset
The project uses the **Colombia Thyroid Ultrasound Collection found on Kaggle**:

- 🖼️ 400 patients with thyroid nodule ultrasound images  
- 📝 Annotations (XML) with nodule regions marked via SVG polygons  
- 👤 Metadata including patient age, sex, nodule composition, echogenicity, margins, calcifications, and TI-RADS score  

---

## 🛠️ Approach
1. **Data Processing & Visualization**  
   - Parse and organize images, XML annotations, and metadata  
   - Generate binary masks from SVG points for segmentation  
   - Visualize nodules with annotations  

2. **Modeling**  
   - CNN for image feature extraction  
   - Metadata features for improved prediction  
   - Multiclass TI-RADS score prediction
   - Custom CNN architecture vs Transfer Learning architecture using DenseNet121

3. **Evaluation**  
   - Accuracy ✅  
   - F1 Score 🎯  
   - Confusion Matrix 📈  

---

## 📈 Results
- **Original 7-class TI-RADS prediction:**  
  - CNN: 60% test accuracy, 0.57 macro F1-score  
  - Struggled with mid-range categories (TI-RADS 3, 4a, 4b)  
- **Consolidated 3-class prediction (Benign / Low-risk / High-risk):**  
  - CNN: 85% test accuracy, 0.67 macro F1-score  
  - High recall for high-risk nodules (0.98), moderate for benign (0.71), lower for low-risk (0.25)  
- **Transfer Learning (DenseNet121):**  
  - Struggled with domain adaptation; frozen weights from natural images were not ideal for ultrasound  

---

## 🔍 Key Takeaways
- CNNs can effectively classify thyroid nodules into **clinically meaningful risk groups**  
- Pretrained natural-image models may not generalize well to specialized medical imaging  
- Simplifying labels into fewer categories improves model reliability, especially for **high-risk detection**  

---

## 🚀 Future Work
- Use **RadImageNet** pretrained weights for domain-specific transfer learning  
- Experiment with **multimodal architectures** to better integrate metadata and image features  
- Deploy as a **clinical decision support tool** to assist radiologists in early detection  

---

## 🗂️ Repo Structure

thyroid-vision-ml/
- data/           # Ultrasound images, annotations, metadata
- notebooks/      # Jupyter notebooks for EDA, modeling, and analysis
- README.md       # Project overview
- executive summary


