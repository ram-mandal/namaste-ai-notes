# Copilot Instructions for Namaste AI Notes

Maintain consistent, high-quality notes across all episodes and resources.

## Accuracy (most important)

- **Only include information the user has provided or that is verifiable** in the existing files.
- **Never invent** episode titles, lesson names, project names, repo names, links, or facts.
- **Check the truth before adding anything** — read the actual files to confirm a link, name, or claim exists before referencing it.
- If details are unknown, leave a clear placeholder (e.g. `_(coming soon)_`) instead of guessing.
- The course is **high-level AI** (LLMs, prompt engineering, RAG, AI for developers, AI tools, AI agents, MCP, AI engineering). It does **not** cover ML, neural networks, backpropagation, or optimization — do not add such content.
- **RAG** and **MCP** are separate topics — never merge them (e.g. no "AI Agents & MCP" combined topic).

## Notes Style (concise but not shallow)

These are **personal learning notes, not a tutorial** — concise, scannable, but with real depth where it matters.

- **Concise, not shallow** — short bullets for facts, but allow 2-4 sentence explanations where a concept genuinely needs them
- **No redundancy** — don't repeat the same point across sections (e.g. no separate "checklist" + "tips" + "takeaways" all saying the same thing)
- **Don't blindly bullet everything** — use prose where it reads better
- **Keep only relevant facts** — roadmap, prerequisites, key points, links
- Prefer a single compact section over several overlapping ones

## Distinguishing Content Types

Clearly separate these in every episode:
- **What the course teaches** — "Key Concepts" and "Deep Dive" sections: neutral, technically accurate
- **My own understanding** — "My Mental Model": first person, analogies, informal
- **My observations/opinions** — "Practical / Engineering Connection": first person, engineering framing
- **Things I still need to investigate** — "Questions / Things to Explore": open questions, flagged as such

If something in the notes appears technically questionable, **flag it** (e.g. `> ⚠️ Needs verification: ...`) rather than silently changing it.

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
7. **Readable formatting** — Keep the page visually calm, use consistent spacing, and avoid decorative formatting that competes with the content.

### Episode Formatting

- Use one clear H1 for the episode title: `# Episode NN: <Title>`
- Put the full season name on the next line as a blockquote: `> **Season N — <Season Name>**`
- Keep the title and season label on separate lines; do not mix font sizes inside one heading
- Use `<details>` accordions for longer sections, keeping the first primary section expanded by default
- Give each accordion a consistent inline title style and `margin-bottom: 1rem;` spacing
- Add a clear `coming soon` label to unfinished accordion titles
- Keep visual notes in a separate, collapsed-by-default `Quick Notes` section, outside the main learning accordions
- Use a table or gallery-style layout for visual notes so additional images can be added without restructuring the page
- Keep `Course Roadmap` open by default when it is the first main learning section
- Preserve semantic heading order inside accordions and keep colors, contrast, and spacing accessible

## Tone

- Conversational: use "you" and "we"
- Start with "why" before "how"
- Active voice, short sentences (max 2 lines)
- Real-world analogies before code
- No jargon without explanation
- No instructor names — focus on concepts
- No rushing or binge-learning

## Episode Structure (flexible)

Every episode starts with a title, season label, and one-line summary:

```markdown
# Episode XX: <Title>

> **Season X — <Season Name>**

> One-line summary of what this episode is about.
```

Then **include only the sections that make sense for this episode** — do NOT force every section on every episode. Available sections (pick as needed):

- 🎯 Episode Overview
- 🧠 Key Concepts
- 🔍 Deep Dive
- 💡 My Mental Model
- 🧩 Visual Explanation
- 📝 Key Takeaways
- 🤔 Questions / Things to Explore
- 🛠️ Practical / Engineering Connection
- 🧪 Experiments / Projects
- 🔗 Resources
- ➡️ Next

**Rules:**
- Only include a section if it has real content for this episode — no filler or empty placeholder sections
- "My Mental Model" and "Questions" are explicitly *personal* — first person, informal
- "Key Concepts" and "Deep Dive" are *course content* — technically accurate, neutral tone
- End every episode with a prev/next navigation table:

```markdown
| ← Previous | Next → |
|:---:|:---:|
| [Episode N-1: Title](./episode-XX-title.md) | [Episode N+1: Title](./episode-XX-title.md) |
```

## Visual Explanation Guidance

For each episode, identify the **ONE** most important concept that deserves a visual representation. Choose the form that fits the topic:

- Timeline (e.g. evolution of AI)
- Concept map / flow diagram (e.g. course roadmap)
- Architecture / pipeline diagram (e.g. RAG pipeline, agent loop)
- Before → After (e.g. with/without RAG)
- Decision tree / comparison diagram

**Rules:**
- Prefer Mermaid for flow/architecture/timeline; ASCII when Mermaid would be excessive
- Tables only when they genuinely improve readability
- **Do not force a diagram** when it doesn't add value — a short note like `_(No diagram needed for this episode.)_` is fine
- Don't force every episode to have the same visual layout — the representation should depend on the topic

## Code Examples

- Small and focused (3-10 lines ideal)
- Comments explaining each step
- Realistic variable names (no single letters except in math)
- Include expected output
- Test before submitting

## Naming Conventions

- **Episode files:** `episode-NN-[descriptive-slug].md` (lowercase, 2-digit number, e.g. `episode-01-welcome-to-namaste-ai.md`)
- **Asset files:** `assets/<season-folder>/episode-NN/<filename>` — one folder per episode (e.g. `assets/season-1-inside-the-mind-of-ai/episode-01/episode-01-welcome-to-namaste-ai.png`, `assets/season-1-inside-the-mind-of-ai/episode-02/Alan_turing_header.jpg` — descriptive names, not `image1.png`)
- **Episode titles:** always include the season — `# Season N — Episode NN: Title`

## Before Committing

- [ ] Episode title is clear and simple (5-7 words)
- [ ] Full season name appears below the title in a consistent blockquote
- [ ] Long sections use consistent, readable accordions with the first primary section expanded
- [ ] Only sections that make sense for this episode are included (no filler/empty sections)
- [ ] Course content vs. personal understanding clearly separated
- [ ] Visual explanation included (or explicitly noted as not needed)
- [ ] Code is runnable, tested, with comments
- [ ] No unexplained jargon
- [ ] Key Takeaways has 3-7 bullets
- [ ] Next episode linked (and prev/next table correct)
- [ ] Season README episode table updated (status column)
- [ ] No broken links

