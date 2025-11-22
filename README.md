# Global Wellness Assistant Chatbot

## Module 1: User Authentication & Profile Management

## Overview

This module provides a complete authentication and profile management system for the Global Wellness Assistant Chatbot project. It includes user registration, secure login/logout with JWT tokens, session handling, and profile management with language preferences and age group tracking.

## Features

- **User Registration**: Email/password registration with validation
- **Secure Authentication**: JWT-based login and logout
- **Session Management**: Token-based session handling on the frontend
- **Profile Management**: View and update user profile information
- **Language Preferences**: Support for English and Hindi
- **Age Group Tracking**: Store and update user age group
- **RESTful API**: JSON API endpoints for programmatic access
- **Web UI**: Clean, Bootstrap 5-based user interface

## Tech Stack

- **Backend**: Python 3 + Flask
- **ORM**: SQLAlchemy (Flask-SQLAlchemy)
- **Authentication**: JWT via flask-jwt-extended
- **Database**: SQLite (development)
- **Templates**: Jinja2 + Bootstrap 5 (CDN)
- **Testing**: pytest

## Setup Instructions

### 1. Create Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Environment Variables

Copy the example environment file and update with your secrets:

```bash
cp .env.example .env
```

Edit `.env` and set:
- `SECRET_KEY`: A secure random string for Flask sessions
- `JWT_SECRET_KEY`: A secure random string for JWT token signing
- `SQLALCHEMY_DATABASE_URI`: Database connection string (default: `sqlite:///wellness.db`)

### 4. Create Database Tables

```bash
flask --app app:create_app create-db
```

### 5. Run the Application

```bash
flask --app app:create_app run
```

Or for development with auto-reload:

```bash
flask --app app:create_app run --debug
```

The application will be available at `http://localhost:5000`

## Project Structure

```
global_wellness_auth/
├── app.py                 # Flask application factory
├── config.py              # Configuration settings
├── extensions.py          # Flask extensions (db, jwt)
├── models.py              # Database models
├── auth/
│   ├── __init__.py
│   ├── routes.py          # Authentication routes
│   └── validators.py      # Validation utilities
├── profile/
│   ├── __init__.py
│   └── routes.py          # Profile routes
├── templates/
│   ├── base.html          # Base template
│   ├── auth/
│   │   ├── register.html  # Registration page
│   │   └── login.html     # Login page
│   └── profile/
│       └── profile.html   # Profile page
├── static/
│   └── css/
│       └── style.css      # Custom styles
├── tests/
│   └── test_auth_api.py   # Test suite
├── requirements.txt       # Python dependencies
├── .env.example           # Example environment variables
└── README.md              # This file
```

## API Endpoints

### Authentication

#### Register User
```bash
POST /auth/api/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123",
  "confirm_password": "password123",
  "name": "John Doe",
  "age_group": "26-35",
  "preferred_language": "en"
}
```

**Response (201):**
```json
{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "preferred_language": "en"
  }
}
```

#### Login
```bash
POST /auth/api/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response (200):**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "preferred_language": "en",
    "name": "John Doe",
    "age_group": "26-35"
  }
}
```

#### Logout
```bash
POST /auth/api/logout
```

**Response (200):**
```json
{
  "message": "Logged out successfully"
}
```

#### Get Current User
```bash
GET /auth/api/me
Authorization: Bearer <access_token>
```

**Response (200):**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "John Doe",
  "age_group": "26-35",
  "preferred_language": "en",
  "created_at": "2024-01-01T00:00:00",
  "updated_at": "2024-01-01T00:00:00"
}
```

### Profile

#### Get Profile
```bash
GET /profile/api/me
Authorization: Bearer <access_token>
```

Same response as `/auth/api/me`

#### Update Profile
```bash
PUT /profile/api/me
Authorization: Bearer <access_token>
Content-Type: application/json

{
  "name": "Jane Doe",
  "age_group": "36-50",
  "preferred_language": "hi"
}
```

**Response (200):**
```json
{
  "id": 1,
  "email": "user@example.com",
  "name": "Jane Doe",
  "age_group": "36-50",
  "preferred_language": "hi",
  "created_at": "2024-01-01T00:00:00",
  "updated_at": "2024-01-01T01:00:00"
}
```

## Example cURL Commands

### Register
```bash
curl -X POST http://localhost:5000/auth/api/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123",
    "confirm_password": "password123",
    "name": "Test User",
    "age_group": "26-35",
    "preferred_language": "en"
  }'
