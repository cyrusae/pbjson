# pbjson

## Project Breadcrumbs JSON 

A lightweight decision and work logger for projects developed with Claude AI assistance.

Track architectural decisions, completed work, open questions, and project context—in human-readable, git-friendly JSON files. Designed to work within constrained environments (like Claude's project file system) with a single-file deployment model.

## Why pbjson?

When working with AI assistants like Claude on long-term projects, context gets lost between conversations. You find yourself re-explaining decisions, searching through chat history for "what did we decide about X?", or losing track of what's already built.

**pbjson solves this by:**
- **Preserving decisions** across conversation boundaries
- **Making context searchable** (git-friendly JSON means you can grep for decisions)
- **Keeping AI assistants aligned** with your project's architectural choices
- **Working within constraints** (single-file deployment, no external dependencies)
- **Staying human-readable** for code review and collaboration

## Using pbjson

### Quick Start

1. **Copy `pbjson.py` to your project root**
2. **Add the custom instructions** (below) to your Claude project
3. **Start tracking:** `./pbjson.py decided "First architectural decision"`

The tool creates `project.json` automatically on first use.

### Intended use model

**Where:** In any project developed with Claude (especially long-term ones) 

### Add this to your custom instructions:

``` 
The name of this project is {YOUR PROJECT}. It's {ONE-LINE DESCRIPTION/CONTEXT}.

At the start of each conversation: 
Review pbjson.py to understand state tracking system
cat project.json

Look for any additional subsystem json files related to initial prompt: cat {subsystem}-state.json

COMMUNICATION PROTOCOL:
- Always discuss decisions, questions, and context with the user in natural language FIRST
- State tracking tools are for RECORD-KEEPING, not communication
- After explaining something to the user, use pbjson.py to record it
- Never use tool calls as a substitute for explaining things to the user
- ALWAYS call present_files() after pbjson.py to show changes to user

DECISION WORKFLOW:
- Proposals/suggestions → Discuss in conversation (no tool calls yet)
- Open questions → Ask user, then: ./pbjson.py question "description" OR ./pbjson.py question:subsystem "description"
- Agreed decisions → After user confirms: ./pbjson.py decided "decision - brief reason"
- Work completed → After finishing: ./pbjson.py built "description (search term)"
- Important context → Anytime: ./pbjson.py context "information for reference"
- Entry point files → When creating key navigation files (max 3-5): ./pbjson.py file "path - purpose (e.g., 'start here for X')"

Within a subsystem: ./pjbson.py decided:subsystem "decision about future architecture"

FIELD PURPOSES:
- what_we_decided: Architectural decisions made with user agreement
- what_we_built: Code/files completed (searchable by filename or description)
- what_we_need_to_decide: Open questions requiring user input
- what_we_resolved: Questions that have been answered (use ./pbjson.py resolve)
- important_files: Entry points only (3-5 max) - "where do I start for X task?"
- context: Background information, constraints, user preferences, anything else to remember
```

## Understanding the JSON Structure

### Main File: `project.json`

The tool creates a simple JSON file with six fields:

```json
{
  "what_we_decided": [
    "2026-01-09 - Use SQLite for caching - simpler than Redis for our scale"
  ],
  "what_we_built": [
    "2026-01-09 - Database layer (search: db.py, sqlite)"
  ],
  "what_we_need_to_decide": [
    "2026-01-09 - Should we add user authentication?"
  ],
  "what_we_resolved": [
    "2026-01-09 - 'caching strategy' → Decided to use SQLite"
  ],
  "important_files": [
    "2026-01-09 - src/main.py - application entry point",
    "2026-01-09 - src/db.py - database interface"
  ],
  "context": [
    "2026-01-09 - Target users are non-technical, UI simplicity is critical"
  ]
}
```

### Subsystem Files: `{name}-state.json`

For complex projects, create separate state files per subsystem:

```bash
./pbjson.py decided:api "Use FastAPI for REST endpoints"
# Creates: api-state.json

./pbjson.py question:frontend "React or Vue?"
# Creates: frontend-state.json
```

Each subsystem file has the same structure as `project.json`.

## Best Practices

### When to Use Each Command

- **`decided`**: Record architectural choices AFTER discussing with your team/AI. Not for proposals, only confirmed decisions.
- **`built`**: Track completed work with searchable keywords in parentheses: `built "User auth (search: auth.py, login)"`
- **`question`**: Capture open decisions that need input. Review these regularly.
- **`file`**: Only for key entry points (3-5 max). Avoid listing every file—focus on "where do I start for X?"
- **`context`**: Store constraints, user preferences, or background info that should persist across sessions.
- **`resolve`**: Move questions to resolutions. Use keywords from the original question.

### Organizing Subsystems

Use subsystems for:
- **Large codebases**: `frontend-state.json`, `backend-state.json`, `infra-state.json`
- **Multi-phase projects**: `mvp-state.json`, `v2-state.json`
- **Specialized areas**: `security-state.json`, `performance-state.json`

### Git Integration

Add to `.gitignore` if tracking sensitive decisions locally:
```
# Keep project structure tracked but not implementation details
*-state.json
!project.json
```

Or commit everything for full team transparency (recommended for open source).

## Contributing & Support

Found a bug? Have a feature idea? Want to improve the docs?

**Open an issue:** [GitHub Issues](https://github.com/cyrusae/pbjson/issues) — We especially welcome:
- Bug reports with reproduction steps
- Feature requests with use cases
- Documentation improvements
- Questions about usage

**Pull requests welcome!** Please include:
- Clear description of what changes and why
- Any relevant examples or test cases

## Development Notes

### Manual Sync Limitation

**Important:** If you use pbjson within a Claude Project, you must manually sync files between the Claude interface and your local git repository. Claude's project file system doesn't auto-sync with external version control.

**Workflow:**
1. Make changes in Claude using pbjson
2. Download updated `project.json` and any `*-state.json` files
3. Commit to your git repository
4. Upload updated files back to Claude if continuing work

This is a limitation of the Claude platform, not pbjson itself. We're exploring better solutions.

### Why "pbjson"?

**Project Breadcrumbs JSON** — leaving a trail of decisions so you (and your AI assistant) can find your way back.

*Full disclosure: I didn't come up with the pun. That was fate.* 😄