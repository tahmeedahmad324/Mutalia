# Mutalia

Mutalia is a full-stack study platform prototype with authentication, password reset via email, and a responsive web UI.

It combines:

- a Node.js + Express backend
- MongoDB user persistence
- cookie-based JWT authentication
- static frontend pages for landing, auth, and dashboard

## Features

### Implemented

- User sign up and login
- Secure JWT session in HTTP-only cookies
- Logout and protected current-user endpoint
- Forgot password flow (token generation + email)
- Reset password flow with token verification
- Static multi-page frontend (landing, auth pages, dashboard)
- Responsive UI with modernized landing page design

### In Progress / Placeholder UI

- Documents upload workflow (UI shell exists)
- Notes creation workflow (UI shell exists)
- Discussion forum workflow (UI shell exists)
- AI chatbot integration (currently mock responses)

## Tech Stack

### Backend

- Node.js
- Express
- MongoDB + Mongoose
- JSON Web Tokens
- bcryptjs
- Nodemailer

### Frontend

- HTML, CSS, Vanilla JavaScript

## Project Structure

```text
Mutalia/
|-- controllers/
|   `-- auth.controller.js
|-- middleware/
|   `-- auth.middleware.js
|-- models/
|   `-- user.model.js
|-- public/
|   |-- index.html
|   |-- login.html
|   |-- sign-up.html
|   |-- forgot-password.html
|   |-- reset-password.html
|   |-- dashboard.html
|   |-- styles.css
|   |-- dashboard.css
|   `-- dashboard.js
|-- routes/
|   `-- auth.routes.js
|-- services/
|   `-- email.service.js
|-- server.js
|-- package.json
`-- README.md
```

## Getting Started

### Prerequisites

- Node.js 18+
- npm 9+
- MongoDB instance (local or Atlas)

### Installation

```bash
npm install
```

### Environment Variables

Create a `.env` file in the project root.

| Variable | Required | Description |
|---|---|---|
| `PORT` | No | Server port (default: `3001`) |
| `MONGODB_URI` | Yes | MongoDB connection string |
| `JWT_SECRET` | Yes | Secret used to sign/verify JWT tokens |
| `FRONTEND_URL` | Recommended | Allowed CORS origin and reset-link base URL |
| `NODE_ENV` | Recommended | `production` enables secure cookies |
| `EMAIL_SERVICE` | Optional* | Email provider name (example: `gmail`) |
| `EMAIL_USER` | Optional* | Email account username/address |
| `EMAIL_PASSWORD` | Optional* | Email account password/app password |
| `SMTP_HOST` | Optional* | SMTP host if not using `EMAIL_SERVICE` |
| `SMTP_PORT` | Optional* | SMTP port (default used: `587`) |
| `SMTP_SECURE` | Optional* | `true`/`false` for secure SMTP |

`*` Use either `EMAIL_SERVICE` + `EMAIL_USER` + `EMAIL_PASSWORD`, or SMTP variables.

If no email settings are present, the app falls back to an Ethereal test account for development.

### Run the App

```bash
# development (with auto-reload)
npm run dev

# production mode
npm start
```

Open:

- `http://localhost:3001`

## API Reference

Base path: `/api/auth`

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `POST` | `/signup` | No | Create account |
| `POST` | `/login` | No | Login and set JWT cookie |
| `POST` | `/logout` | Cookie | Clear auth cookie |
| `GET` | `/user` | Cookie | Get current authenticated user |
| `POST` | `/forgot-password` | No | Generate reset token + send reset email |
| `GET` | `/verify-token` | No | Validate reset token |
| `POST` | `/reset-password` | No | Set new password using token |
| `POST` | `/test-email` | No | Send test email (debug endpoint) |

### Sample Payloads

Sign up:

```json
{
	"username": "student123",
	"email": "student@example.com",
	"password": "StrongPass1"
}
```

Login:

```json
{
	"email": "student@example.com",
	"password": "StrongPass1"
}
```

Reset password:

```json
{
	"token": "reset_token_from_email",
	"password": "NewStrongPass1"
}
```

## Frontend Routes

The server statically serves files from `public/` and falls back to `public/index.html` for unmatched non-API routes.

- `/` -> landing page
- `/login.html` -> login
- `/sign-up.html` -> registration
- `/forgot-password.html` -> request password reset
- `/reset-password.html?token=...` -> reset password
- `/dashboard.html` -> authenticated dashboard UI

## Deployment Notes (Vercel / Node Hosting)

- Configure all required environment variables in your hosting dashboard.
- Ensure `FRONTEND_URL` matches your deployed domain.
- Use a production-grade MongoDB URI.
- For Gmail, use an app password instead of your account password.
- Set `NODE_ENV=production` to enforce secure cookie behavior.

## Security Notes

- JWT is stored in an HTTP-only cookie to reduce XSS token exposure.
- Passwords are hashed with bcrypt before storage.
- Forgot-password endpoint does not reveal whether an email exists.
- Use HTTPS in production for secure cookie transport.

## Current Status

Mutalia currently has a strong authentication foundation and polished UI baseline.
Content management, real forum interactions, and full AI integration are the next major milestones.

## License

ISC
