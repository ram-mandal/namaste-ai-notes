# Copilot Instructions for Namaste AI Notes

**Purpose:** Maintain consistent, high-quality notes across all episodes, lessons, and resources.  
**Audience:** Contributors, maintainers, and AI assistants editing this repo.

---

## Course Learning Philosophy

Before writing or editing content, understand the course's learning approach:

> **Slow, Intentional Learning Over Quick Consumption**

This course prioritizes deep understanding over speed. Writers should:
- Emphasize pausing and reflecting, not binge-watching
- Encourage rewatching and rereading concepts
- Highlight the 2-4+ month timeline for proper learning
- Remind learners to make their own notes
- Stress doing exercises immediately when instructed
- Encourage sequential learning (no skipping ahead)

This philosophy should subtly influence tone, pacing of content, and how examples are explained.

---

## Core Principles

1. **Simple language** — Explain like you're talking to a 5th grader
2. **One concept per section** — Don't mix topics
3. **Visual-first** — Use tables, blockquotes, emphasis before long paragraphs
4. **Example-driven** — Show before you tell
5. **Consistency** — All lessons look and feel the same

---

## 📝 Tone & Voice

**DO:**
- Use "you" and "we" (conversational)
- Start with "why" before "how"
- Break long explanations into short paragraphs
- Use active voice ("I calculate" not "it is calculated")
- Add real-world analogies when possible
- Remind learners to take notes in their own words
- Encourage pausing, reflecting, rewatching

**DON'T:**
- Use jargon without explaining it
- Write sentences longer than 2 lines
- Assume prior knowledge
- Use passive voice
- Be overly formal or robotic
- Mention specific instructor names (focus on concepts)
- Encourage rushing or binge-learning

**Example - Simple way:**
```
Neural networks learn by adjusting weights. Think of weights like recipe ingredients—
adjust them to improve the cake, adjust them to improve predictions.
```

**Example - NOT recommended:**
```
The optimization of synaptic weights is performed through backpropagation
methodology, which iteratively minimizes the loss function via gradient descent.
```

---

## 📄 Lesson Structure Template

Every episode/lesson `.md` file should follow this structure:

```markdown
# Episode/Lesson [Number]: [Clear, Simple Title]

> One-line summary of what this teaches.

---

## What you'll learn
- Key point 1
- Key point 2
- Key point 3

## The simple explanation
[Core concept in 1-2 paragraphs]

## How it works (step by step)
1. Step 1
2. Step 2
3. Step 3

## Visual explanation
[Table, blockquote, or reference to diagram]

## Real-world example
[Concrete example from life]

## Key takeaways
- Takeaway 1
- Takeaway 2

## What comes next
[Link to next lesson]
```

---

## When to Use Markdown Visuals

