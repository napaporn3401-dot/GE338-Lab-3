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
ภารกิจที่ 1: Training Strategy
1. จะใช้กี่ Class + นิยาม
คำตอบ 
ในการศึกษานี้ได้กำหนดประเภทการใช้ที่ดินทั้งหมด 5 ประเภท ได้แก่
1.	Water – แหล่งน้ำ เช่น แม่น้ำ บึง และพื้นที่น้ำขัง 
2.	Forest – พื้นที่ป่าไม้หรือพืชพรรณหนาแน่น 
3.	Agriculture – พื้นที่เกษตรกรรม เช่น นาข้าวและพืชไร่ 
4.	Urban – พื้นที่ชุมชน สิ่งปลูกสร้าง ถนน 
5.	Bare Land – พื้นที่ดินเปล่าหรือพื้นที่ไม่มีพืชปกคลุม 
การเลือก 5 class นี้สอดคล้องกับลักษณะการใช้ที่ดินในพื้นที่ภาคกลางของประเทศไทย และช่วยลดความซับซ้อนของโมเดล

 2. จะหา Training Samples ยังไง
คำตอบ
เนื่องจากไม่สามารถเก็บข้อมูลภาคสนามได้ครบถ้วน จึงใช้วิธีผสมระหว่าง:
•	ใช้ข้อมูล Dynamic World เป็น reference dataset สำหรับ label 
•	ทำการ reclass เพื่อให้เหมาะสมกับพื้นที่ศึกษา 
•	สุ่มตัวอย่าง (random sampling) จากภาพดาวเทียมภายในพื้นที่ศึกษา 
วิธีนี้ช่วยลดต้นทุนและยังคงความน่าเชื่อถือของข้อมูลฝึกสอน

3. Train / Validation Split
คำตอบ
ในการแบ่งข้อมูลได้ใช้วิธี random split 80/20 โดย:
•	80% ใช้สำหรับ training 
•	20% ใช้สำหรับ validation 
นอกจากนี้ยังพิจารณาแนวคิดของ Spatial Cross-validation ซึ่งช่วยลด bias จาก spatial autocorrelation โดยการแบ่งข้อมูลตามพื้นที่แทนการสุ่ม อย่างไรก็ตามในการศึกษานี้ใช้ random split เนื่องจากข้อจำกัดด้านเวลา
4. Feature ที่ใช้ + เหตุผล
 คำตอบ (ตัวนี้สำคัญมาก)
Features ที่ใช้ในการจำแนกประกอบด้วย:
•	Spectral Bands: SR_B2, SR_B3, SR_B4, SR_B5, SR_B6 
•	Spectral Indices: 
o	NDVI (vegetation) 
o	NDWI (water) 
o	NDBI (urban) 
การเพิ่ม spectral indices ช่วยเพิ่มความสามารถในการแยก class เนื่องจากสะท้อนคุณสมบัติทางกายภาพของพื้นผิวโลก เช่น ความเขียวของพืช ปริมาณน้ำ และความหนาแน่นของสิ่งปลูกสร้าง
ภารกิจที่ 2: Algorithm Comparison
1. อัลกอริทึมที่ใช้
 คำตอบ 
ในการศึกษานี้ได้เลือกใช้อัลกอริทึม 2 ประเภท ได้แก่
1.	Random Forest (RF) – เป็น ensemble learning ที่รวม decision tree หลายต้น 
2.	Support Vector Machine (SVM) – ใช้ kernel function ในการแยก class 
โดย Random Forest เหมาะกับข้อมูลที่มีหลาย feature และมีความสัมพันธ์แบบไม่เชิงเส้น ขณะที่ SVM เหมาะกับข้อมูลที่มี boundary ชัดเจน
คำตอบ
จากการทดลองปรับจำนวนต้นไม้ (numberOfTrees) พบว่า:
•	50 trees → accuracy ต่ำกว่า 
•	100 trees → balance ดี 
•	200 trees → เพิ่มขึ้นเล็กน้อย 
แสดงให้เห็นว่าการเพิ่มจำนวนต้นไม้ช่วยลด variance แต่ผลลัพธ์จะเริ่มคงที่เมื่อถึงจุดหนึ่ง
3.	Metrics ที่ใช้
คำตอบ
ในการประเมินโมเดลไม่ได้พิจารณาเพียง Overall Accuracy แต่ยังใช้:
Producer’s Accuracy – ความสามารถในการจำแนก class จริง 
User’s Accuracy – ความน่าเชื่อถือของผลลัพธ์ 
Kappa Coefficient – วัดความสอดคล้องที่มากกว่าการสุ่ม 
F1-score – ความสมดุลระหว่าง precision และ recall 

