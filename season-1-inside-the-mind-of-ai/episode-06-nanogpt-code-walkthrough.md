# Episode 06 (Companion): nanoGPT Code Walkthrough

> **Season 1 — Inside the Mind of AI**

> A line-by-line tour of the important parts of [nanoGPT's `model.py`](https://github.com/karpathy/nanogpt/blob/master/model.py), mapped directly onto the architecture diagram from [Episode 06](./episode-06-the-computational-brain-of-machines.md). Read the episode first; this file is the "where is that in the code?" reference.

> **A useful question:** Which lines of code actually do the embedding, the attention, and the final probability output?

---

## Why nanoGPT?

[nanoGPT](https://github.com/karpathy/nanogpt) is Andrej Karpathy's minimal, readable implementation of a GPT model in PyTorch. The entire model lives in a single file, `model.py`, with helpful comments. That makes it the best place to connect the diagram to real code.

The [official GPT-2 codebase](https://github.com/openai/gpt-2/tree/master) is the canonical reference (nanoGPT's `model.py` even cites it), but it is larger and more production-oriented. Start here, then read GPT-2 when you want to see the "real thing."

> 📌 **How to read this file:** each section names a box from the diagram, shows the matching class or lines from `model.py`, and explains what it does in plain terms. You do not need to memorize the code — you need to recognize it when you see it.

---

## The map: diagram box → code

| Diagram box | Code in `model.py` | What it does |
|:---|:---|:---|
| tok embed | `wte = nn.Embedding(...)` | Token ID → vector |
| pos embed | `wpe = nn.Embedding(...)` | Position → vector |
| layer norm | `LayerNorm` class | Rescale the numbers |
| multi-head causal self-attention | `CausalSelfAttention` class | Tokens relate to each other |
| feed forward | `MLP` class | Per-token non-linear processing |
| transformer i (the block) | `Block` class | attention + MLP, with skips |
| linear + softmax | `lm_head` + `F.softmax` | Vector → probabilities |
| the whole model | `GPT` class | Wires all of the above together |

---

## 1. The configuration: the model's "shape"

Before any code, the model's size is described by a small config. These are the knobs that turn a tiny model into a big one.

```python
@dataclass
class GPTConfig:
    block_size: int = 1024      # max sequence length (context window)
    vocab_size: int = 50304     # how many distinct tokens exist
    n_layer: int = 12           # how many Transformer blocks to stack
    n_head: int = 12            # how many attention "heads" per block
    n_embd: int = 768           # size of each vector (embedding dimension)
    dropout: float = 0.0        # random "off" switch for regularization
    bias: bool = True           # whether linear layers use a bias term
```

> 🧭 **Read it like a recipe:** `n_layer` is how many times the attention+feed-forward block repeats (the "transformer i" in the diagram). `n_head` is how many parallel attention "heads" run inside each block. `n_embd` is how many numbers make up each token's vector. `block_size` is the longest sequence the model can handle at once.

The `from_pretrained` method shows how these scale up for real GPT-2 checkpoints:

```python
config_args = {
    'gpt2':        dict(n_layer=12, n_head=12, n_embd=768),   # 124M params
    'gpt2-medium': dict(n_layer=24, n_head=16, n_embd=1024),  # 350M params
    'gpt2-large':  dict(n_layer=36, n_head=20, n_embd=1280),  # 774M params
    'gpt2-xl':     dict(n_layer=48, n_head=25, n_embd=1600),  # 1558M params
}
```

> 📐 **Bigger in every dimension = more parameters = more capacity (and more cost).** This is the concrete meaning of "a larger model."

---

## 2. Embeddings: `wte` and `wpe`

The first thing the model does is turn token IDs into vectors. That is two lookups added together.

```python
self.transformer = nn.ModuleDict(dict(
    wte = nn.Embedding(config.vocab_size, config.n_embd),  # token embeddings
    wpe = nn.Embedding(config.block_size, config.n_embd),  # position embeddings
    drop = nn.Dropout(config.dropout),
    h = nn.ModuleList([Block(config) for _ in range(config.n_layer)]),  # the blocks
    ln_f = LayerNorm(config.n_embd, bias=config.bias),
))
```

- `wte` is the **token embedding table**: one learned vector for each of the `vocab_size` tokens. Looking up token `4331` returns its vector.
- `wpe` is the **position embedding table**: one learned vector for each possible position in the sequence. This is what tells the model *where* a token sits.

In the forward pass, the two are added:

```python
tok_emb = self.transformer.wte(idx)   # (b, t, n_embd)  — what each token is
pos_emb = self.transformer.wpe(pos)   # (t, n_embd)     — where each token is
x = self.transformer.drop(tok_emb + pos_emb)  # combine, then a little dropout
```

> 🧭 **This is the "tok embed + pos embed" box in the diagram.** The `+` is the literal addition of the two vectors. `drop` randomly zeroes a few values during training (a regularizer that helps generalization); it does nothing at inference.

---

## 3. Layer Norm: keeping the numbers well-behaved

```python
class LayerNorm(nn.Module):
    def __init__(self, ndim, bias):
        super().__init__()
        self.weight = nn.Parameter(torch.ones(ndim))
        self.bias = nn.Parameter(torch.zeros(ndim)) if bias else None

    def forward(self, input):
        return F.layer_norm(input, self.weight.shape, self.weight, self.bias, 1e-5)
```

**Layer Norm** rescales a vector so its values sit in a consistent range (mean near 0, standard deviation near 1), then applies a learned scale (`weight`) and shift (`bias`).

> 🧭 **Why it matters:** as vectors pass through many layers, their values can grow or shrink wildly. Layer Norm keeps them stable so the network can train. It is the `layer norm` boxes in the diagram — there are three of them per block (two inside, one at the very end).

---

## 4. Self-Attention: the heart, in code

This is the most important class. It is where tokens relate to each other.

```python
class CausalSelfAttention(nn.Module):
    def __init__(self, config):
        super().__init__()
        assert config.n_embd % config.n_head == 0
        # one projection that produces Query, Key, and Value for all heads at once
        self.c_attn = nn.Linear(config.n_embd, 3 * config.n_embd, bias=config.bias)
        # output projection
        self.c_proj = nn.Linear(config.n_embd, config.n_embd, bias=config.bias)
        self.attn_dropout = nn.Dropout(config.dropout)
        self.resid_dropout = nn.Dropout(config.dropout)
        self.n_head = config.n_head
        self.n_embd = config.n_embd
        # causal mask: a token may only attend to tokens at or before it
        self.register_buffer("bias", torch.tril(torch.ones(config.block_size, config.block_size))
                             .view(1, 1, config.block_size, config.block_size))
```

The forward pass, step by step:

```python
def forward(self, x):
    B, T, C = x.size()  # batch size, sequence length, embedding dimension

    # 1) Build Query, Key, Value for every head
    q, k, v = self.c_attn(x).split(self.n_embd, dim=2)
    k = k.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)  # (B, nh, T, hs)
    q = q.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)
    v = v.view(B, T, self.n_head, C // self.n_head).transpose(1, 2)

    # 2) Score how relevant every token is to every other token
    att = (q @ k.transpose(-2, -1)) * (1.0 / math.sqrt(k.size(-1)))

    # 3) Mask the future so a token cannot peek ahead (this is "causal")
    att = att.masked_fill(self.bias[:,:,:T,:T] == 0, float('-inf'))

    # 4) Turn scores into weights that add up to 1
    att = F.softmax(att, dim=-1)
    att = self.attn_dropout(att)

    # 5) Blend all the Values using those weights
    y = att @ v  # (B, nh, T, T) x (B, nh, T, hs) -> (B, nh, T, hs)

    # 6) Re-assemble the heads and project the result
    y = y.transpose(1, 2).contiguous().view(B, T, C)
    y = self.resid_dropout(self.c_proj(y))
    return y
```

Let's translate each step into the plain-language version from the episode:

| Code | Plain meaning |
|:---|:---|
| `q, k, v = self.c_attn(x).split(...)` | Each token's vector is turned into a **Query** ("what am I looking for?"), a **Key** ("what do I contain?"), and a **Value** ("what do I contribute?"). |
| `att = (q @ k.transpose(-2, -1)) * (1.0 / sqrt(...))` | Every Query is compared against every Key to get a relevance score. The `1/sqrt(...)` is a scaling factor that keeps the scores from getting too large. |
| `att.masked_fill(..., -inf)` | Scores for *future* tokens are set to negative infinity, so after softmax they become 0. This is the **causal** mask — no peeking at the answer. |
| `att = F.softmax(att, dim=-1)` | The scores become weights that add up to 1 (the "how much to pay attention" numbers). |
| `y = att @ v` | Each token's new vector is a **weighted average** of all the Values, using those weights. |
| `self.c_proj(y)` | The blended result is projected back to the normal vector size. |

> 🧭 **This single class is the "multi-head, causal self-attention" box.** "Multi-head" is the `n_head` split — the same computation runs in parallel `n_head` times, each head free to learn a different kind of relationship. "Causal" is the `masked_fill` line.

> ⚠️ **The `@` symbol is a matrix multiplication.** `q @ k.transpose(-2, -1)` multiplies the Query matrix by the transposed Key matrix. That one line is where "how related are these two tokens?" is actually computed.

---

## 5. The Feed-Forward Network (MLP)

After attention, each token goes through a small network *on its own* (no cross-token interaction here).

```python
class MLP(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.c_fc    = nn.Linear(config.n_embd, 4 * config.n_embd, bias=config.bias)  # expand
        self.gelu    = nn.GELU()                                                       # non-linearity
        self.c_proj  = nn.Linear(4 * config.n_embd, config.n_embd, bias=config.bias)   # shrink back
        self.dropout = nn.Dropout(config.dropout)

    def forward(self, x):
        x = self.c_fc(x)     # widen the vector 4x
        x = self.gelu(x)     # apply a smooth non-linear activation
        x = self.c_proj(x)   # project back to the original size
        x = self.dropout(x)
        return x
```

> 🧭 **This is the "feed forward" box.** The pattern is *expand → activate → shrink*: the vector is widened to 4× its size, passed through a non-linear function (GELU), then projected back. The non-linearity is what lets the model learn complex, non-straight-line patterns. Without it, stacking layers would collapse into a single linear operation.

---

## 6. The Transformer Block: attention + MLP, with skips

The block is the unit that gets repeated `n_layer` times. It wraps attention and the MLP with Layer Norm and **residual (skip) connections**.

```python
class Block(nn.Module):
    def __init__(self, config):
        super().__init__()
        self.ln_1 = LayerNorm(config.n_embd, bias=config.bias)
        self.attn = CausalSelfAttention(config)
        self.ln_2 = LayerNorm(config.n_embd, bias=config.bias)
        self.mlp  = MLP(config)

    def forward(self, x):
        x = x + self.attn(self.ln_1(x))   # x + attention(layer_norm(x))
        x = x + self.mlp(self.ln_2(x))    # x + mlp(layer_norm(x))
        return x
```

Look closely at the two `x + ...` lines. That `+` is the **residual connection** — the `⊕` symbols in the diagram. The original input `x` is added back to the output of each part.

> 🧭 **This is the "transformer i" box, and it is repeated `n_layer` times.** The structure is exactly: *layer norm → attention → add skip → layer norm → feed-forward → add skip.* The skips are what let you stack 12, 24, or 48 of these blocks without the signal vanishing.

---

## 7. The Output: from vector to probabilities

The final part of the model turns the last token's vector into a probability for every token in the vocabulary.

```python
self.lm_head = nn.Linear(config.n_embd, config.vocab_size, bias=False)
```

In the forward pass:

```python
x = self.transformer.ln_f(x)          # final layer norm
logits = self.lm_head(x)              # project to one number per vocab token
loss = F.cross_entropy(logits.view(-1, logits.size(-1)), targets.view(-1), ignore_index=-1)
```

- `lm_head` is the **linear** box: it maps each vector (size `n_embd`) to a number for *every* token (size `vocab_size`). These raw numbers are called **logits**.
- During training, `cross_entropy` compares the logits to the true next token and produces a **loss** — the signal the optimizer uses to nudge every parameter.
- At inference, the logits are passed through **softmax** to become probabilities (see the `generate` method below).

> 🧭 **This is the "linear + softmax" box.** The `softmax` step is what turns a list of raw numbers into a proper probability distribution that adds up to 1.

> 🔗 **Weight tying:** notice this line in `__init__`:
> ```python
> self.transformer.wte.weight = self.lm_head.weight
> ```
> The token-embedding table and the output projection **share the same weights**. The vectors that turn tokens *into* the model are the same vectors that turn the model's output *back into* token scores. It saves parameters and is a detail worth knowing exists.

---

## 8. Putting it together: the forward pass

The `GPT.forward` method is the whole pipeline in a few lines. Read it top to bottom and you are reading the diagram.

```python
def forward(self, idx, targets=None):
    b, t = idx.size()
    pos = torch.arange(0, t, dtype=torch.long, device=idx.device)

    tok_emb = self.transformer.wte(idx)        # 1. token embeddings
    pos_emb = self.transformer.wpe(pos)        # 2. position embeddings
    x = self.transformer.drop(tok_emb + pos_emb)  # combine

    for block in self.transformer.h:           # 3. repeat the block n_layer times
        x = block(x)

    x = self.transformer.ln_f(x)               # 4. final layer norm
    logits = self.lm_head(x)                   # 5. project to vocab probabilities
    # ... (loss computed here during training)
    return logits, loss
```

> 🧭 **Map it to the diagram:** `wte`/`wpe` are the top boxes, the `for block` loop is the repeated "transformer i," and `ln_f` + `lm_head` are the bottom boxes. That is the entire forward pass.

---

## 9. Generating text: the prediction loop

The `generate` method is where the "predict the next token, append it, repeat" loop from the episode lives.

```python
@torch.no_grad()
def generate(self, idx, max_new_tokens, temperature=1.0, top_k=None):
    for _ in range(max_new_tokens):
        idx_cond = idx if idx.size(1) <= self.config.block_size else idx[:, -self.config.block_size:]
        logits, _ = self(idx_cond)                 # run the whole model
        logits = logits[:, -1, :] / temperature    # take the last position, scale by temperature
        if top_k is not None:                      # optionally keep only the top-k options
            v, _ = torch.topk(logits, min(top_k, logits.size(-1)))
            logits[logits < v[:, [-1]]] = -float('Inf')
        probs = F.softmax(logits, dim=-1)          # logits -> probabilities
        idx_next = torch.multinomial(probs, num_samples=1)  # sample one token
        idx = torch.cat((idx, idx_next), dim=1)    # append it and loop again
    return idx
```

> 🧭 **This is the loop from the episode, in code.** Each iteration runs the model, takes the probabilities for the *next* token, samples one, and appends it. `temperature` controls how "spread out" the probabilities are (lower = more deterministic, higher = more random), and `top_k` limits the choice to the `k` most likely tokens.

---

## Where to go next

- 📺 Watch **[Andrej Karpathy — "Let's build GPT from scratch"](https://www.youtube.com/watch?v=kCc8FmEb1nY)** while reading `model.py`. He builds a tiny GPT in code, which makes every line above click.
- 🌐 Watch the data flow in 3D at **[bbycroft.net/llm](https://bbycroft.net/llm)**.
- 📄 When you are ready for the canonical version, read the [official GPT-2 `model.py`](https://github.com/openai/gpt-2/blob/master/src/model.py). The concepts are identical; the implementation is just larger.

> ⚠️ **You do not need to memorize this code.** The goal is recognition: when you see `CausalSelfAttention`, you should think "that is the attention box," and when you see `x + self.attn(...)`, you should think "that is a residual connection." The understanding is in the mapping, not in the syntax.

---

| ← Back to Episode 06 | Next → |
|:---:|:---:|
| [Episode 06: The Computational Brain of Machines](./episode-06-the-computational-brain-of-machines.md) | _coming soon_ |
