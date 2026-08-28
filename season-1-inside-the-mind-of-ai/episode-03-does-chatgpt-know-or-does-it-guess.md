# Episode 03: Does ChatGPT Know or Does It Guess?

> **Season 1 — Inside the Mind of AI**

> ChatGPT does not retrieve a verified answer from a hidden database by default; it generates a response from learned patterns, context, and any tools the assistant uses.

---

## At a Glance

| 🔎 Search engines | 🤖 LLM-based assistants |
|:---|:---|
| Retrieve existing pages from an index | Generate a new response from learned patterns |
| Rank results and show source links | Predict the next token repeatedly |
| Can be checked against the original page | May sound confident while being wrong |

---

<details id="quick-notes" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">📝 Quick Notes — visual revision</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem; min-height: 1rem;">&nbsp;</div>
</details>

---

<details id="search-vs-llm" open style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🔎 Search Engines vs. LLMs</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### How a search engine works

When you search for **"Who is Dr. A. P. J. Abdul Kalam?"**, a search engine generally:

1. Looks through an **index** of discovered web pages.
2. Finds pages that appear relevant to the query.
3. Ranks those pages using many signals.
4. Returns links that you can open and inspect.

📚 The index is like the index of a textbook. It is a fast map to information, not the whole Internet being read from scratch for every question. Crawlers regularly discover and update pages, but coverage and freshness are not guaranteed.

### Crawlers, index, and ranking

🕷️ **Crawlers**, also called spiders, continuously visit accessible web pages. Their job is to help the search engine discover new or changed content and update its index. When a person searches, the engine retrieves matching documents from that index and then ranks them before showing results.

📊 Ranking uses many signals. Examples discussed in this episode include a site's authority, page speed, relevant keywords, backlinks, metadata, and how recent the page is. The exact ranking formula is not public and can change over time.

```mermaid
flowchart LR
	CR["Crawlers / spiders"] --> W["Web pages"]
	W --> I["Search index"]
	Q["Your question"] --> S["Search engine"]
	S --> I
	I --> R["Retrieve relevant documents"]
	R --> K["Rank documents"]
	K --> L["Links you can verify"]

	P["Your prompt"] --> M["Language model"]
	M --> LP["Learned patterns + prompt context"]
	LP --> G["Generated response"]
	G --> V["You verify the claims"]
```

### How an LLM works at a high level

🧠 An LLM does not normally perform a live search when it receives a prompt. It uses patterns learned during training and the text already present in the conversation to generate a response. The response can be new wording that never appeared as one exact page online.

> **Retrieve** means finding existing information. **Generate** means producing new text. Generation can be useful, but it does not automatically make the result factual.

</div>
</details>

---

<details id="hallucinations" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">⚠️ Why ChatGPT Can Sound Right and Be Wrong</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

💭 Imagine asking a deliberately mixed, nonsensical question such as **"Why is Namaste AI red wine from the Himalayan region of India so expensive?"** A model may still give plausible reasons for its price even though the question combines details that do not describe a real thing.

🗣️ This behavior is often called a **hallucination**: the model generates a confident-looking claim without a reliable factual basis. It is not necessarily trying to deceive anyone. Its objective is to produce a likely continuation, not to guarantee that every statement is true.

🔍 Search engines also do not guarantee truth. A search result can be outdated, misleading, or poorly ranked. Their advantage is traceability: you can inspect the page, author, date, domain, and other sources.

### A useful verification habit

| Before trusting an answer | What to do |
|:---|:---|
| It contains a fact | Ask for a source and open it yourself |
| It contains a date or current status | Check a current, authoritative source |
| It names a person, product, or study | Confirm that the thing actually exists |
| It includes calculations or code | Recalculate or run the result |
| The answer is unusually confident | Treat confidence as style, not evidence |

</div>
</details>

---

<details id="next-token" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🧩 The Next-Token Prediction Idea</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

🔤 **"Predict the next word"** is a useful first mental model. More precisely, language models predict the next **token**. A token can be a whole word, part of a word, punctuation, or another small piece of text.

For example:

```text
The sun rises in the ...
```

🎯 The model assigns scores or probabilities to possible continuations. **"East"** is likely in this context because the model learned strong patterns connecting these pieces of language. It then adds a continuation and predicts the next one using the growing context.

```mermaid
flowchart LR
	A["The sun rises in the"] --> B["Predict likely next token"]
	B --> C["east"]
	C --> D["Use the longer context"]
	D --> E["Predict the next token again"]
	E --> F["Continue until the response ends"]
```

### Why repeated prompts can differ

🔀 For a prompt such as **"Roses are"**, several continuations can fit: *red*, *beautiful*, or *flowers*. The model can select among likely options, so two runs may produce different wording. This is not the same as choosing words with no learned structure; the choices are shaped by the prompt, the model's learned parameters, and generation settings.

