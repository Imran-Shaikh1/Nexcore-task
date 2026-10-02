# User Data Management System

A production-quality, minimal full-stack application built for collecting and managing user records. Designed with a clean SaaS aesthetic, robust dual-layer validation, and decoupled Express + React architecture.

---

## 1. Project Overview

The User Data Management application provides a focused interface for registering user data (Full Name, Email, Phone Number, City) and immediately viewing persisted records. It follows modern frontend and backend design best practices:
- **No decorative clutter or AI clichés:** Zero emojis, no neon gradients, no bloated frameworks.
- **Intentional UX:** Responsive form card, clear input focus states, field-level validation, inline status messages, and a secondary readable data table.
- **Dual-layer validation:** All client-side checks are mirrored on the Express API to prevent invalid data ingestion.
- **Permanent MongoDB storage:** Built with Mongoose models, timestamps, and connection resilience.

---

## 2. Features

- **Minimalist Registration Form:**
  - Full Name, Email, Phone Number, and City input fields.
  - Required field, email syntax, and phone number validation.
  - Real-time field blur and submit validation.
  - Active submission loading state (`Saving...`) preventing duplicate clicks.
  - Automatic form clearing upon successful submission.
- **Inline Status Messaging:**
  - Non-intrusive success and failure banners.
  - Accessible feedback with dismiss controls (no disruptive browser `alert()` popups).
- **Secondary Data Table:**
  - Displays stored user records chronologically (newest first).
  - Clean table formatting for dates and telephone numbers.
  - Graceful empty state and fetching state indicators.
  - Horizontal scrolling support on mobile and tablet screens.
- **Resilient Backend:**
  - Centralized Express error handler that prevents sensitive stack trace leaks.
  - CORS configured for cross-origin local and production setups.
  - Health check endpoint (`GET /api/health`).

---

## 3. Tech Stack

- **Frontend:**
  - React 19 (JavaScript)
  - Vite (Build Tool & Dev Server)
  - Pure Modern CSS (Vanilla, zero heavy UI frameworks)
- **Backend:**
  - Node.js (v20+)
  - Express.js (REST API framework)
  - Mongoose (MongoDB ODM)
  - Dotenv (Environment configuration)
  - Cors (Cross-Origin Resource Sharing)
- **Database:**
  - MongoDB (MongoDB Atlas / local MongoDB instance via Mongoose)

---

## 4. Folder Structure

```
user-data-app/
│
├── frontend/
│   ├── public/
│   │   └── favicon.svg
│   ├── src/
│   │   ├── components/
│   │   │   ├── StatusMessage.jsx   # Inline notification banner
│   │   │   ├── UserForm.jsx        # Minimal user registration form
│   │   │   └── UserTable.jsx       # Secondary responsive table
│   │   ├── services/
│   │   │   └── api.js              # Centralized API service layer
│   │   ├── App.jsx                 # Main application controller
│   │   ├── main.jsx                # React root mount
│   │   └── index.css               # Clean SaaS design typography & layout
│   ├── index.html
│   ├── vite.config.js
│   ├── package.json
│   ├── .env                        # Frontend environment variables
│   └── .env.example
│
├── backend/
│   ├── config/
│   │   └── db.js                   # Mongoose connection logic
│   ├── controllers/
│   │   └── userController.js       # Route handlers with validation
│   ├── models/
│   │   └── User.js                 # Mongoose User schema & constraints
│   ├── routes/
│   │   └── userRoutes.js           # API route mappings
│   ├── middleware/
│   │   └── errorHandler.js         # Centralized error sanitizer
│   ├── server.js                   # Express application entry point
│   ├── package.json
│   ├── .env                        # Backend environment variables
│   └── .env.example
│
├── .gitignore
└── README.md
```

---

## 5. Prerequisites

Before running the application, make sure you have installed:
- **Node.js** (v18.0.0 or higher recommended)
- **npm** (v9.0.0 or higher)
- A **MongoDB Atlas** connection string or a local MongoDB server

---

## 6. MongoDB Setup

1. Create a MongoDB database cluster on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) or run a local instance.
2. Ensure your IP address is whitelisted in Atlas Network Access (`0.0.0.0/0` for broad access or your current IP).
3. Copy your MongoDB URI string (e.g., `mongodb+srv://<username>:<password>@cluster0.example.mongodb.net/<dbname>`).