```

### Login
```bash
curl -X POST http://localhost:5000/auth/api/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

### Get Profile (replace TOKEN with actual token)
```bash
curl -X GET http://localhost:5000/profile/api/me \
  -H "Authorization: Bearer TOKEN"
```

### Update Profile (replace TOKEN with actual token)
```bash
curl -X PUT http://localhost:5000/profile/api/me \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Updated Name",
    "age_group": "36-50",
    "preferred_language": "hi"
  }'
```

## Web UI Routes

- `/` - Home page
- `/auth/register` - Registration form
- `/auth/login` - Login form
- `/profile` - Profile view and edit page

## Running Tests

```bash
pytest tests/test_auth_api.py -v
```

## Data Model

### User Model

- `id`: Integer primary key
- `email`: String, unique, not null
- `password_hash`: String, not null (hashed password)
- `name`: String (optional)
- `age_group`: String (allowed: "18-25", "26-35", "36-50", "50+")
- `preferred_language`: String (allowed: "en", "hi"), default "en"
- `created_at`: DateTime
- `updated_at`: DateTime (auto-updated)

## Validation Rules

- **Email**: Must be valid email format, unique
- **Password**: Minimum 8 characters
- **Age Group**: Must be one of: "18-25", "26-35", "36-50", "50+"
- **Preferred Language**: Must be "en" or "hi"

## Security Notes

- Passwords are hashed using Werkzeug's password hashing
- JWT tokens are used for authentication
- Tokens should be stored securely on the client (localStorage or httpOnly cookies)
- For production, use strong SECRET_KEY and JWT_SECRET_KEY values
- Consider implementing token blacklisting for logout in production
- Use HTTPS in production

---

## Module 2: Conversational AI Core (Chatbot Engine)

### Overview

Module 2 adds a conversational AI chatbot powered by Rasa 3.x that provides health and wellness assistance. The chatbot can handle symptom queries, first aid advice, and wellness tips through natural language understanding and dialogue management.

### Features

- **Natural Language Understanding (NLU)**: Recognizes health-related intents in English and Hindi
- **Dialogue Management**: Handles conversation flows for symptoms, first aid, and wellness queries
- **Health Knowledge Base**: Integrated knowledge base for symptom-specific advice
- **Web Chat Interface**: Clean, responsive chat UI integrated into the Flask app
- **RESTful Integration**: Flask communicates with Rasa server via HTTP REST API
- **Error Handling**: Graceful fallbacks when Rasa server is unavailable

### Tech Stack

- **Rasa 3.6.15**: Conversational AI framework
- **Rasa SDK**: Custom actions support
- **Flask Integration**: REST API bridge between Flask and Rasa
- **Bootstrap 5**: Chat UI styling

### Setup Instructions

#### 1. Install Rasa Dependencies

**⚠️ Important**: Rasa 3.5.x requires Python 3.7-3.10. Rasa 3.6.x requires Python 3.8-3.10. It does not support Python 3.11, 3.12, or 3.13. **Use Python 3.10 for best compatibility.**

If you're using Python 3.12+, please see `RASA_INSTALLATION.md` for detailed instructions.

```bash
pip install -r requirements.txt
```

This will install:
- `rasa>=3.0.0,<4.0.0` (requires Python 3.8-3.11)
- `rasa-sdk>=3.0.0,<4.0.0` (requires Python 3.8-3.11)
- `requests==2.31.0`

#### 2. Configure Environment Variables

Update your `.env` file to include the Rasa server URL:

```bash
RASA_BASE_URL=http://localhost:5005
```

#### 3. Train the Rasa Model

Navigate to the Rasa bot directory and train the model:

```bash
cd rasa_bot
rasa train
```

This will:
- Process NLU training data (`data/nlu.yml`)
- Process stories (`data/stories.yml`)
- Process rules (`data/rules.yml`)
- Train the NLU pipeline and dialogue policies
- Generate a trained model in `models/` directory

#### 4. Run Rasa Server

In a separate terminal, start the Rasa server:

```bash
cd rasa_bot
rasa run --enable-api --cors "*" --port 5005
```

