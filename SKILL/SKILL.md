---
name: pbjson
description: Project state management for Claude-assisted development. Lightweight decision and work logger that tracks what was decided, built, questioned, and resolved across conversations using append-only JSON logs. Use when (1) User wants to track project decisions and progress, (2) Working on extended projects across multiple conversations, (3) User mentions wanting Claude to remember project context, (4) Managing multi-subsystem projects, or (5) User asks about project history or what was previously decided/built.
---

# pbjson - Project State Management

## Overview

pbjson is a lightweight state tracking system for projects developed with Claude AI assistance. It provides append-only logging of decisions, work, questions, and context across conversations using human-readable JSON files.

**Core philosophy:** Track decisions, not just work. Create a git-friendly decision history that persists across conversations.

## When to Use This Skill

Use pbjson when:
- Starting or working on extended projects that span multiple conversations
- User wants to track architectural decisions and their reasoning
- Managing projects with multiple subsystems or components
- User asks "what did we decide about X?" or "what have we built so far?"
- Need to onboard into existing project work

## Quick Start

### First Time Setup

1. Copy the pbjson.py script to the project directory
2. Run your first command to initialize state tracking:

```bash
python3 pbjson.py decided "Initial project setup - using pbjson for state tracking"
```

This creates `project.json` with the decision logged.

### Basic Commands

```bash
# Record an architectural decision (after user agreement)
python3 pbjson.py decided "Use PostgreSQL for persistence - need ACID guarantees"

# Log completed work (searchable by filename/feature)
python3 pbjson.py built "authentication module (in auth.py)"

# Track an open question needing user input
python3 pbjson.py question "Should we cache API responses?"

# Record important entry points (max 3-5)
python3 pbjson.py file "src/main.py - application entry point"

# Store project context and constraints
python3 pbjson.py context "Performance not critical - prioritize readability"

# Resolve a question (moves from questions to resolutions)
python3 pbjson.py resolve "caching" "Yes - cache for 1 hour with Redis"
```

## Workflow Integration

### At Conversation Start

**Always begin by reviewing project state:**

```bash
# View main project state
cat project.json

# Check for subsystem states if project has them
cat {subsystem}-state.json
```

This loads current decisions, work, and open questions into context.

### During Development

**Follow this decision workflow:**

1. **Proposals/suggestions** → Discuss with user (no tool calls yet)
2. **Open questions** → Ask user, then record:
   ```bash
   python3 pbjson.py question "description of question"
   ```
3. **Agreed decisions** → After user confirms:
   ```bash
   python3 pbjson.py decided "decision with brief reasoning"
   ```
4. **Work completed** → After finishing implementation:
   ```bash
   python3 pbjson.py built "description (searchable term)"
   ```
5. **Important context** → Anytime relevant:
   ```bash
   python3 pbjson.py context "constraint or preference"
   ```

**CRITICAL:** Always call `present_files()` after running pbjson.py so the user sees the updated state file.

### Communication Protocol

- **State tracking is for record-keeping, not communication**
- Always discuss decisions/questions with the user in natural language FIRST
- Use pbjson.py to record AFTER explaining to the user
- Never use tool calls as a substitute for explaining things

## Multi-Subsystem Projects

For projects with distinct subsystems (e.g., frontend, backend, docs), track state separately:

### Option 1: Flag Syntax
```bash
python3 pbjson.py --subsystem frontend decided "Use React for UI"
python3 pbjson.py --subsystem backend question "API versioning strategy?"
```

### Option 2: Colon Syntax (Shorter)
```bash
python3 pbjson.py decided:frontend "Use React for UI"
python3 pbjson.py question:backend "API versioning strategy?"
```

Both create subsystem-specific state files:
- `frontend-state.json`
- `backend-state.json`

### When to Use Subsystems

Use subsystem tracking when:
- Project has clearly distinct components
- Different parts have different decision contexts
- Team members work on separate areas
- Need to keep decisions organized by domain

Keep using main `project.json` for cross-cutting concerns and overall project decisions.

## Understanding the State Fields

Each state file (project.json or {subsystem}-state.json) contains:

