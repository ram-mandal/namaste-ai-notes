# Copilot Instructions for Namaste AI Notes

Maintain consistent, high-quality notes across all episodes and resources.

## Accuracy (most important)

- **Only include information the user has provided or that is verifiable** in the existing files.
- **Never invent** episode titles, lesson names, project names, repo names, links, or facts.
- **Check the truth before adding anything** — read the actual files to confirm a link, name, or claim exists before referencing it.
- If details are unknown, leave a clear placeholder (e.g. `_(coming soon)_`) instead of guessing.
- The course is **high-level AI** (LLMs, prompt engineering, RAG, AI for developers, AI tools, AI agents, MCP, AI engineering). It does **not** cover ML, neural networks, backpropagation, or optimization — do not add such content.
- **RAG** and **MCP** are separate topics — never merge them (e.g. no "AI Agents & MCP" combined topic).

## Notes Style (concise)

These are **notes, not a tutorial** — keep them short and scannable.

- **Small points only** — short bullets, no long explanations or "why" paragraphs
- **No redundancy** — don't repeat the same point across sections (e.g. no separate "checklist" + "tips" + "takeaways" all saying the same thing)
- **Drop video-oriented filler** — no "how to watch the videos", "questions to think about", or motivational prose
- **Keep only relevant facts** — roadmap, prerequisites, key points, links
- Prefer a single compact section over several overlapping ones

## Learning Philosophy

> **Slow, Intentional Learning Over Quick Consumption**

- Emphasize pausing, reflecting, rewatching — not binge-watching
- Encourage sequential learning (no skipping ahead)
- Remind learners to make their own notes
- 2-4+ month timeline for proper learning

## Core Principles

1. **Simple language** — Explain like talking to a 5th grader
2. **One concept per section** — Don't mix topics
3. **Visual-first** — Tables, blockquotes before long paragraphs
4. **Example-driven** — Show before you tell
5. **Consistency** — All episodes look and feel the same
6. **Split long lists** — When a list has many items (roughly 6+), split it into a two-column (left/right) layout using a table so it reads as two side-by-side groups instead of one long vertical list.

   ```markdown
   | | |
   |:---|:---|
   | - Item 1 | - Item 4 |
   | - Item 2 | - Item 5 |
   | - Item 3 | - Item 6 |
   ```

## Tone

- Conversational: use "you" and "we"
- Start with "why" before "how"
- Active voice, short sentences (max 2 lines)
- Real-world analogies before code
- No jargon without explanation
- No instructor names — focus on concepts
- No rushing or binge-learning

## Episode Structure Template

Every episode `.md` file follows the structure used in `season-1/episode-01-welcome-to-namaste-ai.md`:

```markdown
# Episode [Number]: [Clear, Simple Title]

> One-line summary of what this teaches.

---

## What you'll learn
- Key point 1
- Key point 2
- Key point 3

## [Topic-specific sections]
[Content for this episode]

## Key Takeaways
[Summary of the main points]

## What Comes Next
[What the next episode covers]

## Resources
[Links to related pages]

---

**Status:** Episode [Number] complete

---

| ← Previous | Next → |
|:---:|:---:|
| [Episode N-1: Title](./episode-XX-title.md) | [Episode N+1: Title](./episode-XX-title.md) |
```

## Code Examples

- Small and focused (3-10 lines ideal)
- Comments explaining each step
- Realistic variable names (no single letters except in math)
- Include expected output
- Test before submitting

## Naming Conventions

- **Episode files:** `episode-NN-[descriptive-slug].md` (lowercase, 2-digit number, e.g. `episode-01-welcome-to-namaste-ai.md`)
- **Asset files:** `assets/[topic]-[description].png` (descriptive, not `image1.png`)

## Before Committing

- [ ] Title is clear & simple (5-7 words)
- [ ] Follows the template structure above
- [ ] "What you'll learn" has 3-5 bullets
- [ ] Simple explanation comes before code/diagrams
- [ ] Visual explanation included (ASCII art or asset link)
- [ ] Code is runnable, tested, with comments
- [ ] Real-world example exists
- [ ] No unexplained jargon
- [ ] Takeaways section has 3-5 bullets
- [ ] Next episode linked
- [ ] No broken links