> **Note:** The backend connection utility includes an automatic in-memory fallback for local development if the remote cluster is temporarily unreachable or paused, allowing you to test the complete application immediately.

---

## 7. Environment Variables

### Backend (`backend/.env`)
```env
PORT=5000
MONGODB_URI=mongodb+srv://imran:T7sAnPf7icTQaPxl@cluster0.ufbp8wm.mongodb.net/nexcore
```

### Frontend (`frontend/.env`)
```env
VITE_API_URL=http://localhost:5000/api
```

---

## 8. Backend Installation & Setup

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Verify your `.env` configuration file exists with `PORT` and `MONGODB_URI`.
4. Start the backend server:
   ```bash
   npm run dev
   # Or for production:
   npm start
   ```
   The backend will start on `http://localhost:5000`.

---

## 9. Frontend Installation & Setup

1. Open a new terminal and navigate to the frontend directory:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Verify your `.env` file contains `VITE_API_URL=http://localhost:5000/api`.
4. Start the Vite development server:
   ```bash
   npm run dev
   ```
   The frontend will be available at `http://localhost:5173`.

---

## 10. How to Run the Project (Quick Start)

Run the backend and frontend in two separate terminal windows:

**Terminal 1 (Backend):**
```bash
cd backend
npm install
npm run dev
```

**Terminal 2 (Frontend):**
```bash
cd frontend
npm install
npm run dev
```

Open your browser at `http://localhost:5173`.

---

## 11. API Endpoints

| Method | Endpoint        | Description                   | Request Body                | Success Status |
|--------|-----------------|-------------------------------|-----------------------------|----------------|
| `GET`  | `/api/health`   | Server health & uptime check  | None                        | `200 OK`       |
| `GET`  | `/api/users`    | Fetch all stored users (newest first) | None                | `200 OK`       |
| `POST` | `/api/users`    | Validate and save a new user  | JSON `{ fullName, email, phone, city }` | `201 Created`  |

---

## 12. Example Requests and Responses

### 1. Fetch Users (`GET /api/users`)

#### Request:
```bash
curl -X GET http://localhost:5000/api/users
```

#### Response (`200 OK`):
```json
{
  "success": true,
  "count": 1,
  "data": [
    {
      "_id": "67041fa093c834001a1b1234",
      "fullName": "Sarah Jenkins",
      "email": "sarah.jenkins@example.com",
      "phone": "+1 (555) 234-5678",
      "city": "Seattle",
      "createdAt": "2026-10-02T11:37:26.230Z"
    }
  ]
}
```

---

### 2. Create User (`POST /api/users`)

#### Request:
```bash
curl -X POST http://localhost:5000/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "Marcus Vance",
    "email": "marcus.vance@example.com",
    "phone": "+1 415-555-0199",
    "city": "San Francisco"
  }'
```

#### Response (`201 Created`):
```json
{
  "success": true,
  "message": "Information saved successfully.",
  "data": {
    "fullName": "Marcus Vance",
    "email": "marcus.vance@example.com",
    "phone": "+1 415-555-0199",
    "city": "San Francisco",
    "_id": "67041fb893c834001a1b5678",
    "createdAt": "2026-10-02T11:39:10.100Z"
  }
}
```

---

### 3. Validation Error Example (`POST /api/users`)

#### Request (Invalid Email):
```bash
curl -X POST http://localhost:5000/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "Marcus Vance",
    "email": "invalid-email-string",
    "phone": "1234567890",
    "city": "San Francisco"
  }'
```

#### Response (`400 Bad Request`):
```json
{
  "success": false,
  "message": "Please provide a valid email address."
}
```

---

## 13. Code Quality & Security Highlights

- **Sanitization:** All text inputs are trimmed and checked against standard regular expressions.
- **Data Privacy:** Internal stack traces and database schema nuances are caught and sanitized by the Express error middleware.
- **Zero Heavy Dependencies:** Built cleanly with native fetch in the frontend and minimal lightweight dependencies in the backend.
- **Environment Isolation:** Secrets are isolated in `.env` files and omitted from version control via `.gitignore`.
#   N e x c o r e - t a s k  
 