**Use tables for:**
- Comparisons (do/don't, theory vs practice)
- Structured data
- Prerequisites, tools, or options

**Use blockquotes for:**
- Key insights or philosophies
- Important warnings
- Motivational reminders

**Use bold for:**
- First mention of important terms
- Key concepts you want to emphasize
- Action items

---

## 💻 Code Examples

**DO:**
- Use small, focused examples (3-10 lines ideal)
- Add comments explaining each step
- Use realistic variable names
- Include expected output
- Test the code before submitting

**DON'T:**
- Use overly complex code
- Skip comments
- Use single-letter variable names (except in math)
- Leave code untested

**Example - GOOD:**
````markdown
```python
# Simple neural network forward pass
def forward_pass(input_data, weights):
    # Multiply input by weights
    output = input_data * weights
    # Apply activation function
    return relu(output)

# Expected output: [0.5, 1.2, 0.0]
```
````

**Example - BAD:**
````markdown
```python
def fp(x, w):
    return relu(x * w)
```
````

---

## 🎨 Diagrams & Visual Aids

**DO:**
- Use simple ASCII art for quick concepts
- Reference external diagram files for complex visualizations
- Number and label diagrams clearly
- Place diagrams close to the text they explain

**DON'T:**
- Put huge diagrams inline (link to `assets/` folder instead)
- Use colored text that's hard to read
- Create diagrams without labels

**Example - Inline ASCII:**
````markdown
## How data flows

```
Input Data
    ↓
Weight Multiplication
    ↓
Activation Function
    ↓
Output
```
````

**Example - Reference external diagram:**
````markdown
## Complex architecture
See detailed diagram in [assets/neural-network-architecture.png](../assets/neural-network-architecture.png)
````

---

## 📚 Markdown Formatting Rules

**Headings:**
- `#` for lesson title only (used once per file)
- `##` for major sections (What you'll learn, Explanation, etc.)
- `###` for subsections only when needed

**Emphasis:**
- Use `**bold**` for important terms (first mention)
- Use `_italics_` sparingly (for emphasis, not styling)
- Use `**bold + code** for function names in text

**Lists:**
- Use `-` for unordered lists
- Use `1.` for ordered/step lists
- Keep list items short (one line ideal)
- Use sub-bullets for related details

**Code blocks:**
- Specify language (python, javascript, bash, etc.)
- Always include language identifier
- Add comments explaining what the code does
- Keep examples short and runnable

**Links:**
- Use descriptive link text: `[Lesson 02](./lesson-02.md)` ✅
- Avoid: `[Click here](./lesson-02.md)` ❌
- Link to sections: `[See diagram section](#visual-explanation)`

---

## ✅ Consistency Checklist

Before committing lesson notes, verify:

- [ ] **Title is clear & simple** (5-7 words max)
- [ ] **Headings follow the structure** (copy template above)
- [ ] **"What you'll learn" section exists** (3-5 bullet points)
- [ ] **Simple explanation comes first** (before code/diagrams)
- [ ] **Visual explanation included** (ASCII art or asset link)
- [ ] **Code example is runnable** (tested, with comments)
- [ ] **Real-world example exists** (not just theory)
- [ ] **No jargon** (or if used, explained immediately)
- [ ] **Sentences are short** (avoid 3+ lines)
- [ ] **Active voice used** (not passive)
- [ ] **Takeaways section exists** (3-5 bullets)
- [ ] **Resources linked** (at least 2-3)
- [ ] **Next lesson linked** (or "Check back soon")
- [ ] **No broken links** (verify all internal links work)
- [ ] **Grammar & spelling checked** (proof-read once)

---

## 🔄 Common Formatting Examples

**When introducing a term:**
```markdown
**Machine Learning** is a type of AI that learns from data without being programmed.
Think of it like learning to ride a bike—practice makes perfect.
```

**When showing comparisons:**
```markdown
| Traditional Programming | Machine Learning |
|------------------------|------------------|
| Tell computer all steps | Show computer examples |
| Hard to change logic | Easy to improve with more data |
```

**When explaining step-by-step:**
```markdown
1. **Step 1:** Prepare your data
   - Clean it
   - Split into train/test
2. **Step 2:** Build the model
   - Choose architecture
   - Initialize weights
3. **Step 3:** Train
   - Feed data through
   - Update weights
```

---

## Markdown Formatting Rules

**Headings:**
- `#` for episode/lesson title only (used once per file)
- `##` for major sections
- `###` for subsections only when needed

**Emphasis:**
- Use `**bold**` for important terms and key concepts
- Use `_italics_` sparingly
- Use blockquotes `>` for key insights and philosophies

**Tables:**
- Use for comparisons (do/don't, concept A vs B, pros/cons)
- Use for structured information
- Prefer tables over bullet lists when comparing options

**Lists:**
- Use `-` for unordered lists
- Use `1.` for step-by-step sequences
- Keep items short (one line ideal)

**Dividers:**
- Use `---` to separate major sections
- Creates visual breathing room

**Lesson files:**
- Format: `lesson-01.md`, `lesson-02.md`, etc.
- Always use lowercase
- Always use 2-digit numbers (01, not 1)

**Asset files:**
- Format: `assets/[topic]-[description].png`
- Example: `assets/neural-network-architecture.png`
- Use descriptive names, not generic (not `image1.png`)

**Project folders:**
- Separate repos (not in this repo)
- Naming: `namaste-ai-project-[short-name]`
- Example: `namaste-ai-project-rag-chatbot`

---

## 🚀 Before Creating a New Episode/Lesson

1. **Check existing content** — Avoid duplicating material
2. **Outline first** — Write bullet points before full sentences
3. **Find a real example** — What will readers see in real life?
4. **Create visuals** — Use Markdown tables or blockquotes
5. **Write code** — Test it locally before adding
6. **Run the checklist** — Verify all items in Consistency Checklist
7. **Get feedback** — Have someone else review before merging

---

## Maintenance

**Regular tasks:**
- Check links monthly (broken links?)
- Update episodes/lessons if course content changes
- Review explanations based on feedback
- Add new episodes as course progresses

**When merging contributions:**
- Check formatting against this guide
- Verify code examples work
- Ensure links are correct
- Confirm tone is consistent with course philosophy
- Test diagrams/images load

---

## Questions?

If you're unsure about:
- **Tone:** Read Episode 01, match that style (slow, intentional, encouraging)
- **Formatting:** Copy the template structure above
- **Content depth:** Explain to a beginner, not an expert
- **Examples:** Use real-world before code
- **Learning pace:** Emphasize pausing, reflecting, rewatching

**Remember:** Clarity and simplicity always win. This course values understanding over speed.

