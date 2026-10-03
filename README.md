## 📑 Table of Contents

* [🌟 Overview](#-overview)
* [✨ Key Features](#-key-features)
* [🏗️ System Architecture](#️-system-architecture)
* [🧩 Technology Stack](#-technology-stack)

  * [🎨 Frontend](#-layer-1--frontend)
  * [⚙️ Backend](#️-layer-2--backend)
  * [🗄️ Database](#️-layer-3--database)
  * [🤖 AI Services](#-layer-4--ai-services)
* [🧠 AI Architecture](#-ai-architecture)
* [🔄 Communication Between Layers](#-communication-between-layers)
* [☁️ Deployment](#️-deployment)
* [🔐 Environment Variables](#-environment-variables)
* [📁 Project Structure](#-project-structure)
* [🚀 Getting Started](#-getting-started)
* [🎯 Project Goals](#-project-goals)
* [👥 Project](#-project)

# 🎨 KalaKriti

### AI-Driven Market Linkage & Smart Cataloging Platform for Marginalized Artisans

> **Empowering artisans with AI-powered tools to create, manage, price, and showcase their products digitally.**

---

## 🌟 Overview

**KalaKriti** is an AI-powered web platform designed to help marginalized artisans and micro-entrepreneurs overcome digital barriers and connect their products with wider markets.

The platform combines **multilingual voice input, AI-powered catalog generation, image enhancement, pricing assistance, and digital product management** into a simple and accessible interface.

---

## ✨ Key Features

| Feature                 | Technology                  | Purpose                                                               |
| ----------------------- | --------------------------- | --------------------------------------------------------------------- |
| 🖼️ Image Enhancer      | `@imgly/background-removal` | Remove backgrounds and enhance product images directly in the browser |
| 🎙️ Voice Cataloging    | Web Speech API              | Convert regional-language speech into text                            |
| 🤖 AI Catalog Generator | Google Gemini               | Generate product titles and descriptions                              |
| 💰 Pricing Assistant    | Google Gemini               | Suggest product prices based on product information                   |
| 🌐 Multilingual Support | Gemini + Web Speech API     | Assist artisans using regional languages                              |
| 🔐 Authentication       | JWT                         | Secure user authentication                                            |
| 🛒 Marketplace          | React + FastAPI             | Product browsing and cart management                                  |
| 🗄️ Database            | PostgreSQL                  | Store users, products and cart data                                   |

---

# 🏗️ System Architecture

```text
                         👤 USER
                           │
                           │ HTTPS
                           ▼
              ┌─────────────────────────┐
              │     REACT FRONTEND      │
              │       Vite + JSX        │
              │                         │
              │  • React Router        │
              │  • Context API         │
              │  • Axios               │
              │  • Tailwind / CSS      │
              └────────────┬────────────┘
                           │
                    JSON / REST API
                           │
                           ▼
              ┌─────────────────────────┐
              │     FASTAPI BACKEND     │
              │       Python 3.11+      │
              │                         │
              │  • JWT Authentication   │
              │  • Pydantic             │
              │  • SQLAlchemy           │
              │  • Uvicorn              │
              └───────┬─────────┬───────┘
                      │         │
              SQL Queries       │ HTTPS
                      │         │
                      ▼         ▼
          ┌────────────────┐  ┌─────────────────┐
          │    NEON DB     │  │   GEMINI API    │
          │  PostgreSQL 16 │  │   Google AI     │
          │                │  │                 │
          │ • Users        │  │ • Cataloging    │
          │ • Products     │  │ • Pricing       │
          │ • Carts        │  │ • Translation   │
          │ • Cart Items   │  │                 │
          └────────────────┘  └─────────────────┘
```

---

# 🧩 Technology Stack

## 🎨 Layer 1 — Frontend

**Language:** JavaScript ES6+

**Framework:** React 18
**Build Tool:** Vite
**Styling:** CSS3 / Tailwind CSS
**Markup:** HTML5 / JSX
**HTTP Client:** Axios
**Routing:** React Router v6
**State Management:** React Context API
**Authentication Storage:** Browser `localStorage` using JWT

### Browser-Side AI & APIs

* `@imgly/background-removal` — WASM-based background removal
* **Web Speech API** — Voice-to-text
* **Canvas API** — Image preview and manipulation

> 💡 These operations run directly in the user's browser, reducing server load and improving responsiveness.

---

# ⚙️ Layer 2 — Backend

**Language:** Python 3.11+

**Framework:** FastAPI
**Server:** Uvicorn / ASGI
**Validation:** Pydantic v2
**ORM:** SQLAlchemy 2.0
**Database Driver:** psycopg2-binary
**Authentication:** python-jose + passlib[bcrypt]
**Environment Management:** python-dotenv
**CORS:** FastAPI CORSMiddleware

### Server-Side AI

The backend communicates with Google's Gemini API for AI-powered text processing.

---

# 🗄️ Layer 3 — Database

**Database:** PostgreSQL 16
**Provider:** Neon
**Connection:** `DATABASE_URL` with SSL
**ORM:** SQLAlchemy

### Database Schema

```text
┌──────────────┐
│    USERS     │
├──────────────┤
│ id           │
│ email        │
│ password     │
│ name         │
│ role         │
│ created_at   │
└──────┬───────┘
       │
       │ user_id
       ▼
┌──────────────┐
│   PRODUCTS   │
├──────────────┤
│ id           │
│ user_id      │
│ title        │
│ description  │
│ price        │
│ image_url    │
│ category     │
│ created_at   │
└──────────────┘

┌──────────────┐
│    CARTS     │
├──────────────┤
│ id           │
│ user_id      │
│ created_at   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ CART_ITEMS   │
├──────────────┤
│ id           │
│ cart_id      │
│ product_id   │
│ quantity     │
└──────────────┘
```

---

# 🤖 Layer 4 — AI Services

### Google Gemini

**Provider:** Google
**Model:** Gemini 1.5 Flash
**SDK:** `google-generativeai`

API authentication is handled using an environment variable:

```env
GEMINI_API_KEY=your_api_key_here
```

### 📝 1. AI Description Generator

**Input:**

```text
Voice transcript
+ Regional language
+ Product information
```

**Output:**

```text
SEO-friendly product title
Product description
English translation
Hindi translation
```

---

### 💰 2. AI Pricing Assistant

**Input:**

```text
Product description
+ Category
+ Material cost
```

**Output:**

```text
Suggested price (₹)
+ Reasoning
```

---

### 🚫 What Gemini Is NOT Used For

* ❌ Image generation
* ❌ Voice transcription

Voice transcription is handled by the browser's **Web Speech API**, while image processing is performed locally using **WASM-based background removal**.

---

# 🧠 AI Architecture

```text
                 USER
                   │
          ┌────────┴────────┐
          │                 │
          ▼                 ▼
   🎙️ Voice Input      🖼️ Product Image
          │                 │
          ▼                 ▼
   Web Speech API     IMG.LY WASM
          │                 │
          │                 │
          ▼                 ▼
       Browser            Browser
          │
          │ Text
          ▼
     React Frontend
          │
          │ HTTPS
          ▼
     FastAPI Backend
          │
          ▼
      Gemini API
          │
     ┌────┴─────┐
     ▼          ▼
📝 Catalog    💰 Pricing
Generator    Assistant
```

---

# 🔄 Communication Between Layers

```text
USER'S PHONE
     │
     │ HTTPS + JWT
     ▼
┌─────────────────────┐
│   React Frontend    │
│      Vite           │
└──────────┬──────────┘
           │
           │ Axios / JSON
           ▼
┌─────────────────────┐
│   FastAPI Backend   │
│     Uvicorn         │
└──────┬─────────┬────┘
       │         │
       │         │ HTTPS
       │         ▼
       │   ┌───────────────┐
       │   │  Gemini API   │
       │   │ Google AI     │
       │   └───────────────┘
       │
       │ SQL
       ▼
┌─────────────────────┐
│   Neon PostgreSQL   │
│     Database        │
└─────────────────────┘
```

---

# ☁️ Deployment

| Component    | Platform                    | Deployment            |
| ------------ | --------------------------- | --------------------- |
| 🎨 Frontend  | Render / Vercel / Netlify   | Static Site           |
| ⚙️ Backend   | Render                      | Web Service           |
| 🗄️ Database | Neon                        | Serverless PostgreSQL |
| 🌐 Domain    | Custom / Platform Subdomain | Optional              |

### Backend

The backend can be deployed as a **Render Web Service** using Uvicorn.

> ⚠️ On free-tier hosting, the backend may sleep after periods of inactivity, resulting in a cold-start delay.

### Database

**Neon PostgreSQL** provides the cloud database layer with SSL-secured connections.

---

# 🔐 Environment Variables

Create a `.env` file locally:

```env
DATABASE_URL=your_postgresql_connection_string
GEMINI_API_KEY=your_gemini_api_key
SECRET_KEY=your_jwt_secret
```

⚠️ **Never commit `.env` to GitHub.**

Add it to `.gitignore`:

```gitignore
.env
.env.*
!.env.example
```

---

# 📁 Project Structure

```text
KalaKriti/
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── context/
│   │   ├── services/
│   │   └── App.jsx
│   ├── package.json
│   └── vite.config.js
│
├── backend/
│   ├── models/
│   ├── routes/
│   ├── services/
│   ├── main.py
│   ├── requirements.txt
│   └── .env
│
├── README.md
└── .gitignore
```

---

# 🚀 Getting Started

### 1️⃣ Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY
```

### 2️⃣ Install frontend dependencies

```bash
cd frontend
npm install
```

### 3️⃣ Start the frontend

```bash
npm run dev
```

### 4️⃣ Install backend dependencies

```bash
cd ../backend
pip install -r requirements.txt
```

### 5️⃣ Configure environment variables

Create a `.env` file:

```env
DATABASE_URL=your_database_url
GEMINI_API_KEY=your_gemini_api_key
SECRET_KEY=your_secret_key
```

### 6️⃣ Start the backend

```bash
uvicorn main:app --reload
```

---

# 🎯 Project Goals

KalaKriti aims to make digital commerce more accessible to artisans by reducing common barriers such as:

* 📱 Digital literacy
* 🌐 Language barriers
* 📸 Product photography
* 📝 Product cataloging
* 💰 Price estimation
* 🛍️ Limited digital market access

The goal is to provide artisans with a **simple AI-powered digital business assistant** rather than requiring them to understand complex technology.

---

# 👥 Project

**KalaKriti — AI-Driven Market Linkage & Smart Cataloging Platform**

Built with ❤️ using **React, FastAPI, PostgreSQL & Google Gemini AI**.

> *Technology that helps traditional craftsmanship reach the digital world.* 🎨🌐

---
