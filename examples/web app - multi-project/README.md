# Multi-Subsystem Example: Full-Stack Web Application

A realistic example of using pbjson with multiple subsystems to track architecture decisions across different layers of a web application.

## Scenario

You're building a personal habit tracker app with Claude's help. It has:
- **Backend**: REST API with user authentication
- **Frontend**: React dashboard
- **Infrastructure**: Deployment and database strategy
- **Domain logic**: How habits are modeled and tracked

With multiple threads of work happening simultaneously, you need separate decision logs.

## Initial Setup

```bash
cp pbjson.py ./

# Main project decisions
./pbjson.py decided "Build a habit tracker app to replace spreadsheets"
./pbjson.py context "Target users: non-technical, prefer mobile-first UI"

# Backend architectural decisions
./pbjson.py decided:backend "Use FastAPI for REST API"
./pbjson.py context:backend "Python shop, team familiar with async patterns"

# Frontend decisions
./pbjson.py decided:frontend "Use React with TypeScript"
./pbjson.py context:frontend "Designer has React experience, wants Tailwind for styling"

# Infrastructure/deployment
./pbjson.py decided:infra "Deploy on Render for simplicity over bare metal"
./pbjson.py context:infra "Early stage, don't want to manage servers yet"

# Domain modeling (the business logic)
./pbjson.py question:domain "How to model habit streaks—by calendar day or 24-hour period?"
./pbjson.py context:domain "Users need visual feedback on progress"

# Mark key entry points
./pbjson.py file "README.md - project overview and setup"
./pbjson.py file "docs/architecture.md - system design decisions"
```

## First Development Sprint

### Backend Work

```bash
# Someone (or Claude in a thread) builds authentication
./pbjson.py built:backend "User authentication endpoints (search: auth.py, login, signup, logout)"
./pbjson.py built:backend "JWT token management (search: tokens.py, refresh)"

# Question comes up about password storage
./pbjson.py question:backend "Should we use bcrypt or argon2 for password hashing?"
# ... research, discuss ...
./pbjson.py resolve:backend "password hashing" "Use argon2 (better security than bcrypt), via python-argon2"

# More work continues
./pbjson.py built:backend "User profile endpoints (search: profiles.py, GET/PUT /users/:id)"
./pbjson.py built:backend "Database migrations with Alembic (search: alembic/)"
```

### Frontend Work (Parallel)

```bash
# Designer/frontend dev builds components
./pbjson.py built:frontend "Login/signup page with form validation (search: Auth.tsx)"
./pbjson.py built:frontend "Dashboard layout and routing (search: Dashboard.tsx)"

# Question about state management
./pbjson.py question:frontend "Redux, Zustand, or Context API for state?"
./pbjson.py context:frontend "App is small enough that heavy state management seems like overkill"
# ... discussion ...
./pbjson.py resolve:frontend "state management" "Use React Context + custom hooks for now, migrate to Zustand if it becomes unwieldy"

./pbjson.py built:frontend "Habit card component showing daily progress (search: HabitCard.tsx)"
./pbjson.py built:frontend "Responsive design for mobile (search: styles/responsive.css)"
```

### Domain Modeling

```bash
# The core question that was blocking everything
# gets answered as people implement and learn
./pbjson.py context:domain "Discovered: 'calendar day' easier to implement, 'resets at midnight UTC'"
./pbjson.py resolve:domain "habit streaks" "Use calendar day model with midnight UTC reset"

./pbjson.py built:domain "Habit model (search: models/habit.py, with streak calculation)"
./pbjson.py built:domain "Habit log model for daily tracking (search: models/habitlog.py)"
```

### Infrastructure

```bash
./pbjson.py built:infra "PostgreSQL setup on Render (search: terraform/, schema.sql)"
./pbjson.py question:infra "How to handle database backups?"
./pbjson.py context:infra "Render handles daily backups automatically, good enough for now"

./pbjson.py built:infra "CI/CD pipeline with GitHub Actions (search: .github/workflows/)"
./pbjson.py built:infra "Deployment scripts (search: deploy.sh)"
```

