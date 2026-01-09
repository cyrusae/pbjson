# Single-Project Example: LLM Research Learning Path

A realistic example of using pbjson to track a self-directed learning project on large language models.

## Scenario

You're learning about LLM internals—attention mechanisms, tokenization, interpretability. You're reading papers, writing notes, and building small implementations to understand concepts. You're using Claude to help explain papers and debug code.

## Initial Setup

```bash
# Upload pbjson.py to your project and add your custom instructions

# Start a conversation with Claude

# Claude starts tracking:
./pbjson.py decided "Focus on mechanistic interpretability as main thread"
./pbjson.py context "Reading papers from Distill, Transformer Circuits, and arXiv"
./pbjson.py context "Using Claude Haiku for paper explanations and code review"
./pbjson.py file "README.md - overview of learning path and resources"
./pbjson.py file "papers/ - organized by topic, with reading notes"
```

## As You Work

```bash
# First week: foundational concepts
./pbjson.py built "Attention mechanism visualization (search: attention_viz.py)"
./pbjson.py question "How do layer norms and residual connections interact?"
./pbjson.py built "Residual connection playground notebook (search: residuals.ipynb)"

# Decision point: methodology
./pbjson.py question "Should I implement attention from scratch or use transformers library?"
# ... discuss with Claude, decide ...
./pbjson.py resolve "implement from scratch" "Build from scratch first for understanding, then compare with library implementation"

# Second week: deeper into papers
./pbjson.py built "Paper summary: Attention Is All You Need (search: attention_is_all.md)"
./pbjson.py question "What's the relationship between different attention variants (MQA, GQA)?"
./pbjson.py context "Found that Anthropic's papers on scaling laws are more accessible than original DeepMind work"

# Hitting a wall, need to pivot
./pbjson.py question "Is mechanistic interpretability actually the right focus or should I learn scaling laws?"
./pbjson.py context "Realizing interpretability research requires deeper math background than I have"
# ... discussion with Claude ...
./pbjson.py resolve "interpretability focus" "Pivot to scaling laws first, return to interpretability after building math foundation"
./pbjson.py decided "Study scaling laws as gateway to interpretability work"

# Third week: implementing learnings
./pbjson.py built "Toy transformer implementation (search: toy_transformer.py, 1000 lines)"
./pbjson.py built "Test suite comparing outputs to Hugging Face (search: test_outputs.py)"
./pbjson.py question "How to efficiently test that my implementation matches reference?"

# Fourth week: synthesis
./pbjson.py built "Learning summary blog post (search: blog_post.md)"
./pbjson.py context "Next goal: contribute to open source interpretability projects"
```

## Final state: `project.json`

```json
{
  "what_we_decided": [
    "2026-01-09 - Focus on mechanistic interpretability as main thread",
    "2026-01-12 - Study scaling laws as gateway to interpretability work"
  ],
  "what_we_built": [
    "2026-01-09 - Attention mechanism visualization (attention_viz.py)",
    "2026-01-10 - Residual connection playground notebook (residuals.ipynb)",
    "2026-01-11 - Paper summary: Attention Is All You Need (attention_is_all.md)",
    "2026-01-15 - Toy transformer implementation (toy_transformer.py, 1000 lines)",
    "2026-01-16 - Test suite comparing outputs to Hugging Face (test_outputs.py)",
    "2026-01-18 - Learning summary blog post (blog_post.md)"
  ],
  "what_we_need_to_decide": [],
  "what_we_resolved": [
    "2026-01-10 - Should I implement attention from scratch or use transformers library? → Decided: Build from scratch first for understanding, then compare with library implementation",
    "2026-01-15 - Is mechanistic interpretability actually the right focus or should I learn scaling laws? → Decided: Pivot to scaling laws first, return to interpretability after building math foundation"
  ],
  "important_files": [
    "2026-01-09 - README.md - overview of learning path and resources",
    "2026-01-09 - papers/ - organized by topic, with reading notes",
    "2026-01-15 - toy_transformer.py - reference implementation to learn from"
  ],
  "context": [
    "2026-01-09 - Reading papers from Distill, Transformer Circuits, and arXiv",
    "2026-01-09 - Using Claude Haiku for paper explanations and code review",
    "2026-01-12 - Found that Anthropic's papers on scaling laws are more accessible than original DeepMind work",
    "2026-01-15 - Realizing interpretability research requires deeper math background than I have",
    "2026-01-18 - Next goal: contribute to open source interpretability projects"
  ]
}
```

## Why This Works

- **Decisions are visible**: You can grep for "scaling laws" and see when/why you made that pivot
- **Progress is tangible**: Each entry shows what you actually built and learned
- **Questions create accountability**: Open questions stay open until resolved
- **Context survives between sessions**: Your Claude instance can see constraints and preferences without you re-explaining
- **Future reference**: Six months later, you'll remember *why* you chose this learning path

## Using It With Claude

**Typical conversation thread:**

```
You: Read this paper on scaling laws [uploads PDF]. Explain the key findings.

Claude: [Explains paper]

Claude (then): I've recorded this as context. Would you like to:
1. Dive deeper into specific findings?
2. Move forward with implementing something based on this?
3. Update your learning path based on what you've learned?

[After you respond]

Claude: ./pbjson.py context "Key insight: compute-optimal models differ from inference-optimal ones"

✓ Added to context: Key insight: compute-optimal models differ from inference-optimal ones
```

The state file becomes a searchable history of your learning journey.