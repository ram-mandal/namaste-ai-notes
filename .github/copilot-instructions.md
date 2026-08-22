# Copilot Instructions for Namaste AI Notes

**Purpose:** Maintain consistent, high-quality notes across all lessons and resources.  
**Audience:** Contributors, maintainers, and AI assistants editing this repo.

---

## 🎯 Core Principles

1. **Simple language** — Explain like you're talking to a 5th grader
2. **One concept per section** — Don't mix topics
3. **Visual-first** — Use diagrams before long text
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

**DON'T:**
- Use jargon without explaining it
- Write sentences longer than 2 lines
- Assume prior knowledge
- Use passive voice
- Be overly formal or robotic

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

Every lesson `.md` file should follow this structure:

```markdown
# [Lesson Number]: [Clear, Simple Title]

## 🎯 What you'll learn
- Key point 1 (one line)
- Key point 2 (one line)
- Key point 3 (one line)

## 📖 The simple explanation
[1-2 paragraph explanation of core concept]

## 🔍 How it works (step by step)
1. Step 1 - what happens
2. Step 2 - what happens
3. Step 3 - what happens

## 📊 Visual explanation
[ASCII diagram, flowchart, or reference to asset file]

## 💻 Code example
[Runnable code snippet with comments]

## 🌍 Real-world example
[Concrete example from life, not tech]

## ⚠️ Common mistakes
- Mistake 1 and why it's wrong
- Mistake 2 and why it's wrong

## 🔑 Key takeaways
- Takeaway 1
- Takeaway 2
- Takeaway 3

## 🔗 Resources & further reading
- [Resource name](link)
- [Resource name](link)

## 🚀 Next lesson
[Link to next lesson]
```

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

## 📏 File Naming Convention

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

## 🚀 Before Creating a New Lesson

1. **Check existing lessons** — Avoid duplicating content
2. **Outline first** — Write bullet points before full sentences
3. **Find a real example** — What will readers see in real life?
4. **Create visuals** — ASCII art or diagram (in `assets/`)
5. **Write code** — Test it locally before adding
6. **Run the checklist** — Verify all items above
7. **Get feedback** — Have someone else review before merging

---

## 🛠️ Maintenance

**Regular tasks:**
- Check links monthly (broken links?)
- Update lessons if course content changes
- Review & update explanations based on feedback
- Add new lessons as course progresses

**When merging contributions:**
- Check formatting against this guide
- Verify code examples work
- Ensure links are correct
- Confirm tone is consistent
- Test diagrams/images load

---

## ❓ Questions?

If you're unsure about:
- **Tone:** Read Lesson 1, match that style
- **Formatting:** Copy the template structure
- **Content depth:** Explain it to a beginner, not an expert
- **Examples:** Use real-world before code

**Remember:** Clarity wins over complexity. Every. Single. Time.

