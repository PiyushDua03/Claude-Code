# The `.claude` Directory

<img width="2770" height="1626" alt="image" src="https://github.com/user-attachments/assets/6cf68334-c852-418a-9366-10f99e95f569" />

### 1.1 Why it matters

- Every serious Claude Code project has a **`.claude` folder**. Most tutorials only show you `CLAUDE.md`, but the **`.claude` directory is the single most important folder** you'll ever have for a project.
- Everything that makes Claude Code feel **smart, specialized, and under your control** resides inside this folder.

### 1.2 The "dot" prefix — hidden folders

- A folder/file prefixed with a **dot (`.`)** is **hidden by default** in Linux, macOS, and Windows.
- It's hidden **intentionally** — most casual users never touch it, but **professionals must master it**.

### 1.3 What lives inside `.claude/`

```other
.claude/
├── CLAUDE.md            (can live here OR in the project root — see Part 2)
├── CLAUDE.local.md       (local-only, gitignored)
├── settings.json
├── settings.local.json
├── mcp.json
├── rules/                (optional — see 1.7)
│   ├── code-style.md
│   ├── testing.md
│   └── security.md
├── skills/
├── agents/
└── commands/
```

- If you create anything **outside** `.claude/`, Claude Code may **not** pick it up automatically — Claude is explicitly scoped to read from `.claude/`.

### 1.4 `settings.json` — Permissions & Configuration

- Controls: **permissions** (what commands are allowed/blocked), **hooks**, **model**, **environment variables**, **output style**.
- **Example demonstrated:**

json

```json
{
    "permissions": {
      "allow": ["Bash(npm test)", "Bash(npm run *)"],
      "deny": ["Bash(rm -rf *)", "Bash(rm *)"]
    }
  }
```

- Allows `npm test` / `npm run` commands to run **without prompting**.
- **Blocks `rm -rf`** (and, when added, plain `rm`) so Claude **cannot** delete files/folders recursively without explicit override.

### 1.5 `settings.local.json`

- Same structure/purpose as `settings.json`, but for **local-only overrides** (e.g., environment variables you don't want to share with the team — like your own personal OpenAI key that shouldn't apply when deployed to the cloud).
- Example use case explained: "If on my local I want to use my own OpenAI key, but on the cloud I obviously will not be using it" → put that key in `settings.local.json`.
- This file is **not committed to git** (kept private/machine-specific).

### 1.6 Rules — Splitting CLAUDE.md into Modular Files

- **The problem:** If you put *everything* into one giant `CLAUDE.md`, it becomes very long, harder for Claude to parse well, and bloats context on every single session.
- **The solution — a `rules/` folder** inside `.claude/`, where each concern gets its **own small, focused file**:

```other
.claude/rules/
  ├── code-style.md      (naming conventions: camelCase for JS, kebab-case for CSS, snake_case for Python, docstring requirements, max line length)
  ├── testing.md         (how to test, what "done" looks like)
  └── security.md        (see below)
```

- **Benefit of splitting:** Instead of one long, ever-growing `CLAUDE.md`, you now have **three (or more) focused files** — you can update/replace `security.md` without touching `code-style.md`, delete a file cleanly if it's no longer needed, and keep everything from becoming unmanageably bloated.

# CLAUDE.md — The Anatomy, Rules, and Best Practices

### 2.1 Why CLAUDE.md matters so much

- **Claude Code starts every session with zero knowledge.** It doesn't remember what you built yesterday, your naming conventions, or your project's structure — **every session is a blank state.**

<img width="1896" height="1086" alt="image" src="https://github.com/user-attachments/assets/811968fd-dc90-4e55-9174-2073405e91a8" />

- `CLAUDE.md` is **the briefing document Claude reads before touching a single file.**
- **Core finding:** *As instruction count increases, instruction-following quality decreases.* This effect is **worse on smaller/cheaper models**, which "get much worse, much more quickly."
- There's also a documented **bias toward instructions that are peripheral to the prompt** (related to primacy/recency — see Part 3).

> **Conclusion:** If you bloat your CLAUDE.md with 700–800 lines or paste everything into it, it will **actively work against you** — not just waste tokens, but actively degrade instruction-following accuracy.

### 2.2 What Belongs in CLAUDE.md vs. What Doesn't

<img width="1280" height="808" alt="image" src="https://github.com/user-attachments/assets/905c6703-8726-4376-9487-0a758d4dde02" />

**Golden rule:** *Relevance > Length. Clarity > Quantity.* **60 relevant lines are worth more than 400 irrelevant lines.**

| **✅ Belongs in CLAUDE.md**                             | **❌ Does NOT belong in CLAUDE.md**                       |
| ------------------------------------------------------ | -------------------------------------------------------- |
| Project overview                                       | Full/entire API documentation                            |
| Essential commands (build/run/test)                    | Outdated code snippets                                   |
| Naming conventions & folder/project structure          | Task-specific / one-off instructions                     |
| Verification steps ("how to check your work is right") | Hook-related logic/instructions (these belong elsewhere) |
| Agent/tooling pointers                                 | Style-rule *code samples* that will go stale             |
| Hard rules / guardrails                                | Irrelevant or unnecessarily long info dumps              |

