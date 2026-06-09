# VIA Backend — Tour Booking REST API

A production-ready RESTful API for a tour booking platform, built with **Node.js**, **Express**, and **MongoDB**. Designed with a focus on security, scalability, and clean architecture — covering everything from JWT authentication to advanced query filtering.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MongoDB + Mongoose ODM |
| Authentication | JSON Web Tokens (JWT) |
| Templating | Pug |
| Email | Nodemailer |
| Security | Helmet, HPP, express-mongo-sanitize, xss-clean, express-rate-limit |
| Dev Tools | ESLint (Airbnb config), Prettier, ndb |

---

## Features

- **JWT Authentication & Authorization** — Secure sign-up, login, password reset via email, and role-based access control
- **Advanced API Features** — Filtering, sorting, field selection, and pagination built into query handling
- **Security Hardened** — Rate limiting, HTTP headers via Helmet, NoSQL injection prevention, XSS sanitization, and HTTP parameter pollution protection
- **Password Management** — bcrypt hashing, forgot-password flow with tokenized email links
- **Server-side Rendering** — Pug templates served for select views (e.g. email templates, booking confirmations)
- **Environment-based Config** — Separate development and production modes with `.env` configuration
- **Clean MVC Architecture** — Controllers, Models, Routes, and Utils are clearly separated

---

## Project Structure

```
via-backend/
├── controllers/        # Route handlers & business logic
├── models/             # Mongoose schemas & model methods
├── routes/             # Express router definitions
├── utils/              # Reusable helpers (AppError, APIFeatures, email)
├── public/             # Static files
├── dev-data/           # Seed data for development
├── app.js              # Express app setup & middleware
├── server.js           # DB connection & server bootstrap
└── config.env          # Environment variables
```

---

## API Endpoints (Overview)

### Auth
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/v1/users/signup` | Register a new user |
| POST | `/api/v1/users/login` | Login and receive JWT |
| POST | `/api/v1/users/forgotPassword` | Send password reset email |
| PATCH | `/api/v1/users/resetPassword/:token` | Reset password via token |

### Tours
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/v1/tours` | Get all tours (filterable, sortable, paginated) |
| GET | `/api/v1/tours/:id` | Get a single tour |
| POST | `/api/v1/tours` | Create a tour *(admin/lead-guide)* |
| PATCH | `/api/v1/tours/:id` | Update a tour *(admin/lead-guide)* |
| DELETE | `/api/v1/tours/:id` | Delete a tour *(admin)* |

### Users
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/v1/users/me` | Get current user profile |
| PATCH | `/api/v1/users/updateMe` | Update current user data |
| DELETE | `/api/v1/users/deleteMe` | Deactivate current user account |

---

## Getting Started

### Prerequisites
- Node.js
- MongoDB (local or Atlas)

### Installation

```bash
# Clone the repository
git clone https://github.com/Aman-AS1/via-backend.git
cd via-backend

# Install dependencies
npm install
```

### Environment Setup

Create or update `config.env` with your values:

```env
NODE_ENV=development
PORT=3000

DATABASE=mongodb+srv://<user>:<password>@cluster.mongodb.net/via
DATABASE_PASSWORD=your_db_password

JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRES_IN=90d
JWT_COOKIE_EXPIRES_IN=90

EMAIL_USERNAME=your_mailtrap_user
EMAIL_PASSWORD=your_mailtrap_pass
EMAIL_HOST=smtp.mailtrap.io
EMAIL_PORT=25
```

### Run

```bash
# Development (with auto-reload)
npm start

# Production
npm run start:prod

# Debug mode
npm run debug
```

---

## Security Practices

- **Rate Limiting** — Limits requests per IP to prevent brute-force attacks
- **Helmet** — Sets secure HTTP headers
- **NoSQL Injection Prevention** — `express-mongo-sanitize` strips `$` and `.` from user input
- **XSS Protection** — `xss-clean` sanitizes HTML in request body
- **HPP** — Prevents HTTP parameter pollution by whitelisting safe duplicate query params
- **Password Hashing** — bcryptjs with salt rounds

---

## Key Design Decisions

- **APIFeatures class** in `utils/` abstracts query chaining (filter → sort → project → paginate) for reuse across any model
- **AppError class** provides a consistent, structured error format throughout the app
- **Global error handler** differentiates between operational errors (sent to client) and programming bugs (logged only)
- **Async wrapper** eliminates repetitive try/catch blocks in async route handlers

---