The server will:
- Load the trained model
- Start the REST API on port 5005
- Enable CORS for Flask integration

#### 5. Run Rasa Actions Server (Optional)

If using custom actions, start the actions server in another terminal:

```bash
cd rasa_bot
rasa run actions --port 5055
```

#### 6. Run Flask Application

In your main terminal:

```bash
flask --app app:create_app run
```

The Flask app will be available at `http://localhost:5000`

### Project Structure (Module 2)

```
global_wellness_auth/
├── conversation/              # Flask conversation module
│   ├── __init__.py
│   ├── routes.py             # Chat routes and API endpoints
│   ├── rasa_client.py        # Rasa HTTP client
│   └── health_kb.json        # Health knowledge base
├── rasa_bot/                  # Rasa bot configuration
│   ├── config.yml            # Rasa pipeline and policies
│   ├── domain.yml            # Intents, entities, slots, responses
│   ├── credentials.yml       # Channel credentials
│   ├── endpoints.yml         # Action server endpoints
│   ├── data/
│   │   ├── nlu.yml           # NLU training examples
│   │   ├── stories.yml       # Conversation stories
│   │   └── rules.yml         # Dialogue rules
│   └── actions/
│       ├── __init__.py
│       └── actions.py        # Custom actions (optional)
├── templates/
│   └── conversation/
│       └── chat.html         # Chat interface template
└── static/
    └── js/
        └── chat.js           # Chat JavaScript
```

### Rasa Configuration

#### Intents

- `greet`: Greeting messages
- `goodbye`: Farewell messages
- `ask_about_symptom`: Symptom queries
- `query_first_aid`: First aid questions
- `ask_about_wellness_tip`: Wellness advice requests
- `thank`: Thank you messages
- `fallback`: Unrecognized inputs

#### Entities

- `symptom`: Health symptoms (e.g., headache, fever)
- `body_part`: Body parts (e.g., head, stomach)
- `ailment`: Medical conditions
- `severity`: Symptom severity (mild, moderate, severe)
- `duration`: How long symptoms have lasted

#### Responses

The bot has predefined responses for:
- Greetings and farewells
- Symptom-specific first aid (headache, fever, cold)
- Generic first aid advice
- Wellness tips
- Fallback messages

### API Endpoints

#### Chat Page

```bash
GET /conversation/chat
```

Renders the chat interface.

#### Send Message

```bash
POST /conversation/api/message
Content-Type: application/json

{
  "message": "I have a headache, what should I do?"
}
```

**Response (200):**
```json
{
  "messages": [
    {
      "type": "text",
      "text": "For a headache, try these steps: 1) Rest in a quiet, dark room..."
    }
  ]
}
```

**Error Response (503):**
```json
{
  "error": "Chatbot service is temporarily unavailable",
  "messages": [
    {
      "type": "text",
      "text": "I'm sorry, but I'm having trouble connecting..."
    }
  ]
}
```

### Example cURL Commands

#### Test Rasa REST Webhook Directly

```bash
curl -X POST http://localhost:5005/webhooks/rest/webhook \
  -H "Content-Type: application/json" \
  -d '{
    "sender": "test_user",
    "message": "Hello"
  }'
```

#### Test Flask Conversation API

```bash
curl -X POST http://localhost:5000/conversation/api/message \
  -H "Content-Type: application/json" \
  -d '{
    "message": "I have a mild headache"
  }'
```

### Example Conversations

#### Symptom Query

**User:** "I have a headache, what should I do?"

**Bot:** "For a headache, try these steps: 1) Rest in a quiet, dark room, 2) Apply a cold or warm compress to your forehead, 3) Stay hydrated, 4) Consider over-the-counter pain relief if appropriate, 5) Avoid triggers like bright lights or loud noises. If headaches persist or are severe, consult a healthcare provider."

#### First Aid Query

**User:** "What should I do for a burn?"

**Bot:** "For a minor burn, cool the area with running water for 10-15 minutes, cover with a sterile bandage, and avoid breaking blisters. For severe burns, large burns, or burns on face/hands, seek immediate medical attention."

#### Wellness Tip Query

**User:** "How can I sleep better?"

**Bot:** "Here are some general wellness tips: maintain a balanced diet, stay hydrated, get regular exercise, ensure adequate sleep, manage stress, and practice good hygiene. These habits can help support your overall health and well-being."

