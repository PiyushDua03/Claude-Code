#  📦 Claude Code Sub-Agents

<img width="2472" height="1450" alt="image" src="https://github.com/user-attachments/assets/4b405cad-16f8-4728-bb7c-017ce8c1de39" />

# Why Not Let One Claude Do Everything?

Even though Claude is powerful, using **multiple specialized agents** is often more efficient for three main reasons.

### 1. Context Overload

- One agent has to read **everything** (code, docs, chat history), consuming lots of tokens.
- More context doesn't always mean better answers—it can introduce unnecessary information and reduce focus.
- The middle gets relatively ignored — this is the root cause of needing features like /compact and is referred to in the course as "context rot."
- **Solution:** Give each agent only the context it needs.

### 2. Not Every Task Needs Maximum Intelligence

- Simple tasks (emails, formatting, file renaming) don't require the most capable or expensive model.
- Reserve powerful models for high-value tasks like architecture, debugging, or planning.
- **Solution:** Use smaller/simpler agents for routine work.

### 3. Specialization

- A specialist usually performs better than a generalist on a specific task.
- Create dedicated agents for coding, testing, research, documentation, etc., each optimized for its role.

# The Real Technical Problem: Context & Cost

When Claude analyzes a project, it may read **tens of thousands of tokens** before answering.

Example:

```other
Codebase: 50,000 tokens
Chat history: 20,000 tokens
New prompt: 20 tokens
--------------------------
Total: 70,020 tokens
```

Every follow-up message keeps adding to the total context, increasing **cost, latency, and token usage**.

# Statelessness

LLMs are **stateless**—they don't remember previous conversations.

To simulate memory, the chat application **re-sends the entire conversation history** with every new message.

```other
Previous conversation
+ New message
= Context sent to the model
```

As the chat grows, every request becomes larger and more expensive.

# What is a Sub-Agent?

A **sub-agent** is a **separate AI session** that the main agent delegates a specific task to. Think of it as a **mini Claude instance** with its own independent environment.

Each sub-agent has:

- 🧠 **Its own model** (Haiku, Sonnet, Opus, etc.)
- 📝 **Its own system prompt/role**
- 🛠️ **Its own tools and hooks**
- 🎯 **Its own skills**
- 📦 **Its own context window (session)**

Unlike the main agent, it works **independently**, completes the assigned task, returns a **summary/result**, and then its context is discarded.

## Why is this useful?

Suppose you ask:

> "Read 500 emails and summarize them."

### Without a Sub-Agent

- Main agent reads all 500 emails.
- Hundreds of thousands of tokens fill the main context.
- Future conversations become slower and more expensive.

### With a Sub-Agent

- Sub-agent reads all 500 emails in **its own context**.
- Returns only a short summary (e.g., 2,000 tokens).
- Main agent keeps a clean context.

**Result:** Lower token usage and lower cost.

# Benefits of Sub-Agents

### 1. Context Isolation

- Each sub-agent has its own context window.
- Heavy tasks don't clutter the main conversation.

### 2. Parallel Execution

- Multiple sub-agents can work simultaneously on different tasks.
- Example: One reviews code, another writes tests, another checks quality.
- **Note:** They should work on separate tasks/files since they don't share context.

### 3. Specialization & Modularity

- Each sub-agent can be optimized for a specific job.
- Different models can be used based on task complexity (e.g., Haiku for simple tasks, Opus for complex reasoning).
- Large workflows are broken into smaller, manageable components.

### 4. Controlled Access

- You can limit which tools and permissions a sub-agent has.
- Example: Give a code-review agent read-only access instead of full project permissions.

### 5. Cost Optimization

- Route simple tasks to cheaper/faster models.
- Use powerful models only when necessary, reducing overall cost.

# Why code review and test-writing specifically need a SEPARATE agent

Key insight: if you ask the SAME Claude session that just wrote the code to also review or critique that code, it will be biased — it already built it with a certain mindset and will tend to defend its own choices.

> *It's like asking a book author to review their own book. He will of course have a bias, because he created it with a certain mindset. He'll say 'this is the right way' — but you're still better off asking other people for feedback, because they're a better judge of it.*

