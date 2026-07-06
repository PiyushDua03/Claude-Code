#  📦 Claude Code Skills

> A skill is simply a **Markdown (`.md`) file containing instructions/prompts** — optionally bundled together with **supporting files** like scripts, reference documents, and templates.

- **MD** = Markdown — the same lightweight formatting language used for `CLAUDE.md` files, allowing headings, lists, etc. (just structured text, nothing exotic).
- At the simplest level, a skill can be *just* a prompt saved to a file.
- At a more advanced level, a skill can be an entire **playbook**: step-by-step instructions + pre-written scripts + reference docs + example outputs — everything Claude needs to execute a complex task reliably and repeatably.

<img width="1846" height="1080" alt="image" src="https://github.com/user-attachments/assets/ea1ad5b2-bbe1-481b-853a-dd3e03301ed5" />

A **Skill** is simply a reusable package containing:

- instructions
- workflows
- examples
- scripts
- reference files

Claude only loads that package **when it detects the task needs it**, instead of loading everything into context all the time. Anthropic describes Skills as folders of instructions, scripts, and resources that are loaded dynamically for specialized tasks.

Think of it like this:

```other
Claude
│
├── Skill: React Expert
├── Skill: Docker Expert
├── Skill: Security Reviewer
├── Skill: SQL Migration
├── Skill: Documentation Writer
└── Skill: Testing
```

# Why Skills Exist — The Problem Before Skills

Every AI model has a **limited context window**—the maximum amount of text it can consider at one time.

Without Skills:

```other
Claude receives:

✓ React instructions
✓ Docker instructions
✓ SQL instructions
✓ Security instructions
✓ Documentation instructions
✓ Your actual prompt
```

Even if you only asked about **React**, all the other instructions still consume context (tokens).

With Skills:

```other
You ask:
"Create a React login page."

↓

Claude detects:
"This is a React task."

↓

It loads only the React Skill.
```

So the context becomes:

```other
✓ React instructions
✓ Your prompt
```

Instead of loading hundreds or thousands of unnecessary tokens, Claude loads only what is relevant.



# Skill Creator — The Meta-Skill That Builds Skills For You

### Installing It

> **Prompt/command used:** `claude plugin install skill-creator` *(installed via terminal, then restart Claude)*

Once installed, running `/skill-creator` (or asking Claude directly) unlocks a guided, in-depth skill-building flow.

### Skill Creator's Process, As Demonstrated

1. **You describe what you want** in natural language.

>> **Prompt used:** *"Create a skill which can help me to analyze the data in depth for Kaggle competitions. Make sure that it is having the scripts as well as multiple subfolders where we are having scripts [and] more overall instructions so that it can run on the data and then provide me with a very good analysis."*

1. **It asks clarifying questions**, for example:
    - *"Should this skill be workspace-scoped (shared in your project) or personal?"* → answered **"personal"** in the first pass, then **"project-specific / create it inside my current folder"** in a refinement.
    - *"What analysis components are most important?"* → answered generally ("all of these are good").
    - *"Which programming language should the analysis use?"* → answered **Python**.
1. **It builds a full package**, not just a markdown file:
    - `SKILL.md` (main instructions)
    - `scripts/` folder — e.g., a Python **EDA (Exploratory Data Analysis) script** (`script_01_EDA.py`) it writes once and reuses every time
    - `references/` folder — e.g., `data_analysis_toolkit.md` full of best practices it researched (like scikit-learn usage notes)
    - `requirements.txt` — pinned Python library versions, so the skill behaves identically for anyone who runs it later
    - Configuration templates, a `quickstart.md`, etc.
1. **It even installs missing libraries automatically.** In the demo, `seaborn` wasn't installed on the machine — Claude simply installed it as part of running the skill. This is described as "how smart Claude Code is."

# The Skill File's "Front Matter"

<img width="1850" height="1094" alt="image" src="https://github.com/user-attachments/assets/b98e5f1e-6850-4ef3-97f1-5255c528222c" />