4. เปรียบเทียบ RF vs SVM (ตัวหลัก)
คำตอบ 
จากผลการทดลองพบว่า Random Forest ให้ค่าความแม่นยำสูงกว่า SVM ในทุก metric โดยเฉพาะใน class Agriculture และ Urban
Random Forest สามารถจัดการกับข้อมูลที่มีความซับซ้อนและไม่เป็นเชิงเส้นได้ดีกว่า เนื่องจากใช้ ensemble ของ decision trees ในขณะที่ SVM มีข้อจำกัดในกรณีที่ข้อมูลมีการ overlap สูงระหว่าง class
นอกจากนี้ Random Forest ยังมีความทนทานต่อ noise และไม่ต้องการ parameter tuning มากเท่ากับ SVM
Random Forest เหมาะกับข้อมูล remote sensing มากกว่า เนื่องจากข้อมูลมีความซับซ้อนสูงและมี noise จาก atmospheric conditions ซึ่ง SVM ไม่สามารถจัดการได้ดีเท่ากับ RF
ภารกิจที่ 3: Feature Importance
-	ดึงค่า Feature Importance
-	ทำกราฟ
-	จากการวิเคราะห์ Feature Importance พบว่า NDVI มีความสำคัญสูงสุด รองลงมาคือ NDWI และpectral bands ในช่วง Near-Infrared (SR_B5) 
NDVI มีบทบาทสำคัญเนื่องจากสามารถสะท้อนความเขียวของพืชได้อย่างชัดเจน ซึ่งเหมาะสมกับพื้นที่เกษตรกรรมในจังหวัดสุพรรณบุรี
NDWI มีความสำคัญในการจำแนกพื้นที่น้ำและพื้นที่นาข้าวที่มีน้ำขัง ขณะที่ NDBI ช่วยแยกพื้นที่เมืองออกจากพื้นที่อื่น
ผลลัพธ์นี้สอดคล้องกับหลักการทางกายภาพของการสะท้อนแสง เนื่องจากพืชมีการสะท้อนสูงในช่วง Near-Infrared และดูดกลืนในช่วง Red band
-	ถ้าตัด Feature ออก เมื่อทำการตัด spectral indices ออก พบว่าความแม่นยำของโมเดลลดลงอย่างชัดเจน แสดงให้เห็นว่า indices เช่น NDVI และ NDWI มีบทบาทสำคัญในการเพิ่มความสามารถในการแยก class
ดังนั้นการเลือก feature ที่เหมาะสมมีผลต่อประสิทธิภาพของโมเดลอย่างมีนัยสำคัญ

ภารกิจที่ 4: Uncertainty Analysis
 1. Class ไหนสับสนมากที่สุด
 คำตอบ 
จากการวิเคราะห์ Confusion Matrix พบว่า class ที่มีความสับสนมากที่สุดคือ:
•	Agriculture vs Water 
•	Agriculture vs Urban 
สาเหตุหลักมาจากลักษณะทางกายภาพของพื้นที่:
•	นาข้าวมีน้ำขัง → spectral คล้าย Water 
•	พื้นที่ชุมชนในชนบทมี vegetation → คล้าย Agriculture 
นอกจากนี้ยังพบว่า Bare Land มีความแม่นยำต่ำมาก ซึ่งเกิดจากจำนวน training data ที่ไม่เพียงพอและลักษณะ spectral ที่คล้ายกับพื้นที่เกษตรหลังเก็บเกี่ยว
คำถามท้ายโจทย์ 
1. ถ้าเพิ่ม Training 2 เท่า
คำตอบ
การเพิ่มจำนวน training samples มีแนวโน้มช่วยเพิ่มความแม่นยำของโมเดล โดยเฉพาะใน class ที่มีข้อมูลน้อย เช่น Bare land
อย่างไรก็ตาม ผลลัพธ์จะไม่เพิ่มขึ้นแบบเส้นตรง เนื่องจากเมื่อข้อมูลเพียงพอแล้ว โมเดลจะเข้าสู่จุดอิ่มตัว (saturation point)
2. Spatial Autocorrelation
คำตอบ 
Spatial autocorrelation ทำให้ข้อมูลที่อยู่ใกล้กันมีลักษณะคล้ายกัน ส่งผลให้การแบ่งข้อมูลแบบ random split อาจทำให้โมเดลดูมีความแม่นยำสูงเกินจริง
เนื่องจาก training และ testing data มีความคล้ายคลึงกันทางพื้นที่
 3. Class ที่แย่ที่สุด + วิธีแก้
คำตอบ
Class ที่มีประสิทธิภาพต่ำที่สุดคือ Bare land
วิธีปรับปรุง:
•	เพิ่ม training samples 
•	ใช้ multi-temporal data 
•	เพิ่ม feature เช่น texture หรือ seasonal variation 
 4. ถ้าทำพื้นที่อื่น
คำตอบ
สิ่งที่ต้องเปลี่ยน:
•	Training data (ต้องเหมาะกับพื้นที่ใหม่) 
•	Class definition 
สิ่งที่ใช้ซ้ำได้:
•	workflow 
•	feature extraction 
•	model pipeline

