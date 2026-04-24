# Project Structure

## Root Directory

```
school_learning_platform_template/
├── frontend/              # Next.js 16 frontend application
├── backend/               # PocketBase backend
├── docs/                  # Organized documentation
├── journal.md             # Single source of truth - iteration history
├── README.md              # Project overview
├── AGENTS.md              # AI agent architectural guidelines
├── opencode.json          # OpenCode configuration
└── .gitignore             # Git ignore rules
```

## Documentation Structure (`/docs`)

### `/docs/architecture` - System Design and Patterns
- **UX_IMPLEMENTATION_PLAN.md** - 5-phase UX refactoring roadmap with hook patterns and accessibility methodology
- **PERFORMANCE_PLAN.md** - Performance optimization strategy (code splitting, React Query, lazy loading)
- **UX_FIX_IMPLEMENTATION_GUIDE.md** - Implementation patterns for custom hooks and error handling

### `/docs/testing` - Testing Guides
- **COMPREHENSIVE_TESTING_GUIDE.md** - Complete testing guide with 100+ test cases
- **TESTING_CHECKLIST.md** - 8-phase manual testing checklist
- **MANUAL_TESTING_SETUP.md** - Setup instructions for browser testing

### `/docs/audit` - Quality Assurance
- **UX_VIOLATIONS_SUMMARY.md** - Common React state management anti-patterns and solutions

### `/docs/archive` - Feature Specifications
- **EXCEL_IMPORT_EXPORT_SPEC.md** - Student bulk import/export feature specification

## Application Code

### `/frontend` - Next.js 16 Frontend

```
frontend/
├── src/
│   ├── app/                    # App Router pages (locale-prefixed routes)
│   │   ├── layout.tsx          # Root layout (providers, fonts)
│   │   ├── page.tsx            # Root redirect to default locale
│   │   ├── globals.css         # Design tokens and global styles
│   │   ├── [lang]/             # Locale-prefixed routes
│   │   │   ├── layout.tsx      # Locale layout (dictionary, direction)
│   │   │   ├── page.tsx        # Auth gateway (redirect to dashboard/login)
│   │   │   ├── login/          # Login page
│   │   │   └── dashboard/      # Dashboard routes
│   │   │       ├── layout.tsx  # Dashboard shell (header, auth guard)
│   │   │       ├── page.tsx    # Role redirect
│   │   │       ├── admin/      # Admin dashboard (9 pages)
│   │   │       ├── teacher/    # Teacher dashboard (7 pages)
│   │   │       └── student/    # Student dashboard (7 pages)
│   │   └── api/auth/           # Auth API routes (login, cookie, logout)
│   ├── components/             # Reusable UI components
│   │   ├── ui/                 # Primitive UI components (Button, Input, Dialog, etc.)
│   │   ├── composite/          # Composite components (DataTable, FilterBar, CrudFormModal, etc.)
│   │   ├── forms/              # Form components (FormSelect)
│   │   ├── layouts/            # Layout components (PageStatsLayout)
│   │   ├── error-boundary.tsx  # Error boundary component
│   │   ├── html-attributes.tsx # HTML lang/dir attribute sync
│   │   ├── LazyLoad.tsx        # Lazy loading HOC
│   │   └── index.ts            # Barrel exports
│   ├── context/                # React context providers
│   │   ├── auth-context.tsx    # Authentication state
│   │   ├── dialog-context.tsx  # Imperative alert/confirm dialogs
│   │   ├── locale-context.tsx  # Locale/dictionary/RTL state
│   │   └── settings-context.tsx # Platform settings (school name, feature flags)
│   ├── dictionaries/           # Translation files
│   │   ├── ar.json             # Arabic translations
│   │   └── en.json             # English translations
│   ├── lib/                    # Utilities and hooks
│   │   ├── auth.ts             # Authentication utilities
│   │   ├── csv-parser.ts       # Student CSV/Excel file parser
│   │   ├── design-tokens.ts    # Design system tokens (colors, spacing, typography)
│   │   ├── i18n.ts             # Server-side i18n dictionary loader
│   │   ├── locale-config.ts    # Client-safe locale configuration
│   │   ├── pocketbase.ts       # PocketBase singleton with cookie sync
│   │   ├── react-query.tsx     # React Query provider and client
│   │   ├── text-direction.ts   # Arabic/RTL text detection utilities
│   │   ├── transliteration.ts  # Arabic-to-Latin transliteration and email generation
│   │   └── hooks/              # Custom state management hooks
│   │       ├── useCrudState.ts  # CRUD UI state
│   │       ├── useFormState.ts  # Form data and validation
│   │       ├── useFilterState.ts # Search/filter/pagination
│   │       ├── useTabState.ts   # Tab navigation
│   │       ├── useInfiniteScroll.ts # Intersection Observer infinite scroll
│   │       ├── usePagination.ts # Pagination state
│   │       └── useUserQueries.ts # React Query hooks for users/sections/subjects
│   └── proxy.ts                # Next.js middleware (auth, locale, role routing)
├── public/                     # Static assets
├── package.json                # Dependencies and scripts
├── next.config.ts              # Next.js configuration
├── tsconfig.json               # TypeScript configuration
├── jest.config.js              # Jest configuration
├── jest.setup.js               # Jest setup
├── eslint.config.mjs           # ESLint configuration
├── postcss.config.mjs          # PostCSS configuration
└── .env                        # Environment variables
```