✍️ Calling an LLM "autocomplete" is useful for a first explanation, but incomplete. The model has learned complex patterns involving grammar, programming, languages, facts, and associations. It is a very powerful autocomplete, not a simple text-replacement tool.

</div>
</details>

---

<details id="training" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🧠 Where the Learned Patterns Come From</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

⚙️ During training, a neural network processes very large collections of data. Its internal numerical parameters, also called **weights**, are adjusted repeatedly so that its predictions improve. After training, those parameters store learned statistical patterns; they are not a searchable copy of every page used for training.

```mermaid
flowchart LR
	D["Training data"] --> T["Training process"]
	T --> W["Adjusted weights / parameters"]
	W --> P["Predictive model"]
	P --> X["Generate text from a prompt"]
```

🧷 This is why it is misleading to say that the model simply remembers a web page and fetches it later. It has learned relationships from its training data, but that learning can include errors, gaps, outdated information, and conflicting examples.

</div>
</details>

---

<details id="knowledge-cutoff" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">📅 Knowledge Cutoffs and Fresh Information</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

⏳ Training is expensive and is not continuously repeated for every new web page. A model therefore has a **knowledge cutoff**: a point after which its original training does not include new information.

- It may not know about a recent election, product release, or local event.
- It can state an old answer with confidence if the prompt does not provide newer context.
- An assistant may use web search or another connected source to answer current questions, but that is a separate tool-assisted path.

> A model's stated cutoff date is a property of a particular model or deployment. Do not assume that every model has the same date.

</div>
</details>

---

<details id="assistant" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🤖 Base Model vs. AI Assistant</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

The key distinction is:

| Base model | AI assistant such as ChatGPT |
|:---|:---|
| Primarily trained to predict the next token | Uses a base model inside a larger product |
| Raw continuation behavior | Instruction tuning helps it follow requests |
| Does not automatically know current information | May have web search or retrieval tools |
| Does not automatically calculate reliably | May call a calculator or code tool |
| Fewer product-level controls | Can include safety controls, system instructions, authentication, and conversation management |

🧰 The exact capabilities depend on the product, model, account, and enabled tools. When ChatGPT searches the web or calculates with a tool, the final answer is not coming only from next-token prediction. The assistant is coordinating the model with external capabilities.

### An AI assistant built around a base model

🏗️ Think of the base model as the central engine: its primary job is predicting text. An AI assistant is the larger product built around that engine. It adds capabilities that help it follow instructions, work with information outside its training, and operate within safety and security controls.

```mermaid
flowchart TB
	subgraph ASSISTANT["AI Assistant: ChatGPT, Gemini, Grok, Claude"]
		direction TB
		TOP(["Instruction tuning<br/>Human feedback<br/>System instructions"])
		subgraph MIDDLE[" "]
			direction LR
			LEFT(["Web search and retrieval<br/>Files and memory<br/>Tool access"])
			BASE(("&nbsp;&nbsp;&nbsp;Base Model&nbsp;&nbsp;&nbsp;<br/><br/>&nbsp;&nbsp;predicts text&nbsp;&nbsp;"))
			RIGHT(["Safety training<br/>Content filters<br/>Guardrails"])
		end
		BOTTOM(["Conversation management<br/>Security and authentication"])
		TOP --> BASE
		LEFT --> BASE
		BASE --> RIGHT
		BASE --> BOTTOM
	end
```

</div>
</details>

---

<details id="mental-model" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">💡 My Mental Model</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

📖 I think of a search engine as a librarian who points me to documents, while an LLM is a very capable writer who has learned patterns from a huge reading experience. The writer can explain, combine, translate, and draft ideas quickly, but a convincing explanation is not the same thing as a checked source.

❓ ChatGPT is the writer plus a set of product features and tools. So the first question I should ask is: **Is this response generated from the model's learned patterns, or did the assistant retrieve and verify information with a tool?**

</div>
</details>

---

## Key Takeaways

- [**Search engines retrieve and rank existing documents**](#search-vs-llm) — LLMs generate text.
- [**Next-token prediction is a useful simple explanation**](#next-token) — it represents complex learned patterns rather than random guessing.
- [**Fluent language and confidence do not guarantee truth**](#hallucinations) — verify important claims.
- [**Knowledge cutoffs make built-in knowledge different from live information**](#knowledge-cutoff) — current facts may require a connected source.
- [**ChatGPT is an AI assistant built around a base model**](#assistant) — tools, safety controls, and product features can extend its capabilities.
- [**Check important claims yourself**](#hallucinations) — especially current facts, identities, sources, calculations, and unusual entities.

## Questions / Things to Explore

- How are tokens different from words?
- How does a model represent meaning numerically?
- How does instruction tuning turn a base model into a helpful assistant?
- How do web search and retrieval reduce, but not eliminate, hallucinations?

---

| ← Previous | Next → |
|:---:|:---:|
| [Episode 02: The Evolution of AI](./episode-02-the-evolution-of-ai.md) | — |