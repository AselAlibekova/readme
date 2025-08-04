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
src/
├── core/
│   ├── __init__.py
│   ├── celery_config.py
│   ├── config.py
│   ├── crudbase.py
│   ├── database.py
│   ├── dependencies.py
│   └── logger.py
│
├── integrations/
│   ├── instagram/
│   │   ├── __init__.py
│   │   ├── client.py
│   │   ├── crud.py
│   │   ├── deleted_data_crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── openai_service/
│   │   ├── __init__.py
│   │   ├── client.py
│   │   ├── dependencies.py
│   │   └── service.py
│   ├── telegram/
│   │   ├── __init__.py
│   │   ├── client.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── whatsapp/
│   │   ├── client.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   ├── service.py
│   │   └── utils.py
│   └── whatsapp_web/
│       ├── __init__.py
│       ├── client.py
│       ├── crud.py
│       ├── dependencies.py
│       ├── schemas.py
│       └── service.py
│
├── modules/
│   ├── admin/
│   │   ├── __init__.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── analytics/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── assistant/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── auth/
│   │   ├── __init__.py
│   │   ├── dependencies.py
│   │   ├── jwt_service.py
│   │   ├── password_manager.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── broadcast/
│   │   ├── __init__.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   ├── service.py
│   │   └── utils.py
│   ├── chat/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── company/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── dialogue/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── email/
│   │   ├── templates/
│   │   │   ├── img/
│   │   │   │   ├── black_ru_logo_pradavan_1700x500.png
│   │   │   │   ├── gis.png
│   │   │   │   ├── img3.png
│   │   │   │   ├── inst.png
│   │   │   │   ├── mailing_end.png
│   │   │   │   ├── password_reset.png
│   │   │   │   └── wp.png
│   │   │   ├── bot_block.html
│   │   │   ├── bot_limit.html
│   │   │   ├── broadcast_results.html
│   │   │   ├── client_left_contact.html
│   │   │   ├── profile_create.html
│   │   │   └── update_profile.html
│   ├── leads/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── messages/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── platform_user/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   ├── profile/
│   │   ├── __init__.py
│   │   ├── dependencies.py
│   │   └── service.py
│   ├── request_callback/
│   │   ├── __init__.py
│   │   ├── crud.py
│   │   ├── dependencies.py
│   │   ├── schemas.py
│   │   └── service.py
│   └── templates/
│       ├── __init__.py
│       ├── crud.py
│       ├── dependencies.py
│       ├── schemas.py
│       └── service.py
│
├── routers/
│   └── v1/
│       ├── __init__.py
│       ├── admin_router.py
│       ├── analytics_router.py
│       ├── assistant_router.py
│       ├── auth_router.py
│       ├── broadcast_router.py
│       ├── chat_router.py
│       ├── files_router.py
│       ├── follow_up_router.py
│       ├── instagram_router.py
│       ├── leads_router.py
│       ├── profile_router.py
│       ├── request_callback_router.py
│       ├── system_router.py
│       ├── telegram_router.py
│       ├── whatsapp_router.py
│       └── whatsapp_web_router.py
│
├── services/
│   ├── broker/
│   │   ├── __init__.py
│   │   ├── dependencies.py
│   │   ├── interface.py
│   │   └── service.py
│   └── files_manager/
│       ├── __init__.py
│       ├── client.py
│       ├── dependencies.py
│       ├── schemas.py
│       ├── service.py
│       └── utils.py
│
├── redis/
│   ├── __init__.py
│   ├── dependencies.py
│   └── service.py
│
├── scheduler/
│   ├── __init__.py
│   ├── config.py
│   └── tasks.py
│
├── __init__.py
├── main.py
├── models.py
├── router.py
└── schemas.py
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

Вот пример **детального и современного описания модуля `src/`** — полностью по твоему проекту, в правильном стиле (на английском, как ты просила):

---

## 7. 📂 Module-by-Module Breakdown

---

### `src/main.py`

**Purpose**: Entry point for the FastAPI application.
**Details**:

* Initializes FastAPI app instance
* Loads configuration and logger
* Handles application lifespan: connects/disconnects to message broker and database
* Registers all API routers (via `src/router.py`)
* Adds CORS middleware using allowed hosts from config

---

### `src/models.py`

**Purpose**: Shared SQLAlchemy data models for the platform.
**Details**:

* Defines all major database tables (ORM models) used by modules
* Contains tables for WhatsApp, Telegram, Instagram, Company, PlatformUsers, Assistant, Integrations, Dialogue, Messages, Leads, Tariff, etc.
* Models use relationships for foreign keys and object associations
* Example models:

  * `WhatsAppTable`, `TelegramTable`, `InstagramTable`, `WhatsAppWebTable` — service integrations
  * `CompanyTable` — organization info and relationships
  * `PlatformUsersTable` — platform users and credentials
  * `AssistantTable` — AI assistants
  * `AssistantIntegrationsTable` — mapping assistants to integrations
  * `DialogueTable` — client-assistant chat sessions
  * `MessagesTable` — chat message history
  * `LeadsTable` — sales leads
  * `TariffTable` — subscription/plans
  * `FaasServicesTable` — serverless (faas) integrations
  * `RequestCallbackTable` — callback requests from users

---

### `src/router.py`

**Purpose**: Central API router for the project.
**Details**:

* Imports routers for each business domain from `src.routers.v1.*`
* Combines all routers under `/api/v1/` prefix
* Registers routers for auth, chat, leads, admin, files, system, profile, telegram, whatsapp, instagram, broadcast, assistant, analytics, and others
* This is the only router imported into the main app, ensuring all endpoints are available under one hierarchy

---

### `src/schemas.py`

**Purpose**: Shared Pydantic schemas for API requests and responses.
**Details**:

* Defines generic response formats for consistent API design
* Example schemas:

  * `ListResponse` — a paginated list of items
  * `StatusResponse` — standard status and message reply

---

### `src/__init__.py`

**Purpose**: Marks the `src` directory as a Python package.
**Details**:

* No business logic — used for package/module structure only

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
