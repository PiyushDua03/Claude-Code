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

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/AB5B5F67-5119-444A-87CE-3382D9131D9A/E9E1B47C-2C77-4D0B-AF21-67F5A6F7035C_2/EgTXdn13rqrzyJHlx8ksmXJSCxGqWrcOUeGyy2nk9Eoz/Image.png)

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

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/AB5B5F67-5119-444A-87CE-3382D9131D9A/310BB2FE-1366-4F4E-9DDC-8D0BA13F9A83_2/EZvtKaVaV6InFZB3IX4bkLZ7IxvSRqyQisudTriCgTcz/Image.png)

Developers no longer need to memorize Cron syntax.

Instead, simply ask AI:

> "Give me a Cron expression for every Monday, Wednesday, and Friday at 8 AM."

AI generates it for you.

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/AB5B5F67-5119-444A-87CE-3382D9131D9A/961D4ABC-5D04-4BFC-A2CF-17BE4583B70C_2/OIRG1NDdQyl6m2kYuwmxsNoxnyWR4It1xXl746p2WJgz/Image.png)

## Method 1 — `/loop`

Runs inside the current Claude Code session.

```other
Current Terminal↓Current Session↓Repeat Task
```

✔ Very simple

✔ Great for testing

❌ Stops if session ends

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/AB5B5F67-5119-444A-87CE-3382D9131D9A/9278F99C-E56D-4CF2-A0EA-2E41D83E50FE_2/0J72ecE7yCfNRWyTQVxnvhliY7RrnyFD7bwcT4ofhqQz/Image.png)

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

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/AB5B5F67-5119-444A-87CE-3382D9131D9A/C9FEF3E7-FF0F-4BAC-8E0C-B02FB79AC761_2/ltOOsl1hUldrCZvJZSQjpBYRibiCmJA36q5qsT6CnBgz/Image.png)
