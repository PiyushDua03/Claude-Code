# Slash Commands

<img width="1468" height="792" alt="image" src="https://github.com/user-attachments/assets/eeadcc86-9a36-4ec3-852f-868835ceb4c5" />

[https://claude.ai/public/artifacts/532827c1-ba83-42f2-b0b1-5113d7f8babe](https://claude.ai/public/artifacts/532827c1-ba83-42f2-b0b1-5113d7f8babe)

# Sessions

### 2.1 What a Session Is

Every time you open Claude Code in a folder, it **creates a session** consisting of:

- A unique **session ID**
- Full **history** of the conversation
- **Timestamps**
- **Metadata** (what files were changed, etc.)

Sessions are **automatically stored on your local machine** — you don't have to manually save anything.

### 2.2 The Common Mistake

Most people — even experienced users — make this mistake:

1. They work on a project.
2. At the end of the day, they close everything (VS Code, terminal).
3. The next day, they start a **brand new session from scratch**, losing all prior context, and have to re-explain or re-establish everything.

### 2.3 The Correct Approach

Instead, you should:

- Use `/exit` to gracefully close the session (not just close the window).
- The next day, use `/resume` (or select the session from the UI's session history list) to **pick up exactly where you left off** — same files, same context, same conversation.
- This saves both **tokens** (you're not re-explaining context) and **time**, and avoids unnecessary re-compaction.

### 2.4 Where Sessions Are Physically Stored

- Sessions are saved locally, and the specific path depends on OS (Windows / Linux / Mac).
- On Mac (as demonstrated): inside a hidden folder — `~/.claude/`
    - Inside `.claude/`, there's a `projects/` folder.
    - Inside `projects/`, each project has its own folder containing all of its saved sessions.
- This is a preview of the deeper `.claude` directory topic (see Section 5) — sessions are just one piece of what lives there.

## Permission Modes

### 3.1 Why This Exists

> "This is the most important safety concept in Claude Code, and it's also the most commonly misunderstood."

Claude Code can do powerful — and potentially destructive — things: write files, delete files, run terminal commands. It can also make mistakes. **Permission modes let you control how much autonomy Claude Code has** — i.e., how much supervision you want over its actions.

<img width="2068" height="1096" alt="image" src="https://github.com/user-attachments/assets/e4f77d97-bb31-49c7-a825-b69e0623d980" />

### 3.2 The Modes (from most to least supervised)

| **Mode**                                                | **Behavior**                                                                                                                                                                                                                                        | **Best For**                                                                                                                                                  |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Read-only / Default (standard)**                      | Never creates/edits files; only reads. Asks before literally everything.                                                                                                                                                                            | Getting started; working with sensitive parts of a codebase                                                                                                   |
| **Plan mode**                                           | Also read-only. Claude explores the codebase and **presents a full plan before making any edits.**                                                                                                                                                  | Exploring/understanding a codebase before changing it; starting a brand-new project; using Opus to draft the architecture before switching to Sonnet to build |
| **Accept edits / Edit automatically**                   | Automatically applies file edits (create, move, copy, touch, mkdir, etc.) without asking each time, but still surfaces changes.                                                                                                                     | Iterating quickly on code you're already reviewing                                                                                                            |
| **Auto mode**                                           | Executes actions with **background safety checks**; reduces "prompt fatigue" from repeatedly clicking yes. (Recently launched; **not available on the Pro plan** — requires Admin/Team/Enterprise plans, or testing via the Anthropic console/API.) | Long tasks where constant confirmation would be tedious, but you still want safety guardrails                                                                 |
| **Don't ask (pre-approved only)**                       | Only pre-approved tools/commands can run without asking; everything else still prompts.                                                                                                                                                             | CI pipelines, scripts, lockdown environments                                                                                                                  |
| **Bypass permissions ("dangerously skip permissions")** | Executes literally everything without asking — no safety net.                                                                                                                                                                                       | **Avoid on your main machine.** Only appropriate in isolated containers/VMs where a mistake (e.g., deleting your whole file system) can't cause real damage.  |

**Switching modes:** Press **Shift+Tab** in the terminal/CLI to cycle between plan mode, accept-edits mode, and normal (ask-every-time) mode. In VS Code, you can also configure this via **Claude Code settings** (`Cmd/Ctrl + ,` → search "Claude Code" → look for **"Initial Permission Mode"**), with the same options (default, accept edits, plan, bypass permissions) available across VS Code, JetBrains, desktop, web, and mobile.
