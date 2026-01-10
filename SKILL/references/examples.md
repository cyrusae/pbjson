# pbjson Examples

Real-world examples of pbjson usage across different project types. These demonstrate how the state tracking system adapts to various domains beyond software development.

## Example 1: Research & Learning Project

**Context:** User learning about machine learning by implementing papers

### Initial Setup
```bash
python3 pbjson.py context "Learning ML by implementing classic papers from scratch"
python3 pbjson.py context "Focus on understanding over performance - educational goal"
python3 pbjson.py decided "Implement one paper per week - allows deep dive"
```

### During Implementation
```bash
python3 pbjson.py built "ResNet paper implementation (in resnet.py, 34-layer version)"
python3 pbjson.py question "Should I implement data augmentation or use standard ImageNet preprocessing?"
python3 pbjson.py decided "Use standard ImageNet preprocessing - keeps focus on architecture, not data pipeline"
```

### State File Snapshot
```json
{
  "what_we_decided": [
    "2026-01-05 - Implement one paper per week - allows deep dive",
    "2026-01-05 - Use standard ImageNet preprocessing - keeps focus on architecture, not data pipeline",
    "2026-01-10 - Focus on CPU implementation first - GPU optimization comes after understanding"
  ],
  "what_we_built": [
    "2026-01-05 - ResNet paper implementation (in resnet.py, 34-layer version)",
    "2026-01-08 - Training loop with checkpointing (in train.py)",
    "2026-01-10 - Visualization tools for activation maps (in viz.py)"
  ],
  "what_we_need_to_decide": [
    "2026-01-10 - Next paper to implement - Transformer or LSTM?"
  ],
  "important_files": [
    "2026-01-05 - resnet.py - main model implementation",
    "2026-01-08 - train.py - training entry point"
  ],
  "context": [
    "2026-01-05 - Learning ML by implementing classic papers from scratch",
    "2026-01-05 - Focus on understanding over performance - educational goal",
    "2026-01-12 - User prefers detailed comments explaining math behind each layer"
  ]
}
```

## Example 2: Web Application Development

**Context:** Building a task management SaaS application with frontend and backend

### Multi-Subsystem Organization
```bash
# Main project decisions
python3 pbjson.py decided "Monorepo with separate frontend/ and backend/ directories"
python3 pbjson.py decided "PostgreSQL for data persistence - need complex queries and ACID"
python3 pbjson.py context "Target: small teams (5-10 people), mobile-first design"

# Frontend subsystem
python3 pbjson.py decided:frontend "React with TypeScript - team familiarity"
python3 pbjson.py built:frontend "Task list component (in components/TaskList.tsx)"
python3 pbjson.py question:frontend "State management: Redux vs Zustand?"
python3 pbjson.py file:frontend "src/App.tsx - application entry point"

# Backend subsystem
python3 pbjson.py decided:backend "FastAPI for REST API - async support and auto-docs"
python3 pbjson.py built:backend "User authentication endpoints (in api/auth.py, JWT-based)"
python3 pbjson.py question:backend "Task assignment notifications: polling vs WebSockets?"
python3 pbjson.py file:backend "src/main.py - FastAPI application entry"
```

### Main project.json
```json
{
  "what_we_decided": [
    "2026-01-03 - Monorepo with separate frontend/ and backend/ directories",
    "2026-01-03 - PostgreSQL for data persistence - need complex queries and ACID",
    "2026-01-04 - JWT authentication with 24hr expiry - balances security and UX"
  ],
  "what_we_built": [
    "2026-01-04 - Project structure and initial setup (monorepo with pnpm workspaces)"
  ],
  "important_files": [
    "2026-01-03 - README.md - getting started guide",
    "2026-01-04 - docker-compose.yml - local development environment"
  ],
  "context": [
    "2026-01-03 - Target: small teams (5-10 people), mobile-first design",
    "2026-01-05 - User prefers pragmatic approach over perfect architecture"
  ]
}
```

### frontend-state.json
```json
{
  "what_we_decided": [
    "2026-01-03 - React with TypeScript - team familiarity",
    "2026-01-05 - Tailwind CSS for styling - rapid prototyping",
    "2026-01-06 - Zustand for state management - simpler than Redux for our scale"
  ],
  "what_we_built": [
    "2026-01-04 - Task list component (in components/TaskList.tsx)",
    "2026-01-05 - Login form with validation (in components/Auth/LoginForm.tsx)",
    "2026-01-06 - API client with interceptors (in lib/api.ts)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-06 - State management: Redux vs Zustand? → Decided: Zustand - simpler, less boilerplate"
  ],
  "important_files": [
    "2026-01-03 - src/App.tsx - application entry point",
    "2026-01-06 - src/lib/api.ts - API client setup"
  ]
}
```

