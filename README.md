```
╔═══════════════════════════════════════════════════════════════╗
║                                                               ║
║    ██████╗ ██████╗ ████████╗                                  ║
║   ██╔════╝██╔═══██╗╚══██╔══╝                                  ║
║   ██║     ██║   ██║   ██║                                     ║
║   ██║     ██║   ██║   ██║                                     ║
║   ╚██████╗╚██████╔╝   ██║                                     ║
║    ╚═════╝ ╚═════╝    ╚═╝   Chain-of-Thought for Claude Code  ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

![Version](https://img.shields.io/badge/version-0.1.1-blue) ![License](https://img.shields.io/badge/license-MIT-lightgrey)

> Structured technical reasoning for any language, framework, or paradigm.
> Forces Claude to decompose, resolve, and be honest about what it does not know.

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

```bash
/plugin marketplace add isSpicyCode/cot-skill-claude-code
/plugin install cot@cot
```

---

## Usage

```
┌─ Terminal ──────────────────────────────────────────────────────┐
│                                                                 │
│  $ /cot:mode Why is my React app re-rendering on every tick?    │
│                                                                 │
│  ┌─ How much detail do you need? ──────────────────────────┐    │
│  │                                                         │    │
│  │  1. Flash   · Direct answer. Fast and short.            │    │
│  │  2. Normal  · Full structured reasoning.                │    │
│  │  3. Deep    · Normal + alternatives + tests + future.   │    │
│  │                                                         │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## The three modes

```
┌────────────────────────────────────────────────────────────────┐
│  FLASH                                                         │
│  ─────                                                         │
│  No decomposition. No phases. Direct answer.                   │
│  Token cost: ▓░░░░                                             │
│  Best for: syntax lookups · quick concepts · single-step       │
└────────────────────────────────────────────────────────────────┘
```

For syntax lookups, quick concepts, and single-step questions.
If the question requires more than three steps to answer correctly,
Flash will complete its response and suggest running Normal or Deep.

```
┌────────────────────────────────────────────────────────────────┐
│  NORMAL                                                        │
│  ──────                                                        │
│  Phase 1 · Question type identification                        │
│  Phase 2 · Decomposition (prerequisite → core)                 │
│  Phase 3 · Sequential resolution                               │
│  Phase 4 · Summary, known limits, one principle to retain      │
│  Token cost: ▓▓▓░░                                             │
│  Best for: implementation · debugging · review · architecture  │
└────────────────────────────────────────────────────────────────┘
```

- **Phase 1** — Identifies the question type: concept, implementation, debug, review, architecture, or comparison.
- **Phase 2** — Decomposes into ordered sub-problems, from the simplest prerequisite to the core question.
- **Phase 3** — Resolves each sub-problem sequentially, each one building on the previous.
- **Phase 4** — Closes with a stable summary, known limits, and one language-independent principle to retain.

```
┌────────────────────────────────────────────────────────────────┐
│  DEEP                                                          │
│  ────                                                          │
│  Everything in Normal, plus:                                   │
│  Layer 5 · Trade-off table (all viable alternatives)           │
│  Layer 6 · Full test coverage per sub-problem                  │
│  Layer 7 · Six-month consequence analysis                      │
│  Token cost: ▓▓▓▓▓                                             │
│  Best for: decisions that are hard or costly to reverse        │
└────────────────────────────────────────────────────────────────┘
```

Layer 7 is the one most prompts skip. It forces three concrete questions:
What will become harder to change in six months? What will the next developer misunderstand?
What architectural direction does this decision permanently close off?

Projecting forward in time changes the quality of architectural decisions.
An approach that looks clean today often reveals its cost only when the codebase has grown around it.
Deep makes that cost visible before you commit.

---

## Uncertainty markers

All three modes enforce the same anti-hallucination rules.
These rules override everything else, including the instruction to be concise.

```
┌──────────────────────┬─────────────────────────────────────────────────────────────┐
│ Marker               │ When it appears                                             │
├──────────────────────┼─────────────────────────────────────────────────────────────┤
│ [VERIFY]             │ Behavior depends on unconfirmed version or config           │
│ [UNCERTAIN]          │ Confidence is not full                                      │
│ [KNOWLEDGE LIMIT]    │ Outside the known distribution of the model                 │
│ [AMBIGUITY]          │ Step interpretable in more than one way                     │
└──────────────────────┴─────────────────────────────────────────────────────────────┘
```

These markers are never omitted to keep responses short.
A developer who trusts false code loses hours. An honest marker costs one second to read.

---

## Scientific foundations

This skill is grounded in six research papers.

```
  Wei et al., 2022        ──▶  Phase 3: sequential resolution
  Wang et al., 2022       ──▶  Phase 4: stable summary
  Zhou et al., 2022       ──▶  Phase 2: decomposition from prerequisite to core
  Zheng et al., 2023      ──▶  Iteration rule in Phase 3
  Zhao et al., 2025       ──▶  Anti-hallucination rules
  Aggarwal et al., 2026   ──▶  Verifiability and third-party legibility
```

| Paper | Link |
|---|---|
| Chain-of-Thought — Wei et al., 2022 | [arXiv:2201.11903](https://arxiv.org/abs/2201.11903) |
| Self-Consistency — Wang et al., 2022 | [arXiv:2203.11171](https://arxiv.org/abs/2203.11171) |
| Least-to-Most — Zhou et al., 2022 | [arXiv:2205.10625](https://arxiv.org/abs/2205.10625) |
| Progressive-Hint — Zheng et al., 2023 | [arXiv:2304.09797](https://arxiv.org/abs/2304.09797) |
| Is CoT a Mirage — Zhao et al., 2025 | [arXiv:2508.01191](https://arxiv.org/abs/2508.01191) |
| Reusability and Verifiability — Aggarwal et al., 2026 | [arXiv:2602.17544](https://arxiv.org/abs/2602.17544) |

---

## License

MIT — see [LICENSE](./LICENSE) for details.
