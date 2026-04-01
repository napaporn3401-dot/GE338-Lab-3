#  Land Use Classification using Landsat 8

##  Objective
This project aims to classify land use/land cover (LULC) in Suphanburi Province using Landsat 8 imagery and machine learning algorithms.

---

##  Data
- Landsat 8 Surface Reflectance (2022–2023)
- Dynamic World (training labels)

---

##  Methodology

### 1. Preprocessing
- Cloud masking using QA_PIXEL
- Reflectance scaling

### 2. Feature Extraction
- NDVI (vegetation)
- NDWI (water)
- NDBI (urban)

### 3. Training Data
- Derived from Dynamic World dataset
- Reclassified into 5 classes:
  - Water
  - Forest
  - Agriculture
  - Urban
  - Bare land

### 4. Machine Learning Models
- Random Forest (RF)
- Support Vector Machine (SVM)

---

##  Results

###  Accuracy
- Random Forest: XX%
- SVM: XX%

###  Key Findings
- RF outperforms SVM due to robustness with non-linear data
- NDVI is the most important feature
- Confusion occurs between agriculture and urban areas

---

##  Feature Importance
NDVI > NDWI > NDBI

---

##  Limitations
- Misclassification between agriculture and bare land
- Class imbalance affects performance

---

##  Conclusion
Random Forest provides better performance for LULC classification in Suphanburi due to its ability to handle complex and non-linear relationships.

---

##  Outputs
See `/figures` for:
- Confusion Matrix
- Feature Importance
- LULC Maps