### backend-state.json
```json
{
  "what_we_decided": [
    "2026-01-03 - FastAPI for REST API - async support and auto-docs",
    "2026-01-04 - SQLAlchemy ORM with Alembic migrations",
    "2026-01-07 - WebSockets for real-time notifications - better UX than polling"
  ],
  "what_we_built": [
    "2026-01-04 - User authentication endpoints (in api/auth.py, JWT-based)",
    "2026-01-05 - Task CRUD endpoints (in api/tasks.py)",
    "2026-01-07 - WebSocket connection manager (in ws/manager.py)"
  ],
  "what_we_resolved": [
    "2026-01-07 - Task assignment notifications: polling vs WebSockets? → Decided: WebSockets - better UX, lower latency"
  ],
  "important_files": [
    "2026-01-03 - src/main.py - FastAPI application entry",
    "2026-01-05 - src/db/models.py - database models"
  ]
}
```

## Example 3: Novel Writing Project

**Context:** Writing a science fiction novel with multiple POV characters

### Setup and Initial Decisions
```bash
python3 pbjson.py context "Sci-fi novel set in 2157, multiple POV characters"
python3 pbjson.py context "User prefers discovery writing with light outlining"
python3 pbjson.py decided "Three POV characters - scientist, rebel, politician"
python3 pbjson.py decided "Chapters alternate POV - allows parallel storylines"
python3 pbjson.py file "outline.md - high-level plot structure"
```

### Tracking Writing Progress
```bash
python3 pbjson.py built "Chapter 1 - Scientist POV (introduces Mars colony setting, 2800 words)"
python3 pbjson.py built "Chapter 2 - Rebel POV (underground movement revealed, 2600 words)"
python3 pbjson.py question "Should the scientist discover the conspiracy in Act 1 or Act 2?"
python3 pbjson.py context "User wants themes: corporatization of space, class struggle, environmental ethics"
```

### State File
```json
{
  "what_we_decided": [
    "2026-01-08 - Three POV characters - scientist, rebel, politician",
    "2026-01-08 - Chapters alternate POV - allows parallel storylines",
    "2026-01-10 - Discovery happens in Act 2 - builds tension gradually",
    "2026-01-12 - Novel structure: 3 acts, ~30 chapters, target 90k words"
  ],
  "what_we_built": [
    "2026-01-08 - Chapter 1 - Scientist POV (introduces Mars colony setting, 2800 words)",
    "2026-01-09 - Chapter 2 - Rebel POV (underground movement revealed, 2600 words)",
    "2026-01-10 - Chapter 3 - Politician POV (corporate intrigue, 2500 words)",
    "2026-01-11 - Character profiles document (in characters.md)"
  ],
  "what_we_need_to_decide": [
    "2026-01-12 - What's the politician's ultimate motivation - redemption or power?"
  ],
  "what_we_resolved": [
    "2026-01-10 - Should scientist discover conspiracy in Act 1 or Act 2? → Decided: Act 2 - builds tension gradually"
  ],
  "important_files": [
    "2026-01-08 - outline.md - high-level plot structure",
    "2026-01-11 - characters.md - detailed character profiles",
    "2026-01-08 - worldbuilding.md - Mars colony details"
  ],
  "context": [
    "2026-01-08 - Sci-fi novel set in 2157, multiple POV characters",
    "2026-01-08 - User prefers discovery writing with light outlining",
    "2026-01-09 - User wants themes: corporatization of space, class struggle, environmental ethics",
    "2026-01-12 - Tone: hopeful despite dark themes, character-driven over plot-driven"
  ]
}
```

## Example 4: Academic Dissertation

**Context:** PhD dissertation on climate adaptation in coastal cities

