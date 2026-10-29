<h1 align="center">🥗 Wealthy Eater</h1>
<p align="center">
  <strong>AI-Powered HealthTech Platform · Capstone Project · FPT University Can Tho 2026</strong>
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white"/>
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB"/>
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white"/>
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white"/>
</p>

---

## 📌 Project Overview

**Wealthy Eater** is a comprehensive HealthTech ecosystem that connects users with certified nutritionists. It automates personalized meal planning via AI, tracks nutrition through meal image scanning, and facilitates real-time expert consultations with integrated payment flows.

> **Type:** Graduation / Capstone Project — 5-member team  
> **Period:** May – August 2026  
> **University:** FPT University – Can Tho Campus

---

## 🏗️ System Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                          CLIENT LAYER                                │
│   📱 Flutter Mobile App (iOS / Android / Web)  │  🖥️ React Admin UI  │
└─────────────────┬──────────────────────────────┬─────────────────────┘
                  │  REST + Socket.IO             │  REST
┌─────────────────▼──────────────────────────────▼─────────────────────┐
│              BACKEND CORE API — Node.js / Express 5                   │
│  Auth · RBAC · Recipes · Meal Plans · Chat · Notifications · PayOS   │
└──────┬─────────────────────────────────────────────────┬─────────────┘
       │  HTTP (Internal Secret)                         │  Webhook
┌──────▼──────────────┐                     ┌───────────▼──────────────┐
│  AI Optimization    │                     │   n8n Orchestrator       │
│  Python / FastAPI   │                     │   Gemini Vision API      │
│  PuLP LP + MILP     │                     │   Pantry Detect          │
│  GLPK Solver        │                     │   Auto-Calorie Adjust    │
└─────────────────────┘                     └──────────────────────────┘
        │                                              │
 ┌──────▼───────┐    ┌────────────────┐    ┌──────────▼──────────┐
 │   MongoDB    │    │     Redis       │    │  Firebase FCM       │
 │  (Primary DB)│    │  (Cache/Token) │    │  (Push Notif.)      │
 └──────────────┘    └────────────────┘    └─────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technologies |
|-------|-------------|
| **📱 Mobile App** | Flutter (Dart) · BLoC + Provider · GoRouter · fl_chart · Socket.IO Client · Google Sign-In · WebView Flutter |
| **🖥️ Web Admin** | React 19 · Vite 8 · Tailwind CSS 4 · Recharts · React Router v7 · Lucide Icons |
| **⚙️ Backend API** | Node.js · Express 5 · MongoDB (Mongoose ODM) · Redis (ioredis) · Socket.IO |
| **🔐 Auth & Security** | JWT · Google OAuth2 · bcryptjs · Helmet · Rate Limiting · HMAC-SHA256 Webhook |
| **🤖 AI Optimization** | Python 3.11 · FastAPI · PuLP · GLPK Solver · MILP (Mixed-Integer Linear Programming) |
| **🧠 AI & Automation** | Google Gemini Flash (3.5 / 3.6) · n8n Workflow Automation |
| **☁️ Cloud Services** | Cloudinary (media) · Firebase Admin SDK (FCM push) · Resend + Nodemailer (email/OTP) |
| **💳 Payment** | PayOS — VietQR Bank Transfer · Webhook Signature Verification |
| **🐳 DevOps** | Docker · Docker Compose |

---

## ✨ Feature Highlights

### 👤 For Customers (Mobile)
| Feature | Description |
|---------|-------------|
| 📊 **Biometric Dashboard** | Auto-compute BMI, BMR, TDEE from height/weight/age; track weight history with charts |
| 🍽️ **AI Meal Planning** | LP/MILP optimization engine generates personalized weekly meal plans within calorie targets and allergy constraints |
| 📸 **Meal Image Scan** | Snap food photo → Gemini Vision extracts macro estimates (calories, protein, carbs, fat) |
| 🧺 **Smart Pantry** | Photo-scan fridge → n8n + Gemini Vision auto-detects ingredients and syncs to pantry |
| 🛒 **Shopping List** | Auto-generates ingredient shopping lists from weekly meal plans |
| 💬 **Live Consultation Chat** | Real-time bidirectional chat (Socket.IO) with nutritionists, image attachment support |
| 🤖 **Nutrition AI Chatbot** | Gemini-powered chatbot for on-demand nutrition Q&A |
| 💳 **In-App Payment** | Embedded PayOS / VietQR payment flow via WebView without leaving the app |
| 🔔 **Smart Notifications** | Water reminders, meal reminders (local), push notifications (Firebase FCM) |

