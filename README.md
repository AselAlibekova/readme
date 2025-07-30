# 🚀 FastAPI Project Documentation

This is a modular, production-ready backend system built with FastAPI. The architecture is designed for maintainability and scalability, supporting features such as user authentication, bot integration, broadcasting, chat management, and asynchronous processing through Celery.

---

## Table of Contents

1. Overview  
2. Technology Stack  
3. Full Project Structure  
4. Installation  
5. Environment Configuration  
6. Application Startup  
7. Module-by-Module Breakdown  
8. Celery Tasks  
9. Database and Alembic  
10. Docker  
11. License

---

## 1. Overview

The backend handles API requests, processes user and bot interactions, manages broadcasts and conversations, and offloads background jobs using Celery. It is organized by domain to make the codebase clean and extendable for new developers or contributors.

---

## 2. Technology Stack

- Python 3.10+
- FastAPI
- SQLAlchemy
- Pydantic
- Alembic
- Celery
- Redis
- Docker, Docker Compose

---

## 3. Full Project Structure

```
fastapi-project
├── src
│   ├── auth
│   │   ├── router.py
│   │   ├── schemas.py
│   │   ├── models.py
│   │   ├── dependencies.py
│   │   ├── config.py
│   │   ├── constants.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── bot
│   │   ├── client.py  # client model for external service communication
│   │   ├── schemas.py
│   │   ├── models.py
│   │   ├── router.py
│   │   ├── service.py
│   │   └── utils.py
│   └── broadcast
│   │   ├── client.py  # client model for external service communication
│   │   ├── router.py
│   │   ├── schemas.py
│   │   ├── models.py
│   │   ├── service.py
│   │   ├── tasks.py
│   │   └── utils.py
│   └── chat
│   │   ├── client.py  # client model for external service communication
│   │   ├── router.py
│   │   ├── schemas.py
│   │   ├── models.py
│   │   ├── constants.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── celery_config.py
│   ├── config.py  # global configs
│   ├── models.py  # global models
│   ├── database.py  # db connection related stuff
│   └── main.py
├── .env
├── .gitignore
├── .dockerignore
├── docker-compose.yml
├── dockerfile
├── requirements.txt
├── README.md
└── alembic.ini
```

---

## 4. Installation