### Running Tests

```bash
# Test conversation API
pytest tests/test_conversation_api.py -v

# Test all modules
pytest tests/ -v
```

### Troubleshooting

#### Rasa Server Not Responding

1. Ensure Rasa server is running: `rasa run --enable-api --cors "*" --port 5005`
2. Check RASA_BASE_URL in `.env` matches the Rasa server URL
3. Verify firewall/network settings allow connection

#### Model Training Errors

1. Check YAML syntax in `data/nlu.yml`, `data/stories.yml`, `data/rules.yml`
2. Ensure all intents in stories/rules are defined in `domain.yml`
3. Verify entity annotations in NLU examples match domain entities

#### Custom Actions Not Working

1. Ensure actions server is running: `rasa run actions --port 5055`
2. Check `endpoints.yml` has correct action server URL
3. Verify action names in domain match action class names

### Health Knowledge Base

The `conversation/health_kb.json` file contains symptom-specific advice. To add new entries:

```json
{
  "symptom_name": {
    "short_advice": "Advice text here...",
    "category": "symptom"
  }
}
```

### Security & Disclaimers

⚠️ **Important**: The chatbot provides general wellness information only. It is NOT a substitute for professional medical advice, diagnosis, or treatment. Always:

- Consult healthcare providers for medical concerns
- Seek emergency services for medical emergencies
- Use the chatbot for informational purposes only

### Next Steps

Future enhancements may include:
- Expanded health knowledge base
- Multi-language NLU models
- Integration with external health APIs
- Conversation history and context
- Personalized recommendations based on user profile

---

## Module 3: Health Knowledge Base Expansion & Advanced NLP

### Overview

Module 3 significantly expands the health knowledge base and improves the chatbot's natural language understanding capabilities. It adds structured knowledge base entries with multilingual support (English/Hindi), enhanced NLU pipeline with fallback handling, entity extraction and slot filling, and improved response generation with clear disclaimers.

### Features

- **Expanded Health Knowledge Base**: Structured KB with 8-10 common health issues, including self-care tips, red flags, and disclaimers
- **Multilingual Support**: Full English and Hindi support for knowledge base entries and responses
- **Enhanced NLU Pipeline**: Improved intent and entity recognition with FallbackClassifier for safety
- **Entity Extraction & Slot Filling**: Automatic extraction of symptoms, severity, duration, and body parts
- **Structured Response Generation**: Formatted responses with self-care tips, red flags, and disclaimers
- **User Language Preferences**: Automatic language detection from user profile
- **Knowledge Base Service**: Reusable KB helper for both Flask and Rasa actions
- **Production-Ready**: Clean integration with error handling and testing

### What Changed

#### 1. Expanded Health Knowledge Base

The `conversation/health_kb.json` file now contains structured entries for:
- headache
- fever
- cold
- sore_throat
- cough
- stomach_pain
- nausea
- burn (first aid)
- cut (first aid)
- muscle_pain

Each entry includes:
- `category`: "symptom" or "first_aid"
- `common_causes`: List of common causes
- `self_care`: List of self-care tips
- `red_flags`: List of warning signs requiring immediate medical attention
- `disclaimer`: Medical disclaimer in English
- `hi`: Hindi translations for self-care, red flags, and disclaimer

#### 2. Knowledge Base Service

Created `conversation/kb_service.py` and `rasa_bot/actions/kb_service.py` with `HealthKB` class:
- `get_entry(key)`: Get raw entry from KB
- `get_advice(key, language)`: Get structured advice in specified language
- `list_all_keys()`: List all KB keys
- `search(query)`: Search for entries matching query

#### 3. Enhanced Rasa NLU Pipeline

Updated `rasa_bot/config.yml`:
- Added `FallbackClassifier` with threshold 0.4 to avoid overconfident misclassifications
- Tuned DIETClassifier with learning_rate and weight_sparsity
- Optimized epochs for better performance

#### 4. Enhanced Domain

Updated `rasa_bot/domain.yml`:
- Added `language` entity and slot for multilingual support
- Added `symptom_assessment_form` for structured symptom queries
- Added response templates: `utter_ask_symptom`, `utter_ask_symptom_severity`, `utter_ask_symptom_duration`
- Added `utter_disclaimer` and `utter_disclaimer_hi` for medical disclaimers
- Registered `action_provide_symptom_advice` action

