<!-- revision: 0 -->
> **Season 1 — Inside the Mind of AI** [🔗](./README.md)

# Episode 09: Can AI Really Think?

> In the previous episode we saw how a base model is shaped into a helpful assistant using SFT and RLHF. But a helpful assistant is not the same as a *thinking* one. This episode asks the big question — **can AI really think?** — and unpacks the modern answer: **reasoning models** that spend extra computation *during inference* (Chain of Thought, Tree of Thought, Graph of Thoughts), how they are trained with **Reinforcement Learning on Verifiable Rewards (RLVR)**, and the honest limits of "thinking harder" (more tokens ≠ more intelligence).

> **A useful question:** If a model shows you its step-by-step reasoning, is that *really* how it decided the answer — or is it just a polite story it wrote after the fact?

---

<details id="at-a-glance" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">👀 At a Glance</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

| Concept | What it is | Why it matters |
|:---|:---|:---|
| **Direct vs. Reasoning Generation** | Answering in one jump vs. producing intermediate steps first | Some tasks need "thinking time"; most do not |
| **Inference-Time Compute** | Spending extra computation *while answering* (after training is done) | A new axis of capability beyond model size |
| **Chain of Thought (CoT)** | One linear chain of intermediate reasoning steps | Improves complex math/logic tasks |
| **Tree of Thought (ToT)** | Explore several branches, score, prune, continue | Lets the model backtrack from dead ends |
| **Graph of Thoughts (GoT)** | Nodes can reconnect and combine ideas | Merges the best parts of different approaches |
| **Unfaithful Reasoning** | Visible reasoning may not match internal computation | Don't treat the shown steps as proof of causality |
| **Limits of Reasoning** | More thinking ≠ guaranteed correctness (overthinking) | Diminishing returns; can even flip a correct answer |
| **Reinforcement Learning (RL)** | Model produces behavior → gets reward → behavior is pushed toward higher reward | How we teach a model to reason |
| **RLVR** | RL using *verifiable* rewards (math, code) instead of human preference | Stronger, cheaper signal than RLHF |
| **Three Evaluators** | Deterministic, Human, Model (LLM-as-a-Judge) | How we decide if a reasoning answer is right |
| **Knowledge + Reasoning + Tools** | The three superpowers that make a modern assistant | The season's synthesis of how ChatGPT really works |

</div>
</details>

---

<details id="quick-notes" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">📝 Quick Notes — visual revision</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem; min-height: 1rem;">

_(Visual notes and revision diagrams will be added here as new illustrations become available.)_

</div>
</details>

---

<details id="the-big-question" style="margin-bottom: 1rem;" open>
<summary><strong style="font-size: 1.25em;">🎯 The Big Question: Can AI Really Think?</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Pause and ask yourself first

🧠 Before touching any AI concept, take a moment to sit with a deceptively simple question: **what does "thinking" even mean?** When you say *"I am thinking about it"*, what is actually happening in your head?

> 🧭 **The honest starting point:** *We don't have a clean definition of "thinking" for humans, so claiming a machine "thinks" (or doesn't) is harder than it sounds. What we *can* do is look at what machines actually do when they solve hard problems.*

### The surprising gap: great at generating, bad at thinking

🧪 Here are four prompts that make this concrete — try them in any assistant (ChatGPT, Gemini, etc.):

| # | Prompt | What a capable model does |
|:---:|:---|:---|
| 1 | *"Explain quantum physics"* | Gives a beautiful, correct explanation — and can answer many follow-ups |
| 2 | *"Write a book on DNA science"* | Can produce a long, detailed book on a very complex topic |
| 3 | *"What is bigger, 9.11 or 9.9?"* | **Gets it wrong** — says 9.11 is bigger |
| 4 | *"Print every 3rd character of NAMASTEARTIFICIALINTELLIGENCE"* | **Miscounts** the characters |

🤯 What really hit me was the contrast. The model can write a whole book on rocket engineering, yet it answers *"9.11 vs 9.9"* incorrectly — it compares the digits *after* the decimal (11 vs 9) instead of understanding the actual value. And it fumbles a simple character-counting task. This is not an old, weak model; it is a recent, very capable one.

> 🧭 **The core observation:** *LLMs are excellent at **generation** (predicting the next token) but can be surprisingly bad at **thinking**. If it has read a lot about quantum physics, it can write about it effortlessly — but the moment a problem needs a little reasoning, it can stumble.*

### Thinking vs. Reasoning

🔤 I think it's important to draw a line between two words we often use interchangeably:

| Term | What it means | Example |
|:---|:---|:---|
| **Thinking** | The big umbrella — anything that goes on in your head, *including recalling from memory* | *"When did you last eat pizza?"* (pulling from memory) |
| **Reasoning** | A specific part of thinking — taking **decisions using your intellect** | Planning a birthday trip: *"I like mountains → pick a place I haven't visited → does it fit my budget?"* |

