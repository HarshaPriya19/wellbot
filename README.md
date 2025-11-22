🧠 WellBot – Global Wellness Assistance Chatbot

WellBot is an AI-powered wellness assistance chatbot built using Rasa, NLP, and machine learning to provide personalized guidance across physical and mental health domains. It understands user queries through intent classification, entity extraction, and sentiment analysis, and combines this with a recommendation system to deliver customized advice on exercise, nutrition, sleep, and emotional well-being. The project integrates a Rasa-based conversational engine, data-driven wellness insights, and a backend interface for smooth interaction, creating an intelligent, responsive, and holistic wellness companion accessible through a web-based platform.

🚀 Features

Wellness recommendations (exercise, sleep, diet)

Emotion-aware conversations (sentiment analysis)

Rasa NLU + Dialogue Management

Personalized responses using user profiles

Scalable backend architecture

Secure & structured data handling

🧱 Modules Overview
1. Data Processing & Management

Dataset loading & cleaning

Feature engineering

EDA & preprocessing for ML/NLP

2. NLP Engine (Rasa)

Intent classification

Entity extraction

Stories & rules for dialogue flow

Sentiment/emotion analysis integration

3. Recommendation System

ML models for personalized wellness plans

Hybrid filtering + profile-based suggestions

Adaptive learning using feedback

4. Chatbot Interface & Deployment

Rasa backend for conversational logic

Flask API for serving responses

Web chat interface

Cloud-ready deployment

📂 Project Structure
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
│   ├── app.py      (Flask server)
│   └── models/
│── requirements.txt
│── README.md

⚙️ Setup Instructions (Rasa + Flask)
1️⃣ Clone the Repository
git clone <repo-url>
cd <project-folder>

2️⃣ Create Virtual Environment
python -m venv .venv


Activate (Windows):

.venv\Scripts\activate

3️⃣ Install Dependencies
Install Python packages:
pip install -r requirements.txt

Install Rasa:
pip install rasa


(If needed)

pip install rasa[full]

4️⃣ Train the Rasa Model
rasa train


This generates the model inside:

/models

5️⃣ Start Rasa Action Server (for custom actions)
rasa run actions

6️⃣ Start Rasa Server (NLU + Dialogue)
rasa run --enable-api --cors "*"

7️⃣ Start Flask Backend (if used)
python app.py

🔄 Workflow (Short)

User sends message → web UI

Flask forwards it to Rasa API

Rasa → predicts intent & entities

Custom actions run ML-based recommendation system

Personalized response returned to UI

User feedback saved to DB

🗄 Database (Summary)

Stores:

Users

Profiles

Wellness dataset

Conversations

Messages

Recommendations

Feedback

🔮 Future Enhancements

Multilingual support

Voice-based interaction

Mobile app integration

🛡 License

MIT License.