This is why a separate sub-agent (fresh context, no attachment to the original work) is used for code review, security checks, and test writing — it can critique honestly instead of rationalizing the original author's choices.

# Ways to Invoke a Sub-Agent

<img width="2322" height="1312" alt="image" src="https://github.com/user-attachments/assets/a2bc5fb9-68e5-487d-9ee0-0c22bcd69ea6" />

There are **3 ways** to call a sub-agent, each offering a different level of control.

| **Method**              | **Example**                                           | **Reliability**            |
| ----------------------- | ----------------------------------------------------- | -------------------------- |
| **1. Natural Language** | *"Please explore my codebase."*                       | ⭐ Low                      |
| **2. Named Request**    | *"Use my `code-reviewer` agent to review this code."* | ⭐⭐⭐ Medium                 |
| **3. @ Mention**        | *"Explore the codebase using `@explorer`."*           | ⭐⭐⭐⭐⭐ Highest (Guaranteed) |

# Anatomy of an Agent File (agents.md)

## Where it lives

```other
.claude/
    └── agents/
          └── my-agent.md
```

# Creating a Custom Sub-Agent (`/agents`)

The instructor recommends **using Claude Code's built-in agent generator** for your first few agents instead of writing the Markdown file manually. This helps you learn the proper structure.

## Steps

1. Run `/agents`.
2. Select **Library → Create New Agent**.
3. Choose the scope:
    - **Project** → Available only in the current project (`.claude/agents/`)
    - **Personal/User** → Available across all projects
1. Choose **Generate with Claude**.
2. Describe your agent in plain English.
3. Select the agent's **tools**.
4. Choose the **model** (Haiku, Sonnet, Opus, or inherit from parent).
5. Configure **memory scope**.
6. Review the generated system prompt.
7. Press **S** to save.

# Prompt Used in the Demo

```other
Please help me create an agent which can improve the quality of my code.The agent should act as an SDE-3 who is well aware about the best practices and also make sure that it is not unnecessarily bloating the code but rather giving the right advice.The whole aim of the agent is to go through the code and then provide advice on how the code can be improved.
```

# Tool Selection

The instructor followed the **Principle of Least Privilege**.

