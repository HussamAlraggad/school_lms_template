# School Learning Platform Template

A comprehensive, bilingual (Arabic and English) educational platform template designed to centralize school activities, resources, and communication. Built to serve students from Kindergarten to higher grades, this template provides a calm, structured, and professional digital environment for the entire school community.

---

## Platform Overview

The School Learning Platform bridges the gap between the classroom and home by providing specialized, role-based tools:

### Core Features

- **Homework Management:** A complete system to assign, submit, and track homework with automatic grading for interactive tasks.
- **Educational Materials:** A centralized resource library where study files, videos, and documents can be accessed by subject and grade level.
- **Interactive Quizzes:** Timed, multiple-choice quizzes featuring automated grading and progress tracking.
- **News and Announcements:** A central feed for school-wide or section-specific updates, events, and notices, complete with a community commenting system.
- **Exam Schedules:** Clear, accessible timetables for upcoming midterm, final, and practical exams.
- **Offline Access:** Core content remains accessible even without an active internet connection.

### User Roles and Capabilities

- **Students:** Access a personalized, grade-specific dashboard. View and download study materials, submit online or on-site homework, take timed interactive quizzes, view exam schedules, and engage with school announcements.
- **Teachers:** Manage assigned subjects and class sections. Create and publish rich-text educational materials, assign homework, build interactive quizzes, review and grade student submissions, and post announcements for their specific classes.
- **Administrators:** Maintain ultimate global control over the platform. Manage all user accounts, structure the academic year (creating grades, sections, and subjects), unconditionally moderate all content and communications, and monitor system-wide engagement metrics.

---

## Tech Stack

This project utilizes a modern, decoupled architecture designed for speed, reliability, and strict design adherence:

- **Frontend:** Next.js 16 (App Router) and React 19
- **Backend and Database:** PocketBase (Single-file SQLite database, authentication, and file storage)
- **Styling:** Tailwind CSS v4 (using native CSS variables for a strict, minimalistic design system)
- **Rich Text Editing:** Tiptap v3 (with DOMPurify for secure HTML rendering)
- **Typography:** Cairo font (optimized for Arabic-First RTL layout and English LTR fallback)
- **Data Fetching:** TanStack React Query v5

---

## Setup Guide

Because the backend relies on PocketBase, spinning up the development environment is fast and native to any operating system without needing Docker.

### 1. Prerequisites

Ensure you have the following installed on your machine:

- **Node.js** (v18.x or higher, v20 recommended)
- **Git**
- **PocketBase Executable** (Download the single binary for your specific OS from the [PocketBase website](https://pocketbase.io/docs/))

### 2. Clone the Repository

```bash
git clone <your-repo-url>
cd school_learning_platform_template
```

### 3. Start the Backend (PocketBase)

Navigate to the backend directory and place your PocketBase executable there.

```bash
cd backend

# Linux / macOS
./pocketbase serve

# Windows
pocketbase.exe serve
```

The backend API and Admin Dashboard will now be running at `http://127.0.0.1:8090`.

### 4. Seed Initial Data (First Time Only)

After starting PocketBase for the first time, create a superuser account through the admin UI at `http://127.0.0.1:8090/_/`. Then run the seed script:

```bash
cd backend
node seed.js
```

This creates initial subjects, class sections, and an admin user.

### 5. Start the Frontend (Next.js)

Open a new terminal window, leave the backend running:

```bash
cd frontend

# Install all required dependencies
npm install

# Start the Next.js development server
npm run dev
```

The web application will now be running at `http://localhost:3000`.

---

## Default Credentials

The database is pre-seeded with test accounts across all roles. Navigate to `http://localhost:3000/login` and use the following credentials:

| Role | Email | Password |
| :--- | :--- | :--- |
| **Admin** | `admin@school.edu` | `Admin@12345` |

*(Note: To access the raw PocketBase database interface, navigate to `http://127.0.0.1:8090/_/` and log in with your superuser credentials)*

---

## Customization Checklist

Before deploying this template for your school, customize the following:

1. **School Name:** Update defaults in `frontend/src/context/settings-context.tsx`
2. **Page Metadata:** Update title and description in `frontend/src/app/layout.tsx`
3. **Email Domain:** Update in `frontend/src/lib/transliteration.ts`
4. **Translations:** Update school name references in `frontend/src/dictionaries/ar.json` and `en.json`
5. **Seed Data:** Update admin credentials and school data in `backend/seed.js`
6. **School Logo:** Add your logo to `frontend/public/logo.webp`
7. **Design Tokens:** Adjust colors in `frontend/src/app/globals.css` and `frontend/src/lib/design-tokens.ts`
8. **PocketBase URL:** Update in `frontend/.env` if not using localhost

---

## Project Structure

```
school_learning_platform_template/
├── frontend/              # Next.js 16 frontend application
├── backend/               # PocketBase backend
├── docs/                  # Organized documentation
├── journal.md             # Single source of truth - iteration history
├── README.md              # Project overview (this file)
├── AGENTS.md              # AI agent architectural guidelines
├── opencode.json          # OpenCode configuration
└── .gitignore             # Git ignore rules
```

See [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md) for detailed file-level documentation.

---

## Contribution and Development Rules

If you are developing or running AI agents on this codebase, you **must** adhere to the following strict guidelines:

1. **Single Source of Truth:** Always refer to `journal.md` for project milestones, completed tasks, and iteration history.
2. **Design System:** Maintain a minimal, gentle, and clean aesthetic. No playful naming conventions, no emojis, and no heavy animations.
3. **Iteration Logging:** Every completed task or bug fix must be manually logged in the `journal.md` file, including details on what was fixed and any issues encountered to preserve context for future development.

---

## License

MIT