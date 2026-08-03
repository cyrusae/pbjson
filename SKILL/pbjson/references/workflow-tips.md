# Workflow Tips for pbjson

This guide provides detailed workflow patterns, troubleshooting, and best practices for using pbjson across long-running projects and multiple conversations.

## Managing State Across Conversations

### The Manual Sync Challenge

**Key insight:** Claude doesn't automatically re-read project files between messages. Once a file is loaded into context, changes made via tool calls aren't automatically reflected unless you explicitly re-read them.

### When to Refresh State

**Always refresh at conversation start:**
```bash
cat project.json
cat {subsystem}-state.json  # if using subsystems
```

**When to manually re-read during conversation:**
- After multiple pbjson.py calls (every 5-10 updates)
- Before answering questions like "what did we decide about X?"
- When reviewing project status or creating summaries
- If Claude's responses seem out of sync with recent decisions

**How to refresh:**
```bash
# Quick refresh of main state
cat project.json

# Or view specific subsystems
cat frontend-state.json
cat backend-state.json
```

### Best Practice Workflow

1. **Start of conversation:** Load all state files
2. **During work:** Update via pbjson.py, call present_files()
3. **Before status checks:** Re-read state files
4. **End of session:** Final state review (optional)

## Effective Decision Recording

### What Makes a Good Decision Entry

**Good:**
```bash
python3 pbjson.py decided "Use PostgreSQL over MongoDB - need ACID guarantees for financial transactions"
python3 pbjson.py decided "API rate limiting at 100 req/min - balances UX with server costs"
```

**Less helpful:**
```bash
python3 pbjson.py decided "Use PostgreSQL"  # Missing reasoning
python3 pbjson.py decided "Decided to implement rate limiting based on our discussion about performance and costs and the trade-offs between..."  # Too verbose
```

**Key elements:**
- What: The decision itself
- Why: Brief reasoning (one clause)
- Keep it under 100 characters when possible

### Recording Work Effectively

**Include searchable terms:**

```bash
# Good - searchable by filename and concept
python3 pbjson.py built "User authentication (in auth.py, handles JWT)"
python3 pbjson.py built "Product search API (endpoint /api/search, uses Elasticsearch)"

# Less useful
python3 pbjson.py built "Finished the authentication stuff"
```

**Pattern:** "What was built (searchable context)"

## Handling Questions and Resolutions

### Question Lifecycle

1. **Identify uncertainty:** Discussion reveals open question
2. **Record immediately:**
   ```bash
   python3 pbjson.py question "Caching strategy - Redis vs in-memory?"
   ```
3. **Discuss with user:** Get input and agreement
4. **Resolve when decided:**
   ```bash
   python3 pbjson.py resolve "caching" "Redis with 1hr TTL - allows horizontal scaling"
   ```

### When Questions Pile Up

This is normal! Open questions list grows as project evolves. Strategies:

**Triage periodically:**
- Review `what_we_need_to_decide` list
- Some questions answer themselves as project evolves
- Resolve or remove outdated questions

**Use subsystems to separate:**
```bash
# Keep frontend and backend questions separate
python3 pbjson.py question:frontend "Component state management approach?"
python3 pbjson.py question:backend "Database migration strategy?"
```

## Multi-Subsystem Organization

### When to Create Subsystems

**Create subsystems for:**
- Distinct technical domains (frontend, backend, infra)
- Separate team responsibilities
- Different release cycles
- Domain boundaries in the problem space

**Keep in main project.json:**
- Cross-cutting decisions affecting multiple subsystems
- High-level architecture
- Project-wide constraints and context

### Subsystem Naming Conventions

**Good names:**
- `frontend-state.json`
- `backend-state.json`
- `docs-state.json`
- `infra-state.json`

**Avoid:**
- Nested names: `backend-api-state.json` (too granular)
- Generic names: `module1-state.json` (not descriptive)

### Accessing Subsystem State

```bash
# At conversation start, load all relevant subsystems
cat project.json
cat frontend-state.json
cat backend-state.json

# Or discover subsystems programmatically
ls *-state.json
```

## Important Files Field Strategy

### The 3-5 File Rule