#### 5. Enriched Training Data

Updated `rasa_bot/data/nlu.yml`:
- Added 15-20 English examples per intent
- Added 5-8 Hindi examples per health-related intent
- Properly annotated entities (symptom, body_part, severity, duration)
- Added ambiguous examples for robust fallback handling

#### 6. Updated Stories & Rules

Updated `rasa_bot/data/stories.yml` and `rasa_bot/data/rules.yml`:
- Added stories for symptom queries with entity extraction
- Added stories for form-based symptom assessment
- Added rules for form activation and fallback handling

#### 7. Custom Action with KB Integration

Updated `rasa_bot/actions/actions.py`:
- Implemented `ActionProvideSymptomAdvice` action
- Uses `HealthKB` service to fetch structured advice
- Supports language detection from metadata or slot
- Builds formatted responses with self-care tips, red flags, and disclaimers
- Handles missing KB entries gracefully

#### 8. Flask Integration Enhancements

Updated `conversation/rasa_client.py` and `conversation/routes.py`:
- Added `metadata` parameter to `send_message()` method
- Automatically sends user's `preferred_language` from profile to Rasa
- Rasa actions use metadata to determine response language

### Setup Instructions

#### 1. Train the Rasa Model

After updating the knowledge base and NLU data, retrain the model:

```bash
cd rasa_bot
rasa train
```

This will:
- Process the enriched NLU training data
- Train the enhanced pipeline with FallbackClassifier
- Generate a new model in `models/` directory

#### 2. Run Rasa Server

Start the Rasa server with the new model:

```bash
cd rasa_bot
rasa run --enable-api --cors "*" --port 5005
```

#### 3. Run Rasa Actions Server

Start the actions server (required for custom actions):

```bash
cd rasa_bot
rasa run actions --port 5055
```

#### 4. Run Flask Application

In your main terminal:

```bash
flask --app app:create_app run
```

### Project Structure (Module 3)

```
global_wellness_auth/
├── conversation/
│   ├── health_kb.json          # EXPANDED: Structured KB with 8-10 entries
│   └── kb_service.py           # NEW: KB helper for Flask
├── rasa_bot/
│   ├── config.yml              # UPDATED: Enhanced pipeline with FallbackClassifier
│   ├── domain.yml              # UPDATED: Forms, language slot, enhanced responses
│   ├── data/
│   │   ├── nlu.yml             # UPDATED: Enriched examples (15-20 English, 5-8 Hindi)
│   │   ├── stories.yml         # UPDATED: Form flows and symptom assessment
│   │   └── rules.yml           # UPDATED: Form activation and fallback rules
│   └── actions/
│       ├── kb_service.py       # NEW: KB helper for Rasa actions
│       └── actions.py          # UPDATED: ActionProvideSymptomAdvice with KB
└── tests/
    └── test_kb_service.py      # NEW: Tests for KB helper
```

### Example Conversations

#### Symptom Query (English)

**User:** "I have a mild headache for two days, what should I do?"

**Bot:**
```
**Self-Care Tips:**
1. Rest in a quiet, dark room
2. Drink water to stay hydrated
3. Avoid screen time for a while
4. Practice gentle neck and shoulder stretches
5. Apply a cold or warm compress to your forehead
6. Consider over-the-counter pain relief if appropriate

**⚠️ Red Flags - Seek Immediate Medical Attention If:**
1. sudden, severe headache
2. headache after head injury
3. confusion, trouble speaking, or vision changes
4. neck stiffness with fever
5. headache that wakes you from sleep
6. persistent headache that doesn't improve

This is general wellness information, not medical advice. Please consult a doctor for diagnosis or if symptoms are serious.
```

#### Symptom Query (Hindi)

**User:** "मुझे दो दिनों से हल्का बुखार है, क्या करूँ?"

