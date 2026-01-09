# pbjson

## Project Breadcrumbs JSON 

A lightweight decision and work logger for projects developed with Claude AI assistance.

Let your AI assistant track architectural decisions, completed work, open questions, and project context—in human-readable, git-friendly JSON files. Designed to work within constrained environments (like Claude's project file system) with a single-file deployment model. 

## Why pbjson?

When working with AI assistants like Claude on long-term projects, context gets lost between conversations. You find yourself re-explaining decisions, searching through chat history for "what did we decide about X?", or losing track of what's already built.

### pbjson solves this by:

- **Preserving decisions** across conversation boundaries
- **Making context searchable** (git-friendly JSON means you can grep for decisions)
- **Keeping AI assistants aligned** with your project's architectural choices
- **Working within constraints** (single-file deployment, no external dependencies)
- **Staying human-readable** for code review and collaboration

**Crucially:** *You* don't run `pbjson.py`--Claude does. **pbjson** lets your AI assistant document progress in a format you can read, that's lightweight and append-only for consistent results, without making the human in the loop responsible for documentation.

## Using pbjson

### Quick Start

1. **Copy `pbjson.py` to your project context files**
2. **Add the custom instructions** (below) to your Claude project
3. **Start tracking:** `./pbjson.py decided "First architectural decision"`
4. **Sync your progress:** See "Manual Sync" instructions below

The tool should create `project.json` automatically on first use and any subproject files when invoked. If you're having issues with this, initialize `.json` files manually in project context (see skeleton).

### Intended use model

**Where:** In any project developed with Claude (especially long-term ones) 

**How:** `pbjson.py` is designed to go in a Claude project context in the web/app interface. **The script creates commands that Claude can run in its own server environment to document your conversations,** and the attached custom instructions provide structure for usage.

**Why:** Pick your favorite reason--

1. The original developer prefers working in the conversation interface with Claude and then screening code manually, and **pbjson** is designed to accommodate that. (We're actively interested in accommodating other workflows--see "Contributing"!)
2. Depending on Claude AI's web interface means **pbjson is as portable as Claude itself**--do you want to work on your project's architecture on the bus? In a waiting room? Anywhere you can have the Claude app open? Now you have your full context and structure, with the state-tracking it needs.

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

### Manual Sync and Directory Limitations

**Important:** If you use pbjson within a Claude Project, you must manually sync files between the Claude conversation interface and a) your *project context*, b) your local git repository. 

Claude's project file system doesn't auto-sync with external version control and cannot write directly to the project context from /outputs. It also does not support nested file structures: effective "nesting" is contingent on use of file prefixes. 

(Claude instances are *not* aware of these limitations of their UI unless told directly, and they will often infer otherwise.) 

**The workarounds:** 

- Track "nested" files with prefixes: `*-state.json` files
- Claude uses `present_files()` after each **pbjson** call, showing you the updated `.json` file
- Select "Add to project" or manually copy into project context to sync (delete superseded old files)

This approach is most effective with **one file per working conversation thread**. Use the subproject system to accommodate this if you have multiple running threads at a time.

**Workflow:**

1. Have Claude autonomously track changes in a conversation thread using `./pbjson.py` in its environment
2. When Claude presents the updated `.json` file, add it to your project, making sure to **replace** the previous version
3. To sync for your own repos, download `project.json` and any `*-state.json` files
4. Commit to your git repository

**This is a limitation of the Claude platform, not pbjson itself.** We're open to better solutions, but Claude instances aren't capable of troubleshooting the issue themselves.

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

**pbjson.py** should be capable of initializing its own project files when run by a Claude instance, but if you run into issues, you can resolve this by initializing an empty `.json` file in your project context:

```json
{
  "what_we_decided": [],
  "what_we_built": [],
  "what_we_need_to_decide": [],
  "what_we_resolved": [],
  "important_files": [],
  "context": []
}
```

Name the file in your first message and Claude will be able to use it going forward.

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
- Applicability to other AI assistants and workflows
- Use cases in other domains (writing, creative projects)

**Pull requests welcome!** Please include:
- Clear description of what changes and why
- Any relevant examples or test cases
- Any questions or clarifications regarding your implementation

### Why "pbjson"?

**Project Breadcrumbs JSON** — leaving a trail of decisions so you (and your AI assistant) can find your way back.

*Full disclosure: I didn't come up with the pun. That was fate, in the form of Claude Sonnet 4.5 itself: this project was named by its main user audience!* 