## Second Sprint: Integration & Pivots

```bash
# Someone tries to integrate frontend with backend
./pbjson.py question:backend "Need to add habit categories—affects database schema and API"
./pbjson.py context:backend "Frontend already scoped this in designs but we didn't discuss schema"

# Decision gets made and implemented
./pbjson.py decided:backend "Add categories as separate table with many-to-many relationship"
./pbjson.py built:backend "Categories API (search: categories.py)"
./pbjson.py built:backend "Updated habit model with category foreign keys (search: models/habit.py v2)"

# Frontend needs to update to match
./pbjson.py built:frontend "Category selector component (search: CategorySelect.tsx)"
./pbjson.py built:frontend "Update habit creation form (search: NewHabitForm.tsx)"

# Someone discovers a UX problem
./pbjson.py question:frontend "Mobile users can't easily see which habit they're logging—text too small?"
./pbjson.py resolved:frontend "mobile viewing" "Increase habit name font size on card, add visual color coding for categories"
./pbjson.py built:frontend "Enhanced visual feedback on mobile (search: HabitCard.tsx mobile styles)"
```

## Cross-Cutting Concerns

```bash
# A decision that affects multiple subsystems
./pbjson.py question "Should users be able to export their data?"
./pbjson.py context "Privacy-conscious users asking for it, GDPR best practice"

# Gets answered and everyone implements their part
./pbjson.py decided "Add CSV export for user data"
./pbjson.py built:backend "Export endpoint (search: export.py, CSV generation)"
./pbjson.py built:frontend "Export button and download UX (search: ExportButton.tsx)"

# Domain logic involved too
./pbjson.py built:domain "Data serialization for export (search: models/export.py)"
```

## Final State: Multiple Files

### `project.json` (main project decisions)

```json
{
  "what_we_decided": [
    "2026-01-09 - Build a habit tracker app to replace spreadsheets",
    "2026-01-18 - Add CSV export for user data"
  ],
  "what_we_built": [
    "2026-01-20 - Documentation site (search: docs/)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [],
  "important_files": [
    "2026-01-09 - README.md - project overview and setup",
    "2026-01-09 - docs/architecture.md - system design decisions"
  ],
  "context": [
    "2026-01-09 - Target users: non-technical, prefer mobile-first UI",
    "2026-01-18 - Privacy-conscious users asking for data export, GDPR best practice"
  ]
}
```

### `backend-state.json`

```json
{
  "subsystem": "backend",
  "what_we_decided": [
    "2026-01-09 - Use FastAPI for REST API",
    "2026-01-15 - Add categories as separate table with many-to-many relationship"
  ],
  "what_we_built": [
    "2026-01-10 - User authentication endpoints (search: auth.py, login, signup, logout)",
    "2026-01-10 - JWT token management (search: tokens.py, refresh)",
    "2026-01-11 - User profile endpoints (search: profiles.py, GET/PUT /users/:id)",
    "2026-01-11 - Database migrations with Alembic (search: alembic/)",
    "2026-01-15 - Categories API (search: categories.py)",
    "2026-01-15 - Updated habit model with category foreign keys (search: models/habit.py v2)",
    "2026-01-18 - Export endpoint (search: export.py, CSV generation)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-11 - Should we use bcrypt or argon2 for password hashing? → Decided: Use argon2 (better security than bcrypt), via python-argon2"
  ],
  "important_files": [
    "2026-01-09 - main.py - FastAPI app entry point",
    "2026-01-10 - auth.py - authentication logic"
  ],
  "context": [
    "2026-01-09 - Python shop, team familiar with async patterns",
    "2026-01-10 - Using PostgreSQL on Render"
  ]
}
```

### `frontend-state.json`

