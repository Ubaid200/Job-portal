# JobPortal - MERN Stack

**Overview:**

JobPortal is a modern web application built with the **MERN stack (MongoDB, Express, React, Node.js)**. It includes **JWT authentication**, secure middleware, and role-based access for **Job Seekers** and **Companies**. Both job seekers and employers can create, manage, and apply to jobs. The app also supports **file uploads** (resumes and company logos) and includes some placeholder images that can be replaced with real ones.

---

## 🔧 Features

* User roles: **Job Seeker** and **Company**
* JWT-based authentication (Access Token)
* Protected routes with role-based middleware
* RESTful API for Jobs, Users, Companies, and Applications
* Resume upload via `multer`
* Company profile with logo upload
* Job search, filter, and pagination
* Input validation with `express-validator`
* Centralized error handling
* CORS, Helmet (secure headers), and rate limiting

---

## 🧰 Tech Stack

* **Frontend:** React (CRA / Vite)
* **Backend:** Node.js + Express
* **Database:** MongoDB (Mongoose)
* **Authentication:** JWT (jsonwebtoken)
* **File Upload:** Multer
* **Dev Tools:** ESLint, Prettier, Nodemon

---

## 📁 Project Structure

```
/job-portal
├─ /client                # React app
│  ├─ /public
│  └─ /src
│     ├─ /components
│     ├─ /pages
│     └─ /services (API calls)
└─ /server                # Express app
   ├─ /config
   ├─ /controllers
   ├─ /middlewares
   ├─ /models
   ├─ /routes
   ├─ /utils
   └─ server.js
```

---

## ⚙️ Authentication Flow (JWT)

1. User registers or logs in to receive a **JWT token**.
2. Token is stored securely (HttpOnly cookie or localStorage for dev).
3. Protected routes use an `authMiddleware` that:

   * Reads `Authorization: Bearer <token>` header.
   * Verifies token using a secret key.
   * Attaches user info to `req.user` (id, role, email).
4. `roleMiddleware(['company'])` or `roleMiddleware(['seeker'])` is used to restrict access.

**Sample Middleware Example:**

```js
// authMiddleware.js
const jwt = require('jsonwebtoken');

module.exports = (req, res, next) => {
  const header = req.headers.authorization;
  if (!header) return res.status(401).json({ message: 'No token provided' });

  const token = header.split(' ')[1];
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ message: 'Invalid or expired token' });
  }
};
```

---

## 🛠 Middleware Examples

* **authMiddleware** — verifies JWT and attaches user data.
* **roleMiddleware(roles)** — checks user’s role access.
* **validateRequest(schema)** — validates inputs using Joi or express-validator.
* **errorHandler** — handles all thrown errors gracefully.
* **uploadMiddleware** — handles file uploads with Multer.
* **rateLimiter** — prevents abuse through excessive API calls.

---

## 🔌 API Endpoints

### Auth

* `POST /api/auth/register` — Register new user `{ name, email, password, role }`
* `POST /api/auth/login` — Login user and return JWT token

### Jobs

* `GET /api/jobs` — List jobs with filters & pagination
* `GET /api/jobs/:id` — Get job details
* `POST /api/jobs` — (Company only) Create job
* `PUT /api/jobs/:id` — (Company only) Update job
* `DELETE /api/jobs/:id` — (Company only) Delete job

### Applications

* `POST /api/jobs/:id/apply` — (Seeker) Apply with resume
* `GET /api/companies/:id/applications` — (Company) View applicants

### Companies

* `GET /api/companies/:id` — Get company profile
* `PUT /api/companies/:id` — (Company only) Update company profile

---

## 🧩 Environment Variables (`.env` Example)

```
PORT=5000
MONGO_URI=mongodb+srv://<user>:<password>@cluster.mongodb.net/jobportal
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=1d
CLIENT_URL=http://localhost:3000
UPLOAD_DIR=./uploads
```

---

## 🚀 Installation & Run

**Backend Setup**

```bash
cd server
npm install
cp .env.example .env
npm run dev
```

**Frontend Setup**

```bash
cd client
npm install
npm start
```

---

## 🧪 Seed Data (Optional)

You can create a `seed.js` script to pre-load dummy users, companies, and jobs for quick testing.

---

## 🖼️ Screenshots

Store images in `client/public/assets/` or `server/uploads/`.

![Landing Page](assets/landing-page.png)

![Job Details Page](assets/job-details.png)

> Replace these placeholders with your actual project screenshots.

---

## ✅ Security Best Practices

* Keep JWT secret strong and private.
* Implement refresh token flow for persistent login.
* Validate and sanitize file uploads.
* Use HTTPS in production.
* Enable CORS properly and apply rate limiting.

---

## 🧾 Testing

* **Unit Tests:** Jest + Supertest for API testing.
* **E2E Tests:** Cypress for frontend workflows.

---

## 🤝 Contribution Guide

1. Fork the repository.
2. Create a new feature branch.
3. Commit changes with a clear message.
4. Open a Pull Request.

---

## 📜 License

MIT License — Free to use and modify.

---

Would you like me to include **API response examples** (request + response JSON) or **deployment instructions** (Vercel + Render setup)?
