# 🍽️ Ingredient-Based Intelligent Recipe Recommendation System

A comprehensive, intelligent web application that recommends recipes based on ingredients available to the user. Integrates machine learning, OCR-based ingredient extraction, voice input, multilingual support, and user personalization into a unified cooking assistance platform.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [Recommendation Algorithm](#recommendation-algorithm)
- [Known Limitations](#known-limitations)
- [Future Scope](#future-scope)
- [Acknowledgements](#acknowledgements)

---

## Project Overview

This system addresses the everyday challenge of deciding what meals can be prepared from available kitchen ingredients. Instead of searching by recipe name, users simply provide their ingredients — via text, image upload, or voice — and the system returns ranked, relevant recipe suggestions.

**Key Goals:**
- Reduce food wastage by promoting use of on-hand ingredients
- Simplify meal planning for diverse users, including regional language speakers
- Provide an accessible, lightweight alternative to cloud-heavy AI solutions

---

## Features

| Module | Description |
|--------|-------------|
| **Multi-Modal Input** | Text entry, OCR image upload, and browser voice recognition |
| **ML Recommendation Engine** | TF-IDF + Cosine Similarity + KNN + Overlap Scoring |
| **OCR Extraction** | OpenCV + Tesseract OCR for grocery lists, labels, handwritten notes |
| **Voice Input** | Web Speech API for hands-free ingredient entry |
| **Multilingual Support** | English, Hindi, and Marathi interface |
| **User Personalization** | Registration, login, saved recipes, recommendation history |
| **Guided Cooking** | Step-by-step cooking instructions per recipe |
| **Responsive UI** | Tailwind CSS-based design for desktop, tablet, and mobile |

---

## Technology Stack

| Component | Technology |
|-----------|------------|
| Backend Framework | Python 3.10, Flask 2.3 |
| ML Recommendation | Scikit-learn 1.3 (TF-IDF, Cosine Similarity, KNN) |
| Data Processing | Pandas 2.0, NumPy 1.24 |
| OCR Engine | Tesseract OCR 5.0 |
| Image Processing | OpenCV 4.8, Pillow 10.0 |
| Frontend | HTML5, Tailwind CSS 3.3, JavaScript (ES6+) |
| Template Engine | Jinja2 |
| Database | MySQL 8.0 |
| ORM | SQLAlchemy 2.0 |
| Voice Input | Web Speech API |
| Version Control | Git / GitHub |

---

## Prerequisites

Ensure the following are installed on your system before proceeding:

- **Python** 3.10 or higher — https://www.python.org
- **MySQL** 8.0 or higher — https://dev.mysql.com
- **Tesseract OCR** 5.0 — https://github.com/UB-Mannheim/tesseract/wiki
- **Git** — https://git-scm.com
- A modern web browser (Chrome or Edge recommended for voice input)

---

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/recipe-recommendation-system.git
cd recipe-recommendation-system
```

### 2. Create and Activate a Virtual Environment

```bash
# Create virtual environment
python -m venv venv

# Activate on Windows
venv\Scripts\activate

# Activate on macOS/Linux
source venv/bin/activate
```

### 3. Install Python Dependencies

```bash
pip install -r requirements.txt
```

A typical `requirements.txt` includes:

```
flask==2.3.0
scikit-learn==1.3.0
pandas==2.0.0
numpy==1.24.0
opencv-python==4.8.0
pytesseract==0.3.10
Pillow==10.0.0
SQLAlchemy==2.0.0
flask-sqlalchemy
flask-login
pymysql
```

### 4. Configure Tesseract OCR Path

In your application configuration or `config.py`, set the Tesseract executable path:

```python
import pytesseract

# Windows example
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'

# Linux/macOS (usually auto-detected; or set explicitly)
# pytesseract.pytesseract.tesseract_cmd = '/usr/bin/tesseract'
```

### 5. Set Up the MySQL Database

```sql
-- Log into MySQL and run:
CREATE DATABASE recipe_db;
CREATE USER 'recipe_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON recipe_db.* TO 'recipe_user'@'localhost';
FLUSH PRIVILEGES;
```

### 6. Configure Environment Variables

Create a `.env` file in the project root:

```env
FLASK_APP=app.py
FLASK_ENV=development
SECRET_KEY=your_secret_key_here
DATABASE_URI=mysql+pymysql://recipe_user:your_password@localhost/recipe_db
```

### 7. Initialize the Database

```bash
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

Or if using a seed script:

```bash
python seed_database.py
```

### 8. Run the Application

```bash
flask run
```

The application will be available at: **http://127.0.0.1:5000**

---

## Project Structure

```
recipe-recommendation-system/
│
├── app.py                    # Main Flask application entry point
├── config.py                 # Configuration settings
├── requirements.txt          # Python dependencies
├── .env                      # Environment variables (not committed to Git)
│
├── models/                   # SQLAlchemy database models
│   ├── user.py
│   └── recipe.py
│
├── modules/                  # Core system modules
│   ├── preprocessor.py       # Text cleaning and normalization
│   ├── recommender.py        # TF-IDF, Cosine Similarity, KNN engine
│   ├── ocr_extractor.py      # OpenCV + Tesseract OCR pipeline
│   └── voice_handler.py      # Voice input processing utilities
│
├── routes/                   # Flask route definitions
│   ├── auth.py               # Registration, login, logout
│   ├── recommendation.py     # Ingredient input and recipe results
│   └── dashboard.py          # User dashboard and saved recipes
│
├── static/                   # Static assets
│   ├── css/
│   ├── js/
│   └── uploads/              # Temporary uploaded ingredient images
│
├── templates/                # Jinja2 HTML templates
│   ├── base.html
│   ├── index.html
│   ├── results.html
│   ├── recipe_detail.html
│   └── dashboard.html
│
├── data/
│   └── recipes.csv           # Recipe dataset
│
└── tests/                    # Unit and integration tests
    ├── test_recommender.py
    └── test_ocr.py
```

---

## Usage

### Entering Ingredients

**Option 1 — Text Input:**
1. Navigate to the home page.
2. Type your available ingredients into the input field (comma-separated), e.g.: `tomato, onion, garlic, paneer`.
3. Click **Get Recipes**.

**Option 2 — Image Upload (OCR):**
1. Click the **Upload Image** button.
2. Upload a photo of your grocery list, handwritten note, or food packaging label.
3. The system will automatically extract ingredient names via OCR.
4. Review extracted ingredients and click **Get Recipes**.

**Option 3 — Voice Input:**
1. Click the **Microphone** icon (Chrome or Edge recommended).
2. Speak your ingredients clearly, e.g.: *"tomato, onion, and garlic"*.
3. Recognized text will populate the ingredient field automatically.
4. Click **Get Recipes**.

### Viewing Recommendations

- Results are displayed as ranked recipe cards with similarity scores.
- Click any recipe card to view full ingredient list, step-by-step cooking instructions, and serving details.

### User Account Features

- **Register / Login** to access personalized features.
- **Save recipes** to your personal collection from any result.
- **View recommendation history** from your dashboard.
- **Switch language** (English / Hindi / Marathi) using the language selector in the navigation bar.

---

## Recommendation Algorithm

The engine uses a four-stage pipeline:

1. **TF-IDF Vectorization** — Converts recipe ingredient lists into sparse numerical vectors.
2. **Cosine Similarity** — Measures the angle between the user's ingredient vector and each recipe vector.
3. **K-Nearest Neighbor (KNN) Retrieval** — Efficiently finds the top-k closest recipes in vector space.
4. **Ingredient Overlap Scoring** — Calculates the fraction of user ingredients present in each candidate recipe.

**Final Score:**

```
score = α × cosine_similarity + (1 − α) × overlap_score
```

Recipes are returned ranked by this combined score in descending order.

---

## Known Limitations

- OCR accuracy depends on image quality; blurry or low-resolution images may yield incomplete extractions.
- Voice input requires a supported browser (Chrome or Edge) and may need internet connectivity.
- Recommendation quality scales with the diversity and completeness of the recipe dataset.
- No nutritional analysis or dietary restriction filtering in the current version.
- Very large recipe datasets may increase recommendation latency without approximate nearest neighbor optimizations.

---

## Future Scope

- Deep learning-based recommendation using Transformer models or Graph Neural Networks
- Real-time camera ingredient recognition using CNNs and object detection
- Nutritional analysis, calorie estimation, and dietary planning
- Cross-platform mobile app (Flutter or React Native)
- Ingredient substitution suggestions for missing items
- Smart grocery list generation and pantry management
- Social features: ratings, reviews, and community recipe sharing
- IoT / smart refrigerator integration for automatic inventory tracking

---

## Acknowledgements

- **Project Guide:** Ms. Kale J. S., Assistant Professor, Department of CSE, MGM's College of Engineering, Nanded
- **Head of Department:** Dr. A. M. Rajurkar, Department of CSE
- **Director:** Dr. G. S. Lathkar, MGM's College of Engineering, Nanded
- **Internship Framework:** Infosys Springboard Virtual Internship 6.0
- Open-source contributors behind Tesseract OCR, OpenCV, Scikit-learn, Flask, and Tailwind CSS

---

*Developed at MGM's College of Engineering, Nanded — Dr. Babasaheb Ambedkar Technological University, Maharashtra, India*
