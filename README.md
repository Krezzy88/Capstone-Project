# 🥗 Wealthy Eater — AI-Powered HealthTech Platform

> **Graduation Project · FPT University Can Tho · 2026**  
> A comprehensive HealthTech ecosystem connecting users with certified nutritionists, automating personalized meal planning via AI, and facilitating expert consultations with integrated payments.

---

## 📐 Architecture Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                              │
│   Flutter Mobile App (iOS / Android / Web)  │  React Admin UI   │
└────────────────────┬─────────────────────────┬───────────────────┘
                     │ REST + Socket.IO         │ REST
┌────────────────────▼─────────────────────────▼───────────────────┐
│                   BACKEND CORE API (Node.js / Express 5)         │
│   Auth · Meal Plans · Consultations · Chat · PayOS · Cloudinary  │
└──────┬────────────────────────────────────────────┬──────────────┘
       │ HTTP (Internal)                            │ Webhook / HTTP
┌──────▼──────────┐                      ┌──────────▼──────────────┐
│  AI Optimizer   │                      │    n8n Orchestrator      │
│ Python / FastAPI│                      │  Gemini Vision · n8n    │
│ PuLP LP / MILP  │                      │  Pantry Detect · Calorie │
└─────────────────┘                      └─────────────────────────┘
              │                  │
       ┌──────▼──────┐    ┌──────▼──────┐
       │   MongoDB   │    │    Redis     │
       │  (Primary)  │    │  (Caching)   │
       └─────────────┘    └─────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technologies |
|-------|-------------|
| **Mobile App** | Flutter (Dart) · BLoC · GoRouter · fl_chart · Socket.IO Client · Google Sign-In |
| **Web Admin** | React 19 · Vite 8 · Tailwind CSS 4 · Recharts · React Router v7 |
| **Backend API** | Node.js · Express 5 · MongoDB (Mongoose) · Redis (ioredis) · Socket.IO |
| **Auth & Security** | JWT · Google OAuth2 · bcryptjs · Helmet · Rate Limiting · HMAC-SHA256 |
| **AI Optimization** | Python 3.11 · FastAPI · PuLP · GLPK (LP / MILP solver) |
| **AI & Automation** | Google Gemini Flash (3.5 / 3.6) · n8n Workflow Automation |
| **Cloud Services** | Cloudinary (media) · Firebase FCM (push notifications) · Resend (email) |
| **Payment** | PayOS (VietQR Bank Transfer) · Webhook Signature Verification |
| **DevOps** | Docker · Docker Compose · Git / GitHub |

---

## ✨ Key Features

### 👤 For Users (Mobile App)
- 📊 **Biometric Tracking** — BMI, BMR, TDEE auto-calculation with weight log history
- 🍽️ **AI Meal Planning** — Weekly personalized meal plan generated via LP/MILP optimization engine respecting calorie targets and allergy constraints
- 📸 **Meal Image Scan** — Snap a food photo → Gemini Vision extracts nutritional estimates automatically
- 🧺 **Pantry Management** — Photo-scan your fridge, auto-detect ingredients via n8n + Gemini Vision
- 💬 **Real-time Chat** — Bidirectional consultation chat with nutritionists (Socket.IO, image support)
- 🤖 **AI Chatbot** — Nutrition Q&A assistant powered by Google Gemini Flash
- 💳 **PayOS Payment** — Book consultations with in-app VietQR payment flow

### 🧑‍⚕️ For Nutritionists (Mobile App)
- 📋 Manage client profiles, biometrics, and meal plan requests
- ✏️ Edit and customize AI-generated meal plans for each client
- 💬 Live chat with real-time notifications

### 🖥️ For Admins (Web Dashboard)
- 📈 Platform analytics (revenue, user growth, nutritionist performance)
- 👥 User & nutritionist management
- 🥘 Recipe & ingredient management (with XLSX export)
- 💰 Transaction logs & commission settings

---

## 🧬 AI Optimization Engine (Python / FastAPI)

The `wealthy-eater-ai` microservice implements two Linear Programming solvers via **PuLP + GLPK**:

| Endpoint | Algorithm | Purpose |
|----------|-----------|---------|
| `POST /api/v1/ai/compute-diet` | **LP** (Linear Programming) | Allocate exact gram portions of ingredients to hit daily macro targets (Calories, Protein, Carbs, Fat) |
| `POST /api/v1/ai/compute-recipe-plan` | **MILP** (Mixed-Integer LP) | Select optimal recipe combinations from a pool using binary decision variables, enforcing min/max variety constraints |

