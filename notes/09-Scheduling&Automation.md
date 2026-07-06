### 📅 Scheduling in Claude Code
## What is Scheduling?

**Scheduling** is the ability to make Claude Code run **automatically at predefined times or intervals**, without requiring you to manually open your computer and start it.

Instead of waiting for a user prompt, Claude executes tasks on its own according to a schedule.

---

# Why Do We Need Scheduling?

Without scheduling:

- You must manually run Claude every time.
- Repetitive daily tasks consume valuable time.
- AI only works when you are actively using it.

With scheduling:

- Claude starts automatically.
- Routine tasks are completed before you begin work.
- You receive the results (email, Slack, reports, etc.) instead of performing the work yourself.

---

# Example

### ❌ Before Scheduling

Every morning you:

- Check failing tests
- Review overnight error logs
- Check CI/CD pipeline status
- Review open Pull Requests (PRs)
- Write a Slack update

Time spent: **~30 minutes every day**

---

### ✅ After Scheduling

Claude automatically:

- Checks failing tests
- Reviews overnight logs
- Monitors pipeline status
- Flags risky PRs
- Generates stand-up summaries
- Sends reports via Email or Slack

When you arrive at work, everything is already prepared.

---

# Automation Loop

Scheduling enables an **Automation Loop**, where Claude repeatedly performs tasks without human intervention.

```other
Scheduled Time
      │
      ▼
Claude Starts Automatically
      │      
      ▼
Performs Assigned Tasks
      │
      ▼
Generates Reports / Notifications
      │
      ▼
Waits for Next Scheduled Time
      │
      ▼
Repeats Automatically
```

---

# What Can Be Scheduled?

Common tasks include:

### Software Development

Every morning:

- Check GitHub PRs
- Run tests
- Build project
- Notify developers

### Data Engineering

Every night:

- Process new data
- Validate database
- Generate dashboard

### DevOps

Every Sunday:

- Backup database
- Restart services
- Security scan

### AI Automation

Every hour:

- Read Gmail
- Summarize emails
- Send summary to Slack

### Personal Automation

Every morning:

- Read calendar
- Check weather
- Create today's task list

<img width="1205" height="769" alt="image" src="https://github.com/user-attachments/assets/a2d86877-d30e-4cbe-8500-6725bed7bfdf" />

# What is a Cron Job?

A **Cron Job** is a scheduler available on Linux/macOS.

It simply runs commands at fixed times.

Example:

```other
Every day
↓
8:00 AM
↓
Run script.py
```

Nothing more.

> **A worker with no brain.**

❌ No reasoning

❌ No recovery

❌ No retry

❌ No notification

# Key Concept

> **Claude Scheduling is not just execution. It is execution + reasoning.**

# Understanding Cron Expressions

A Cron Job needs to know **when** to execute.

Instead of writing:

> Every Monday at 8 AM

Cron uses numbers.

Example:

```other
0 8 * * 1
```
<img width="1087" height="799" alt="image" src="https://github.com/user-attachments/assets/cf0b1f6c-5d7b-4afb-85b1-27e56a15d58d" />


Developers no longer need to memorize Cron syntax.

Instead, simply ask AI:

> "Give me a Cron expression for every Monday, Wednesday, and Friday at 8 AM."

AI generates it for you.

<img width="955" height="530" alt="image" src="https://github.com/user-attachments/assets/36069c82-4812-4c35-a27d-0c7a5e2a5afa" />

## Method 1 — `/loop`

Runs inside the current Claude Code session.

```other
Current Terminal
↓
Current Session
↓
Repeat Task
```

✔ Very simple

✔ Great for testing

❌ Stops if session ends

<img width="1131" height="704" alt="image" src="https://github.com/user-attachments/assets/e241d399-daee-47fa-9c75-cb21978105dd" />

---

## Method 2 — Desktop Routines

Runs using Claude Desktop.

```other
Claude Desktop
↓
Schedules Tasks
↓
Accesses Local Files
```

Better than `/loop` because it can start new sessions, but it still depends on your machine being powered on.

---

## Method 3 — Cloud Routines

Runs entirely on Anthropic's cloud infrastructure.

```other
Internet
↓
Cloud Servers
↓
Runs Anytime
↓
Laptop Can Be Off
```

This is the most powerful option and is designed for production automation.

<img width="1165" height="709" alt="image" src="https://github.com/user-attachments/assets/8abe52e4-1ddf-43fa-8f22-350aa9ca4ab3" />
