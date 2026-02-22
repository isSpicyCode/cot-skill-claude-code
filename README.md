# Chain-of-Thought for Claude Code

A structured technical reasoning skill for Claude Code.
Works with any language, framework, or paradigm.

---

## Why this skill exists

Most AI responses to technical questions are fast but shallow.
They give you an answer. They rarely show you the reasoning behind it.
And they almost never tell you when they are uncertain.

This skill changes that.

It forces Claude to decompose the question, resolve each sub-problem in order,
and be explicit about what it does not know.
The result is a response you can verify, audit, and hand to a colleague without context.

---

## Installation

Add the marketplace to Claude Code:

```bash
/plugin marketplace add isSpicyCode/cot-skill-claude-code
```

Install the skill:

```bash
/plugin install cot@cot
```

---

## Usage

**With the question provided:**

```bash
/cot:mode <Your technical question here ? >
```

Claude will only ask : 

**How much detail do you need?** ( 1. Flash | 2. Normal | 3. Deep )

---

## The three modes

### 1. Flash

Direct answer. No decomposition. No phases.

For syntax lookups, quick concepts, and single-step questions.
Token cost: low.

### 2. Normal

Full structured reasoning in four phases.

- **Phase 1** — Identifies the question type (concept, implementation, debug, review, architecture, comparison)
- **Phase 2** — Decomposes into ordered sub-problems, from the simplest prerequisite to the core question
- **Phase 3** — Resolves each sub-problem sequentially, each one building on the previous
- **Phase 4** — Closes with a stable summary, known limits, and one language-independent principle to retain

For implementation, debugging, code review, and architecture questions.
Token cost: medium.

### 3. Deep

Everything in Normal, plus three additional layers.

- **Layer 5** — A trade-off table comparing every viable alternative across complexity, testability, performance, maintainability, and reversibility
- **Layer 6** — Full test coverage for every sub-problem resolved in Phase 3
- **Layer 7** — A six-month consequence analysis

Layer 7 is the one most prompts skip. It forces three concrete questions:
What will become harder to change in six months? What will the next developer misunderstand?
What architectural direction does this decision permanently close off?

Projecting forward in time changes the quality of architectural decisions.
An approach that looks clean today often reveals its cost only when the codebase has grown around it.
Deep makes that cost visible before you commit.

For decisions that are hard or costly to reverse.
Token cost: high.

---

## Uncertainty markers

All three modes enforce the same anti-hallucination rules.
These rules override everything else, including the instruction to be concise.

| Marker | When it appears |
|---|---|
| `[VERIFY]` | Code whose behavior depends on a specific version or configuration that was not confirmed |
| `[UNCERTAIN]` | Any answer where confidence is not full |
| `[KNOWLEDGE LIMIT]` | Question outside the known distribution of the underlying model |
| `[AMBIGUITY]` | Reasoning step that can be interpreted in more than one way — the chosen interpretation is always stated explicitly |

These markers are never omitted to keep responses short.
A developer who trusts false code loses hours. An honest marker costs one second to read.

---

## Scientific foundations

This skill is grounded in six research papers.

| Paper | Role in the skill |
|---|---|
| Chain-of-Thought — Wei et al., 2022 ([arXiv:2201.11903](https://arxiv.org/abs/2201.11903)) | Drives Phase 3: sequential resolution |
| Self-Consistency — Wang et al., 2022 ([arXiv:2203.11171](https://arxiv.org/abs/2203.11171)) | Drives Phase 4: stable summary |
| Least-to-Most — Zhou et al., 2022 ([arXiv:2205.10625](https://arxiv.org/abs/2205.10625)) | Drives Phase 2: decomposition from prerequisite to core |
| Progressive-Hint — Zheng et al., 2023 ([arXiv:2304.09797](https://arxiv.org/abs/2304.09797)) | Drives the iteration rule in Phase 3 |
| Is CoT a Mirage — Zhao et al., 2025 ([arXiv:2508.01191](https://arxiv.org/abs/2508.01191)) | Drives the anti-hallucination rules |
| Reusability and Verifiability — Aggarwal et al., 2026 ([arXiv:2602.17544](https://arxiv.org/abs/2602.17544)) | Drives the verifiability rule and third-party legibility requirement |

---

## License

MIT [LICENSE](./LICENSE) for details.