Both solvers enforce **allergen exclusion rules** and **activation thresholds** per ingredient.

---

## 🔄 n8n Automation Workflows

| Workflow | Trigger | Description |
|----------|---------|-------------|
| **UC-41 Detect Pantry** | User uploads fridge photo | Calls Gemini Vision → extracts ingredient list → syncs to Pantry collection |
| **UC-42 Auto-Adjust Calories** | Weight log updated | Recalculates BMR/TDEE → adjusts future meal plan calorie targets |
| **Template Matching** | Meal plan request | Matches user profile to pre-built templates before invoking the LP solver |

---

## 🚀 Quick Start (Docker)

```bash
# Clone the repo
git clone https://github.com/Krezzy88/Capstone-Project.git
cd Capstone-Project

# Set up environment variables
cp wealthy_eater_backend/.env.example wealthy_eater_backend/.env
cp wealthy-eater-ai/.env.example wealthy-eater-ai/.env
# Edit the .env files with your API keys

# Start n8n orchestrator
docker compose up -d

# Start AI service
cd wealthy-eater-ai
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

# Start Backend API
cd wealthy_eater_backend
npm install && npm run dev      # http://localhost:5000

# Start Admin UI
cd wealthy-eater-admin
npm install && npm run dev      # http://localhost:5173

# Start Mobile App
cd wealthy_eater_mobile
flutter pub get && flutter run
```

---

## 📁 Project Structure

```
Capstone-Project/
│
├── 📱 wealthy_eater_mobile/      # Flutter cross-platform app (iOS / Android / Web)
│   └── lib/
│       ├── core/                 # Theme, network client, error handling
│       ├── data/                 # Models, services, repositories (impl)
│       ├── domain/               # Entities, use-cases, repository interfaces
│       └── presentation/         # Screens, widgets, providers (BLoC + Provider)
│
├── 🖥️  wealthy-eater-admin/       # React 19 Admin Dashboard
│   └── src/
│       ├── pages/                # Dashboard, users, recipes, analytics, etc.
│       ├── components/           # Reusable UI components (charts, tables)
│       └── services/             # Axios API client
│
├── ⚙️  wealthy_eater_backend/     # Node.js / Express 5 Core API
│   └── src/
│       ├── models/               # 25+ Mongoose schemas
│       ├── controllers/          # Request handlers (RBAC: user / nutritionist / admin)
│       ├── services/             # Business logic (Gemini, n8n, PayOS, email…)
│       ├── routes/               # Express routers
│       ├── middlewares/          # Auth, upload, rate-limit
│       └── config/               # DB, Firebase, Cloudinary, Socket.IO, PayOS
│
├── 🤖 wealthy-eater-ai/          # Python / FastAPI AI Optimization Service
│   └── app/
│       ├── routers/              # /compute-diet · /compute-recipe-plan
│       ├── services/             # LP & MILP solver logic (PuLP + GLPK)
│       └── dto/                  # Pydantic request / response schemas
│
├── 📋 docs/                      # Technical design documents
├── 🐳 docker-compose.yml         # n8n orchestrator container
└── 📖 ROOT_GUIDE.md              # Developer guide (naming conventions, API routing)
```

---

## 📄 Documentation

| Document | Description |
|----------|-------------|
| [ROOT_GUIDE.md](./ROOT_GUIDE.md) | API routing conventions, naming rules, dev commands |
| [Database Schema](./docs/database_schema.md) | Full MongoDB collection schemas (25+ models) |
| [AI Meal Generation Logic](./docs/ai_meal_generation_logic.md) | LP/MILP algorithm design & n8n workflow integration |
| [PayOS Webhook Flow](./docs/payos_webhook_flow.md) | Payment webhook lifecycle & HMAC verification |
| [State Management](./docs/state_management.md) | Flutter BLoC + Provider architecture guide |

---

## 👨‍💻 Team

| Member | Role |
|--------|------|
| **Trần Phan Trung Kiên** | Backend API · AI Optimization Service · n8n Automation |
| Team Member 2 | Mobile App (Flutter) |
| Team Member 3 | Mobile App (Flutter) |
| Team Member 4 | Admin Dashboard (React) |

---

## 🏫 Project Info

- **University:** FPT University – Can Tho Campus
- **Program:** Software Engineering
- **Type:** Capstone / Graduation Project
- **Period:** May – August 2026

---

<div align="center">
  <sub>Built with ❤️ at FPT University Can Tho · 2026</sub>
</div>
