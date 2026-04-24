# Manual Testing Setup Guide

## Prerequisites

1. **Node.js** v18+ installed
2. **PocketBase** binary downloaded for your OS from https://pocketbase.io/docs/
3. **Git** installed

## Step 1: Start the Backend

1. Place the PocketBase binary in the `backend/` directory
2. Start PocketBase:

```bash
cd backend
./pocketbase serve
```

3. PocketBase Admin UI will be available at http://127.0.0.1:8090/_/
4. On first run, create a superuser account through the admin UI

## Step 2: Seed Initial Data

After creating your superuser account, run the seed script:

```bash
cd backend
node seed.js
```

This creates:
- 4 subjects (Mathematics, Science, Arabic Language, English Language)
- 3 class sections (1st Grade Section A, 2nd Grade Section A, 3rd Grade Section A)
- 1 admin user (admin@school.edu / Admin@12345)

## Step 3: Start the Frontend

In a new terminal:

```bash
cd frontend
npm install
npm run dev
```

The app will be available at http://localhost:3000

## Step 4: Create Test Users

Through the PocketBase Admin UI at http://127.0.0.1:8090/_/:

1. Create a teacher user:
   - Email: teacher@school.edu
   - Password: Teacher@12345
   - Role: teacher
   - Assign sections and subjects

2. Create a student user:
   - Email: student@school.edu
   - Password: Student@12345
   - Role: student
   - Assign a section

## Step 5: Test Login

Navigate to http://localhost:3000/login and test with:

| Role | Email | Password |
| :--- | :--- | :--- |
| Admin | admin@school.edu | Admin@12345 |
| Teacher | teacher@school.edu | Teacher@12345 |
| Student | student@school.edu | Student@12345 |

## Step 6: Verify Functionality

After logging in, verify:
- Dashboard loads correctly for each role
- Navigation sidebar/bottom tabs work
- Language switching (AR/EN) works
- CRUD operations work for each role
- Rich text editor loads without freezing

## Troubleshooting

- **PocketBase won't start:** Make sure port 8090 is not in use
- **Frontend won't start:** Run `npm install` first, ensure Node.js v18+
- **Login fails:** Check that PocketBase is running and users exist
- **Blank page:** Check browser console for errors (F12)
- **RTL issues:** Make sure you're accessing via /ar or /en prefix