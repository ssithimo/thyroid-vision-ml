# 📓 Thyroid Vision ML Notebook

This folder contains a single Jupyter notebook for the **Thyroid Vision ML: Thyroid Nodule Ultrasound Analysis & TI-RADS Classification** project.

---

## 🧩 Notebook Overview

The notebook covers the entire workflow:

1. **Data Loading, Exploration, and Cleaning**  
   - Load ultrasound images, XML annotations, and metadata  
   - Visualize thyroid nodules and metadata distributions  

2. **Mask Generation**  
   - Convert SVG polygon annotations into binary masks for segmentation  

3. **Modeling**  
   - Build CNN and Transfer Learning models using image and metadata features  
   - Train models to predict TI-RADS scores
   - Consolidate classes and retrain models

4. **Evaluation & Results**  
   - Compute accuracy, F1-score, and confusion matrices  
   - Analyze performance across TI-RADS categories