```bash
git clone https://your-repo-url.git
cd fastapi-project
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## 5. Environment Configuration

Create a `.env` file in the root directory with:

```
DATABASE_URL=postgresql://user:password@localhost/dbname
SECRET_KEY=your-secret-key
REDIS_URL=redis://localhost:6379/0
```

---

## 6. Application Startup

### Development

```bash
uvicorn src.main:app --reload
```

### Production

```bash
uvicorn src.main:app --host 0.0.0.0 --port 8000
```

---

## 7. 📂 Module-by-Module Breakdown

### `src/main.py`  
**Purpose**: Entry point of the FastAPI application.  
**Details**:
- Initializes FastAPI app instance  
- Registers routers from all internal modules  
- Applies CORS, middleware, exception handlers

### `src/config.py`  
**Purpose**: Application-wide configuration  
**Details**:
- Uses `pydantic.BaseSettings` to read from `.env`  
- Stores secrets, token lifetimes, CORS origins, DB URL, etc.

### `src/database.py`  
**Purpose**: SQLAlchemy database engine and session factory  
**Details**:
- Sets up `engine`, `SessionLocal`  
- Provides `get_db()` dependency for safe session usage

### `src/models.py`  
**Purpose**: Shared abstract SQLAlchemy models  
**Details**:
- Base model and reusable mixins (e.g., `TimestampMixin`, `UUIDMixin`)  
- Used by models in `auth`, `chat`, etc.

### `src/celery_config.py`  
**Purpose**: Celery worker setup  
**Details**:
- Defines Celery app, broker URL  
- Prepares module for async task execution

---

### 📁 `src/auth/` — User Authentication & Authorization

- `router.py`: API routes — login, signup, token refresh  
- `schemas.py`: User input validation (signup, login, etc.)  
- `models.py`: SQLAlchemy model for `User`, credentials  
- `dependencies.py`: `get_current_user()`, role verification  
- `config.py`: Auth-specific constants (token expiry)  
- `constants.py`: Role definitions (`admin`, `user`)  
- `service.py`: Core login/signup logic, hashing, token creation  
- `utils.py`: Password hashing, JWT encode/decode

---

### 📁 `src/bot/` — Integration with External Messaging Bots

- `client.py`: Makes outbound API calls to external bots  
- `router.py`: Webhook routes from bots  
- `schemas.py`: Message/request structures  
- `models.py`: Bot logs, metadata  
- `service.py`: Bot message handler, AI decision-making  
- `utils.py`: Format message text, detect language, etc.

---

### 📁 `src/broadcast/` — Campaign-Based Mass Messaging

- `client.py`: Sends messages (Telegram, etc.)  
- `router.py`: Endpoints for campaign management  
- `schemas.py`: Input for creating/updating campaigns  
- `models.py`: Campaign, MessageLog, Recipient  
- `service.py`: Core business logic for campaign control  
- `tasks.py`: Celery background task for async dispatch  
- `utils.py`: Helpers — schedule parsing, template fill

---

### 📁 `src/chat/` — Chat Conversation and AI Processing

- `client.py`: (Optional) integration with CRM or NLP service  
- `router.py`: Chat API routes — send/receive messages  
- `schemas.py`: Pydantic chat structures  
- `models.py`: Chat threads/messages in DB  
- `constants.py`: Message statuses, predefined tags  
- `service.py`: Save/query/filter messages; trigger actions  
- `utils.py`: Text preprocessing, tag detection, formatting
---

## 8. 📡 API Endpoints by Module

###  `src/auth/router.py`

Handles all authentication-related actions.

- `POST /auth/register`
  - Registers a new user.
  - Request body: `UserCreate`
  - Response: Success message or error

- `POST /auth/login`
  - Authenticates user and returns JWT.
  - Request body: `UserLogin`
  - Response: `TokenResponse`

- `GET /auth/me`
  - Retrieves details of the currently authenticated user.
  - Headers: `Authorization: Bearer <token>`
  - Response: `UserResponse`

---

###  `src/bot/router.py`

Handles bot messages and commands, often from external services.

- `POST /bot/webhook`
  - Receives webhook from messaging platform (e.g. Telegram).
  - Request body: `BotMessagePayload`
  - Response: Success message

- `GET /bot/status`
  - Health check for the bot service.
  - Response: JSON with service status

---

###  `src/broadcast/router.py`

Used for managing message campaigns and sending to multiple recipients.

- `POST /broadcast/campaign`
  - Creates a new campaign.
  - Body: `CampaignCreate`
  - Response: Campaign ID

- `POST /broadcast/campaign/{campaign_id}/start`
  - Starts sending a campaign via Celery tasks.
  - Path: `campaign_id`
  - Response: Task status

- `GET /broadcast/logs`
  - Retrieves logs of past broadcast deliveries.
  - Response: List of delivery entries

- `DELETE /broadcast/campaign/{campaign_id}`
  - Deletes an existing campaign.
  - Response: Success/failure

---

###  `src/chat/router.py`

Handles user messages and interaction history.

- `POST /chat/send`
  - Stores and processes a user message.
  - Request body: `ChatMessageRequest`
  - Response: Message ID or response from AI

- `GET /chat/history`
  - Retrieves full chat history for a session.
  - Query params: `user_id`, `limit`, `offset`
  - Response: List of messages

- `GET /chat/threads`
  - Lists all active or archived threads by user.
  - Response: Thread list

- `GET /chat/message/{message_id}`
  - Retrieves a single message by ID.
  - Response: Message object

---

## 9. Celery Tasks

Start the Celery worker:

```bash
celery -A src.broadcast.tasks worker --loglevel=info
```

Used for background job execution, especially for broadcasting tasks.

---

## 10. Database and Alembic

Initialize migrations and upgrade the database:

```bash
alembic revision --autogenerate -m "Initial migration"
alembic upgrade head
```

Alembic is configured via `alembic.ini`.

---

## 11. Docker

Build and run the services using Docker:

```bash
docker-compose up --build
```

This will launch:
- FastAPI server
- PostgreSQL database
- Redis (for Celery)
- Celery worker 

---

## 12. License

This project is proprietary and confidential. Unauthorized use or distribution is prohibited.