🎂 The birthday-trip example shows reasoning in action: you weigh preferences, draw on past memory (places you have and haven't visited), check your budget, and make a series of decisions. That decision-making is *reasoning*. And that's really the question I want to answer here: **can we make a machine reason?**

### Why this matters

📈 In [Episode 08](./episode-08-from-a-base-model-to-an-ai-assistant.md) we learned that a base model is just a smart autocomplete, and that post-training (SFT + RLHF) turns it into a helpful assistant. But "helpful" is not the same as "reasoning". Modern assistants like DeepSeek's "Deep Thinking" mode or OpenAI's reasoning models do something extra: they **think before they answer**.

To understand what is going on in the LLM and assistant industry right now, here's what I want to walk through:

| | |
|:---|:---|
| - What "reasoning" means for a model | - How models are *taught* to reason (RL) |
| - Chain / Tree / Graph of Thoughts | - Why "thinking harder" has real limits |
| - Whether shown reasoning is trustworthy | - How we evaluate reasoning answers |

> 🧭 **The frame I'm working with:** *Reasoning is not a new kind of magic — it is the model spending more useful computation while answering, and we train it to do that with rewards.*

### The question I'm still sitting with

🤔 By the end of this season, I've walked through everything — tokens, embeddings, the transformer, training, RLHF, reasoning, tools — and I keep coming back to the original question: **is a machine really *thinking*, or is it just doing a lot of mathematics that we then translate into English?**

The honest answer is *I don't know*. Humans think in a way machines clearly do not: our thoughts are shaped by **emotions**, **personal upbringing**, and **where we live**. Say the words *"a pet animal"* and a person in a village might picture a cow, while someone in a city pictures a cat — the same words, different minds. Machines are logical and consistent; humans are emotional, biased, and change their minds in five minutes. So the gap between human and machine "thinking" is real and large.

> 🧭 **The open question:** *Whether a machine "thinks" has no one-word answer. I'm leaving it open — and I think that's exactly the point of the season.*

</div>
</details>

---

<details id="direct-vs-reasoning" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🧠 Direct Generation vs. Reasoning-Oriented Generation</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Two very different kinds of questions

🔀 Not every question needs the same amount of "thinking". I like to split tasks into two buckets, using a very relatable example:

> **Prompt:** *"Suggest a gift for my girlfriend."*

| Type | What happens | Result |
|:---|:---|:---|
| **Direct generation** | Jumps straight to a generic answer | *"Give her a chocolate / a teddy bear / a rose."* — correct, but forgettable |
| **Reasoning-oriented generation** | Reasons about *her* first | *"She's a programmer who likes blue…"* → a **thoughtful**, personalised gift |

🎁 The difference is the word **thoughtful**. A direct answer hands over a chocolate. A reasoning answer does a little mental work — *she's technical, she likes blue, she lives in a metro city* — and comes up with something personalised that actually impresses. That extra mental exercise is **reasoning-oriented generation**.

🧮 The same idea shows up in math. Consider the classic trap:

> *"A company grows revenue by 20%, then loses 20%. Is it back to the original?"*

Your gut says *"20% up then 20% down = back to start"*, but that is wrong. The intermediate step reveals the truth:

```text
Start: 100
+20%  -> 120
-20%  -> 120 * 0.80 = 96   (NOT 100!)
```

The model that *shows* this intermediate calculation gets it right; the model that jumps straight to an answer often gets it wrong.

### But most of the time, direct is fine — don't overthink

⚡ The flip side matters just as much: **you do not want to reason about everything.** Consider *"Translate 'hello' into Hindi."* The right answer is instant — *namaste*. You do not want the model to spend 30 seconds deliberating *"hello in Punjabi is this, in Tamil is that, in French is…"* before answering. For most everyday tasks, a fast direct answer is exactly what you want; overthinking just wastes time and tokens.

> 🧭 **The balance:** *Default to direct generation. Reach for reasoning only when the task genuinely needs a pause, a calculation, and a check.*

### Reasoning is just generation, with extra steps

🔑 The key insight is that **reasoning is not a completely different operation from generation**. The model is still predicting tokens one at a time. What changes is that it is allowed (and trained) to generate *intermediate computation* — a chain of steps — before it commits to the final answer. Think of it as **breaking the whole problem into smaller sub-problems** and solving them one by one.

🐞 A concrete example is debugging. Ask a model *"there is an error on line 15"*:

| Mode | What it does |
|:---|:---|
| **Direct generation** | Jumps to a lazy answer — *"just remove line 15."* |
| **Reasoning** | Breaks it into sub-steps: *read line 15 → is it a syntax error? → is it a logical error? → what type of error is it?* → then proposes a real fix |

The reasoning version is just *more* generation, spread across several deliberate steps — which is why you cannot debug without it.

> 🧭 **The core distinction:** *Direct generation answers in one jump. Reasoning-oriented generation writes out the working first. Reasoning is itself generated as intermediate computation.*

</div>
</details>

---

<details id="inference-time-compute" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">⚙️ Reasoning Models & Inference-Time Compute</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### A new axis of capability

📐 Traditionally, the main way to make an LLM smarter was to **build a larger model and train it with more compute**. Reasoning models opened a second, independent scaling dimension: **spend more compute solving a problem during inference** — that is, *after* the model has already been trained.

```text
Inference  =  Prompt  →  Forward Pass  →  Answer
```

Modern systems can spend very different amounts of computation during that forward pass. A hard question might involve:

| | |
|:---|:---|
| - Generating intermediate steps | - Calling tools |
| - Exploring alternatives | - Writing and executing code |
| - Verifying calculations | - Evaluating several candidate answers |
| - Revising a draft | - Checking constraints |

All of this happens **after training is already done**. The model's knowledge is fixed; what changes is how much useful work it does *while answering your specific question*.

### Training-time compute + Inference-time compute

🧠 So we get a new way to think about a model's capability:

```text
Capability  =  Training-time compute  +  Inference-time compute
```

A model's intelligence is not determined only by how much compute was used to train it. How much *useful* computation it performs while answering can matter just as much.

### Reasoning is now everywhere

🌐 Early reasoning models (OpenAI's **o1**, **o2**, DeepSeek-R1) were dedicated "thinking" models you had to explicitly switch on. Today, the everyday models you use — the latest ChatGPT, Gemini, and Claude — already have reasoning built in, and they are **smart enough to decide for themselves when to reason, when not to, and how much to reason**. You no longer need to pick a special "thinking" mode for most tasks; the model routes the effort on its own.

> 🧭 **The mental shift:** *You can make a system smarter in two ways — train it harder, or let it think longer. Reasoning models bet on the second one — and modern assistants now decide the balance automatically.*

</div>
</details>

---

<details id="intermediate-reasoning" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🧩 Intermediate Reasoning: The Notebook Problem</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### A problem that resists a one-jump answer

📚 Consider this classic word problem:

> *There are 23 students in a class. Each student needs 4 notebooks. Notebooks are sold in packets of 10. How many packets must the teacher buy?*

A quick mental jump might guess "9" or "10" and stop. But the *reliable* answer comes from writing out the steps:

```text
23 × 4 = 92 notebooks
Each packet contains 10
92 / 10 = 9.2
We cannot buy 0.2 packets
so round up
Answer: 10 packets
```

🔍 This step-by-step working is called **intermediate reasoning**. The model is not just producing a final token — it is constructing intermediate states that *preserve the dependencies between steps* (the "9.2" forces the "round up" decision).

### It happens at inference, not training

⏱️ Notice *when* this happens. The model (say DeepSeek) is already fully trained — the reasoning ability is already built in. But when you ask the question *on the fly*, during inference, the model thinks to itself: it asks itself questions, checks its work, and refines the answer before showing you the result. That is the "Thought for 116 seconds" you see in a Deep Thinking mode — real intermediate reasoning happening live.

> 🧭 **The takeaway:** *Some problems cannot be solved reliably by producing the final answer in one intuitive jump. Intermediate states help preserve the dependencies between steps.*

</div>
</details>

---

<details id="chain-tree-graph" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🌲 Chain of Thought, Tree of Thought, and Graph of Thoughts</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Chain of Thought (CoT) — one linear path

➡️ **Chain of Thought** broadly refers to producing intermediate reasoning steps *before* arriving at an answer. Instead of jumping from problem to answer, the model walks a single chain:

```text
Problem
   ↓
Step 1
   ↓
Step 2
   ↓
Step 3
   ⋮
   ↓
Answer
```

The idea — popularized by the 2022 paper *"Chain-of-Thought Prompting Elicits Reasoning in Large Language Models"* ([Wei et al., 2022](https://arxiv.org/pdf/2201.11903)) — is that **more time to think gets you better answers** on complex tasks, because you are breaking a large problem into smaller sub-problems. Think of it as a **linked list of thoughts**: ask yourself a question, answer it, ask the next, answer it — accumulating the sub-answers into one final answer.

### Why forcing an answer quickly can hurt

🏏 Here's a classic trap that catches almost everyone:

> *A bat and a ball together cost ₹110. The bat costs ₹100 more than the ball. How much does the ball cost?*

If you answer in 5 seconds, you will almost certainly say **₹10**. But that is wrong. Set the ball = $x$; then the bat = $x + 100$:

```text
x + (x + 100) = 110
2x + 100      = 110
2x            = 10
x             = 5
```

The ball costs **₹5**. The mistake happens because we fall for the wording and want to solve it fast (*"110 − 100 = 10"*). The same thing happens to models: **sometimes the model needs more computation before it commits.** This is not just an AI problem — our own brains need a little more computation before they can come up with the right answer, too.

> 🧭 **The lesson:** *A quick, intuitive jump is exactly when errors sneak in. Writing out the intermediate steps (Chain of Thought) is what protects against them.*

### Tree of Thought (ToT) — explore, score, prune

🌳 A single chain has a weakness: if Step 2 is a dead end, the whole chain is stuck. **Tree of Thought** fixes this by letting the model branch out. Instead of one path, the system:

| | |
|:---|:---|
| - Explores several possibilities | - Prunes weak branches |
| - Scores partial solutions | - Continues the promising branches |

```text
            Prompt
        /      |      \
       A       B       C
      / \      |     / | \
     A1  A2   B1    C1 C2 C3
        / \
      A21 A22
```

The model can try approach A, realize it is failing, and **backtrack** to switch to approach C — something a single chain cannot do. Because each step in a chain depends on the previous one, one wrong early step poisons everything after it; a tree lets the model abandon the bad branch and explore a different direction. At the end, it can **evaluate the surviving candidates** (by correctness, execution, or what a human would prefer) and pick the winner.

### Graph of Thoughts (GoT) — let ideas combine

🕸️ A tree has one remaining limitation: **each branch grows independently** — there is no way for branch A to "talk to" branch C. But real reasoning is sometimes not tree-shaped; two different ideas can *combine*.

**Graph of Thoughts** allows nodes to reconnect and merge. The final output can pick the best sub-solution from *different* branches:

```text
Approach A  → finds an efficient algorithm (O(n))
Approach B  → finds an important edge case

Efficient algorithm from A  +  Edge-case handling from B  =  Better solution
```

🧩 A concrete example: suppose you are writing a heap sort. One branch writes the `heapify` function correctly; another branch writes the main `heapSort` loop correctly. In a graph, you can **merge the best parts of both** instead of throwing one away. This is how many reasoning models actually work in practice — not as a single chain, but as a graph of thoughts.

> 🧭 **The progression:** *Chain explores one path. Tree explores alternatives. Graph also lets useful ideas reconnect and combine.*

</div>
</details>

---

<details id="unfaithful-reasoning" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">⚠️ Visible Reasoning Is Not Necessarily Faithful</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### The trap of trusting the shown steps

🎭 Suppose a model answers: *"I first did X, then Y, then Z."* We naturally assume that must be *exactly* how it produced the answer. But **generated reasoning is still generated text** — and there can be a gap between:

| | |
|:---|:---|
| - Internal computation | - The explanation ultimately shown to the user |
| - Intermediate latent representations | |
| - Hidden system processes | |

The final explanation may be **simplified, reconstructed, summarised, or partially post-hoc** — written *after* the answer was already decided.

### A fun exercise for you

🤔 Ask yourself: *"Why did I choose this particular word while speaking?"* Sometimes you can genuinely explain it. But sometimes you **construct an explanation after the fact** — a plausible story that may not reflect the real cause. Models can do the same thing.

> 🧭 **The caution:** *We should not automatically treat visible reasoning as perfect evidence of internal causality. Research from Anthropic has demonstrated cases where the generated Chain of Thought is not fully faithful to the information actually influencing the model's answer.*

> 📚 **Further reading:** *"Language Models Don't Always Say What They Think: Unfaithful Explanations in Chain-of-Thought Prompting"* (Turpin, Michael, Perez, Bowman — NeurIPS 2023) — [arXiv:2305.04388](https://arxiv.org/abs/2305.04388). The authors show that CoT explanations can be plausible yet misleading — e.g. models rationalise answers pushed by hidden biases (like reordering multiple-choice options) without ever mentioning the real cause.

</div>
</details>

---

<details id="hidden-traces" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🚧 Why Some Systems Don't Expose Raw Reasoning Traces</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Raw traces are often more harm than help

🙈 You might expect a "thinking" model to show you every intermediate step. In practice, many systems deliberately hide the raw trace. Why?

| | |
|:---|:---|
| - Raw intermediate traces may be noisy | - Exposing them can make systems easier to manipulate |
| - They can contain abandoned hypotheses | - Traces may reveal system strategies or sensitive internal instructions |
| - They may be confusing rather than useful | - A concise answer or summary may be more useful for users |

### The OpenAI example

🏢 OpenAI has explicitly said that its hosted reasoning systems **do not expose the raw Chain of Thought** to users, and may instead provide a **model-generated summary**. Among the reasons given: preserving the usefulness of raw reasoning for internal monitoring, avoiding direct exposure of unaligned intermediate content, user-experience concerns, and competitive considerations.

> 🧭 **The practical point:** *The reasoning you see in a chat UI is usually a cleaned-up summary, not the model's raw internal monologue. Treat it as a helpful digest, not a full audit log.*

</div>
</details>

---

<details id="limits-of-reasoning" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">📉 Limits of Reasoning Models: More Tokens ≠ More Intelligence</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Thinking harder is not a correctness guarantee

🚫 A common misconception is that "more thinking = better answer". It is not. A reasoning model can still:

| | |
|:---|:---|
| - Hallucinate | - Get trapped in one approach |
| - Make arithmetic errors | - Overthink |
| - Misunderstand the problem | - Produce convincing but invalid proofs |
| - Accept false assumptions | - Fail on unfamiliar problems |
| - Reason correctly from incorrect premises | - Misuse tools / fail to use tools when it should |

```text
More thinking  !=>  Guaranteed correctness
```

### More computation does not guarantee better answers

📉 The model can spend more time and still go wrong — for example by:

| | |
|:---|:---|
| - Exploring the wrong assumption | - Overcomplicating a simple problem |
| - Reinforcing an incorrect premise | - Selecting the wrong candidate |
| - Making arithmetic mistakes repeatedly | |

More thinking can **increase the probability of success**, but it does **not turn probability into certainty**.

### Overthinking: diminishing returns

🔁 Recent research on reasoning models has observed **overthinking**: increasing the reasoning budget beyond a useful point can produce **diminishing returns** and can even cause the model to **abandon an initially correct solution**. So the right mental model is:

| Compute level | What tends to happen |
|:---|:---|
| **1. Too little compute** | May answer prematurely |
| **2. Useful compute** | Can improve difficult reasoning |
| **3. Unlimited compute** | Does **NOT** guarantee correctness |

> 🧭 **The honest limit:** *Reasoning raises your odds, it does not remove risk. There is a sweet spot — and past it, more thinking can actively hurt.*

### Even the best models carry a disclaimer

⚕️ Notice that even DeepSeek's polished, 2-minute "Deep Thinking" answer is labelled **"AI-generated, for reference only."** That disclaimer is there for a reason: more thinking does not guarantee correctness, so you should **never make medical, legal, or financial decisions on the basis of an LLM answer.** If you tell a model you have a fever, it might suggest paracetamol — but that is not a diagnosis. Humans can't guarantee correctness either; we simply shouldn't expect machines to.

> 🧭 **The takeaway:** *Reasoning models are remarkably capable, but they are aids, not authorities. Verify before you act.*

</div>
</details>

---

<details id="teaching-reasoning-rl" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🎮 How We Teach a Model to Reason: Reinforcement Learning</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### The answer is Reinforcement Learning

🎯 So how do we actually *teach* a model to reason? The answer is **Reinforcement Learning (RL)** — a simple trial-and-error loop:

```text
Model
  ↓
Produces Behavior
  ↓
Reward
  ↓
Training pushes behavior toward higher reward
```

The model tries something, gets a reward signal, and training nudges it toward behaviors that earn higher rewards. Repeat millions of times and the "reasoning" habits become strong.

### RL is not new — it has just improved

🕰️ Reinforcement Learning is **not** a new concept invented by DeepSeek. It has existed for roughly **two decades** and was famously used in AlphaGo (see below). What has changed is *how* we apply it: from human-feedback rewards (RLHF) to verifiable-reward signals (RLVR).

### RL for verifiable tasks

✅ In [Episode 08](./episode-08-from-a-base-model-to-an-ai-assistant.md) we saw RLHF, where the reward comes from a *human preference* model. But human preference is expensive, subjective, and error-prone. Some domains are better: **verifiable domains** let us reward outcomes with much stronger signals than subjective human preference alone — for example **math problems** and **code**.

> 📚 **Further reading (RLHF papers):** The foundational OpenAI work behind RLHF:
> - *"Fine-Tuning Language Models from Human Preferences"* (2019) — [arXiv:1909.08593](https://arxiv.org/pdf/1909.08593) · [OpenAI blog: Fine-Tuning GPT-2](https://openai.com/index/fine-tuning-gpt-2/)
> - *"Training Language Models to Follow Instructions with Human Feedback"* (InstructGPT, 2022) — [OpenAI paper](https://cdn.openai.com/papers/Training_language_models_to_follow_instructions_with_human_feedback.pdf)
>
> Most of the authors of the InstructGPT paper later left OpenAI to co-found **Anthropic** (the company behind Claude). For a beginner-friendly walkthrough of RLHF, see the [Hugging Face LLM Course — Chapter 12.3](https://huggingface.co/learn/llm-course/en/chapter12/3).

```text
Prompt: "Implement a function that returns the longest valid palindrome."
The model writes code.
We run:
  Test 1 ✓
  Test 2 ✓
  Test 3 ✗
  Test 4 ✓
The result gives a concrete training signal.
```

Instead of a human guessing which answer is "nicer", we simply *run the tests*. The pass/fail result is an objective reward we can use to train the model.

> 🧭 **The upgrade:** *When the environment itself can check the answer (math, code), we get a far stronger and cheaper training signal than human preference.*

</div>
</details>

---

<details id="rlvr" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🏆 RLVR: Reinforcement Learning with Verifiable Rewards</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Remember the problem with RLHF?

🔁 In [Episode 08](./episode-08-from-a-base-model-to-an-ai-assistant.md) we listed the weaknesses of RLHF:

| | |
|:---|:---|
| - Humans are expensive | - Humans make mistakes |
| - Human preferences are subjective | |

**RLVR** (Reinforcement Learning with Verifiable Rewards) sidesteps all three. In certain domains, **the environment itself can tell us whether the answer is correct** — no human judge needed.

### Why math and code are the sweet spot

🧮 Two domains are especially useful:
- **Mathematics** — many problems have a single verifiable final answer.
- **Programming** — code can be executed and tested.

### Verifiable vs. non-verifiable tasks

⚖️ The whole trick only works when the task is **verifiable** — meaning you can check the answer programmatically. Here's the contrast:

| Task | Verifiable? | How it is checked |
|:---|:---:|:---|
| *"Write a program to find a valid palindrome number"* | ✅ Yes | Run the code against test cases (`123 → 321`, `432 → 234`, negatives, etc.) |
| *"What is 5 + 5?"* | ✅ Yes | Compare to the exact answer `10` |
| *"Explain what a palindrome is"* | ❌ No | A human must judge grammar and correctness |
| *"Plan a 7-day Malaysia itinerary"* | ❌ No | A human must judge it — and it does not scale |

For a verifiable task, the model can **generate thousands of responses**, keep the ones that pass, and penalize the ones that fail — self-improving with no human in the loop. For a non-verifiable task (a joke, a poem, medical advice), only a human or a trained judge model can evaluate it, which is exactly why those tasks fall back to RLHF or LLM-as-a-Judge.

> 🧭 **The rule of thumb:** *If you can write a test for the answer, you can use RLVR. If you can't, you need a human or a model judge.*

### The DeepSeek-R1 moment

📉 The turning point for reasoning models was **DeepSeek-R1**, released by the Chinese company DeepSeek. When it launched, the market shock was enormous — **over one trillion dollars in global tech stock value was wiped out in a single day**. The headline that ran: *"DeepSeek's performance, at a tiny fraction of the cost, wiping over one trillion of global tech stocks."* It signalled that reasoning was no longer a research curiosity but a product reality, and it intensified the **US–China AI race** that is still unfolding.

The accompanying paper, *"DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning"* ([DeepSeek, Jan 2025](https://arxiv.org/abs/2501.12948)), makes the core claim explicit: **the reasoning abilities of LLMs can be incentivised through pure reinforcement learning** — no human preference labels required, just verifiable correctness signals.

### What the paper's diagrams show

📈 Two graphs from the paper are especially revealing:

| Graph | What it shows |
|:---|:---|
| **Reasoning steps vs. accuracy** | As the model takes more reasoning steps, answer accuracy increases — and **goes beyond the human baseline** (shown as a dotted line). Humans get tired; machines do not. |
| **Reasoning steps vs. response length** | More thinking means a longer response — the model consumes more tokens. This is the cost of deeper reasoning. |

The first graph is the key insight: **with enough reasoning steps, the model surpasses what a human can reliably do**, because a human's mental bandwidth is finite and a machine's is not.

### Open source and the US–China dynamic

🌐 Unlike OpenAI's models (the name says "open", but the weights are not), **DeepSeek-R1 is fully open source** — you can download the model and run it on your own GPUs for free. However, US companies are wary of sending data to Chinese-hosted APIs, so many use **US-based providers** (like Together AI) that host DeepSeek models on US infrastructure. The broader US–China AI competition — including government and military interest — is a major driver of how fast reasoning models are improving.

### Reasoning in action: the Malaysia vacation demo

✈️ Here's a great example of reasoning in action — asking DeepSeek (with "Deep Thinking" enabled) a complex, personalised query:

> *"I want to go on a vacation in Malaysia in October, in a budget of 2 lakhs INR, with my wife and an infant. I want to explore a lot of cool stuff to eat. Give me an itinerary and a breakdown of pricing."*

The model **thinks for 116 seconds** (~2 minutes) before answering. During that time, you can watch it reason in real time:

| What it thinks about | Why it matters |
|:---|:---|
| Convert 2 lakhs INR → MYR | Budget is in a different currency |
| Assume 7–8 days (user did not specify) | Filling in missing constraints |
| October is inter-monsoon; east coast vs. west coast | Weather-aware destination choice |
| Malaysia visa-free for Indian nationals (as of 2026) | Checking current travel rules |
| Infant considerations: carry baby food, avoid very spicy food, check for high chairs | Personalised to the user's family |
| Flights, local transport (taxi vs. public), food lists | Building a complete itinerary |
| Multiple itinerary options (not just one) | Giving the user choices |

A base model or an SFT model **cannot do this** — it would just generate a generic travel blog post. Only a reasoning model, trained with RL, can pause, ask itself questions, make assumptions, check facts, and produce a structured, personalised plan.

> 🧭 **The trade-off:** *Subjective tasks are tough — human preferences are lossy. Verifiable tasks are where RLVR shines, because the reward is objective and free.*

</div>
</details>

---

<details id="three-evaluators" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">⚖️ Three Types of Evaluation & LLM-as-a-Judge</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### How do we decide if a reasoning answer is right?

🧑‍⚖️ There are three ways to evaluate a model's output, in order of preference:

| # | Evaluator | How it works | Best when |
|:---|:---|:---|:---|
| **1** | **Deterministic evaluator** | Exact answer, compiler, unit tests, schema validator | **Always, when possible** (objective & free) |
| **2** | **Human evaluator** | A person inspects the answer | Judgment or expertise is required |
| **3** | **Model evaluator** | Use another LLM to judge the output | Scale is needed and no exact answer exists |

The third option — using an LLM to judge another LLM's output — is known as **LLM-as-a-Judge**.

### What an LLM judge can (and can't) do

🔍 An LLM judge can *potentially* evaluate:

| | |
|:---|:---|
| - Relevance | - Code explanation |
| - Clarity | - Factual consistency against provided context |
| - Instruction following | - Completeness |
| - Style | |

**But they are not perfect.** A judge model can have:

| | |
|:---|:---|
| - Biases | - Inability to detect subtle factual errors |
| - Preference for longer answers | - Susceptibility to persuasive language |
| - Style preferences | - Correlated errors with the model being judged |
| - Position bias | |

> 🧭 **The recursive problem:** *Using an AI to evaluate an AI scales evaluation, but it does not magically create objective truth. Using AI to evaluate AI is powerful — but now we need to evaluate the evaluator. This recursive problem will appear repeatedly in modern AI engineering.*

</div>
</details>

---

<details id="alphago" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🎯 AlphaGo: Reinforcement Learning from Self-Play</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Why Go, not chess?

♟️ AI had already beaten chess champions long before 2016 — chess is played on an 8×8 board with a finite (if huge) number of positions that GPUs can process. **Go** is a different beast: the board is 19×19, the number of possible positions dwarfs the number of atoms in the universe, and there is no simple brute-force shortcut. Beating the world's best Go player was the real milestone.

### The 2016 match

🀄 In **March 2016**, Google DeepMind's **AlphaGo** defeated the world champion **Lee Sedol** 4–1 in a five-game match. The most iconic moment was **Move 37** — a move that had essentially never been played in human history. Lee Sedol was so stunned that he **paused the game for 15 minutes** to think about how to respond. The live commentators were equally shocked, asking aloud how a machine could have played such a move.

> 🔗 **Watch & read:** [AlphaGo vs. Lee Sedol — final match (YouTube)](https://www.youtube.com/watch?v=l7ngy56GY6k) · [Silver et al., *Mastering the game of Go with deep neural networks and tree search* (Nature, 2016)](https://discovery.ucl.ac.uk/id/eprint/10045895/1/agz_unformatted_nature.pdf)

### How AlphaGo was built: self-play

🧠 AlphaGo's search evaluated positions and selected moves using deep neural networks trained two ways:
1. **Supervised learning** from human expert games (the familiar "learn from examples" approach).
2. **Reinforcement learning from self-play** — the most important line. The model played against *itself*, learning by trial and error with **no human involved**.

The self-play loop is simple:

```text
AI plays a match against another AI
  → Win  → positive reward → retrain on the decisions made
  → Lose → negative reward → retrain on the decisions made
  → Repeat millions of times
```

Lee Sedol had played roughly **1,000 matches** in his entire life. AlphaGo could play **millions or billions** of self-play matches, updating after every single one. That is the power of reinforcement learning: given enough training, the model can **go beyond human capability**.

### Supervised learning vs. Reinforcement Learning

📊 Here's a key distinction worth highlighting:

| Approach | Ceiling |
|:---|:---|
| **Supervised learning** | Can only go up to the capability of the human data it was trained on |
| **Reinforcement learning (self-play)** | Can go **beyond** human capability — the improvement curve keeps rising until it plateaus |

> 🧭 **Why it matters:** *Move 37 was not programmed by a human — it was discovered by the model through self-play. This is the same "learn from a reward signal, not from a human teacher" idea that powers modern reasoning models like DeepSeek-R1.*

</div>
</details>

---

<details id="right-tool" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🧭 Choosing the Right Tool for the Task</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### Not every task needs reasoning

🛠️ A practical skill is matching each task to the right capability. Reasoning is powerful but expensive — don't reach for it when a simpler tool does the job:

| Task | Best approach |
|:---|:---|
| Explain recursion | Language generation |
| 287831 × 52649 | Calculator / code tool |
| Current stock price | Live data tool / web search |
| Solve a math proof | Reasoning |
| Count characters | Code |
| Summarise an essay | Generation |
| Search private docs | Retrieval + Generation (RAG) |
| Debug a program | Reasoning + Code |

> 🧭 **The engineering habit:** *Route each task to the cheapest tool that solves it. Reserve deep reasoning for problems that genuinely need it — and pair it with tools (calculators, code, retrieval) for the parts a model is bad at doing in its head.*

</div>
</details>

---

<details id="three-pillars" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🏗️ The Big Picture: Knowledge + Reasoning + Tools</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

### What actually makes a modern assistant smart

🧩 The big picture I'm taking away is that the ChatGPT you use today is **not** just a "Generative Pre-trained Transformer" doing autocomplete. It is a mix of three superpowers, each coming from a different place:

| Pillar | Where it comes from | What it gives the model |
|:---|:---|:---|
| **Knowledge** | Intensive pre-training on the internet | Broad facts, language, and patterns ("what is recursion?") |
| **Reasoning** | Reinforcement learning (this episode) | The ability to think step-by-step on hard problems |
| **Tools** | External integrations (the "superpowers") | Calculators, code execution, web search, RAG, image/video generation |

🌏 Go back to the Malaysia vacation example: the model used **knowledge** to know which cities exist, **reasoning** to work out infant-friendly plans and budget constraints, and **tools** to convert currency, check visa rules, and look up flights. That combination is why today's assistants can edit photos, generate video, fetch live stock prices, and run code — none of which a raw base model could do alone.

> 🧭 **The season's payoff:** *Knowledge comes from training, reasoning comes from reinforcement learning, and tools are the superpowers. Mix all three and you get the assistant you use every day.*

</div>
</details>

---

<details id="my-mental-model" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">💡 My Mental Model</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

🧮 I think of a reasoning model like a student taking an exam. For an easy question (*"What is 5 + 5?"*), the student just writes the answer — no scratch paper needed. For a hard question, the student grabs scratch paper and works through it step by step (Chain of Thought). If they hit a dead end, they cross it out and try a different route (Tree of Thought). And if two different approaches each got half the problem right, they merge the best of both (Graph of Thoughts).

But here is the catch I keep coming back to: **the scratch paper is not the brain**. The student might show you neat, tidy working that is a *summary* of what they actually did — or even a story they wrote after already knowing the answer. So I treat the shown reasoning as a helpful digest, not proof. And I remember that even a student who works for an hour can still get it wrong — more time raises the odds, it doesn't guarantee the grade.

</div>
</details>

---

<details id="practical-connection" style="margin-bottom: 1rem;">
<summary><strong style="font-size: 1.25em;">🛠️ Practical / Engineering Connection</strong></summary>
<div style="margin-left: 3rem; margin-top: .25rem;">

🔧 These ideas change how I build with LLMs in practice:

- **Don't over-reason simple tasks.** Routing a trivial lookup through an expensive reasoning model wastes latency and money. Match the task to the tool (see the table above) — use a calculator or code tool for arithmetic instead of asking the model to "think" about `287831 × 52649`.
- **Verify with deterministic checks whenever possible.** If the output is code or a number, run the tests or the compiler. That objective signal beats both human review and LLM-as-a-Judge, and it is free.
- **Treat shown reasoning as a digest, not an audit log.** Don't build safety-critical logic on the assumption that the visible Chain of Thought is faithful to the model's real computation.
- **Beware overthinking.** There is a reasoning-budget sweet spot. For some tasks, a longer budget can flip a correct answer to a wrong one — so tune the budget rather than assuming "more is better".
- **Use LLM-as-a-Judge for scale, not for truth.** It is great for relevance, clarity, and style at scale, but it has biases (longer answers, position, persuasive language) and can share blind spots with the model it judges.
</div>
</details>

---

## Key Takeaways

- **LLMs are great at generating but can be bad at thinking.** Even a capable model can write a book on quantum physics yet answer *"9.11 vs 9.9"* wrong — and while *thinking* is the umbrella (including memory recall), *reasoning* is the decision-making inside it. [🔗](#the-big-question)
- **Reasoning is generation with extra steps.** A reasoning model still predicts tokens one at a time; it just generates intermediate computation before committing to an answer. [🔗](#direct-vs-reasoning)
- **Inference-time compute is a new axis of capability.** Beyond training a bigger model, you can let a model spend more useful computation *while answering* — `Capability = Training-time compute + Inference-time compute`. [🔗](#inference-time-compute)
- **Chain → Tree → Graph of Thoughts.** CoT walks one path; ToT explores, scores, and prunes branches; GoT lets ideas from different branches reconnect and combine. [🔗](#chain-tree-graph)
- **Visible reasoning is not necessarily faithful — and more tokens ≠ more intelligence.** The shown steps may be a post-hoc summary, and more thinking raises the *probability* of success but never guarantees correctness (overthinking can even flip a right answer). [🔗](#unfaithful-reasoning)
- **We teach reasoning with RL — and RLVR is the upgrade.** Reinforcement Learning pushes behavior toward higher reward; verifiable rewards (math, code) are objective and cheaper than human preference, and self-play is how AlphaGo surpassed humans. [🔗](#rlvr)
- **A modern assistant = Knowledge + Reasoning + Tools.** Knowledge comes from training, reasoning from reinforcement learning, and tools are the superpowers — mix all three and you get the ChatGPT you use daily. [🔗](#three-pillars)

## Questions / Things to Explore

- How do **test-time compute** budgets (e.g. "thinking" tokens) actually trade off against latency and cost in production?
- What is **speculative decoding**, and how does it relate to spending more compute during inference?
- How do **process rewards** (scoring each reasoning step) differ from **outcome rewards** (scoring only the final answer) in RLVR?
- How can we detect **unfaithful reasoning** in practice, and are there techniques to make Chain of Thought more faithful?
- How does **LLM-as-a-Judge** bias (position, verbosity, self-preference) show up in real evaluation pipelines, and how do teams mitigate it?
- How does **RAG** (Retrieval + Generation) fit into the "right tool for the task" routing — and when should retrieval beat reasoning? _(coming soon — Season 4)_

---

| ← Previous | Next → |
|:---:|:---:|
| [Episode 08: From a Base Model to an AI Assistant](./episode-08-from-a-base-model-to-an-ai-assistant.md) | [Season 2 — Episode 01: Fundamentals of Building with AI](../season-2-ai-native-software-engineer/episode-01-fundamentals-of-building-with-ai.md) |
