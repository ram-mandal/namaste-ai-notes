# Contributing to Namaste AI Notes

Thank you for your interest in improving these notes! 🙏 Whether it's a typo, a clearer explanation, a new section, or an idea you'd like to share, your contributions are welcome and appreciated.

## How to Contribute

### Spot a Mistake or Unclear Explanation?

1. **Open an issue** — describe what's wrong or confusing (with a link to the episode if possible).
2. **Submit a pull request** — fork the repo, make your fix, and send a PR with a clear title and description.

### Want to Add Content?

- **Before you start**, open an issue to discuss your idea — this prevents overlapping work and ensures your contribution aligns with the repo's scope.
- Follow the [Episode Structure Guide](#episode-structure-guide) below to maintain consistency.

### Scope

This repo covers **high-level AI** — LLMs, prompt engineering, RAG, AI agents, MCP, and AI engineering. It does **not** include deep ML math, neural networks, or model training.

## Episode Structure Guide

All episodes follow a consistent format. See [.github/copilot-instructions.md](.github/copilot-instructions.md) for the full guide, but here are the essentials:

### Filename & Frontmatter

- Filename: `episode-NN-[descriptive-slug].md` (e.g., `episode-01-welcome-to-namaste-ai.md`)
- Start with season blockquote and H1 title:
  ```markdown
  > **Season X — <Season Name>** [🔗](./README.md)
  
  # Episode XX: <Title>
  
  > One-line summary with core keywords (LLM, RAG, AI agents, etc.)
  ```

### Content Sections

Include **only** the sections that make sense for your episode (don't force all of them):

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

### Key Rules

- **Accuracy first** — verify all facts and links in existing files; never invent details.
- **Simple language** — explain like talking to a beginner; avoid jargon without explanation.
- **No redundancy** — don't repeat the same point across sections.
- **Visual explanations** — max one diagram per episode (Mermaid preferred); don't force one if not needed.
- **Keyword-rich summaries** — the one-line summary should include core topic keywords (e.g., "How do LLMs understand context?" not "Understanding LLMs").
- **Descriptive image alt text** — all images must have meaningful alt text.
- **Cross-links** — link related episodes where relevant (improves discoverability).
- **End with navigation** — every episode ends with a prev/next table.

### Before Submitting

- [ ] Episode title is clear and simple (5–7 words)
- [ ] Full season name appears on first line in blockquote with link to season README
- [ ] One-line summary contains core keywords
- [ ] Only relevant sections included (no empty placeholders)
- [ ] Course content (Key Concepts, Deep Dive) is neutral; personal sections (My Mental Model, Questions) use first person
- [ ] Visual explanation included or explicitly noted as not needed
- [ ] All images have descriptive alt text
- [ ] Related episodes cross-linked in prose
- [ ] No broken links or typos
- [ ] Season README episode table updated (status column)
- [ ] Root README episode index updated with new episode link

## Style & Tone

- **Conversational** — use "you" and "we"
- **Explain why first** — before jumping to how
- **Active voice, short sentences** — max 2 lines per sentence
- **Real-world analogies** — before code or formulas
- **No rushing** — emphasize slow, intentional learning

## Questions?

Open an issue on [GitHub](https://github.com/ram-mandal/namaste-ai-notes/issues) or reach out! Every contribution—no matter how small—helps these notes grow. 🌱

---

**Thank you for helping make AI learning accessible and clear!** ✨