**Bot:**
```
**स्व-देखभाल सुझाव:**
1. आराम करें और पर्याप्त नींद लें
2. बहुत सारे तरल पदार्थ पिएँ (पानी, चाय, शोरबा)
3. हल्के कपड़े पहनें
4. ठंडा कंप्रेस लगाएँ या गुनगुने पानी से नहाएँ
5. यदि उचित हो तो बुखार कम करने वाली दवा पर विचार करें
6. अपने तापमान की नियमित रूप से निगरानी करें

**⚠️ चेतावनी - तुरंत डॉक्टर से संपर्क करें यदि:**
1. 103°F (39.4°C) से अधिक बुखार
2. 3 दिन से अधिक समय तक बुखार
3. गंभीर सिरदर्द या गर्दन में अकड़न
4. साँस लेने में कठिनाई
5. गंभीर निर्जलीकरण के लक्षण
6. 3 महीने से कम उम्र के शिशुओं में बुखार

यह केवल सामान्य जानकारी है, डॉक्टर की सलाह नहीं। किसी भी गंभीर लक्षण पर तुरंत डॉक्टर से संपर्क करें।
```

### Running Tests

```bash
# Test KB service
pytest tests/test_kb_service.py -v

# Test conversation API
pytest tests/test_conversation_api.py -v

# Test all modules
pytest tests/ -v

# Test Rasa NLU (optional)
cd rasa_bot
rasa test nlu
```

### Knowledge Base Structure

Each KB entry follows this structure:

```json
{
  "symptom_name": {
    "category": "symptom",
    "common_causes": [
      "cause 1",
      "cause 2"
    ],
    "self_care": [
      "tip 1",
      "tip 2"
    ],
    "red_flags": [
      "warning 1",
      "warning 2"
    ],
    "disclaimer": "English disclaimer",
    "hi": {
      "self_care": [
        "Hindi tip 1",
        "Hindi tip 2"
      ],
      "red_flags": [
        "Hindi warning 1",
        "Hindi warning 2"
      ],
      "disclaimer": "Hindi disclaimer"
    }
  }
}
```

### Adding New KB Entries

To add a new symptom or ailment:

1. Edit `conversation/health_kb.json`
2. Add a new entry with the structure above
3. Include English and Hindi translations
4. Retrain Rasa model: `cd rasa_bot && rasa train`
5. Restart Rasa server and actions server

### Safety & Disclaimers

⚠️ **Important**: The chatbot provides general wellness information only. It is **NOT** a substitute for professional medical advice, diagnosis, or treatment.

**Key Safety Features:**
- Clear disclaimers in every response
- Red flag warnings for serious symptoms
- Non-diagnostic language
- Encourages consulting healthcare providers
- FallbackClassifier prevents overconfident misclassifications

**Always:**
- Consult healthcare providers for medical concerns
- Seek emergency services for medical emergencies
- Use the chatbot for informational purposes only
- Do not rely on the chatbot for diagnosis or treatment decisions

### Troubleshooting

#### KB Entries Not Found

1. Check that the symptom/ailment key matches exactly (case-insensitive)
2. Verify the KB entry exists in `conversation/health_kb.json`
3. Check that the KB service can access the file
4. Restart the Rasa actions server after updating KB

#### Language Not Switching

1. Verify user's `preferred_language` is set in profile
2. Check that metadata is being sent from Flask to Rasa
3. Verify Rasa action is reading metadata correctly
4. Check that Hindi translations exist in KB entry

#### FallbackClassifier Issues

1. Check that FallbackClassifier is in the pipeline
2. Verify threshold settings (0.4 is default)
3. Ensure fallback intent has sufficient examples
4. Test with ambiguous queries to verify fallback works

### Performance Notes

- **NLU Performance**: DIETClassifier with 100 epochs provides robust intent/entity recognition
- **Fallback Handling**: FallbackClassifier threshold of 0.4 balances accuracy and safety
- **KB Lookup**: KB service uses simple dictionary lookup (fast, O(1))
- **Language Detection**: Language is determined from user profile or defaults to English

### Next Steps

Future enhancements may include:
- More KB entries (20+ common symptoms and first aid scenarios)
- Integration with external health APIs
- Conversation history and context tracking
- Personalized recommendations based on user profile and age group
- Advanced NLU with transformer models (BERT, multilingual BERT)
- Multi-turn symptom assessment forms
- Integration with telemedicine platforms

---

## Module 4: Admin Dashboard & System Refinement

### Overview

Module 4 adds comprehensive admin functionality for managing the wellness assistant, including knowledge base management, user feedback review, and analytics. It also implements conversation logging and user feedback mechanisms to improve the system over time.

