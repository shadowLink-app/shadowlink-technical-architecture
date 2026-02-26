# Shadow Link - Always-On-Top Browser Overlay for macOS

**You need the web without leaving your flow.** This project is a unified solution: a floating, always-on-top browser overlay on your Mac with shadow typing and screenshot OCR, backed by a landing site, Stripe checkout, and activation API so you can sell and activate the desktop app (shadow-link / shadow-link-app) from a single backend (ShadowLink).

> **One window, always on top.** Browse, type, and capture without switching apps.

[Electron](https://www.electronjs.org/) · [Node.js](https://nodejs.org/) · [Express](https://expressjs.com/) · [MongoDB](https://www.mongodb.com/) · [Stripe](https://stripe.com/)

<p align="center">
  <img src="https://img.shields.io/badge/Platform-macOS-007AFF?style=for-the-badge&logo=apple" alt="Platform">
  <img src="https://img.shields.io/badge/Electron-28.x-47848F?style=for-the-badge&logo=electron" alt="Electron">
  <img src="https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=node.js" alt="Node">
  <img src="https://img.shields.io/badge/Stripe-Payments-6772E5?style=for-the-badge&logo=stripe" alt="Stripe">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License">
</p>

---

## 🎯 The problem (and what we built to address it)

You need quick access to the web (references, docs, forms) without leaving your current app or breaking flow. Switching windows kills focus; opening a full browser is overkill, and your workflow gets scattered across tabs and apps. This project is a **unified solution**: a floating, always-on-top browser overlay on macOS that stays out of the way until you need it, with shadow typing and screenshot OCR built in, plus a landing site, Stripe checkout, and activation API so you can sell and activate the desktop app from a single backend (ShadowLink).

---

## ✨ Key features

| Area | What you get |
|------|----------------|
| **Always on top** | Overlay stays above all windows, including fullscreen apps. |
| **Shadow typing** | Type into the overlay while using other apps (macOS Accessibility). |
| **Screenshots & OCR** | Capture the screen and extract text with one shortcut. |
| **Global shortcuts** | Show/hide, pin, and control the overlay from anywhere. |
| **Multi-tab** | Browse multiple sites in one floating window. |
| **Licensing & payments** | Stripe checkout, activation codes, and email delivery. |
| **Auto-updates** | Desktop app updates from GitHub releases. |

---

## 🔄 How it works

| Step | What happens |
|------|----------------|
| **1. Visit landing** | User opens the ShadowLink site (landing page). |
| **2. Choose plan & checkout** | User goes to `/payment`, picks a plan, and completes Stripe Checkout. |
| **3. Get activation code** | After payment, Stripe webhook fires → backend stores the activation in MongoDB and emails the code to the buyer (Nodemailer). |
| **4. Enter code in app** | User installs the Shadow Link desktop app (shadow-link or shadow-link-app), opens it, and enters the activation code. |
| **5. App validates** | Desktop app calls ShadowLink `/api/validate` (with `X-API-Secret`). Backend returns plan, expiry, and status; app activates features. |
| **6. Use the overlay** | User can show/hide the overlay (`Cmd+Shift+M`), keep it on top (`Cmd+Shift+P`), use shadow typing (`F1`), take screenshots with OCR (`Cmd+Shift+S`), and browse in multiple tabs. |

**In short:** Landing → Stripe → email code → enter in app → validate → use overlay.

---

## 🎨 Product & UI 
 <!-- **Media placeholders**: replace these with your own GIFs or screenshots for a polished launch page. -->
### Hero / overview

**[Insert GIF or screenshot: floating overlay in action over a fullscreen app]**

*Suggested: 5–10 second loop of the overlay appearing over another app, then typing or taking a screenshot.*

---

### Desktop app experience

**[Insert screenshot: Shadow Link window with tabs and clean UI]**

*Suggested: Main window with 2–3 tabs, draggable frameless design visible.*

---

### Shadow typing in use

**[Insert GIF: user in another app, overlay visible, typing into overlay]**

*Suggested: Focus on the overlay receiving keystrokes while another app is in the foreground.*

---

### Checkout & activation flow

**[Insert screenshot: payment page or success screen with activation code]**

*Suggested: Stripe checkout or “Code sent to your email” confirmation.*

---

## 💻 Tech stack

| Layer | Technologies |
|-------|--------------|
| **Desktop app** | Electron 28, uiohook-napi (global input), Tesseract.js (OCR), electron-updater, electron-builder |
| **Backend & web** | Node.js, Express, EJS, MongoDB (Mongoose), Stripe (Checkout + webhooks), Nodemailer |
| **DevOps / run** | Docker & Docker Compose (backend), npm scripts |

**Tech stack badges**

*Desktop app:*  
![Electron](https://img.shields.io/badge/Electron-28.x-47848F?style=for-the-badge&logo=electron&logoColor=white) ![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=nodedotjs&logoColor=white) ![uiohook-napi](https://img.shields.io/badge/uiohook--napi-Global_Input-5C2D91?style=for-the-badge) ![Tesseract.js](https://img.shields.io/badge/Tesseract.js-OCR-009999?style=for-the-badge) ![electron-updater](https://img.shields.io/badge/electron--updater-Auto_Update-47848F?style=for-the-badge) ![electron-builder](https://img.shields.io/badge/electron--builder-Packaging-47848F?style=for-the-badge)

*Backend & web:*  
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white) ![EJS](https://img.shields.io/badge/EJS-Templating-000000?style=for-the-badge) ![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white) ![Mongoose](https://img.shields.io/badge/Mongoose-880000?style=for-the-badge&logo=mongodb&logoColor=white) ![Stripe](https://img.shields.io/badge/Stripe-6772E5?style=for-the-badge&logo=stripe&logoColor=white) ![Nodemailer](https://img.shields.io/badge/Nodemailer-Email-339933?style=for-the-badge)

*DevOps & tooling:*  
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white) ![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white) ![npm](https://img.shields.io/badge/npm-CB3837?style=for-the-badge&logo=npm&logoColor=white)

---

## 🏗️ High-level architecture (system overview)

```
                    ┌─────────────────────────────────────┐
                    │              End users               │
                    └─────────────────┬───────────────────┘
                                      │
          ┌───────────────────────────┼───────────────────────────┐
          ▼                           ▼                           ▼
┌───────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│  Shadow Link      │     │  Landing &          │     │  Stripe           │
│  desktop app      │     │  checkout           │     │  (Checkout UI      │
│  (Electron)       │     │  (browser)          │     │   redirect)        │
│  shadow-link /    │     │  EJS pages          │     │                   │
│  shadow-link-app  │     │  /payment, /        │     │                   │
└─────────┬─────────┘     └─────────┬───────────┘     └─────────┬─────────┘
          │                          │                         │
          └──────────────────────────┼─────────────────────────┘
                                     ▼
                         ┌───────────────────────┐
                         │  ShadowLink backend   │
                         │  (Express + EJS)      │
                         │  Landing, API,        │
                         │  webhooks             │
                         └───────────┬───────────┘
                                     │
          ┌──────────────────────────┼──────────────────────────┐
          ▼                          ▼                          ▼
┌───────────────────┐     ┌───────────────────┐     ┌───────────────────┐
│  App API          │     │  Payments &       │     │  Data & mail       │
│  /api/validate    │     │  activation       │     │  MongoDB           │
│  /api/status      │     │  Stripe webhooks  │     │  (licenses,        │
│  (X-API-Secret)   │     │  checkout session │     │   activations)     │
│                   │     │  activation codes  │     │  Nodemailer        │
│                   │     │  emailed to buyer │     │  (activation mail) │
└───────────────────┘     └───────────────────┘     └───────────────────┘
```

---

## 🏗 Project architecture (one repo, three parts)

This repository is **one product** split into three folders:

```
Project/
├── ShadowLink/          # Backend + web: landing, payments, activation API
├── shadow-link/         # Desktop app (Electron), primary build
└── shadow-link-app/     # Desktop app variant / alternate build
```

### 1. **ShadowLink** - Backend & web

- **Role:** Landing page, Stripe checkout, activation code generation, and API for the desktop app.
- **Highlights:** EJS-rendered pages, Stripe webhooks, MongoDB for licenses, Nodemailer for activation emails, protected endpoints for the macOS app (`/api/validate`, `/api/status/:code`).
- **Run:** `npm run dev` (or Docker); default landing at `http://localhost:3334`.

### 2. **shadow-link** - Desktop app (Electron)

- **Role:** The main macOS desktop app: floating overlay, shadow typing, screenshots, global shortcuts, multi-tab browser.
- **Highlights:** Frameless always-on-top window, license validation against the backend, auto-update from GitHub releases.
- **Run:** `npm start` (dev) or `npm run build:mac` (production).

### 3. **shadow-link-app** - Desktop app variant

- **Role:** Same Electron product with possible config or build differences (e.g. update channel, targets).
- **Use:** When you maintain two build variants; otherwise treat as the same app as `shadow-link`.

**End-to-end flow:** Customer visits landing (ShadowLink) → pays via Stripe → receives activation code by email → enters code in desktop app (shadow-link / shadow-link-app) → app validates with ShadowLink API and activates features.

---

## 🚀 Installation & quick start

### Prerequisites

- **Node.js** 18+
- **npm** 9+
- **macOS** 10.15 (Catalina) or newer (for the desktop app)
- **MongoDB** (local or Atlas) for the backend

---

### Backend (ShadowLink)

1. **Clone and enter the backend folder:**
   ```bash
   cd ShadowLink
   ```
2. **Configure environment:**  
   Copy `env.example` to `.env` and set:
   - MongoDB connection string  
   - `API_SECRET` (for app API auth)  
   - Stripe keys and webhook secret  
   - SMTP settings for activation emails  

3. **Install and run:**
   ```bash
   npm install
   npm run dev
   ```
4. **Open:** `http://localhost:3334` (landing), `/payment` (checkout).

**Docker (production-style):**
```bash
docker compose up --build
```

---

### Desktop app (shadow-link or shadow-link-app)

1. **Enter the app folder:**
   ```bash
   cd shadow-link
   # or: cd shadow-link-app
   ```
2. **Install and run:**
   ```bash
   npm install
   npm start
   ```
3. **Build for macOS (universal):**
   ```bash
   npm run build:mac
   ```
   Output: `dist/mac-universal/Shadow Link.app`

---

## 🔐 Permissions Required

The desktop app (shadow-link / shadow-link-app) may need the following macOS permissions for full functionality.

### macOS Accessibility

Required for **shadow typing** (global keyboard input so you can type into the overlay while another app is focused).

| Permission | Required for | How to grant |
|------------|--------------|--------------|
| **Accessibility** | Shadow typing (global keyboard input) | **System Preferences** → **Security & Privacy** → **Privacy** → **Accessibility** → add **Shadow Link** → **restart the app** |
| **Screen Recording** (if needed) | Screenshot / capture (when capturing outside the overlay) | **System Preferences** → **Security & Privacy** → **Privacy** → **Screen Recording** → add **Shadow Link** → **restart the app** |

**Steps for Accessibility:** Open **System Preferences** → **Security & Privacy** → **Privacy** → **Accessibility**, add **Shadow Link**, then restart the app. If shadow typing still doesn’t work, remove the app from the list, add it again, and restart.

---

## ⌨️ Keyboard shortcuts (desktop app)

| Shortcut | Action |
|----------|--------|
| `Cmd+Shift+M` | Show / hide window |
| `Cmd+Shift+P` | Toggle always-on-top |
| `F1` | Toggle shadow typing |
| `Cmd+Shift+S` | Take screenshot |
| `Cmd+T` | New tab |
| `Cmd+W` | Close tab |
| `Cmd+R` | Refresh page |

---

## 📁 Where to find more

| Resource | Location |
|----------|----------|
| Backend API (validate, status, checkout, webhooks) | `ShadowLink/README.md` |
| Desktop app architecture | `shadow-link/docs/ARCHITECTURE.md` or `shadow-link-app/docs/ARCHITECTURE.md` |
| Electron & build details | `shadow-link/docs/ELECTRON.md`, `shadow-link-app/docs/ELECTRON.md` |

---

## 📄 License

MIT - see the LICENSE file in the repo for details.

---

<p align="center">
  Made with ❤️ by Shadow Link Team
</p>
