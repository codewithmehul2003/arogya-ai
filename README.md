# Arogya AI — Agentic Disease Prediction with Ayurvedic Recommendations

Arogya AI is a hybrid healthcare assistant that predicts likely diseases from user-reported symptoms using a Multinomial Naive Bayes model, validates and refines predictions with an agentic rule layer, and provides culturally relevant Ayurvedic recommendations and human-readable explanations via an LLM.

## Table of Contents
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Usage](#usage)
  - [Training the Model](#training-the-model)
  - [Running the Service](#running-the-service)
  - [API Endpoints (example)](#api-endpoints-example)
- [Data & Models](#data--models)
- [Technologies](#technologies)
- [Scalability & Reliability](#scalability--reliability)
- [Roadmap / Future Enhancements](#roadmap--future-enhancements)
- [Contributing](#contributing)
- [Authors](#authors)
- [License](#license)

## Key Features
- Symptom-to-disease prediction using Multinomial Naive Bayes.
- Agentic rule layer for validation and hypothesis locking.
- Ayurvedic knowledge-base integration for tailored, culturally-aware recommendations.
- LLM-based explanations that translate model output into user-friendly text.
- Cloud-native design with Docker-friendly components for easy deployment.

## Architecture
Simple flow:
User (Web/Mobile UI) → Cloud API → ML microservice (predict + agent) → DB (symptom-disease, ayurveda KB)  
LLM service (explanations) is called to produce human-readable recommendations and reasoning.

## Getting Started

### Prerequisites
- Python 3.8+ (3.9+ recommended)
- Node.js and npm/yarn (if the repo includes a frontend)
- Docker (optional, for containerized deployment)
- Git

### Installation
1. Clone the repo:
   git clone https://github.com/rohit29032005/Arogya-.git
   cd Arogya-

2. Create and activate a virtual environment:
   python -m venv venv
   source venv/bin/activate  # Linux / macOS
   venv\Scripts\activate     # Windows

3. Install Python dependencies:
   pip install -r requirements.txt

4. (Optional) Install frontend dependencies (if applicable):
   cd frontend
   npm install
   cd ..

### Configuration
Create a `.env` or environment variables with at least:
- LLM_API_KEY or OPENAI_API_KEY — API key for explanation LLM
- DATABASE_URL — database connection string (or use local SQLite)
- SECRET_KEY — application secret for sessions/auth (if applicable)

Adjust other environment variables as required by the repo (ports, debug flags, etc.).

## Usage

### Training the Model
- Prepare the dataset: ensure a symptom ↔ disease CSV is present (e.g., `data/symptom_disease.csv`).
- Run the training script (example):
  python scripts/train.py --data data/symptom_disease.csv --out models/mnb_model.pkl

The project uses a Multinomial Naive Bayes classifier to train on symptom occurrence counts or binary features. Check `scripts/train.py` (or the training module) for exact options.

### Running the Service
Start backend API:
- python app/main.py  # or `flask run` / `uvicorn app.main:app --reload` depending on framework

Start frontend (if present):
- cd frontend
- npm run dev

Docker (example):
- docker build -t arogya-backend .
- docker run -e LLM_API_KEY=... -p 8000:8000 arogya-backend

### API Endpoints (example)
- POST /api/predict
  - Body: { "symptoms": ["fever", "cough", "fatigue"] }
  - Response: { "predictions": [{ "disease": "Flu", "probability": 0.72 }, ...] }

- POST /api/validate
  - Agentic checks and hypothesis enforcement

- POST /api/recommend
  - Body: { "disease": "Flu" }
  - Response: Ayurvedic recommendations and suggested lifestyle/dietary guidance

- GET /api/explain
  - Returns an LLM-generated explanation of prediction & rationale

Adjust endpoints to match actual implementation files.

## Data & Models
- Keep all sensitive user data encrypted and follow local regulations for medical data storage.
- Example data files: `data/symptom_disease.csv`, `data/ayurveda_recommendations.csv`
- Trained model artifacts: `models/mnb_model.pkl` (or similar)

## Technologies
- Python (ML + backend)
- JavaScript (frontend)
- scikit-learn (MultinomialNB)
- Docker
- RESTful API frameworks (Flask / FastAPI / Express as applicable)
- LLM provider (OpenAI, Anthropic, or similar) for explanations

## Scalability & Reliability
- Containerize services and use orchestration (Kubernetes) to scale horizontally.
- Use load balancing, monitoring, centralized logging, and database replication/backups.
- Implement authentication & authorization for all endpoints handling personal health information.

## Roadmap / Future Enhancements
- Integrate wearable data (heart rate, sleep, activity) to enrich predictions.
- Add voice input/output and multi-language support.
- Telemedicine integration for connecting users with clinicians when high risk is detected.
- Add severity scoring and visual risk indicators on the UI.

## Contributing
- Fork the repository and open a pull request with a clear description of changes.
- Add tests for new functionality and ensure existing tests pass.
- Follow code style guidelines (PEP8 for Python, consistent linting for JS).

## Authors
- Rohit Shrivastava (23BCE0052)
- Anshika Rai (23BCE0080)

## License
Specify a license for the project (e.g., MIT). If you want, I can add an MIT or other license file.

## Contact
For questions or collaboration, open an issue or reach out via the GitHub profile: https://github.com/rohit29032005