### Organizing by Chapter/Section
```bash
# Main dissertation tracking
python3 pbjson.py context "PhD dissertation: climate adaptation strategies in coastal cities"
python3 pbjson.py context "Focus on 5 case studies: Miami, Jakarta, Rotterdam, Shanghai, Lagos"
python3 pbjson.py decided "Mixed methods: quantitative sea-level data + qualitative policy analysis"

# Chapter-specific subsystems
python3 pbjson.py decided:lit-review "Organize by themes not chronology - clearer argument"
python3 pbjson.py built:lit-review "Climate science section (15 pages, 45 sources)"
python3 pbjson.py question:methods "Should I include the failed Shanghai interview attempts in methodology section?"

python3 pbjson.py built:case-studies "Miami case study draft (22 pages, includes adaptation timeline)"
python3 pbjson.py question:case-studies "How much detail on Everglades restoration - tangential to core argument?"
```

### Main State (project.json)
```json
{
  "what_we_decided": [
    "2025-11-10 - Mixed methods: quantitative sea-level data + qualitative policy analysis",
    "2025-11-15 - 5 case studies: Miami, Jakarta, Rotterdam, Shanghai, Lagos",
    "2025-12-01 - Target: submit by June 2026, aim for 200-250 pages",
    "2026-01-05 - Structure: Intro, Lit Review, Methods, 5 case study chapters, Comparative Analysis, Conclusion"
  ],
  "what_we_built": [
    "2025-12-15 - Introduction chapter draft (18 pages)",
    "2026-01-03 - Research database setup (Zotero with 247 sources)"
  ],
  "important_files": [
    "2025-11-10 - outline.md - dissertation structure",
    "2025-12-01 - timeline.md - submission deadlines and milestones"
  ],
  "context": [
    "2025-11-10 - PhD dissertation: climate adaptation strategies in coastal cities",
    "2025-11-10 - Focus on 5 case studies: Miami, Jakarta, Rotterdam, Shanghai, Lagos",
    "2025-12-20 - Committee feedback: needs more emphasis on policy implementation barriers"
  ]
}
```

### lit-review-state.json
```json
{
  "what_we_decided": [
    "2025-12-05 - Organize by themes not chronology - clearer argument",
    "2025-12-10 - Four main sections: Climate science, Urban planning, Policy frameworks, Social equity",
    "2026-01-02 - Include emerging literature on nature-based solutions"
  ],
  "what_we_built": [
    "2025-12-20 - Climate science section (15 pages, 45 sources)",
    "2026-01-05 - Urban planning section (18 pages, 52 sources)",
    "2026-01-08 - Policy frameworks section draft (14 pages, 38 sources)"
  ],
  "what_we_need_to_decide": [
    "2026-01-08 - Should social equity be integrated throughout or separate chapter?"
  ]
}
```

### methods-state.json
```json
{
  "what_we_decided": [
    "2025-12-15 - Qualitative: semi-structured interviews with 30+ stakeholders per city",
    "2025-12-20 - Quantitative: NOAA sea-level data 1980-2024 for each city",
    "2026-01-04 - Use NVivo for qualitative coding - better than manual"
  ],
  "what_we_built": [
    "2026-01-06 - Methods chapter draft (25 pages)",
    "2026-01-07 - Interview protocol document (12 pages)"
  ],
  "what_we_resolved": [
    "2026-01-07 - Should I include failed Shanghai interview attempts in methodology? → Decided: Yes, in limitations section - shows transparency and challenges of international research"
  ]
}
```

## Key Patterns Across Examples

### 1. Context Field as Project Memory
All examples use `context` for:
- Project goals and constraints
- User preferences and style
- Important background information
- Changes in direction or priorities

### 2. Subsystems for Organization
- **Web app:** Frontend/backend separation
- **Novel:** Could use POV characters or acts as subsystems
- **Dissertation:** Chapters as subsystems for complex documents

### 3. Questions Drive Progress
- Open questions capture uncertainty
- Resolved questions show decision evolution
- Questions help identify blockers

### 4. Built Entries as Searchable Log
All examples include searchable keywords:
- Software: filenames and features
- Writing: chapter numbers and word counts
- Research: page counts and source counts

### 5. Important Files for Navigation
- Max 3-5 files
- Focus on entry points
- Updated as project evolves

## Usage Notes

These examples demonstrate:
- **Flexibility:** pbjson works for software, creative, and academic projects
- **Scalability:** From single-file to multi-subsystem organization
- **Clarity:** Append-only log creates natural project timeline
- **Searchability:** Consistent format enables easy grep/search

The system adapts to different domains while maintaining the same core workflow.