### `/backend` - PocketBase Backend

```
backend/
├── pb_migrations/              # Database schema migrations (51 files)
├── seed.js                     # Initial data seeding script
├── seed_admin.sh               # Admin user creation helper
├── fix_rules.js                # PocketBase API rule fixer
├── package.json                # Dependencies
├── CHANGELOG.md                # PocketBase changelog
└── LICENSE.md                  # PocketBase license
```

## PocketBase Collections

| Collection | Purpose | Key Fields |
|---|---|---|
| **users** (`_pb_users_auth_`) | User accounts | email, password, name_ar, name_en, role, sections, subjects, section |
| **class_sections** | Grade sections | grade_ar, grade_en, grade_order, section_ar, section_en |
| **subjects** | School subjects | name_ar, name_en, code |
| **materials** | Learning materials | title, body, link_url, teacher, section, subject, attachment |
| **homework** | Homework assignments | title, description, due_date, submission_type, teacher, section, subject |
| **submissions** | Homework submissions | homework, student, content, status, grade, feedback |
| **announcements** | School announcements | title, body, author, scope (global/section), section |
| **quizzes** | Interactive quizzes | title, description, time_limit, teacher, section, subject, opens_at, closes_at |
| **quiz_questions** | Quiz questions | quiz, question_text, options (JSON), correct_answer, order |
| **quiz_attempts** | Student quiz attempts | quiz, student, answers (JSON), score, total_questions, started_at, submitted_at |
| **exam_schedules** | Exam timetables | title, subject, section, exam_date, start_time, end_time, exam_type, notes |
| **comments** | Content comments | content, author, target_type (announcement/material), target_id |
| **reactions** | Content reactions | type (like/love/helpful), user, target_type, target_id |
| **media** | Uploaded files | file, uploaded_by |
| **platform_settings** | Global settings | key, value (JSON) |

## Essential Root Files

| File | Purpose |
|---|---|
| `journal.md` | **SINGLE SOURCE OF TRUTH** - Complete iteration history |
| `README.md` | Project overview and quick start guide |
| `AGENTS.md` | AI agent architectural guidelines and tech stack |
| `.gitignore` | Git ignore rules (excludes pb_data, .env, node_modules) |
| `opencode.json` | OpenCode configuration with PocketBase MCP |

---

Generated: Template initialization