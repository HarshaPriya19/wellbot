# WellBot – Global Wellness Assistance Chatbot

WellBot is an AI-powered wellness assistance chatbot built using Rasa, NLP, and machine learning to provide personalized guidance across physical and mental health domains. It understands user queries through intent classification, entity extraction, and sentiment analysis, and combines this with a recommendation system to deliver customized advice on exercise, nutrition, sleep, and emotional well-being. The project integrates a Rasa-based conversational engine, data-driven wellness insights, and a backend interface for smooth interaction, creating an intelligent, responsive, and holistic wellness companion accessible through a web-based platform.

## Features
- Personalized wellness recommendations
- Emotion-aware conversational AI
- Rasa NLU + Dialogue Management
- User profile–based recommendations
- Cloud-ready architecture
- Conversation history & feedback loops

## Modules Overview

### 1. Data Processing & Management
Handles dataset loading, cleaning, feature engineering, and preparation for machine learning pipelines.

### 2. NLP Engine (Rasa)
Intent classification, entity extraction, sentiment analysis, and multi-turn dialogue management.

### 3. Recommendation System
ML-based personalized plans for exercise, nutrition, and sleep with adaptive learning.

### 4. Chatbot Interface & Deployment
Web-based UI, Flask backend integration, and cloud deployment support.

## Project Structure
```
project/
│── data/
│── rasa/
│   ├── domain.yml
│   ├── nlu.yml
│   ├── stories.yml
│   ├── rules.yml
│   ├── actions.py
│   └── config.yml
│── backend/
│   ├── app.py
│   └── models/
│── requirements.txt
│── README.md
```

## Setup Instructions (Rasa + Flask)

### 1. Clone the Repository
```
git clone <repo-url>
cd <project-folder>
```

### 2. Create Virtual Environment
```
python -m venv .venv
```

Activate (Windows):
```
.venv\Scripts\activate
```

### 3. Install Dependencies
```
pip install -r requirements.txt
pip install rasa
```

### 4. Train Rasa Model
```
rasa train
```

### 5. Run Action Server
```
rasa run actions
```

### 6. Run Rasa Server
```
rasa run --enable-api --cors "*"
```

### 7. Start Flask Backend
```
python app.py
```

## License
MIT License.
