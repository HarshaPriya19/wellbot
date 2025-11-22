# 🧘 WellnessBot: Global Wellness Authentication & Chatbot

This project is a sophisticated Wellness Chatbot system built using **Rasa** for conversational AI and **Flask** for the backend API, user management, and authentication.

## 🌟 Features

* **Two-Factor Architecture:** Separates the web application (user authentication, profile, dashboard) from the core conversational AI logic.
* **Secure Authentication:** User registration, login, and secure token handling via Flask/SQLAlchemy.
* **Rasa Integration:** Communicates with the Rasa bot and Action servers over HTTP.
* **Multi-Language Ready:** Configured for easy adaptation and includes initial steps for Hindi language support.
* **SQLite Database:** Uses SQLite for local development (configurable to PostgreSQL or MySQL).

---

## 🛠️ Prerequisites

Before you begin, ensure you have the following installed on your system:

* **Python 3.10.x** (This version is confirmed to work with the specified Rasa dependencies).
* **Git** (For cloning the repository).
* **Windows Command Prompt (cmd)**

---

## 💻 Getting Started (Windows)

Follow these steps exactly in the Windows Command Prompt (`cmd`).

### 1. Setup (One-Time)

This assumes your project is located at `C:\Users\manig\WellnessBot-main\global_wellness_auth`.

| Command | Description |
| :--- | :--- |
| `cd C:\Users\manig\WellnessBot-main\global_wellness_auth` | Navigate to the project root. |
| `python -m venv .venv` | Create the virtual environment. |
| `.venv\Scripts\activate` | Activate the environment (prompt shows `(.venv)`). |
| `pip install --upgrade pip` | Update pip. |
| `pip install -r requirements.txt` | Install all dependencies (Flask, Rasa, etc.). |

### 2. Configuration & Initialization

| Command | Description |
| :--- | :--- |
| `copy .env.example .env` | Create the environment file. |
| **ACTION:** Manually edit the **`.env`** file to set **`SECRET_KEY`**, **`JWT_SECRET_KEY`**, **`SQLALCHEMY_DATABASE_URI=sqlite:///wellness.db`**, and **`RASA_BASE_URL=http://localhost:5005`**. |
| `flask --app app:create_app create-db` | Initialize the database schema. |
| `python create_admin_user.py` | (Optional) Create an admin user account (follow prompts). |

---

### 3. Running the Project (Three Windows Required)

The project requires three concurrent services to run. Keep all three Command Prompt windows open.

#### **Window 1: Flask Server (Main App)**

This window should already be in the project root with `(.venv)` active.

| Command | Description |
| :--- | :--- |
| `flask --app app:create_app run --debug` | Starts the main web application on port 5000. **Keep running.** |

*App is available at: **`http://localhost:5000`***

#### **Window 2: Rasa Model Server**

Open a **new Command Prompt window**.

| Command | Description |
| :--- | :--- |
| `cd C:\Users\manig\WellnessBot-main\global_wellness_auth\rasa_bot` | Navigate to the Rasa project folder. |
| `..\.venv\Scripts\activate` | Activate the virtual environment. |
| `rasa run --enable-api --cors "*" --port 5005` | Starts the Rasa chatbot engine. **Keep running.** |

*Wait for the server to confirm it is listening on **port 5005**.*

#### **Window 3: Rasa Actions Server**

Open a **third Command Prompt window**.

| Command | Description |
| :--- | :--- |
| `cd C:\Users\manig\WellnessBot-main\global_wellness_auth\rasa_bot` | Navigate to the Rasa project folder. |
| `..\.venv\Scripts\activate` | Activate the virtual environment. |
| `rasa run actions --port 5055` | Starts the server for custom Python actions. **Keep running.** |

*Wait for the server to confirm it is listening on **port 5055**.*

---

## 🛑 Stopping Services

Press **`Ctrl+C`** in all three active Command Prompt windows to gracefully shut down the Flask, Rasa, and Actions servers.

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