This field is **not** for comprehensive file tracking. It's for navigation.

**Ask:** "If someone new joins, what files answer 'where do I start for X?'"

**Example for web app:**
```bash
python3 pbjson.py file "src/server.py - application entry point"
python3 pbjson.py file "tests/test_integration.py - start here for testing"
python3 pbjson.py file "config.example.yaml - configuration template"
```

**Don't add:**
- Every file you create
- Utility modules
- Model definitions
- Helper functions

### When to Update

**Add entries when:**
- Creating primary entry points
- Setting up key navigation points
- Defining configuration templates

**Remove entries when:**
- File becomes obsolete
- Entry point changes
- Exceeding 5 files (replace, don't just add)

## Troubleshooting Common Issues

### "Claude forgot what we decided"

**Likely cause:** State file not loaded at conversation start

**Solution:**
```bash
cat project.json  # Load state into context
```

**Prevention:** Always start conversations by loading state

### "State file shows old information"

**Cause:** File hasn't been re-read after updates

**Solution:**
```bash
cat project.json  # Refresh context
```

**When this happens:** After many pbjson.py calls in one conversation

### "Too many decisions, hard to find things"

**Strategies:**

1. **Use subsystems** to separate concerns
2. **Search within files:**
   ```bash
   grep -i "database" project.json
   grep -i "auth" backend-state.json
   ```
3. **Generate summaries:**
   ```python
   import json
   with open('project.json') as f:
       state = json.load(f)
   print("Recent decisions:", state['what_we_decided'][-5:])
   ```

### "Accidentally recorded wrong decision"

**Solution:** Manually edit the JSON file
```bash
# Edit in place
nano project.json

# Or in Python
python3 -c "
import json
with open('project.json', 'r') as f:
    state = json.load(f)
# Remove or edit entry
state['what_we_decided'] = [d for d in state['what_we_decided'] if 'wrong text' not in d]
with open('project.json', 'w') as f:
    json.dump(state, f, indent=2)
"
```

## Advanced Patterns

### Git Integration

pbjson files are git-friendly:

```bash
# Track state files
git add project.json *-state.json

# Commit decision points
git commit -m "Decided: Use PostgreSQL for persistence"

# Review history
git log --oneline project.json
git diff HEAD~1 project.json
```

### Generating Reports

Create project summaries from state:

```python
import json
from pathlib import Path

def generate_summary():
    state = json.load(open('project.json'))
    
    print("## Project Summary")
    print(f"\n### Decisions Made ({len(state['what_we_decided'])})")
    for decision in state['what_we_decided'][-5:]:
        print(f"- {decision}")
    
    print(f"\n### Recent Work ({len(state['what_we_built'])})")
    for work in state['what_we_built'][-5:]:
        print(f"- {work}")
    
    print(f"\n### Open Questions ({len(state['what_we_need_to_decide'])})")
    for question in state['what_we_need_to_decide']:
        print(f"- {question}")

generate_summary()
```

### Cross-Conversation Context

When returning to a project after time away:

1. **Load all state files** to rebuild context
2. **Review what_we_need_to_decide** for pending items
3. **Check what_we_resolved** for recent decisions
4. **Scan what_we_built** for latest implementations
5. **Read context field** for constraints and preferences

This rebuilds project understanding without rehashing everything.

## Tips for Long-Running Projects

### Periodic Maintenance

**Monthly review:**
- Resolve stale questions
- Archive old decisions (git tag milestone)
- Update important_files if entry points changed
- Refresh context field with new constraints

### Avoiding Bloat

**The append-only log will grow.** This is expected and valuable for history.

**If it becomes unwieldy:**
- Split by subsystem
- Archive old entries to `archive-{year}.json`
- Keep last 6 months in active files
- Maintain searchability via git history

### Team Coordination

When multiple people work on same project:

1. **Commit state files to git** - everyone syncs
2. **Review state before merging** - resolve conflicts manually
3. **Use subsystems** - reduces conflict surface
4. **Convention:** Include state update in commit message

Example:
```bash
git commit -m "Add user authentication

pbjson: built authentication module (auth.py)"
```