### 🧑‍⚕️ For Nutritionists (Mobile)
| Feature | Description |
|---------|-------------|
| 📋 **Client Management** | View client biometrics, meal logs, and macro deviation alerts |
| ✏️ **Meal Plan Editor** | Customize and override AI-generated meal plans per client |
| 💬 **Live Chat** | Real-time consultation chat with client diet audit summary |

### 🖥️ For Admins (Web Dashboard)
| Feature | Description |
|---------|-------------|
| 📈 **Platform Analytics** | Revenue charts (Recharts), user growth, nutritionist performance |
| 👥 **User Management** | Full CRUD on users and nutritionists; approval/rejection of expert applications |
| 🥘 **Recipe & Ingredient Management** | Add/edit recipes, ingredients, micronutrients; bulk import via Excel (.xlsx) |
| 💰 **Transaction Logs** | View and export payment transactions; configure commission rates |
| ⚙️ **System Settings** | Platform-wide configuration management |

---

## 🤖 AI Optimization Engine (Python / FastAPI)

The `wealthy-eater-ai` microservice implements two solvers via **PuLP + GLPK**:

### Endpoint 1: `POST /api/v1/ai/compute-diet` — Linear Programming (LP)
Allocates exact **gram-level portions** of available ingredients to precisely hit macro targets:

```
Minimize: total_mass = Σ x_i
Subject to:
  Σ (x_i × calories_i / 100) == target_calories
  Σ (x_i × protein_i / 100)  == target_protein
  Σ (x_i × carbs_i / 100)    == target_carbs
  Σ (x_i × fat_i / 100)      == target_fat
  x_i ∈ [0, 500g]  ∀ ingredient i
  x_i = 0          if ingredient i ∈ allergy_exclusions
```

### Endpoint 2: `POST /api/v1/ai/compute-recipe-plan` — MILP (Mixed-Integer LP)
Selects the **optimal recipe combination** for a full meal plan using binary decision variables to enforce variety constraints, while filtering allergens.

Both solvers use `X-INTERNAL-SECRET` header authentication for service-to-service security.

---

## 🔄 n8n Automation Workflows

| Workflow | Trigger | Actions |
|----------|---------|---------|
| **UC-41 Pantry Detection** | User uploads fridge photo | Calls Gemini Vision → extracts ingredient list → syncs to MongoDB Pantry collection |
| **UC-42 Calorie Auto-Adjust** | Weight log updated | Recalculates BMR/TDEE → adjusts calorie targets for upcoming meal plans |
| **Template Matching** | Meal plan requested | Matches user profile against pre-built templates before invoking the LP solver |

---

## 💳 PayOS Payment Flow

```
User selects Nutritionist → [Backend] Creates ConsultationContract (status: pending_payment)
                          → [PayOS SDK] Generates payment link + VietQR code
                          → [Mobile] Renders PayOS UI inside WebView
User completes payment    → [PayOS] Sends POST to /api/webhooks/payos
                          → [Backend] Verifies HMAC-SHA256 signature
                          → Updates Transaction + ConsultationContract (status: active)
                          → Sends confirmation push notification via Firebase FCM
```

---

## 🗄️ Database Overview (MongoDB — 25+ Collections)

| Category | Collections |
|----------|------------|
| **Identity** | `User` · `UserProfile` · `UserDietary` · `Nutritionist` · `MedicalCondition` |
| **Recipes** | `Recipe` · `RecipeStep` · `RecipeNutrition` · `RecipeIngredient` · `Micronutrient` · `RecipeMicronutrientValue` |
| **Meal Planning** | `MealPlan` · `MealPlanItem` · `MealPlanRequest` · `MealPlanTemplate` · `CustomerMealLog` · `MacroDeviationFlag` |
| **Consultations** | `ConsultationContract` · `ConsultationMessage` |
| **Payments** | `Transaction` |
| **User Actions** | `RecipeReview` · `RecipeLike` · `ShoppingList` · `Pantry` · `WeightLog` · `UserAiRecipe` |
| **Notifications** | `Notification` · `NotificationSetting` |
| **System** | `AuditLog` · `SystemSetting` · `ChatbotSession` |

---

## 🚀 Quick Start

### Prerequisites
- Node.js ≥ 18 · Python 3.11 · Flutter 3.x · Docker

### 1. Clone & configure environment

```bash
git clone https://github.com/Krezzy88/Capstone-Project.git
cd Capstone-Project

# Configure each service's environment
cp wealthy_eater_backend/.env.example wealthy_eater_backend/.env
# Fill in: MONGODB_URI, REDIS_URL, GOOGLE_API_KEY, PAYOS_*, CLOUDINARY_*, FIREBASE_*, RESEND_API_KEY
```

