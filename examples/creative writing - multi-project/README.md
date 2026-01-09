# Domain Example: Creative Writing Project

A realistic example of using pbjson for a novel or long-form story project. Do you use Claude for creative brainstorming? This might be for you! 

## Why pbjson works for writing

Writers can face the same challenges as software developers:
- **Decisions about structure** ("Should this be a trilogy or standalone?")
- **Questions about continuity** ("When did the character learn magic?")
- **Tracking what's been written** ("Did I describe the city layout yet?")
- **Context that changes during writing** ("The main character's motivation shifted in Chapter 4")
- **Collaborating with editors/AI assistants** ("Why did I make this character choice?")

## Scenario: Writing a Science Fiction Novel

You're writing a novel about first contact with an alien civilization. You're using Claude to brainstorm worldbuilding, test dialogue, and work through plot problems.

## Initial Planning

```bash
# Upload pbjson.py to your project context 

# Start discussing!

# High-level project decisions
./pbjson.py decided "First contact story, told from human perspective, not aliens"
./pbjson.py decided "Hard sci-fi grounded in physics, not fantasy"
./pbjson.py context "Target audience: fans of Ted Chiang, Project Hail Mary, The Three-Body Problem"
./pbjson.py context "Novel length target: 100,000-120,000 words"

# Worldbuilding decisions
./pbjson.py decided:worldbuilding "Earth date: 2087, near-future tech"
./pbjson.py decided:worldbuilding "First contact happens at Jupiter, not Earth orbit"
./pbjson.py context:worldbuilding "Gives more time for negotiation before aliens reach political centers"

# Character development
./pbjson.py decided:characters "Protagonist is a translator, not a scientist or military"
./pbjson.py context:characters "Unique perspective on communication and cultural misunderstanding"
./pbjson.py file:characters "character_profiles.md - detailed profiles and motivations"

# Plot structure
./pbjson.py decided:plot "Three act structure: discovery → communication → choice"
./pbjson.py context:plot "Act 1 is the mystery, Act 2 is the relationship, Act 3 is the consequence"

# Key entry points
./pbjson.py file "outline.md - chapter-by-chapter breakdown"
./pbjson.py file "worldbuilding/ - maps, tech specs, cultural notes"
```

## First Draft: Worldbuilding Phase

```bash
# Fleshing out the world
./pbjson.py built:worldbuilding "Earth timeline (search: timeline.md, 2040-2087)"
./pbjson.py built:worldbuilding "Spacecraft specifications (search: spacecraft.md, Achilles class)"
./pbjson.py context:worldbuilding "Decided that humans have Mars colonies but no jump drive yet"

# Alien worldbuilding
./pbjson.py question:worldbuilding "What do aliens look like? Humanoid? Truly alien?"
./pbjson.py context:worldbuilding "Want them recognizable enough to relate to, not so weird it strains reader belief"
# ... brainstorm with Claude ...
./pbjson.py built:worldbuilding "Alien physiology (search: aliens.md, silicon-based, hive-minded)"
./pbjson.py resolve:worldbuilding "alien appearance" "Non-humanoid but bilaterally symmetric, hive intelligence but individual-capable, communicates via color patterns"

# Character profiles
./pbjson.py built:characters "Dr. Evelyn Chen - protagonist (search: chen_profile.md)"
./pbjson.py built:characters "Commander Okonkwo - military liaison (search: okonkwo_profile.md)"
./pbjson.py question:characters "Should Chen have a romantic subplot or keep focus on first contact?"
./pbjson.py context:characters "Worried romance would distract from main themes"
```

## Second Phase: Outlining & Discovery

```bash
# As you outline and discover the story
./pbjson.py built:plot "Chapter-by-chapter outline (search: chapter_outline.md, 25 chapters)"
./pbjson.py question:plot "Does the climax work logically? Can humans really choose differently?"

# Worldbuilding questions come up during outlining
./pbjson.py question:worldbuilding "How would 24-hour light cycle at Jupiter affect aliens?"
./pbjson.py built:worldbuilding "Jupiter environment specifications (search: jupiter_env.md)"

# A key realization
./pbjson.py context:plot "Realized the middle act is sagging—need higher stakes earlier"
./pbjson.py question:plot "Should I move the misunderstanding/conflict to Chapter 7 instead of Chapter 12?"

# Discussing with Claude, you decide
./pbjson.py decided:plot "Move first major conflict to Chapter 7 for pacing"
./pbjson.py context:plot "Gives more time for trust-building aftermath in later chapters"
```

