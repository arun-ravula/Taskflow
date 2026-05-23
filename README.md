# TaskFlow — Team Task Manager

A full-stack web application for managing projects, assigning tasks, and tracking team progress with role-based access control.

## 🚀 Live Demo
> **[https://your-app.railway.app](https://your-app.railway.app)**
> 
> Demo credentials:
> - **Admin:** admin@demo.com / demo123
> - **Member:** member@demo.com / demo123

---

## ✨ Features

- **Authentication** — Signup/Login with JWT tokens, 7-day session
- **Role-Based Access Control** — Global Admin & Member roles + per-project Admin/Member roles
- **Project Management** — Create, view, edit, delete projects; manage team members
- **Task Management** — Create tasks with title, description, priority, due date, assignee
- **Kanban Board** — Visual To Do / In Progress / Done columns per project
- **Dashboard** — Stats overview (total, todo, in-progress, done, overdue, my tasks)
- **Overdue Tracking** — Highlighted overdue tasks with visual warnings
- **Filters** — Filter tasks by status and priority
- **Responsive UI** — Works on desktop and mobile

---

## 🏗️ Tech Stack

| Layer | Tech |
|---|---|
| Backend | Node.js + Express |
| Database | SQLite (via better-sqlite3) |
| Auth | JWT + bcryptjs |
| Frontend | React 18 + React Router v6 |
| HTTP Client | Axios |
| Icons | Lucide React |
| Build | Vite |
| Deployment | Railway |

---

## 🗄️ Database Schema

```
users           — id, name, email, password (hashed), role, created_at
projects        — id, name, description, owner_id, created_at
project_members — project_id, user_id, role (admin/member), joined_at
tasks           — id, title, description, project_id, assignee_id, created_by,
                  status, priority, due_date, created_at, updated_at
```

---

## 🔑 Role-Based Access Control

| Action | Global Admin | Project Admin | Project Member | Non-member |
|---|:-:|:-:|:-:|:-:|
| View all projects | ✅ | — | — | ❌ |
| Create project | ✅ | ✅ | ✅ | ✅ |
| Delete project | ✅ | ✅ | ❌ | ❌ |
| Add/remove members | ✅ | ✅ | ❌ | ❌ |
| Create task | ✅ | ✅ | ✅ | ❌ |
| Edit any task | ✅ | ✅ | ❌ | ❌ |
| Update own task status | ✅ | ✅ | ✅ (assignee) | ❌ |
| Delete task | ✅ | ✅ | (creator only) | ❌ |

---

## 📡 REST API Reference

### Auth
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/signup` | Register new user |
| POST | `/api/auth/login` | Login |
| GET | `/api/auth/me` | Current user info |
| GET | `/api/auth/users` | List all users (for member search) |

### Projects
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/projects` | List user's projects |
| POST | `/api/projects` | Create project |
| GET | `/api/projects/:id` | Project + members |
| PUT | `/api/projects/:id` | Update project |
| DELETE | `/api/projects/:id` | Delete project |
| POST | `/api/projects/:id/members` | Add member |
| DELETE | `/api/projects/:id/members/:userId` | Remove member |

### Tasks
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/tasks/dashboard` | Dashboard stats |
| GET | `/api/tasks/my` | My assigned tasks |
| GET | `/api/tasks/project/:id` | Tasks for project (filterable) |
| POST | `/api/tasks` | Create task |
| PUT | `/api/tasks/:id` | Update task |
| DELETE | `/api/tasks/:id` | Delete task |

---

## 🛠️ Local Development

### Prerequisites
- Node.js 18+
- npm

### Setup

```bash
# Clone
git clone https://github.com/yourusername/taskflow.git
cd taskflow

# Install all dependencies
npm run install:all

# Configure backend
cd backend
cp .env.example .env
# Edit .env — set JWT_SECRET

# Start backend (port 5000)
npm run dev:backend

# In another terminal, start frontend (port 5173)
cd ..
npm run dev:frontend
```

Visit `http://localhost:5173`

---

## 🚂 Deploy to Railway

### Step-by-Step

1. **Push to GitHub**
   ```bash
   git init && git add . && git commit -m "Initial commit"
   git remote add origin https://github.com/YOUR/REPO.git
   git push -u origin main
   ```

2. **Create Railway project**
   - Go to [railway.app](https://railway.app) → New Project → Deploy from GitHub repo
   - Select your repository

3. **Set Environment Variables** in Railway dashboard:
   ```
   NODE_ENV=production
   JWT_SECRET=<strong-random-secret>
   PORT=5000
   DB_PATH=/data/taskmanager.db
   ```

4. **Add a Volume** (optional, for persistent DB):
   - Railway → your service → Volumes → Add Volume
   - Mount path: `/data`

5. **Deploy** — Railway auto-builds and deploys. Your app goes live at the generated URL.

6. **Create demo accounts** — visit your live URL, sign up with:
   - admin@demo.com / demo123 (role: Admin)
   - member@demo.com / demo123 (role: Member)

---

## 📁 Project Structure

```
taskflow/
├── backend/
│   ├── routes/
│   │   ├── auth.js          # Signup, login, /me
│   │   ├── projects.js      # Project CRUD + members
│   │   └── tasks.js         # Task CRUD + dashboard
│   ├── db.js                # SQLite init & schema
│   ├── middleware.js         # JWT auth + RBAC guards
│   ├── server.js            # Express entry point
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── Layout.jsx   # Sidebar + navigation
│   │   ├── context/
│   │   │   └── AuthContext.jsx
│   │   ├── pages/
│   │   │   ├── Login.jsx
│   │   │   ├── Signup.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Projects.jsx
│   │   │   └── ProjectDetail.jsx  # Kanban board
│   │   ├── utils/
│   │   │   └── api.js       # Axios instance
│   │   ├── App.jsx
│   │   ├── index.css
│   │   └── main.jsx
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
├── nixpacks.toml            # Railway build config
├── railway.toml
├── package.json             # Root scripts
├── .gitignore
└── README.md
```