```json
{
  "subsystem": "frontend",
  "what_we_decided": [
    "2026-01-09 - Use React with TypeScript"
  ],
  "what_we_built": [
    "2026-01-10 - Login/signup page with form validation (search: Auth.tsx)",
    "2026-01-10 - Dashboard layout and routing (search: Dashboard.tsx)",
    "2026-01-11 - Habit card component showing daily progress (search: HabitCard.tsx)",
    "2026-01-11 - Responsive design for mobile (search: styles/responsive.css)",
    "2026-01-15 - Category selector component (search: CategorySelect.tsx)",
    "2026-01-15 - Update habit creation form (search: NewHabitForm.tsx)",
    "2026-01-16 - Enhanced visual feedback on mobile (search: HabitCard.tsx mobile styles)",
    "2026-01-18 - Export button and download UX (search: ExportButton.tsx)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-12 - Redux, Zustand, or Context API for state? → Decided: Use React Context + custom hooks for now, migrate to Zustand if it becomes unwieldy",
    "2026-01-16 - Mobile users can't easily see which habit they're logging—text too small? → Resolved: Increase habit name font size on card, add visual color coding for categories"
  ],
  "important_files": [
    "2026-01-09 - App.tsx - main React component and routing",
    "2026-01-10 - components/ - reusable UI components"
  ],
  "context": [
    "2026-01-09 - Designer has React experience, wants Tailwind for styling",
    "2026-01-09 - Mobile-first design priority"
  ]
}
```

### `infra-state.json`

```json
{
  "subsystem": "infra",
  "what_we_decided": [
    "2026-01-09 - Deploy on Render for simplicity over bare metal"
  ],
  "what_we_built": [
    "2026-01-11 - PostgreSQL setup on Render (search: terraform/, schema.sql)",
    "2026-01-12 - CI/CD pipeline with GitHub Actions (search: .github/workflows/)",
    "2026-01-12 - Deployment scripts (search: deploy.sh)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [],
  "important_files": [
    "2026-01-11 - deploy.sh - automated deployment script",
    "2026-01-12 - .github/workflows/ - CI/CD configuration"
  ],
  "context": [
    "2026-01-09 - Early stage, don't want to manage servers yet",
    "2026-01-11 - Render handles daily backups automatically, good enough for now"
  ]
}
```

### `domain-state.json`

```json
{
  "subsystem": "domain",
  "what_we_decided": [],
  "what_we_built": [
    "2026-01-12 - Habit model (search: models/habit.py, with streak calculation)",
    "2026-01-12 - Habit log model for daily tracking (search: models/habitlog.py)",
    "2026-01-18 - Data serialization for export (search: models/export.py)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-15 - How to model habit streaks—by calendar day or 24-hour period? → Decided: Use calendar day model with midnight UTC reset"
  ],
  "important_files": [
    "2026-01-12 - models/habit.py - core habit domain model"
  ],
  "context": [
    "2026-01-12 - Users need visual feedback on progress",
    "2026-01-12 - Calendar day easier to implement, resets at midnight UTC"
  ]
}
```

## Why This Structure Works

- **Parallel development**: Different teams/threads work on backend, frontend, infra simultaneously without stepping on each other's toes
- **Cross-subsystem decisions**: Main `project.json` tracks decisions that span multiple subsystems (like data export)
- **Search and history**: You can grep across all state files to see when decisions were made and what context they were made in
- **Claude continuity**: Each conversation thread can focus on its subsystem, and when you switch threads, Claude reads the relevant state file
- **Onboarding**: New contributors can read `infra-state.json` to understand deployment decisions without wading through frontend component discussions

## Real-World Usage

In practice, you might have:

```bash
# Check what's blocking frontend development
cat frontend-state.json | grep "what_we_need_to_decide"

# See all decisions made in the last sprint
git log -p project.json | grep "2026-01-1[5-8]"

# Find the context for why we chose PostgreSQL
grep -r "PostgreSQL" *-state.json
```

The subsystem structure makes it easy to stay focused while maintaining full project context.