## First Draft: Writing Phase

```bash
# Progress tracking
./pbjson.py built "Chapter 1: First Signal (search: ch01_first_signal.md, 3000 words)"
./pbjson.py built "Chapter 2-3: Preparation (search: ch02-03.md, 6500 words)"
./pbjson.py built "Chapter 4: First Contact (search: ch04_contact.md, 4200 words)"

# Continuity checks
./pbjson.py question "Did I establish Chen's background with alien languages yet?"
./pbjson.py context "Chapter 1 mentioned she studied animal communication, not alien contact"
./pbjson.py resolved "Chen backstory" "Added footnote in Chapter 2 about animal language work, connects to alien communication"

# Writing decisions
./pbjson.py question "Dialogue: should aliens speak in complete sentences or fragments?"
./pbjson.py context "Fragments might feel more alien but harder for reader to parse"
./pbjson.py resolved "alien dialogue" "Use fragments for first encounter, shift to complete sentences as communication improves"

# Character discovery
./pbjson.py built "Chapters 5-8: Relationship Building (search: ch05-08.md, 14000 words)"
./pbjson.py context "Chen's character deepened in Chapter 6—she's more defensive than I initially wrote"
./pbjson.py question:characters "Should I go back and revise Chapters 1-5 to show this defensiveness earlier?"

# Decision about revision timing
./pbjson.py resolved:characters "early revision" "Keep first draft consistent, revise in editing pass for character consistency"
```

## First Draft: Problem Solving

```bash
# You hit the middle act slowdown that was warned about
./pbjson.py question "Chapter 12-15 feel repetitive. How many communication attempts before breakthrough?"
./pbjson.py context "First draft has 5, probably overkill"

# Decision
./pbjson.py decided:plot "Reduce failed communication attempts to 2, make them count more"
./pbjson.py built "Revised Chapters 12-14 (search: ch12-14_v2.md, pacing tightened)"

# Worldbuilding issue discovered mid-draft
./pbjson.py question:worldbuilding "Timeline problem: alien homeworld is 8 light-years away. How did they reach Earth so quickly?"
./pbjson.py context:worldbuilding "This affects first contact plausibility and the entire story premise"
./pbjson.py resolved:worldbuilding "light-year problem" "Aliens were already in the solar system exploring—first signal was from outer solar system probe, not homeworld"
./pbjson.py built:worldbuilding "Alien exploration timeline (search: exploration_timeline.md)"

# That decision ripples
./pbjson.py built "Chapter 17: The Probe Revelation (search: ch17_probe.md, 3500 words, new scene)"
./pbjson.py context "Opens up questions about alien motivation—explorers vs invaders"
```

## Finishing First Draft

```bash
# Final push
./pbjson.py built "Chapters 18-25: Climax and Resolution (search: ch18-25_draft.md, 22000 words)"
./pbjson.py context "Completed first draft: 102,000 words"
./pbjson.py file "manuscript_v1.md - complete first draft"

# Final check questions
./pbjson.py question "Did I resolve all plot threads from Act 1?"
./pbjson.py question "Does Chen's character arc feel earned?"
./pbjson.py question:worldbuilding "Any continuity errors with timeline or alien biology?"
```

## Revision Phase

