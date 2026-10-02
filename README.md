# VisionQC

## AI-Powered Visual Quality Inspection for Small Manufacturers

VisionQC is an AI-powered computer vision inspection system designed for small manufacturers who cannot afford expensive machine-vision systems or large labelled defect datasets.

The system learns what a normal/good product looks like from approximately 20–30 good product images and then detects visual deviations in new products. It provides an anomaly score, heatmap, defect localisation, and PASS / REVIEW / FAIL decision.

---

## 👥 Team Details

**Team Name:** Quadratech  
**Team ID:** T-16  
**Project Name:** VisionQC

### Team Members

| Name | Roll Number |
|---|---|
| Member 1 | SECO-A-47 |
| Member 2 | SECO-A-28 |
| Member 3 | SECO-A-55 |
| Member 4 | SECO-A-6|

---

# 📌 Problem Statement

Small manufacturers often cannot afford dedicated machine-vision integrators or build large labelled datasets containing defective products.

Traditional computer-vision inspection systems can require:

- Expensive hardware
- Large labelled defect datasets
- Manual defect annotation
- Complex deployment
- Specialised technical expertise

### Our Approach

VisionQC follows an anomaly detection approach.

Instead of requiring hundreds or thousands of defective samples, the system learns the appearance of good products and identifies significant deviations from that normal appearance.

---

# 🎯 Selected Domain

**Artificial Intelligence / Machine Learning / Computer Vision**

---

# 💡 Project Overview

VisionQC allows a manufacturer to:

1. Create a product profile.
2. Upload 20–30 images of good products.
3. Train an anomaly detection model.
4. Upload a new product image.
5. Detect unusual regions automatically.
6. Visualise the detected region using a heatmap.
7. Generate a PASS / REVIEW / FAIL decision.
8. Review previous inspections through the dashboard.

Each product has its own trained model, preventing one product's characteristics from being incorrectly applied to another product.

---

# ⚙️ How VisionQC Works

text
             GOOD PRODUCT IMAGES
                    │
                    ▼
          Background Removal
                    │
                    ▼
             Product Cropping
                    │
                    ▼
             PatchCore Training
                    │
                    ▼
             Learn "Normal"
                    │
                    │
        ┌───────────▼───────────┐
        │    NEW PRODUCT IMAGE  │
        └───────────┬───────────┘
                    │
                    ▼
          Background Removal
                    │
                    ▼
             Product Cropping
                    │
                    ▼
             Anomaly Detection
                    │
                    ▼
          Anomaly Score + Heatmap
                    │
                    ▼
          Region Localisation
                    │
                    ▼
        PASS / REVIEW / FAIL

🧠 AI / ML Approach
1. Background Removal
Images are processed using rembg with U2NetP to remove unnecessary background information.
This allows the model to focus primarily on the product instead of the table, floor, or surrounding environment.
2. Product Cropping
After background removal, the product is cropped before being resized for model processing.
This helps preserve important visual details that could otherwise become too small after resizing.
3. PatchCore Anomaly Detection
VisionQC uses PatchCore through the Anomalib framework.
PatchCore learns feature representations from good product images and compares new images against the learned normal feature distribution.
The system does not require defective images for the initial training process.
4. Anomaly Scoring
The detected anomaly information is converted into a normalised score.
Default decision thresholds:
Score < 0.4      → PASS
0.4 – 0.6        → REVIEW
Score ≥ 0.6      → FAIL

A significant anomalous region can also cause the product to fail based on the configured localisation rules.
All thresholds are configurable in:
backend/config.py

5. Defect Localisation
VisionQC does not only provide a final decision.
It also identifies where the deviation occurs.
The system generates:
- Anomaly heatmap
- Annotated image
- Bounding boxes
- Region location
- Region score
- Severity
- Percentage of product area affected
This makes the inspection result easier for an operator to understand.
🚀 Key Features
Product Management
- Create and manage product profiles
- Separate model for each product
- Upload multiple training images
- Duplicate image detection
AI Training
- Learn from good product images
- PatchCore-based anomaly detection
- CPU-based training
- Live training progress
Inspection
- Upload product images
- Detect visual anomalies
- Generate anomaly scores
- PASS / REVIEW / FAIL classification
Visualisation
- Original image
- Heatmap overlay
- Annotated image
- Bounding boxes
- Defect region information
Analytics & History
- Inspection history
- Dashboard
- Product-wise results
- Decision statistics
- Score information
- Human review information
Human-in-the-Loop
Operators can:
- Accept an item as Good
- Confirm a Defect
- Store the human decision alongside the AI decision
🏗️ Technology Stack
Backend
- Python
- FastAPI
- SQLAlchemy
- SQLite
- Uvicorn
AI / Computer Vision
- Anomalib
- PatchCore
- PyTorch
- TorchVision
- rembg
- OpenCV
- NumPy
Frontend
- HTML
- CSS
- JavaScript
Development Tools
- Git
- GitHub
- VS Code
📁 Project Structure
T-16-VisionQC/
│
├── backend/
│   ├── main.py
│   ├── models.py
│   ├── schemas.py
│   ├── config.py
│   ├── routes/
│   ├── ai_engine/
│   │   ├── segmentation.py
│   │   ├── cropping.py
│   │   ├── trainer.py
│   │   ├── inference.py
│   │   ├── localization.py
│   │   └── visualize.py
│   ├── scripts/
│   └── tests/
│
├── frontend/
│   └── index.html
│
├── VisionQc/
│
├── old_ai/
│
├── legacy/
│   └── node-server/
│
├── .gitignore
├── CLAUDE.md
└── README.md