### Features

- **Admin Dashboard**: Analytics dashboard with summary statistics and charts
- **Knowledge Base Management**: Web UI for creating, editing, and deleting KB entries
- **Feedback System**: User feedback collection and review interface
- **Conversation Logging**: Automatic logging of all user-bot interactions
- **Analytics**: Statistics on conversations, symptoms, and user feedback
- **Access Control**: Admin-only routes with JWT-based authentication

### What Changed

#### Database Models (`models.py`)

- Added `is_admin` field to `User` model
- Added `ConversationLog` model for logging conversations:
  - `user_id`, `session_id`, `user_message`, `bot_response`
  - `intent`, `symptom` (optional metadata)
  - `created_at` timestamp
- Added `Feedback` model for user feedback:
  - `user_id`, `session_id`, `rating` (1-5), `comment`
  - `created_at` timestamp

#### Conversation Routes (`conversation/routes.py`)

- Updated `POST /conversation/api/message`:
  - Accepts optional `session_id` in request body
  - Returns `session_id` in response (generates UUID if not provided)
  - Logs all conversations to `ConversationLog` table
  - Returns `logged` status in response
- Added `POST /conversation/api/feedback`:
  - Accepts `session_id`, `rating` (1-5), optional `comment`
  - Creates `Feedback` entry linked to session
  - Works for both authenticated and anonymous users

#### Chat UI (`templates/conversation/chat.html`, `static/js/chat.js`)

- Added session management:
  - Stores `session_id` in localStorage
  - Sends `session_id` with each message request
  - Reuses session across page refreshes
- Added feedback UI:
  - Rating buttons (1-5) appear after each bot response
  - Optional comment field for detailed feedback
  - Thank you message after submission

#### Admin Blueprint (`admin/`)

- **`admin/__init__.py`**: Blueprint initialization
- **`admin/routes.py`**: Admin routes with `@admin_required` decorator:
  - `GET /admin/` - Dashboard
  - `GET /admin/kb` - KB list
  - `GET/POST /admin/kb/<key>/edit` - Edit KB entry
  - `GET/POST /admin/kb/create` - Create KB entry
  - `POST /admin/kb/<key>/delete` - Delete KB entry
  - `GET /admin/feedback` - Feedback list
  - `GET /admin/api/stats/summary` - Summary statistics API
  - `GET /admin/api/stats/conversations_by_day` - Conversations chart data
  - `GET /admin/api/stats/top_symptoms` - Top symptoms chart data
- **`admin/services.py`**: Analytics helper functions:
  - `get_summary_stats()` - Overall statistics
  - `get_conversations_by_day()` - Time series data
  - `get_top_symptoms()` - Most common symptoms
  - `get_recent_feedback()` - Recent feedback entries

#### Admin Templates (`templates/admin/`)

- **`dashboard.html`**: Main admin dashboard with:
  - Summary cards (conversations, feedback, ratings, symptoms)
  - Line chart for conversations over time
  - Bar chart for top symptoms
  - Quick links to KB and feedback management
- **`kb_list.html`**: Table of all KB entries with edit/delete actions
- **`kb_edit.html`**: Form for editing KB entries (English + Hindi)
- **`kb_create.html`**: Form for creating new KB entries
- **`feedback_list.html`**: Table of user feedback with ratings and comments

#### Knowledge Base Service (`conversation/kb_service.py`)

- Added `save_entry(key, entry_data)` method:
  - Updates or creates KB entry
  - Creates backup before saving
  - Writes with UTF-8 encoding and pretty formatting
- Added `delete_entry(key)` method:
  - Removes entry from KB
  - Creates backup before deletion

#### Admin Charts (`static/js/admin_charts.js`)

- Uses Chart.js for visualization
- Fetches data from admin API endpoints
- Handles authentication via JWT tokens
- Displays conversations over time (line chart)
- Displays top symptoms (bar chart)

#### Tests (`tests/test_admin_views.py`)

- Tests for admin access control
- Tests for KB management endpoints
- Tests for feedback API
- Tests for conversation logging
- Tests for analytics endpoints

### Setup Instructions

1. **Update Database Schema**:
   ```bash
   flask --app app:create_app create-db
   ```
   This will create the new `conversation_logs` and `feedback` tables, and add the `is_admin` column to `users`.

