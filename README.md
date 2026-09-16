# 🔔 Subscription Tracker API

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-v18+-green?style=for-the-badge&logo=node.js" alt="Node.js" />
  <img src="https://img.shields.io/badge/Express.js-Backend-black?style=for-the-badge&logo=express" alt="Express" />
  <img src="https://img.shields.io/badge/MongoDB-Mongoose-brightgreen?style=for-the-badge&logo=mongodb" alt="MongoDB" />
  <img src="https://img.shields.io/badge/Security-Arcjet-blueviolet?style=for-the-badge" alt="Arcjet" />
  <img src="https://img.shields.io/badge/Automation-Upstash%20Workflow-red?style=for-the-badge" alt="Upstash" />
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License" />
</p>

<p align="center">
  A production-ready, highly secure REST API for managing and tracking recurring subscriptions. Features automated email reminder sequences powered by <strong>Upstash Workflows</strong>, advanced threat defense and rate limiting with <strong>Arcjet</strong>, and robust authentication with <strong>JWT</strong>.
</p>

---

## 📑 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture)
- [Directory Structure](#-directory-structure)
- [API Endpoints](#-api-endpoints)
- [Data Models](#-data-models)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Automated Workflow & Reminders](#-automated-workflow--reminders)
- [Security Features](#-security-features)
- [License](#-license)

---

## ✨ Features

- 🔐 **Secure Authentication:** JWT-based user authentication, password hashing with `bcryptjs`, and route protection middleware.
- 💳 **Complete Subscription CRUD:** Track monthly/yearly plans, renewal dates, categories, currencies, and cancellation statuses.
- ⏰ **Automated Renewal Reminders:** Smart email notification workflows scheduled at renewal intervals (e.g., 7 days, 5 days, 2 days, 1 day prior) via Upstash QStash.
- 🛡️ **Advanced Security (Arcjet):** Built-in rate limiting, bot protection, and email validation to prevent abuse and spam.
- 📧 **Dynamic Email Templates:** Beautiful, responsive HTML email templates powered by Nodemailer.
- ⚡ **Centralized Error Handling:** Consistent custom API error handling and validation middleware.

---

## 🛠️ Tech Stack

- **Runtime:** Node.js (ES6+ modules)
- **Framework:** Express.js
- **Database:** MongoDB via Mongoose ODM
- **Workflow & Scheduling:** Upstash Workflow / QStash
- **Security & Bot Detection:** Arcjet
- **Authentication:** JSON Web Tokens (JWT) & bcryptjs
- **Email Delivery:** Nodemailer
- **Code Quality:** ESLint

---

## 🏗️ System Architecture

```text
[ Client / Frontend ]
         │
         ▼
[ Express Router ] ──► [ Arcjet Shield & Rate Limiter ]
         │
         ▼
[ Auth Middleware (JWT) ]
         │
         ▼
[ Controllers & Services ] ──────► [ MongoDB Database ]
         │
         ▼
[ Upstash Workflow (QStash) ] ───► [ Nodemailer Service ] ──► [ User Inbox ]
```

---

## 📂 Directory Structure

```text
subscription-tracker/
├── app.js                    # Express app entry point & server bootstrap
├── package.json              # Project dependencies and scripts
├── eslint.config.js          # ESLint rules and configuration
├── config/
│   ├── env.js                # Centralized environment variable loader
│   ├── arcjet.js             # Arcjet security & rate-limit client setup
│   ├── nodemailer.js         # SMTP transporter setup
│   └── upstash.js            # Upstash Workflow / QStash client config
├── controllers/
│   ├── auth.controller.js    # Sign-up, sign-in, sign-out handlers
│   ├── subscription.controller.js # Subscription management logic
│   ├── user.controller.js    # User profile & administrative logic
│   └── workflow.controller.js# Upstash reminder step handlers
├── database/
│   └── mongodb.js            # MongoDB connection logic
├── middleware/
│   ├── auth.middleware.js    # JWT token verification
│   ├── arcjet.middleware.js  # Security & rate limiting middleware
│   └── error.middleware.js   # Global error handling middleware
├── models/
│   ├── user.model.js         # User schema & validation
│   └── subscription.model.js # Subscription schema & lifecycle hooks
├── routes/
│   ├── auth.routes.js        # /api/v1/auth
│   ├── user.routes.js        # /api/v1/users
│   ├── subscription.routes.js# /api/v1/subscriptions
│   └── workflow.routes.js    # /api/v1/workflows
└── utils/
    ├── email-template.js     # Responsive HTML email layout
    └── send-email.js         # Email dispatch helper
```

---

## 📡 API Endpoints

### 1. Authentication (`/api/v1/auth`)
| Method | Endpoint | Description | Auth |
| :--- | :--- | :--- | :---: |
| `POST` | `/api/v1/auth/sign-up` | Register a new user | No |
| `POST` | `/api/v1/auth/sign-in` | Authenticate user & issue JWT | No |
| `POST` | `/api/v1/auth/sign-out` | Log out and invalidate token | Yes |

### 2. Users (`/api/v1/users`)
| Method | Endpoint | Description | Auth |
| :--- | :--- | :--- | :---: |
| `GET` | `/api/v1/users` | List all users | Yes |
| `GET` | `/api/v1/users/:id` | Get user details by ID | Yes |
| `PUT` | `/api/v1/users/:id` | Update profile information | Yes |
| `DELETE` | `/api/v1/users/:id` | Remove user account | Yes |

### 3. Subscriptions (`/api/v1/subscriptions`)
| Method | Endpoint | Description | Auth |
| :--- | :--- | :--- | :---: |
| `GET` | `/api/v1/subscriptions` | Get all subscriptions | Yes |
| `POST` | `/api/v1/subscriptions` | Create a new subscription | Yes |
| `GET` | `/api/v1/subscriptions/:id` | Retrieve subscription details | Yes |
| `PUT` | `/api/v1/subscriptions/:id` | Update subscription metadata | Yes |
| `DELETE` | `/api/v1/subscriptions/:id` | Delete a subscription | Yes |
| `GET` | `/api/v1/subscriptions/user/:id` | Get all subscriptions for a specific user | Yes |
| `PUT` | `/api/v1/subscriptions/:id/cancel`| Mark subscription as cancelled/inactive | Yes |

### 4. Workflows (`/api/v1/workflows`)
| Method | Endpoint | Description | Auth |
| :--- | :--- | :--- | :---: |
| `POST` | `/api/v1/workflows/subscription/reminder` | Trigger scheduled reminder sequence | Internal |

---

## 📊 Data Models

### User Schema
| Field | Type | Validation / Options |
| :--- | :--- | :--- |
| `name` | `String` | Required, length: 2 - 50 |
| `email` | `String` | Required, unique, valid email format |
| `password` | `String` | Required, hashed via bcrypt |
| `createdAt` | `Date` | Auto-generated timestamp |
| `updatedAt` | `Date` | Auto-generated timestamp |

### Subscription Schema
| Field | Type | Validation / Options |
| :--- | :--- | :--- |
| `name` | `String` | Required, length: 2 - 100 |
| `price` | `Number` | Required, min: `0` |
| `currency` | `String` | Enum: `['USD', 'EUR', 'GBP']` (Default: `USD`) |
| `frequency` | `String` | Enum: `['daily', 'weekly', 'monthly', 'yearly']` |
| `category` | `String` | Enum: `['sports', 'news', 'entertainment', 'lifestyle', 'technology', 'finance', 'politics', 'other']` |
| `paymentMethod`| `String` | Required |
| `status` | `String` | Enum: `['active', 'cancelled', 'expired']` (Default: `active`) |
| `startDate` | `Date` | Required, cannot be future date |
| `renewalDate` | `Date` | Auto-computed based on frequency |
| `user` | `ObjectId` | Reference to `User` model |

---

## ⚙️ Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) (v18.x or higher)
- [MongoDB](https://www.mongodb.com/) (Local instance or MongoDB Atlas)
- Accounts for [Upstash](https://upstash.com/) and [Arcjet](https://arcjet.com/)

### 1. Clone and Install
```bash
git clone https://github.com/Navraj86/subscription-tracker.git
cd subscription-tracker
npm install
```

### 2. Configure Environment Variables
Create your local environment file:
```bash
cp .env.example .env.development.local
```

Fill in the environment variables described below.

### 3. Run the Application
```bash
# Development mode with Nodemon (auto-reload)
npm run dev

# Production mode
npm start
```
The server will boot on `http://localhost:3000`.

---

## 🔑 Environment Variables

| Variable | Description | Example |
| :--- | :--- | :--- |
| `PORT` | Server listening port | `3000` |
| `NODE_ENV` | Runtime environment | `development` / `production` |
| `SERVER_URL` | Base API URL | `http://localhost:3000` |
| `DB_URI` | MongoDB connection string | `mongodb+srv://...` |
| `JWT_SECRET` | Secret key for JWT signing | `your-secret-key` |
| `JWT_EXPIRES_IN`| Token lifespan | `7d` |
| `ARCJET_ENV` | Arcjet environment | `development` / `production` |
| `ARCJET_KEY` | Arcjet project API key | `ajkey_...` |
| `QSTASH_URL` | Upstash QStash URL | `https://qstash.upstash.io/...` |
| `QSTASH_TOKEN` | Upstash QStash Token | `ey...` |
| `EMAIL_HOST` | SMTP server host | `smtp.gmail.com` |
| `EMAIL_PORT` | SMTP server port | `587` |
| `EMAIL_USER` | Email account username/address | `you@gmail.com` |
| `EMAIL_PASS` | Email app password (e.g., Gmail App Password) | `xxxx xxxx xxxx xxxx` |

---

## 🔁 Automated Workflow & Reminders

When a user creates a new subscription:
1. **Renewal Calculation:** The API computes the next `renewalDate` based on `startDate` and `frequency`.
2. **Workflow Scheduling:** A call is made to the Upstash QStash workflow engine targeting `/api/v1/workflows/subscription/reminder`.
3. **Smart Delivery:** Upstash pauses and triggers reminder notifications sequentially (e.g. 7 days, 5 days, 2 days, and 1 day before the due date).
4. **Email Dispatch:** Nodemailer renders personalized HTML templates and delivers the reminder directly to the user's inbox.

---

## 🛡️ Security Features

- **Rate Limiting:** Managed through Arcjet to mitigate brute-force and DDoS attacks.
- **Bot Protection:** Detects and blocks automated bots and malicious scrapers.
- **JWT Protection:** Protected endpoints enforce valid bearer tokens.
- **Input Sanitization:** Mongoose validations prevent malformed or invalid inputs.

---

## 📄 License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.