### 2. Start n8n Orchestrator (Docker)

```bash
docker compose up -d
# n8n → http://localhost:5678
```

### 3. Start AI Optimization Service

```bash
cd wealthy-eater-ai
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
# AI Service → http://localhost:8000
```

### 4. Start Backend Core API

```bash
cd wealthy_eater_backend
npm install
npm run dev
# Backend API → http://localhost:5000
```

### 5. Start Admin Dashboard

```bash
cd wealthy-eater-admin
npm install
npm run dev
# Admin UI → http://localhost:5173
```

### 6. Run Mobile App

```bash
cd wealthy_eater_mobile
flutter pub get
flutter run
```

---

## 📁 Project Structure

```
Capstone-Project/
│
├── 📱 wealthy_eater_mobile/        # Flutter cross-platform app
│   └── lib/
│       ├── core/                   # Theme (Material 3), network client, errors
│       ├── data/                   # Models, services, repository implementations
│       ├── domain/                 # Entities, use-cases, repository interfaces
│       └── presentation/           # Screens, widgets, BLoC/Provider state
│
├── 🖥️  wealthy-eater-admin/         # React 19 Admin Dashboard
│   └── src/
│       ├── pages/                  # Dashboard, users, recipes, analytics, transactions
│       ├── components/             # Reusable UI (charts, tables, badges)
│       └── services/               # Axios API client with session guard
│
├── ⚙️  wealthy_eater_backend/       # Node.js / Express 5 Core API
│   └── src/
│       ├── models/                 # 25+ Mongoose schemas
│       ├── controllers/            # RBAC handlers (user / nutritionist / admin)
│       ├── services/               # Business logic (Gemini, n8n, PayOS, Cloudinary…)
│       ├── routes/                 # Express routers (actor-prefixed)
│       ├── middlewares/            # Auth JWT, upload, rate-limit, validation
│       └── config/                 # DB, Firebase, Cloudinary, Socket.IO, PayOS
│
├── 🤖 wealthy-eater-ai/            # Python FastAPI Optimization Microservice
│   └── app/
│       ├── routers/                # /compute-diet · /compute-recipe-plan
│       ├── services/               # LP & MILP solver logic (PuLP + GLPK)
│       └── dto/                    # Pydantic request/response schemas
│
├── 📋 docs/                        # Technical design documents
│   ├── database_schema.md
│   ├── ai_meal_generation_logic.md
│   ├── payos_webhook_flow.md
│   └── state_management.md
│
├── 🐳 docker-compose.yml           # n8n orchestrator container
└── 📖 ROOT_GUIDE.md                # API routing conventions & naming rules
```

---

## 📄 Documentation

| Document | Description |
|----------|-------------|
| [ROOT_GUIDE.md](./ROOT_GUIDE.md) | API routing conventions, actor-based naming rules, dev commands |
| [Database Schema](./docs/database_schema.md) | Full MongoDB collection schemas (25+ models with field types) |
| [AI Meal Generation Logic](./docs/ai_meal_generation_logic.md) | LP/MILP algorithm design and n8n + Gemini integration |
| [PayOS Webhook Flow](./docs/payos_webhook_flow.md) | Payment lifecycle, HMAC-SHA256 verification, idempotency logic |
| [State Management](./docs/state_management.md) | Flutter BLoC + Provider architecture guide |

---

## 👨‍💻 Team

| GitHub | Contributions |
|--------|--------------|
| **[@Krezzy88](https://github.com/Krezzy88)** (Trần Phan Trung Kiên) | Backend Core API · AI Optimization Service · n8n Automation · Gemini Integration · Socket.IO Chat |
| **[@longntce182041](https://github.com/longntce182041)** | Team Lead · Mobile App Architecture · Meal Planning Flow · Diet Audit · Flutter UI |
| **[@KinoKaikan](https://github.com/KinoKaikan)** (VietT) | Mobile App · Meal Scan (UC-16) · Flutter Features |
| **[@MinhDan004](https://github.com/MinhDan004)** | Nutrition Management · Admin Features (UC-85, UC-86) |
| **[@HoangLong1709](https://github.com/HoangLong1709)** (La Võ Hoàng Long) | Admin Dashboard · System Analytics · Excel Import/Export |

---

<div align="center">
  <sub>Built with ❤️ by Team Wealthy Eater · FPT University Can Tho · 2026</sub>
</div>