```json
{
  "what_we_decided": ["2026-01-08 - Decision with reasoning"],
  "what_we_built": ["2026-01-08 - Feature (search: keyword)"],
  "what_we_need_to_decide": ["2026-01-08 - Open question"],
  "what_we_resolved": ["2026-01-08 - Question → Decision"],
  "important_files": ["2026-01-08 - path/to/file - purpose"],
  "context": ["2026-01-08 - Background information"]
}
```

### Field Usage Guidelines

**what_we_decided**
- Architectural decisions made WITH user agreement
- Not proposals or suggestions
- Include brief reasoning
- Example: "Use Obsidian for output - fast generation + graph view support"

**what_we_built**
- Completed work and implementations
- Include searchable keywords in parentheses
- Example: "Authentication module (in auth.py, handles JWT)"

**what_we_need_to_decide**
- Open questions requiring user input
- Unresolved design choices
- Example: "Should we support offline mode?"

**what_we_resolved**
- Questions that have been answered
- Shows question → decision mapping
- Automatically created when using `resolve` command

**important_files**
- Entry points ONLY (max 3-5 files)
- Answer "where do I start for X task?"
- Not for exhaustive file tracking
- Example: "src/orchestrator.py - main entry point for paper processing"

**context**
- Constraints and user preferences
- Background facts for reference
- Project-specific information
- Example: "User prefers functional programming style"

## Best Practices

### DO:
- ✅ Review state files at conversation start
- ✅ Explain decisions to user before recording them
- ✅ Use searchable keywords in `built` entries
- ✅ Keep `important_files` to 3-5 entry points only
- ✅ Record decisions after user agreement, not before
- ✅ Always call `present_files()` after pbjson.py updates

### DON'T:
- ❌ Record proposals as decisions before user agrees
- ❌ Use state files for communication (explain in conversation)
- ❌ Track every single file in `important_files`
- ❌ Forget to review state at conversation start
- ❌ Skip calling `present_files()` after updates

## Common Patterns

### Pattern: Resolving Questions

When a question has been answered:

```bash
# Original question was logged as:
# "Should we cache API responses?"

# Resolve it with the decision:
python3 pbjson.py resolve "cache" "Yes - Redis with 1hr TTL"

# This removes from what_we_need_to_decide and adds to what_we_resolved:
# "Should we cache API responses? → Decided: Yes - Redis with 1hr TTL"
```

### Pattern: Entry Point Files

Only track files that help with navigation:

```bash
# Good - clear entry points
python3 pbjson.py file "src/main.py - application entry point"
python3 pbjson.py file "tests/test_main.py - start here for testing"
python3 pbjson.py file "config/example.yaml - configuration template"

# Bad - too granular
python3 pbjson.py file "src/utils/helpers.py"
python3 pbjson.py file "src/models/user.py"
```

### Pattern: Subsystem Organization

For a web application with frontend and backend:

```bash
# Main project decisions
python3 pbjson.py decided "Monorepo structure with separate frontend/backend dirs"

# Frontend subsystem
python3 pbjson.py decided:frontend "React with TypeScript"
python3 pbjson.py built:frontend "Login component (in components/Login.tsx)"

# Backend subsystem  
python3 pbjson.py decided:backend "FastAPI for REST API"
python3 pbjson.py question:backend "Authentication: JWT vs sessions?"
```

## Resources

### scripts/

**pbjson.py** - The core state tracking tool. This is identical to the standalone drop-in version and can be used independently of the skill.

Features:
- Single-file Python script (stdlib only, no dependencies)
- Append-only JSON logging
- Multi-subsystem support via prefix-based file naming
- Works in constrained environments (Claude Projects)

### references/

**workflow-tips.md** - Detailed guidance on managing state across conversations, handling manual sync, and best practices for long-running projects.

**examples.md** - Real-world examples of pbjson usage across different project types (software, creative writing, research, academic).

Load these when users need deeper guidance on workflow patterns or want to see how pbjson works in practice.

## Troubleshooting

**State file not found on conversation start?**
- User may be starting fresh or in a new environment
- Run first command to initialize (e.g., `decided` or `context`)
- File will be created automatically

**Too many decisions cluttering the log?**
- This is normal and intentional - append-only design preserves history
- Use `resolve` to move questions out of active list
- Consider using subsystems to separate concerns

**Forgot to call present_files()?**
- User won't see the updated state file
- Always call after pbjson.py executes
- Critical for user to track what was logged
