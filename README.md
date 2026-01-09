# pbjson

## Project Breadcrumbs JSON 

A lightweight decision and work logger for projects developed with Claude AI assistance.

Let your AI assistant track architectural decisions, completed work, open questions, and project context—in human-readable, git-friendly JSON files. Designed to work within constrained environments (like Claude's project file system) with a single-file deployment model. 

## Why pbjson?

When working with AI assistants like Claude on long-term projects, decisions happen in conversation. Code gets written, approaches get tried, questions get answered—but this context lives in chat logs, not in your actual codebase. 

Then context gets lost. You find yourself re-explaining decisions, searching through chat history for "what did we decide about X?", or losing track of what's already built.

### pbjson solves this by:

- **Preserving decisions** across conversation boundaries
- **Making context searchable** (git-friendly JSON means you can grep for decisions)
- **Keeping AI assistants aligned** with your project's architectural choices
- **Working within constraints** (single-file deployment, no external dependencies)
- **Staying human-readable** for review and collaboration

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

1. The original developer prefers working in the conversation interface with Claude and then screening code manually, and **pbjson** is designed to accommodate that. 
2. Depending on Claude AI's web interface means **pbjson is as portable as Claude itself**.

Do you want to work on your project's architecture on the bus? In a waiting room? Anywhere you can have the Claude app open on your phone? Now you have your full context and structure, with the state-tracking it needs.

We're actively interested in accommodating other workflows--see "Contributing"!

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
    "2026-01-09 - Database layer (db.py, sqlite)"
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
# Logs decision in: api-state.json

./pbjson.py question:frontend "React or Vue?"
# Logs question in: frontend-state.json
```

Each subsystem file has the same structure as `project.json`.

**pbjson** should be capable of initializing its own project files when run by a Claude instance, but if you run into issues, you can resolve this by **initializing an empty `.json` file in your project context:**

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

## Usage Examples

### Generated examples

The `examples/generated/` directory contains four realistic **AI-generated** projects demonstrating **pbjson** use across different domains—including mapping it beyond the initial software development use case.

*Note: These examples were created with Claude to demonstrate realistic usage patterns. As pbjson gains real-world adoption, we may add examples from actual projects with permission from their creators. Feel free to suggest/contribute your own!*

- **`examples/generated/academic research - multi-project`**: Multi-file project showing development of a research study--literature review, methodology, data collection
- **`examples/generated/reative writing - multi-project`**: Multi-file project showing development of a science fiction novel--worldbuilding, characters, plot/events, editing concerns
- **`examples/generated/self-study - single-project`**: Single-file project showing self-study synthesis--papers, concepts, synthesis, personal applied project
- **`examples/generated/web app - multi-project`**: Multi-file project tracking a full-stack web application across frontend, backend, infrastructure and other concerns 

Each folder contains a `README.md` usage story and the `project.json` + `*-state.json` files that would be generated by that user over time.

### Real samples

- **`meta-example`**: Snapshot of **pbjson**'s development with the contents of an actual Claude project tracking it, including revisions to this README.

## Best Practices

### Quick Reference

- **`decided`**: Record architectural choices AFTER discussing with your team/AI. Not for proposals, only confirmed decisions.
- **`built`**: Track completed work with searchable keywords in parentheses: `built "User auth (auth.py, login)"`
- **`question`**: Capture open decisions that need input. Review these regularly.
- **`file`**: Only for key entry points (3-5 max). Avoid listing every file—focus on "where do I start for X?"
- **`context`**: Store constraints, user preferences, or background info that should persist across sessions.
- **`resolve`**: Move questions to resolutions.

### When to Use Each Command

**`decided` - Architectural decisions made with user agreement**

Not proposals or suggestions—only decisions you've committed to.

Examples:
- "Use PostgreSQL for citation index"
- "Implement caching at the API layer, not the DB layer"
- "Defer OCR support to v0.2"

Include your reasoning if it's not obvious. Future you will appreciate it.

**`built` - Work completed and committed**

Record it when it's done (not when you start it). Include searchable keywords so you can find it later.

Examples:
- "arxiv_fetcher.py - fetches papers from arXiv API"
- "markdown_writer.py (generates Obsidian notes)"
- "End-to-end test for paper pipeline (test_pipeline.py)"

**`question` - Decisions needing input or discussion**

Track open questions that are blocking progress or need a decision.

Examples:
- "Should we implement multi-file package structure or keep it monolithic?"
- "What database should we use for citations—SQLite or CouchDB?"
- "How to handle scanned PDFs with poor OCR?"

**`resolved` - Questions that have been answered**

When you answer a question, use `resolve` to move it here. This field shows the full mapping: "We asked X, and decided Y because Z."

You'll see entries like:
```
2026-01-08 - Should we cache results? → Decided: Cache by DOI, invalidate on manual edit
```

**`important_files` - Entry points only (max 3-5 per project)**

This is *not* for tracking all files—it's for answering "Where do I start?"

Examples:
- "orchestrator.py - main entry point for paper processing"
- "test_pipeline.py - run this to test end-to-end"
- ".env.example - copy and fill with API keys"

**`context` - Constraints, preferences, background facts**

Anything that helps future you (or a contributor) understand the project without asking.

Examples:
- "Token cost not a concern—can send full papers to Claude"
- "Running on homelab K3s cluster, writes to Obsidian vault via CouchDB Livesync"
- "User is learning Python—include ELI5 comments in code"

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

## Philosophy

### Minimalism

**Single-file development:** Most Python tools expect you to manage package structures, virtual environments, and import paths. pbjson is intentionally a single `.py` file. Drop it in your project, run it, done.

This works especially well in constrained environments—like Claude's project interface, where you can't manage multi-file packages.

**Append-only log:** Every entry gets a date prefix. Nothing is ever overwritten or deleted (except when you resolve a question, and that preserves both the question and the answer for history).

**No external dependencies:** **pbjson** uses only Python standard library: `json`, `sys`, `pathlib`, `datetime`. Nothing to install, nothing to break.

**Minimal human involvment:** Designed to let the AI assistant handle the documentation--human work is only necessary when the literal interface requires it.

### Decision-focused, not task-focused

There are tools for tracking tasks (todo lists), features (GitHub issues), and timelines (project boards). pbjson doesn't compete with those.

Instead, it tracks **why** things are the way they are:
- Why did you choose this architecture?
- What open questions are blocking progress?
- What constraints are you operating under?
- When you answered a question, how did you decide?

### Limitations & What pbjson is NOT

- **Not a task tracker.** Use GitHub Issues, Linear, or a todo app for tasks. pbjson is for capturing *why* decisions were made, not *what work* is pending.
- **Not a metrics tool.** If you need burn-down charts, velocity tracking, or sprint planning, this isn't it. pbjson is append-only; it doesn't aggregate or analyze.
- **Not a knowledge base.** For a searchable wiki of your project, use Notion, Obsidian, or a wiki tool. pbjson is a decision log, not a reference.
- **Not a chat history.** It doesn't replace keeping your Claude conversations. It extracts the *decisions* from those conversations, not the full discussion.
- **Not a code review tool.** Use pull requests for code review. pbjson tracks architectural decisions, not code changes.

### Design Constraints

pbjson was designed with these constraints in mind:

- **Claude project file system**: No nested directories, single entry point preferred
- **Human-readable**: JSON you can read and edit, not a binary database
- **Git-friendly**: Clean diffs, works with version control
- **No setup**: Copy one file, start using it
- **No dependencies**: Works anywhere Python runs
- **Portable:** Works anywhere the Claude app or web interface can run

If you're building something without these constraints, you might prefer a full-featured tool. But if you're working in a constrained environment and need something that "just works," **pbjson** is for you.

## Contributing & Support

Found a bug? Have a feature idea? Want to improve the docs?

### Open an issue:
[GitHub Issues](https://github.com/cyrusae/pbjson/issues) — We especially welcome:

- Bug reports with reproduction steps
- Feature requests with use cases
- Documentation improvements and real-world examples
- Questions about usage
- Applicability to other AI assistants and workflows
- Use cases in other domains (writing, creative projects)

### Pull requests welcome!

The entire tool is in a single `pbjson.py` file for portability. Each section is clearly marked:

- `# === CONFIGURATION & STATE STRUCTURE ===`
- `# === STATE FILE I/O ===`
- `# === STATE MANAGEMENT OPERATIONS ===`
- `# === CLI PARSING & ENTRY POINT ===`

Type hints and docstrings are comprehensive, so the code documents itself.

If you want to extend pbjson:
1. Add new commands by updating `COMMAND_TO_FIELD`
2. Add new state file operations by adding functions in the STATE MANAGEMENT section
3. Update the CLI in `main()` to route to your new function

All of it fits in one file and stays readable.

**When filing a pull request, please include:**

- Clear description of what changes and why
- Any relevant examples or test cases
- Any questions or clarifications regarding your work

### When using AI assistance:
- **pbjson** is designed around constraints that AI assistants like Claude have no first-hand experience of
- Agents can be exceptionally helpful in revising and troubleshooting code, but may misunderstand its use case
- They do not necessarily understand that **the user is the AI agent itself**--but you can! 

Keep in mind that **pbjson is intentionally minimal**—it's a single-purpose, focused tool, not a Swiss Army knife.

## Why "pbjson"?

**Project Breadcrumbs JSON** — leaving a trail of decisions so you (and your AI assistant) can find your way back.

*Full disclosure: I didn't come up with the pun. That was fate, in the form of Claude Sonnet 4.5 itself: this project was named by its main user audience!* 