```bash
# Editorial feedback and decisions
./pbjson.py context "Agent feedback: strengthen Chen's voice, make aliens more alien"
./pbjson.py decided "Revision priorities: voice > consistency > pacing"

# Revision work
./pbjson.py built "Revision pass 1: Chen's voice (search: ch01-08_v2.md, first 8 chapters)"
./pbjson.py built "Revision pass 2: Alien descriptions (search: alien_descriptions_v2.md)"

# New questions in revision
./pbjson.py question "Should Okonkwo's subplot resolve? Current ending leaves it open"
./pbjson.py resolved "Okonkwo ending" "Keep it open—readers should wonder what happens to him, implies consequences ongoing"

# Beta reader feedback
./pbjson.py context "Beta readers: Chapter 3 pacing issue confirmed, Chapter 19 emotional beat landed well"
./pbjson.py built "Revision pass 3: Chapters 1-4 restructure (search: ch01-04_v3.md)"

# Final polish
./pbjson.py built "Final read-through and copy edits (search: manuscript_final.md)"
./pbjson.py context "Manuscript ready for querying"
```

## Final State: 

### `project.json`

```json
{
  "what_we_decided": [
    "2026-01-09 - First contact story, told from human perspective, not aliens",
    "2026-01-09 - Hard sci-fi grounded in physics, not fantasy",
    "2026-01-12 - Move first major conflict to Chapter 7 for pacing",
    "2026-02-01 - Revision priorities: voice > consistency > pacing"
  ],
  "what_we_built": [
    "2026-01-15 - Outline and chapter breakdown (search: outline.md)",
    "2026-01-20 - First draft: Chapters 1-8 (search: ch01-08.md)",
    "2026-02-01 - First draft: Chapters 9-17 (search: ch09-17.md)",
    "2026-02-15 - First draft: Chapters 18-25 (search: ch18-25.md)",
    "2026-02-20 - Revision pass 1: Chen's voice (search: ch01-08_v2.md)",
    "2026-03-01 - Revision pass 2: Alien descriptions (search: alien_descriptions_v2.md)",
    "2026-03-10 - Revision pass 3: Chapters 1-4 restructure (search: ch01-04_v3.md)",
    "2026-03-20 - Final manuscript ready for querying (search: manuscript_final.md)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-15 - Does the climax work logically? Can humans really choose differently? → Resolved: Yes, adds weight to the choice and invites reader interpretation"
  ],
  "important_files": [
    "2026-01-09 - outline.md - chapter-by-chapter breakdown",
    "2026-01-09 - worldbuilding/ - maps, tech specs, cultural notes",
    "2026-03-20 - manuscript_final.md - final draft ready to query"
  ],
  "context": [
    "2026-01-09 - Target audience: fans of Ted Chiang, Project Hail Mary, The Three-Body Problem",
    "2026-01-09 - Novel length target: 100,000-120,000 words",
    "2026-02-15 - Completed first draft: 102,000 words",
    "2026-02-20 - Agent feedback: strengthen Chen's voice, make aliens more alien",
    "2026-03-01 - Beta readers: Chapter 3 pacing issue confirmed, Chapter 19 emotional beat landed well",
    "2026-03-20 - Manuscript ready for querying"
  ]
}
```

### Subsystem: `worldbuilding-state.json`

```json
{
  "subsystem": "worldbuilding",
  "what_we_decided": [
    "2026-01-09 - Earth date: 2087, near-future tech",
    "2026-01-09 - First contact happens at Jupiter, not Earth orbit",
    "2026-01-12 - Aliens were already in the solar system exploring—first signal from outer solar system probe"
  ],
  "what_we_built": [
    "2026-01-11 - Earth timeline (search: timeline.md, 2040-2087)",
    "2026-01-11 - Spacecraft specifications (search: spacecraft.md, Achilles class)",
    "2026-01-12 - Alien physiology (search: aliens.md, silicon-based, hive-minded)",
    "2026-01-15 - Jupiter environment specifications (search: jupiter_env.md)",
    "2026-01-20 - Alien exploration timeline (search: exploration_timeline.md)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-11 - What do aliens look like? Humanoid? Truly alien? → Resolved: Non-humanoid but bilaterally symmetric, hive intelligence but individual-capable, communicates via color patterns",
    "2026-01-15 - Timeline problem: alien homeworld is 8 light-years away. How did they reach Earth so quickly? → Resolved: Aliens were already in the solar system exploring"
  ],
  "important_files": [
    "2026-01-09 - worldbuilding/ - master folder for all world details"
  ],
  "context": [
    "2026-01-09 - Gives more time for negotiation before aliens reach political centers",
    "2026-01-11 - Humans have Mars colonies but no jump drive yet",
    "2026-01-12 - Aliens: silicon-based, hive-minded but capable of individuality"
  ]
}
```