- ✅ Enabled only the tools needed.
- ✅ Mostly **read-only access** (reviewer doesn't need to edit code).
- ✅ Enabled a few execution tools (e.g., linters/tests if needed).
- ❌ No unnecessary write permissions.
- ❌ No MCP tools for this agent.

# Important: Review the Generated Agent

**Don't blindly trust the generated configuration.**

The instructor recommends copying the generated agent into another AI (e.g., ChatGPT or another Claude chat) for review.

**Review Prompt**

```other
Please go through my Claude Code sub-agent and correct anything if required.Give me the final file in a copyable code block.
```

## Parallel Agents

You can run multiple sub-agents **simultaneously** to speed up independent tasks.

**Example Prompt**

```other
Run the test writer agent and quality improver agent in parallel.
```

### Benefits

- Multiple agents work at the same time.
- Reduces overall execution time.
- Best when tasks are **independent** (avoid editing the same files).

> **Note:** Some visualization tools may not display all parallel agents correctly, even though they are running.

## Background Agents

Setting **`background: true`** allows an agent to run in the background while the main agent continues working.

**Example**

```other
Use @code-quality-sub-agent on the /prisma folder,and in parallel create five test users.
```

Result:

- Background agent reviews the `/prisma` folder.
- Main agent creates test users simultaneously.
- Both tasks finish independently.

**Use case:** Long-running tasks like code reviews, indexing, or analysis.

## Stopping Agents

⚠️ **Important:** Stopping the main session (**Ctrl+C**) **does not automatically stop** its sub-agents.

If you have:

- Main agent
- Agent A
- Agent B

You may need to stop **each one separately**.

**Tip:** If multiple agents are running, expect multiple stop commands.

# Agent Memory

By default, **sub-agents start with a fresh context** every time.

This keeps them focused and avoids carrying unnecessary information.

## When Memory Helps

Enable memory when an agent should remember previous learnings.

Example:

- An email agent learns to ignore promotional emails.
- On future runs, it remembers this preference without being told again.

# Avoid Overusing Parallel Agents

<img width="2762" height="1642" alt="image" src="https://github.com/user-attachments/assets/f79fd788-3532-46a5-b8d8-afeb2517848e" />

⚠️ **Just because you can run agents in parallel doesn't mean you should.**

The instructor warns that many people overuse parallel agents, assuming they'll always produce better results. In reality, **parallelism only works when tasks are independent**.

## Why Over-Parallelization is Risky

If an existing bug or incorrect assumption exists:

```other
Bug
   ↓
Agent A
Agent B
Agent C
...
```

Every agent may build on the same mistake, multiplying the problem instead of fixing it.

## Never Let Two Agents Edit the Same File

🚫 **Hard Rule:** Don't have multiple parallel agents modify the same file.

Since sub-agents don't share context, they are unaware of each other's changes, leading to:

- File conflicts
- Overwritten changes
- Inconsistent code

**Instead:**

- Run them **sequentially**, or
- Use an **Agent Team**, where agents can coordinate.

# 17. Full agents.md Configuration Field Reference

<img width="840" height="564" alt="image" src="https://github.com/user-attachments/assets/ca8fbcee-68be-4530-8ad6-cad7d834299f" />

| **Field**        | **Description**                                                                                                                                                                                                                |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name             | Required unique identifier.                                                                                                                                                                                                    |
| description      | What the agent does; drives natural-language auto-invocation.                                                                                                                                                                  |
| tools            | Which tools the agent may use.                                                                                                                                                                                                 |
| disallowed-tools | Explicitly blocked tools — e.g., blocking write/edit so a reviewer can never accidentally modify files ("you cannot see my bank details, you cannot see my emails, you cannot go through my WhatsApp" — explicit denial list). |
| model            | Model used by this agent (e.g., claude-haiku). Can also be "inherit" to use the parent's model.                                                                                                                                |
| permission-mode  | Controls how permissions are handled: default / accept-edits / auto (don't ask) / bypass-permission / plan.                                                                                                                    |
| max-turns        | Maximum number of agent turns before the sub-agent automatically stops.                                                                                                                                                        |
| skills           | Skills to load into the sub-agent's context at startup — sub-agents CAN use skills.                                                                                                                                            |
| mcp-servers      | Scopes which MCP servers this specific sub-agent has access to.                                                                                                                                                                |
| hooks            | Conditional rules — e.g., a pre-tool-use hook that blocks a Bash command matching a delete pattern, even if the agent technically has write access (a safety net).                                                             |
| memory           | Enables persistent memory for the sub-agent (see Section 21).                                                                                                                                                                  |
| background       | Set to true to always run this sub-agent as a background task (see Section 20).                                                                                                                                                |
| isolation        | Set to 'worktree' to run the sub-agent in a temporary git worktree, isolating its filesystem changes.                                                                                                                          |
| color / icon     | Visual identification in the UI (e.g., blue for a code reviewer, could use red for a 'critical' agent, green for a 'quality' agent).                                                                                           |
| initial-prompt   | Auto-submitted as the first user turn when this agent is run as the main session agent.                                                                                                                                        |
| version          | For your own tracking purposes.                                                                                                                                                                                                |

## Markdown body ("the brain")

Everything below the front matter defines the agent's actual behavior, knowledge, and style:

|                           |                                                                                   |
| ------------------------- | --------------------------------------------------------------------------------- |
| **Section**               | **Purpose**                                                                       |
| \# Title (recommended)    | Name of the agent — used for clarity in lists and conversations.                  |
| ## Goals                  | What the agent is here to achieve. Keep it short and outcome-focused.             |
| ## Instructions           | Step-by-step guidance for HOW the agent should think and act.                     |
| ## Constraints            | Rules and guardrails the agent must follow — keeps behavior safe and consistent.  |
| ## Examples (recommended) | Shows good inputs/outputs or workflows — helps the agent understand expectations. |
| ## Resources (optional)   | Links, docs, patterns, or references the agent can rely on.                       |
