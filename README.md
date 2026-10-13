# KrishiAI — AI-Powered Smart Farming Platform

> **KrishiAI** is an advanced agricultural intelligence and smart farm management platform engineered to support farmers across the full lifecycle:
> **PLAN → GROW → MONITOR → DIAGNOSE → PREDICT → MANAGE → HARVEST → SELL**

---

## 1. Project Overview

Agriculture is transforming through real-time intelligence, remote telemetry, and artificial intelligence. KrishiAI bridges modern agronomic science and cutting-edge software engineering to deliver actionable, accessible insights for modern farming.

**Part 1: Project Foundation** establishes the robust, decoupled production foundation upon which future agricultural AI models, computer vision diagnostic pipelines, and market systems are built.

---

## 2. Technology Stack

### Frontend
- **Framework**: [Next.js](https://nextjs.org/) (App Router, React 18 / 19)
- **Language**: TypeScript (Strict Mode enabled)
- **Styling**: Tailwind CSS with custom nature-inspired agricultural design tokens
- **Icons**: Lucide React
- **Design System**: Modular, accessible component architecture

### Backend
- **Framework**: [FastAPI](https://fastapi.tiangolo.com/) (Python 3.10+)
- **Validation**: Pydantic v2
- **ORM & Database**: SQLAlchemy 2.0 & PostgreSQL
- **Migrations**: Alembic
- **Server**: Uvicorn ASGI

### Infrastructure & Tooling
- **Containerization**: Docker & Docker Compose
- **Configuration**: Pydantic Settings & `.env` files

---

## 3. High-Level Architecture

```
+-----------------------------------------------------------+
|                   Client Browser (Web/Mobile)             |
+-----------------------------------------------------------+
                              |
                              | HTTP / Next.js SSR
                              v
+-----------------------------------------------------------+
|             Next.js Frontend (Port 3000)                  |
|    - TypeScript, Tailwind CSS, Reusable Design System     |
|    - Client-side API fetcher (frontend/lib/api.ts)        |
+-----------------------------------------------------------+
                              |
                              | REST API (CORS-restricted)
                              v
+-----------------------------------------------------------+
|              FastAPI Backend (Port 8000)                  |
|    - App Router, Global Exception Handling                |
|    - Pydantic v2 Schemas, SQLAlchemy 2.0 ORM Engine       |
+-----------------------------------------------------------+
                              |
                              | Connection Pool
                              v
+-----------------------------------------------------------+
|              PostgreSQL Database (Port 5432)              |
|    - Alembic Database Migrations                          |
+-----------------------------------------------------------+
```

---

## 4. Folder Structure

```
krishiai/
├── frontend/                     # Next.js App Router Frontend
│   ├── app/                      # Next.js App directory (layout, page)
│   ├── components/               # Modular UI Design System
│   │   ├── ui/                   # Button, Card, Badge, Input, Select, Modal, States
│   │   ├── layout/               # Navbar, Footer
│   │   └── sections/             # Hero, FeaturePreview, BackendStatus
│   ├── hooks/                    # Custom React hooks
│   ├── lib/                      # API client and utility helpers
│   ├── services/                 # Frontend service layer
│   ├── types/                    # Shared TypeScript interfaces
│   ├── public/                   # Static icons and assets
│   ├── Dockerfile                # Frontend production container
│   ├── package.json              # Node dependencies
│   ├── tailwind.config.ts        # Tailwind design tokens
│   └── tsconfig.json             # TypeScript configuration
│
├── backend/                      # FastAPI Application
│   ├── app/
│   │   ├── api/                  # API routers & endpoints
│   │   │   ├── v1/
│   │   │   │   ├── endpoints/    # Route handlers (health, root)
│   │   │   │   └── router.py     # Aggregated v1 API router
│   │   ├── core/                 # Core utilities
│   │   │   ├── config.py         # Pydantic Settings & environment config
│   │   │   ├── database.py       # SQLAlchemy engine & session factory
│   │   │   └── exceptions.py     # Global error handlers
│   │   ├── models/               # SQLAlchemy ORM models
│   │   ├── schemas/              # Pydantic validation schemas
│   │   ├── repositories/         # Abstracted database access layer
│   │   ├── services/             # Application business logic layer
│   │   └── main.py               # FastAPI entry point
│   ├── migrations/               # Alembic database migrations
│   │   ├── versions/             # Migration scripts
│   │   └── env.py                # Alembic runtime environment
│   ├── tests/                    # Backend automated tests
│   │   └── test_health.py        # Health & info endpoint tests
│   ├── alembic.ini               # Alembic configuration
│   ├── Dockerfile                # Backend production container
│   └── requirements.txt          # Python dependencies
│
├── docs/                         # Technical Architecture & Setup Documentation
│   ├── architecture.md           # Architecture flow & separation rationale
│   └── development.md            # Developer setup & testing guide
│
├── docker-compose.yml            # Multi-service local orchestration
├── .gitignore                    # Git exclusions
├── .env.example                  # Environment configuration template
└── README.md                     # Project documentation
```

---

## 5. System Requirements

- **Node.js**: v18.0.0 or higher
- **Python**: v3.10 or higher
- **PostgreSQL**: v14.0 or higher (or Docker)
- **Docker & Docker Compose** (optional)

---

## 6. Installation & Environment Setup

### 1. Clone the repository and configure environment variables
```bash
cp .env.example .env
```

Create frontend local environment (`frontend/.env.local`):
```env
NEXT_PUBLIC_API_URL=http://localhost:8000
```

Create backend environment (`backend/.env`):
```env
ENVIRONMENT=development
SECRET_KEY=change-me-to-a-secure-secret-key-in-production
FRONTEND_URL=http://localhost:3000
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/krishiai_db
```

---

## 7. Starting PostgreSQL

### Option A: Via Docker (Quickest)
```bash
docker run --name krishiai_postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=krishiai_db \
  -p 5432:5432 \
  -d postgres:16-alpine
```

### Option B: Local Native PostgreSQL
Ensure the PostgreSQL service is started, connect with `psql`, and create the database:
```sql
CREATE DATABASE krishiai_db;
```

---

## 8. Starting the Backend

From the repository root:
```bash
cd backend

# Create virtual environment
python -m venv .venv

# Activate virtual environment
# Windows (PowerShell):
.venv\Scripts\Activate.ps1
# Windows (CMD):
.venv\Scripts\activate.bat
# Linux/macOS:
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run database migrations
alembic upgrade head

# Start FastAPI server
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

---

## 9. Starting the Frontend

From the repository root:
```bash
cd frontend

# Install dependencies
npm install

# Run the Next.js development server
npm run dev
```

The frontend will be available at [http://localhost:3000](http://localhost:3000).

---

## 10. How to Use Docker (Full Stack)

To run the entire platform (PostgreSQL, Backend, Frontend) with a single command:
```bash
docker-compose up --build
```

To stop all services:
```bash
docker-compose down
```

---

## 11. API Health Check

The backend exposes health and system information endpoints:

### GET `/api/health`
```bash
curl http://localhost:8000/api/health
```
Response:
```json
{
  "status": "ok",
  "service": "krishiai-backend"
}
```

### GET `/api/`
```bash
curl http://localhost:8000/api/
```
Response:
```json
{
  "name": "KrishiAI API",
  "tagline": "AI-Powered Smart Farming Platform",
  "version": "0.1.0",
  "status": "active"
}
```

Interactive Swagger documentation is available at [http://localhost:8000/docs](http://localhost:8000/docs).

---

## 12. Development Workflow

1. Always use TypeScript strict mode in `frontend/`.
2. Adhere to modular separation in `backend/` (`api`, `core`, `models`, `schemas`, `services`, `repositories`).
3. Run migrations via `alembic revision --autogenerate -m "description"` followed by `alembic upgrade head`. Never create tables manually.
4. Run automated backend tests via `pytest` inside `backend/`.

---

## 13. Future Modules (Part 2+)

The following modules will be systematically implemented in upcoming parts:
- **AI Crop Doctor & Computer Vision Diagnostic Engine**
- **Smart Farm Lifecycle & Soil Management**
- **Hyperlocal Weather Intelligence & Satellite Telemetry**
- **Agricultural Market Intelligence & Price Forecasting**
- **Expert Advisory Network & Audio/Voice Interface**
- **Farmer Authentication, PWA Offline Sync & Multilingual Support**
#   k r i s h i  
 #   k r i s h i  
 #   k r i s h i  
 #   k r i s h i  
 #   k r i s h i  
 #   k r i s h i  
 