### 2.3 Anatomy of a Well-Structured CLAUDE.md (as built live, project: "Job Application Tracker")

<img width="1936" height="1150" alt="image" src="https://github.com/user-attachments/assets/65b1e94d-eb23-4201-8f6c-e27193af57d9" />

1. **Project overview** — what the app is (e.g., "a web page for task/job-application management").
2. **Project structure** — reference to the folder layout.
3. **Essential/key commands** — described as **one of the highest-value things you can add**, because Claude Code frequently verifies its own work by *running commands*; without these, Claude has to guess.
4. **Naming conventions** — kept clean and terse (not full sentences — just the convention itself, e.g., component/function naming rules).
5. **Testing instructions** — *"always run tests right after implementation"* (this mirrors Anthropic's internal team guidance).
6. **Hard rules / guardrails**, e.g.:
    - Never commit directly to `main`.
    - Never hardcode credentials.
    - Always handle errors explicitly — no silent failures.
    - Prefer explicit over clever code.
    - Readable code beats "compact"/clever code.
1. **Verification steps**, e.g.:
    - After code changes → run `npm run typecheck` and `npm test`.
    - After UI changes → take a screenshot and compare to requirements.
    - After database changes → verify with a test query.
    - When uncertain → **ask the user rather than assume.**

### 2.4 Levels & Hierarchy of CLAUDE.md files (per official docs, cited)

| **Level**                                                          | **Priority**                       |
| ------------------------------------------------------------------ | ---------------------------------- |
| **Local instructions** (`CLAUDE.local.md`)                         | **Highest priority**               |
| **User instructions** (global, `~/.claude`)                        | Next                               |
| **Project instructions** (`.claude/CLAUDE.md` or root `CLAUDE.md`) | Next                               |
| **Organization / Enterprise level**                                | Lowest/base layer, applies broadly |

### 2.5 `CLAUDE.local.md` — Local-Only Memory

- **Purpose:** Instructions meant *only for your own machine*, which get **ignored/excluded when you push code to GitHub or any version control system.**
- Analogy used: just like Google Docs / Google Sheets keep a **version history** of every change (visible via "Version History"), **git** does the same for code — you `commit` changes to save a snapshot, and a `.gitignore` entry ensures certain files are **never uploaded**.
- **Example content:**

markdown

```other
# Local overrides — not committed to git
  - Running this on Windows 11
  - My Python version is X
  - Local API runs on port 9000 (not 8000, which is used in production)
  - Personal environment preferences...
```

- **Why it's useful:** e.g., your code might run on `localhost:9000` locally but on `8000` in the real/deployed application — `CLAUDE.local.md` lets you override this **only for your machine**, without affecting the shared project file.

### 2.6 Using `/init` (auto-generating CLAUDE.md from an existing project)

- The `/init` slash command scans an **already-built** project and tries to auto-generate a first-draft `CLAUDE.md` for you (development commands, testing setup, architecture overview, key patterns/conventions, known limitations, etc.).
- **Rule of thumb:** *Don't take whatever `/init` gives you as automatically correct — always review and prune it.*

### 2.7 Primacy & Recency Bias (applies directly to CLAUDE.md structure)

- LLMs behave similarly to human memory in a meeting: you tend to **remember the start** and **remember the end**, but the **middle** is where attention/quality drops.

<img width="2220" height="1480" alt="image" src="https://github.com/user-attachments/assets/796fff99-3c63-4260-8032-6ba4c0346e4d" />

- **Primacy** = information at the very start of context — rarely "forgotten," consistently used.
- **Recency** = information most recently provided — also weighted heavily.
- **Middle content** is where instructions are most likely to be **missed or under-weighted.**
- **Practical guidance given:**
    - Don't just bloat your file — but **if** it does get long, deliberately place your **most non-negotiable instructions at the top or bottom**, not buried in the middle.

# Context Management ("Context Rot")

<img width="1982" height="1232" alt="image" src="https://github.com/user-attachments/assets/ef6bac6d-53de-4eb8-adc4-5a8d6ece4beb" />

### 3.1 What Context Rot Is

- **Context rot** = the gradual degradation of Claude's output quality as its context window fills up over a long session.
- **It is *not* the model "getting worse."** It is a direct symptom of **not managing context well** — i.e., a *you* problem, not a *Claude* problem.

<img width="1404" height="1028" alt="image" src="https://github.com/user-attachments/assets/1cc7c3bf-755d-44ba-b503-93575fc92724" />

- Symptoms explicitly listed:
    - Claude starts **ignoring your CLAUDE.md rules** (e.g., you said camelCase, but it starts writing snake_case).
    - Responses become **generic** instead of project-specific.
    - Claude **repeats work it already did** or **recreates files that already exist.**
    - Claude makes mistakes it **would not have made 20 messages earlier.**
    - Overall quality is **visibly worse** later in a long session — code that was "pinned"/solid 30 messages ago is now sloppy or buggy.
- **Model context window sizes cited:**
    - Haiku: **~200k tokens**
    - Sonnet: **~200k tokens**
    - On the web / Opus: **~1 million tokens** (in certain configurations)
    - What `/compact` Actually Preserves:

        **A structured summary** of the entire prior conversation — condensing (e.g.) 21 conversation events into a single structured summary that preserves: your original request/intent, key technical goals, and important established facts.

### 3.2 `/compact` vs. `/clear`

<img width="1950" height="1314" alt="image" src="https://github.com/user-attachments/assets/9919d14c-2d5e-4b28-8171-5478231aa8cb" />

| **Command**                            | **What it does**                                                                                                                                                                                                      |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`/compact [optional instructions]`** | Compresses/summarizes the current conversation to free up context, while preserving key facts. You can pass custom instructions, e.g., *"keep the full file structure, all API endpoints, and established patterns."* |
| **`/clear`**                           | Starts a brand-new session with **empty context.** The previous session is **not deleted** — it stays saved on disk and is **resumable** via `/resume`.                                                               |

- **General guidance for when to use which:**
    - If quality feels like it's decreasing but you're continuing the **same task** → `/compact`.
    - If you're unsure, or starting something genuinely **new/unrelated** → `/clear` and start fresh.

### 3.3 `CHANGELOG.md` — Persistent Memory Across Sessions/Failures

- **The problem it solves:** If your current session breaks/fails (or you *choose* to start a brand-new chat), you lose all in-session context — you're not simply resuming inside "Claude's chat window" the way you might with a typical AI chat UI.
- **The solution — add this pattern to your CLAUDE.md:**

markdown

```other
## Change Log Protocol
  After completing any significant action, update CHANGELOG.md:
  - Add what was built or changed
  - Add any decisions made that affect future work
  - Add any known bugs or incomplete items
  
  Read CHANGELOG.md at the start of every session to understand
  the current project state.
```

- **Why this works:** Even if you must start a **completely new session**, `CHANGELOG.md` gives Claude an instant, line-by-line history of what's been done, what decisions were made, and what's still broken — without needing the old conversation's context at all.
- **Demonstrated result:** After adding this instruction and clearing context (dropping from ~132k tokens down to ~36.3k), Claude reliably created and updated `CHANGELOG.md` as new changes were made (e.g., renaming the tracker, fixing dark mode, adjusting UI columns) — each meaningful change got logged.

### 3.4 Six Practical Token-Budgeting Techniques (as explicitly enumerated)

1. **Keep CLAUDE.md lean/concise.** (Directly reinforces Part 2's guidance — this is listed as the #1 lever.)

<img width="2310" height="1408" alt="image" src="https://github.com/user-attachments/assets/0e56119d-a24e-48d2-af4c-fae8f2837166" />

2. **Move instructions out of CLAUDE.md into skills, agents, and rule files.**
    - These are only **loaded on demand**, when actually needed for a task — unlike CLAUDE.md, which loads on **every single session.**
    - This is the practical payoff of the modular `.claude/rules/`, `skills/`, `agents/` structure from Part 1.9.
1. **Disable unused MCP servers.**
    - MCP tool definitions get loaded into context **up front, every session**, and this can silently cost **10,000+ tokens** if you have several MCP servers connected but aren't using them for the current task.
    - **Demonstrated fix:** going into MCP server management and manually **disabling** each unused server, then running `/compact` — this dropped context loaded at the start of a new session from **~14k+ tokens down to just ~812 tokens.**
    - **Practical rule:** if you're not using a given MCP integration for the current task/session, **disable it.**
1. **Enable Extended Thinking — it actually *saves* context (counter-intuitive but proven).**
    - **Without** extended thinking: Claude's reasoning process gets written directly into the **visible conversation history**, which counts against your context budget and accumulates with every message.
    - **With** extended thinking **enabled**: the reasoning happens in a **separate "thinking" channel/tab** that does **not** pollute the main conversation history.
    - Net effect: even though extended thinking superficially "uses more tokens to think," it **reduces overall context bloat** in the visible/counted conversation, and tends to complete tasks in less overall back-and-forth.
1. **Plan before you build.**
    - Simple but logical: planning up front reduces wasted iterations, trial-and-error, and redundant context consumption later.
1. **Review your code/output at each version — don't let errors propagate ("DRY" principle applied to AI workflow).**
    - If Claude's **very first draft (v1)** contains a mistake, subsequent versions (v2, v3...) will **build on top of that mistake**, propagating and compounding it — much like accumulating technical debt.
    - **Recommended workflow:** After the initial skeleton/draft is generated, **manually review it** (or have a fresh session specifically review it) before continuing to build on top of it. Catching and correcting errors **early** (right after the first version) prevents them from spreading through the entire codebase.
    - Practical tip: if you find an issue, you can **start a fresh session, ask it to correct just that specific part, then return to your main session** — rather than letting the flawed pattern keep multiplying inside a single long, contaminated context.
