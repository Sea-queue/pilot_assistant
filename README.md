<h1 align="center">Pilot_Assistant</h1>

## **Automated Landing Assistance System ✈️**

The automation model designed to assist pilots by:

- Extracting essential information from approach charts 📜
- Retrieving real-time aircraft data ✈️
- Calculating the optimal landing procedure for a safe and efficient descent 🛬

This system enhances situational awareness and decision-making, helping pilots execute precise landings with ease.

## **Pilot Instrument Approach Charts**

| Image 1                            | Image 2                             |
| ---------------------------------- | ----------------------------------- |
| ![Alt1](pilot_asistant/pudong.png) | ![Alt2](pilot_asistant/taiyuan.png) |

# **Solution:**

## **1. Key Information to Extract**

Instrument approach charts contain various sections. Here are the key elements to extract:

### **Header Information:**

- **Airport Name** and **ICAO/IATA Code**
- **Approach Type** (ILS, RNAV, VOR, NDB, etc.)
- **Runway Number** (e.g., "ILS or LOC RWY 24")

### **Navigation Data:**

- **Frequencies** (ILS localizer, VOR, DME, NDB, ATIS, Tower, Ground)
- **Final Approach Course** (degrees)
- **Decision Altitude (DA)** or **Minimum Descent Altitude (MDA)**
- **Glideslope Angle** (if ILS)
- **Missed Approach Instructions** (text-based)

### **Fixes & Waypoints:**

- **Initial Approach Fix (IAF)**
- **Final Approach Fix (FAF)**
- **Missed Approach Point (MAP)**
- **Coordinates (Lat/Long)**

### **Altitude & Speed Restrictions:**

- **Stepdown Fix Altitudes**
- **Holding Patterns**
- **Category A, B, C, D minimums**
- **Wind & Temperature Data**

### **Diagram Information:**

- **Runway Layout & Distance**
- **ILS Glidepath**
- **Obstacles and MSA (Minimum Safe Altitude)**
- **Missed Approach Holding Pattern**

---

## **2. CV & OCR Pipeline for Approach Chart Processing**

To extract this information, a **multi-model approach** combining OCR, layout analysis, and structured text processing is required.

### **Step 1: Preprocessing the Approach Chart Image**

- Convert to **grayscale** to remove noise.
- Apply **binarization (Otsu's Thresholding or Adaptive Thresholding)** for better OCR performance.
- Apply **deskewing** (Hough Transform) to correct any image tilt.
- Use **image segmentation** to separate text from graphical components.

### **Step 2: Optical Character Recognition (OCR)**

- Use **Tesseract OCR** (with custom language models trained on aviation text).
- Alternatively, use **Google Vision API** or **AWS Textract** for better accuracy.
- Extract structured text data from the header, nav data, altitudes, and other key sections.

### **Step 3: Layout Parsing & Text Structuring**

- Use **Detectron2, LayoutLM, or Donut (Document Understanding Transformer)** to recognize text placement.
- Detect sections of the chart such as the title, frequencies, approach minima, and instructions.

### **Step 4: Key Information Extraction & Parsing**

- Use **regular expressions (regex) and NLP** to extract relevant data (e.g., `Runway 24`, `ILS Freq 110.3`).
- Use **Named Entity Recognition (NER)** to classify extracted values (e.g., `110.3` as `ILS Frequency`).
- Store extracted data in a structured format (JSON, database).

### **Step 5: Diagram Understanding (Optional but Advanced)**

- Use **YOLO or Faster R-CNN** to detect elements like:
  - Aircraft path
  - Runway layout
  - Waypoints and fixes
  - Holding patterns
- Convert the detected elements into structured spatial data.

---

## **3. Model Choices for Each Task**

| Task                            | Model Options                                      |
| ------------------------------- | -------------------------------------------------- |
| **Text OCR**                    | Tesseract, EasyOCR, Google Vision, AWS Textract    |
| **Layout Parsing**              | LayoutLMv3, Detectron2, Donut                      |
| **Key Information Extraction**  | Regex, Spacy NLP, Transformers                     |
| **Graphical Element Detection** | YOLOv8, Faster R-CNN, SAM (Segment Anything Model) |

---

## **4. Implementation Plan**

1. **Dataset Collection**: Gather a dataset of approach charts from FAA, Jeppesen, or Navblue.
2. **Data Annotation**: Use **LabelImg or Roboflow** to label runway layouts, waypoints, etc.
3. **Train OCR & Layout Models**: Fine-tune **LayoutLM or Donut** on approach chart documents.
4. **Deploy & Test**: Build a pipeline where a user uploads a chart, and the system extracts structured data.
5. **Refinement & Automation**: Integrate with flight planning software or EFB (Electronic Flight Bag).

---

## **5. Expected Challenges & Solutions**

| Challenge                    | Solution                                                  |
| ---------------------------- | --------------------------------------------------------- |
| Complex Fonts & Symbols      | Fine-tune OCR with aviation datasets                      |
| Variability in Chart Layouts | Train with diverse chart samples (FAA, Jeppesen, Navblue) |
| Handwritten Annotations      | Use CNNs or Transformers for handwritten text recognition |
| Diagram Parsing              | Use object detection models (YOLO, Faster R-CNN)          |

---