2. **Create Admin User**:
   You need to manually set `is_admin=True` for at least one user. You can do this via Python:
   ```python
   from app import create_app
   from models import User
   from extensions import db
   
   app = create_app()
   with app.app_context():
       user = User.query.filter_by(email='your-email@example.com').first()
       if user:
           user.is_admin = True
           db.session.commit()
           print(f"User {user.email} is now an admin")
   ```

3. **Access Admin Dashboard**:
   - Login as an admin user
   - Navigate to `/admin/` or `/admin/dashboard`
   - You should see the admin dashboard with statistics and charts

### Usage

#### Admin Dashboard

1. **View Statistics**:
   - Total conversations, feedback count, average rating, distinct symptoms
   - Charts showing conversations over time and top symptoms

2. **Manage Knowledge Base**:
   - Click "Manage Knowledge Base" to view all entries
   - Click "Edit" to modify an entry
   - Click "Create New Entry" to add a new symptom/ailment
   - Click "Delete" to remove an entry (with confirmation)

3. **Review Feedback**:
   - Click "View Feedback" to see user ratings and comments
   - Use feedback to identify areas for improvement

#### User Feedback

1. **In Chat Interface**:
   - After each bot response, rating buttons (1-5) appear
   - Click a rating to submit quick feedback
   - Optionally add a comment for detailed feedback
   - Thank you message confirms submission

2. **Feedback is Anonymous**:
   - Works for both logged-in and anonymous users
   - Linked to session ID for tracking

#### Conversation Logging

- All conversations are automatically logged
- Logs include user message, bot response, session ID, and optional metadata
- Logs can be used for analytics and improving responses

### API Endpoints

#### Conversation API

- `POST /conversation/api/message`
  - Body: `{ "message": "...", "session_id": "..." }` (session_id optional)
  - Returns: `{ "session_id": "...", "messages": [...], "logged": true }`

- `POST /conversation/api/feedback`
  - Body: `{ "session_id": "...", "rating": 1-5, "comment": "..." }`
  - Returns: `{ "success": true, "message": "..." }`

#### Admin API

- `GET /admin/api/stats/summary`
  - Returns: `{ "total_conversations": ..., "total_feedback": ..., "average_rating": ..., "distinct_symptoms": ... }`

- `GET /admin/api/stats/conversations_by_day?days=30`
  - Returns: `[{ "date": "...", "count": ... }, ...]`

- `GET /admin/api/stats/top_symptoms?limit=5`
  - Returns: `[{ "symptom": "...", "count": ... }, ...]`

### Security

- **Admin Access Control**: All admin routes require:
  - Valid JWT token
  - User with `is_admin=True`
  - Returns 403 Forbidden for non-admin users

- **KB File Safety**:
  - Backup created before each save/delete operation
  - Atomic write operations to prevent corruption
  - UTF-8 encoding preserved for Hindi text

### Testing

Run admin tests:
```bash
pytest tests/test_admin_views.py -v
```

Test coverage includes:
- Admin access control (403 for non-admins)
- KB management (create, edit, delete)
- Feedback submission and validation
- Analytics API endpoints
- Conversation logging

### Troubleshooting

#### Admin Dashboard Not Accessible

1. Verify user has `is_admin=True` in database
2. Check JWT token is valid and included in requests
3. Verify admin blueprint is registered in `app.py`

#### KB Changes Not Reflecting

1. Check that KB file has write permissions
2. Verify backup file was created (`.json.bak`)
3. Restart Rasa actions server after KB changes
4. Check application logs for save errors

#### Charts Not Loading

1. Verify Chart.js is loaded (check browser console)
2. Check that API endpoints return valid JSON
3. Verify JWT token is included in API requests
4. Check browser network tab for API errors

#### Feedback Not Saving

1. Verify `session_id` is provided in request
2. Check that rating is between 1-5
3. Check database connection and permissions
4. Review application logs for errors

### Next Steps

Future enhancements for Module 4:
- Export conversation logs and feedback to CSV/JSON
- Advanced analytics (sentiment analysis, response time metrics)
- KB entry versioning and change history
- Bulk KB import/export functionality
- User management interface (promote to admin, view user stats)
- Automated feedback analysis and insights

---

## License

This is part of the Global Wellness Assistant Chatbot project.

