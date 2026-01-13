# Project Documentation: Dynamic Intent Chatbot with RAG

## 1. Overview

This project is a **Streamlit-based Chatbot Application** that integrates **Retrieval-Augmented Generation (RAG)**, **Intent Recognition**, and **Voice Interaction**. It is designed to handle customer support scenarios (e.g., order tracking, account management) as well as general information retrieval via Wikipedia.

While the project's README mentions a fine-tuned BART model, the current implementation (`app.py`) primarily leverages **Google's Gemini Pro** API for natural language generation, enhanced by a local RAG system using conversation history and Wikipedia.

## 2. System Architecture

The application is built as a monolithic Streamlit app with the following components:

### 2.1 Frontend

- **Framework:** [Streamlit](https://streamlit.io/)
- **Interface:**
  - Sidebar for Authentication (Login/Register) and Thread Management.
  - Main chat interface with text input and voice recording button.
  - Dynamic display of user profile and conversation history.

### 2.2 Backend Logic

- **Language Model:** Google Gemini Pro (`models/gemini-pro`) via `google-generativeai`.
- **Intent Recognition:** Hybrid approach:
  - **Regex-based:** For specific actions like `track_order`, `create_account`, `search`.
  - **LLM-based:** Fallback for general queries.
- **RAG (Retrieval-Augmented Generation):**
  - **External Knowledge:** Wikipedia API for "search" intents.
  - **Contextual Memory:** TF-IDF Vectorization and Cosine Similarity (`scikit-learn`) to retrieve relevant past conversation turns.
- **Voice Processing:**
  - **Input:** `speech_recognition` (Google Web Speech API).
  - **Output:** `pyttsx3` for Text-to-Speech (TTS).

### 2.3 Database

- **System:** SQLite (`chatbot.db`)
- **ORM:** Raw SQL queries via Python's `sqlite3` module.
- **Security:** Passwords are hashed using `bcrypt`.

## 3. Database Schema

The application uses a local SQLite database with two main tables:

### `Users` Table

Stores user authentication and profile details.
| Column | Type | Description |
|--------|------|-------------|
| `user_id` | INTEGER (PK) | Unique identifier. |
| `name` | TEXT | User's full name. |
| `email` | TEXT | User's email address. |
| `phone` | TEXT | User's phone number. |
| `username` | TEXT (Unique) | Login username. |
| `password` | TEXT | Hashed password. |
| `preferences` | TEXT | JSON string for user preferences. |
| `last_updated` | TEXT | Timestamp of last profile update. |
| `reset_code` | TEXT | Temporary code for password reset. |

### `ConversationHistory` Table

Stores chat logs for memory and RAG.
| Column | Type | Description |
|--------|------|-------------|
| `conversation_id` | INTEGER (PK) | Unique identifier. |
| `user_id` | INTEGER (FK) | Links to `Users` table. |
| `thread_id` | INTEGER | Groups messages into threads. |
| `intent` | TEXT | Detected intent of the user message. |
| `instruction` | TEXT | User's input message. |
| `response` | TEXT | Bot's generated response. |
| `timestamp` | TEXT | Time of the interaction. |

## 4. Key Features & Modules

### 4.1 Authentication

- **Login/Logout:** Secure session management.
- **Registration:** New users can create accounts; data is validated and stored.
- **Password Reset:** Simulation of a reset flow using a generated code.

### 4.2 Intent Parsing (`parse_instruction`)

The system uses Regular Expressions to extract structured data from user inputs before sending them to the LLM.

- **Supported Intents:** `payment_issue`, `place_order`, `track_order`, `create_account`, `search`, etc.
- **Entity Extraction:** Extracts order numbers, names, emails, and search queries directly from the text.

### 4.3 Contextual Memory (`build_prompt`)

To maintain context without exceeding token limits, the app retrieves relevant past interactions:

1.  **Vectorization:** Converts current instruction and past history into TF-IDF vectors.
2.  **Similarity Search:** Calculates Cosine Similarity to find the top 3 most relevant past exchanges.
3.  **Prompt Engineering:** Injects these relevant exchanges and user profile data into the system prompt for Gemini.

### 4.4 Wikipedia Integration

If the user asks to "search" or "find info about" a topic:

1.  The intent is detected as `search`.
2.  The `wikipedia` library fetches a summary.
3.  The summary is presented directly or summarized further if too long.

## 5. Setup & Installation

### Prerequisites

- Python 3.8+
- A Google Gemini API Key

### Installation Steps

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/hadumon/Finetuned_chatbot.git
    cd Finetuned_chatbot
    ```

2.  **Install dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

3.  **Environment Configuration:**
    Create a `.env` file in the root directory and add your API key:

    ```env
    GEMINI_API_KEY=your_actual_api_key_here
    ```

4.  **Run the Application:**
    ```bash
    streamlit run app.py
    ```

## 6. Usage Guide

1.  **Start the App:** Open the URL provided by Streamlit (usually `http://localhost:8501`).
2.  **Login/Register:** Use the sidebar to create an account or log in.
3.  **Chat:**
    - **Type:** Enter text in the "Instruction" box and click "Generate Response".
    - **Speak:** Click "Record Instruction" to use your microphone.
    - **Intents:** Select a specific intent from the dropdown or let the system auto-detect it.
4.  **Manage Threads:** Create new conversation threads or switch between existing ones using the sidebar.

## 7. Troubleshooting

- **Voice Input Fails:** Ensure your microphone is accessible and you are not in a noisy environment. The `speech_recognition` library relies on Google's web API.
- **Database Errors:** If you encounter schema errors, try deleting `chatbot.db` to let the app recreate it fresh (warning: this deletes all data).
- **API Errors:** Ensure your `GEMINI_API_KEY` is valid and has quota available.