| **Field**                      | **Required?** | **Purpose**                                                                                                                                                                                                   |
| ------------------------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`name`**                     | No            | Display name for the skill. If omitted, Claude uses the **folder/directory name** by default.                                                                                                                 |
| **`description`**              | Recommended   | Explains *what the skill does and when to use it*. **Claude uses this specifically to decide whether to auto-apply the skill.** If omitted, Claude falls back to the first paragraph of the markdown content. |
| **`when-to-use`**              | Optional      | Additional context appended to the description in the skill listing, helping Claude decide when to invoke it.                                                                                                 |
| **`arguments`**                | Optional      | Lets the skill accept **dynamic inputs** at call-time (explained in depth below).                                                                                                                             |
| **`allowed-tools`**            | Optional      | Restrict/permit which tools the skill can use                                                                                                                                                                 |
| **`model`**                    | Optional      | Override which model runs this specific skill (e.g., force a cheaper/faster model for simple skills)                                                                                                          |
| **`effort`**                   | Optional      | Control reasoning effort per skill                                                                                                                                                                            |
| **`disable-model-invocation`** | Optional      | If set, Claude will **never auto-trigger** this skill on its own — you must manually invoke it via slash command                                                                                              |
| **`context: fork`**            | Optional      | Runs the skill in an **isolated context window**, separate from your main conversation (explained more in Section 9)                                                                                          |
| **`hooks`**                    | Optional      | Ties into the Hooks system (covered in a later part of the course)                                                                                                                                            |
| **`agent`**                    | Optional      | Ties into sub-agents (also covered later)                                                                                                                                                                     |

Claude only scans each skill's short `description` field (not the full file) to decide "is this relevant to what the user just asked?" That's cheap — even with 50 skills installed, you're only paying for 50 short descriptions, not 50 full playbooks.

# Arguments — Making a Skill Reusable and Dynamic

This is one of the most practically important features.

### The Problem Arguments Solve

Imagine you build a **"generate report"** skill that always scans a `src/` folder. That's fine — until tomorrow you need a report on a *different* folder (e.g., a `Prisma` folder instead of a Python `src/` folder). Without arguments, you'd have to write a whole new skill. With arguments, you make the **same skill flexible**.

### Example Skill Built Live: `generate-report`

> **Prompt used to create it (paraphrased core instructions written into `SKILL.md`):** *"Input goal: generate a comprehensive daily status report for the project. Target directory: `src` (default), or user-specified path. Report output location: `reports`. Include TODOs: yes.* *Step 1: Scan the codebase — read all files (of a given file type).* *Step 2: Summarize findings.* *Step 3: Write the report.* *Step 4: Confirm completion — print a confirmation message with how many files were scanned, how many TODOs were found, and the path to the report file."*

Calling it:

> **Prompt used:** `/generate-report`

Result: Claude asked permission to run a shell command (e.g., *"find all TypeScript and JavaScript files"*), scanned the codebase, searched for `TODO`/`FIXME`/`HACK` comments, created a `reports/` directory, and generated a full Markdown report including:

- Codebase breakdown (API routes, UI components, state management, utilities)
- Code health summary and grade (e.g., "Health Grade: A")
- Known limitations, recent changes, statistics, and next-step recommendations
- TODOs found (e.g., "0 TODOs found")

## Demo: Calling a Skill *With* Arguments

Two ways to invoke an argument-based skill were demonstrated:

1. **Technical/precise way** (directly specifying arguments):

```other
/kaggle-analysis titanic.csv --model=haiku --effort=medium --output=/output
```

1. **Natural language way** (Claude infers the arguments for you):

>> **Prompt used:** *"Run on titanic.csv with shallow [analysis] using Haiku and medium effort, output to be in `/output`."*

Both work — the natural-language version proves Claude is smart enough to map casual instructions onto the correct structured arguments, which is friendlier for non-technical users.

# Letting a Skill Improve Itself Over Time

Just like a `CLAUDE.md` file can be edited and refined over time, a skill can be **asked to improve its own instructions or scripts** based on real results.

> **Prompt used:** *"Can you enhance my files based on your analysis, and also change [the] skill to create HTML analysis inside a [reports] folder [within the output folder]?"*

What happened:

- Claude didn't just add a note to `SKILL.md` saying "put reports in a subfolder" (which would require re-deciding this every run).
- Instead, it **edited the actual script code** so the correct output path is baked in permanently.


<img width="2208" height="1292" alt="image" src="https://github.com/user-attachments/assets/d410f4f1-c544-4457-8b8a-065ae8967589" />


|                | **Skill**                                                                                                   | **Sub-Agent**                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Session        | Runs **inside the same session** as your main conversation (by default)                                     | Runs in **its own separate session**                                          |
| Context window | Shares the parent's context window (unless `context: fork` is used)                                         | Has its **own independent** context window                                    |
| Model          | Historically fixed to the parent's model; **recently updated** to allow a different/cheaper model per skill | Can freely use a different or cheaper model                                   |
| Permissions    | Inherits the parent Claude Code session's read/execute permissions                                          | Typically has more restricted permissions (e.g., **read-only**)               |
| Lifecycle      | Follows the steps written in `SKILL.md`, within the parent conversation                                     | **Spawned and terminated by the parent** — like hiring a temporary specialist |
| Best for       | Repeatable workflows you want to run *within* your ongoing conversation                                     | Delegating a self-contained sub-task away from your main context              |