🔄 System Architecture
                    ┌─────────────────────┐
                    │      Frontend       │
                    │   HTML/CSS/JS UI    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │       FastAPI       │
                    │       Backend       │
                    └──────────┬──────────┘
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
       Product/Data       AI Pipeline       Inspection
        Management       PatchCore +         Results
                           rembg
             │                 │                 │
             └─────────────────┼─────────────────┘
                               ▼
                    ┌─────────────────────┐
                    │       SQLite        │
                    │      Database       │
                    └─────────────────────┘

🛠️ Installation
Requirements
- Windows
- Python 3.12 or 3.13
- Git
- Internet connection for the first model-weight download
VisionQC can run on a CPU. A GPU is not required.
Create Virtual Environment
cd VisionQc

python -m venv .venv

.venv\Scripts\Activate.ps1

Install PyTorch CPU Version
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu

Install Backend Dependencies
pip install -r backend\requirements.txt

The first training run downloads the required model weights.
▶️ Running VisionQC
Start Backend
Open Terminal 1:
cd backend

..\.venv\Scripts\python.exe -m uvicorn main:app --port 8000

Backend:
http://127.0.0.1:8000

API documentation:
http://127.0.0.1:8000/docs

Start Frontend
Open Terminal 2:
cd frontend

..\.venv\Scripts\python.exe -m http.server 5501 --bind 127.0.0.1

Open:
http://127.0.0.1:5501/index.html

📷 Using VisionQC
Step 1 — Create Product
Create a product profile with:
- Product Name
- Product ID
- Sector
Step 2 — Teach Normal
Upload approximately 20–30 images of good and undamaged products.
Start the training process.
Step 3 — Inspect
Upload a new product image.
VisionQC analyses the image and generates:
- Anomaly score
- Heatmap
- Defect region
- Bounding box
- Decision
Step 4 — Review
Open the inspection history and review the result.
The operator can confirm or override the AI decision.
📊 Dataset / API Information
Dataset
VisionQC is designed around a few-shot / normal-only anomaly detection approach.
The system can be trained using approximately 20–30 good product images supplied by the user.
No large labelled defect dataset is required for the primary training workflow.
For optional evaluation, known good and defective images can be placed in:
data/products/<product_id>/test_good/
data/products/<product_id>/test_bad/

These images are used for evaluation and are not required for normal model training.
External APIs
VisionQC does not depend on a third-party cloud API for its core inspection pipeline.
The main AI processing runs locally using:
- Anomalib
- PatchCore
- PyTorch
- rembg
- OpenCV
📸 Screenshots / Demo
The application provides an interactive inspection interface containing:
- Product management
- Training interface
- Image inspection
- Heatmap visualisation
- Annotated defect regions
- Inspection history
- Analytics dashboard
Demo Flow
Create Product
      ↓
Upload Good Images
      ↓
Train Model
      ↓
Upload New Product
      ↓
AI Inspection
      ↓
Heatmap + Bounding Box
      ↓
PASS / REVIEW / FAIL
      ↓
Human Review

🧪 Testing
Run the backend tests:
cd backend

..\.venv\Scripts\python.exe -m unittest discover -s tests

For model evaluation:
..\.venv\Scripts\python.exe scripts\evaluate_product.py <product_id> --train

⚠️ Limitations
1. Reflective / Shiny Products
Highly reflective surfaces can be challenging because lighting and reflections may change significantly between images.
2. Threshold Calibration
The initial PASS / REVIEW / FAIL thresholds are based on normal training examples and are not a replacement for calibration using a large real-world defective dataset.
3. Anomaly Detection vs Defect Classification
VisionQC identifies that an area is visually different.
It does not automatically classify the exact defect type, such as:
- Scratch
- Dent
- Crack
- Missing component
A labelled dataset and additional classification model would be required for detailed defect-type classification.
4. CPU Performance
The system is designed for practical inspection and experimentation on CPU hardware. High-speed industrial conveyor inspection would require further optimisation and potentially GPU acceleration.
🔮 Future Scope
- Shape and deformation detection
- 360° multi-camera inspection
- Real-time camera inspection
- Faster inference optimisation
- Defect-type classification
- Automatic threshold calibration
- PDF inspection reports
- Advanced analytics
- Production-line integration
- Edge-device deployment
🌟 Project Impact
VisionQC aims to make computer-vision-based quality inspection more accessible to small manufacturers.
Instead of requiring:
Large Defect Dataset
        +
Expensive Vision System
        +
Specialised Integration

VisionQC focuses on:
20–30 Good Images
        +
Normality Learning
        +
Local Anomaly Detection
        =
Accessible Quality Inspection

👥 Team
Team Name: Quadratech
Team ID: T-16
Project: VisionQC
Member	Role	Roll Number
Member 1	AI / ML	47
Member 2	Backend	55
Member 3	Frontend	28
Member 4	Integration / Testing	6


🏆 TechForge 2026
Team Name: Quadratech
Team ID: T-16
Project Title: VisionQC
Domain: Artificial Intelligence / Machine Learning / Computer Vision
VisionQC was developed as part of the TechForge 2026 Hackathon.
📄 License
This project was developed by Team Quadratech for the TechForge 2026 Hackathon.