### Subsystem: `characters-state.json`

```json
{
  "subsystem": "characters",
  "what_we_decided": [
    "2026-01-09 - Protagonist is a translator, not a scientist or military",
    "2026-01-12 - Keep first draft consistent, revise in editing pass for character consistency"
  ],
  "what_we_built": [
    "2026-01-12 - Dr. Evelyn Chen - protagonist (search: chen_profile.md)",
    "2026-01-12 - Commander Okonkwo - military liaison (search: okonkwo_profile.md)",
    "2026-02-20 - Character revision notes for Chen's defensive personality (search: chen_notes_v2.md)"
  ],
  "what_we_need_to_decide": [
    "2026-01-12 - Should Chen have a romantic subplot or keep focus on first contact?",
    "2026-02-01 - Should I go back and revise Chapters 1-5 to show this defensiveness earlier?"
  ],
  "what_we_resolved": [
    "2026-02-01 - early revision → Decided: Keep first draft consistent, revise in editing pass for character consistency",
    "2026-03-05 - Should Okonkwo's subplot resolve? Current ending leaves it open → Decided: Keep it open—readers should wonder what happens to him, implies consequences ongoing"
  ],
  "important_files": [
    "2026-01-09 - character_profiles.md - detailed profiles and motivations"
  ],
  "context": [
    "2026-01-09 - Unique perspective on communication and cultural misunderstanding",
    "2026-01-12 - Worried romance would distract from main themes",
    "2026-02-01 - Chen's character deepened in Chapter 6—she's more defensive than I initially wrote"
  ]
}
```

### Subsystem: `plot-state.json`

```json
{
  "subsystem": "plot",
  "what_we_decided": [
    "2026-01-09 - Three act structure: discovery → communication → choice",
    "2026-01-12 - Move first major conflict to Chapter 7 for pacing",
    "2026-01-20 - Reduce failed communication attempts to 2, make them count more"
  ],
  "what_we_built": [
    "2026-01-12 - Chapter-by-chapter outline (search: chapter_outline.md, 25 chapters)",
    "2026-01-20 - Revised Chapters 12-14 (search: ch12-14_v2.md, pacing tightened)",
    "2026-02-01 - Chapter 17: The Probe Revelation (search: ch17_probe.md, 3500 words, new scene)"
  ],
  "what_we_need_to_decide": [
    "2026-02-15 - Did I resolve all plot threads from Act 1?",
    "2026-02-15 - Does Chen's character arc feel earned?"
  ],
  "what_we_resolved": [
    "2026-01-15 - Does the climax work logically? Can humans really choose differently? → Decided: Yes, adds weight to the choice and invites reader interpretation",
    "2026-01-20 - Should I move the misunderstanding/conflict to Chapter 7 instead of Chapter 12? → Decided: Yes, gives more time for trust-building aftermath in later chapters",
    "2026-01-20 - Chapter 12-15 feel repetitive. How many communication attempts before breakthrough? → Decided: Reduce to 2 failed attempts, make them count more"
  ],
  "important_files": [],
  "context": [
    "2026-01-09 - Act 1 is the mystery, Act 2 is the relationship, Act 3 is the consequence",
    "2026-01-12 - Realized the middle act is sagging—need higher stakes earlier",
    "2026-01-12 - Gives more time for trust-building aftermath in later chapters",
    "2026-01-20 - First draft has 5 communication attempts, probably overkill",
    "2026-02-01 - Opens up questions about alien motivation—explorers vs invaders"
  ]
}
```

## Why This Matters for Writers

- **Continuity tracking**: No more "Wait, did I mention this character's background?"
- **Decision reasoning**: "Why did I make them an alien explorer rather than invader?" is answerable by looking at the journal
- **Brainstorming record**: Questions you asked Claude are preserved—you can revisit them later
- **Revision confidence**: You can see what changed and why
- **Collaboration**: Editors, beta readers, or writing partners can read the context for decisions

The state files become a writer's notes, but organized and searchable in a way that survives across tools and conversations.