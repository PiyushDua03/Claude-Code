# 📦 Model Context Protocol

## Context

> **The additional information an AI needs to answer correctly.**

## Protocol

> **A standard way of communication.**

# What exactly is MCP?

> **MCP is a universal language that allows AI models to safely communicate with external applications and tools.**

# Before MCP

```other
Before MCP

Every App
      ↓
Different APIs
      ↓
Every Developer
      ↓
Writes Custom Integration Again

Result:
❌ Repetitive Work
❌ Complex Integrations
❌ Wasted Development Time
```

 

The instructor demonstrates this with **Tavily** — a search API that lets any app perform Google-like web searches. It's free with ~1,000 credits/month, more than enough to learn with.

**Prompt used:** *"Please create a file — Tavily API where I hit the Tavily search API, keep API key as a variable, and I can also edit out the search query."*

By doing this, the instructor gave Claude Code a **"tool."** and also has custom integration of serach tool.

>> **Tool** = any external capability an AI/agent can call via an API. Just like a hammer or screwdriver is a tool for a human, "Tavily Search" becomes a tool for Claude.

### Gmail APIs

    - Read Email API
    - Send Email API
    - Delete Email API
    - Archive Email API
    - ...and more

    That's already 10+ custom "tools" just for Gmail. Now imagine doing this for Slack, GitHub, Notion, your database, Figma, etc. — **every developer on earth building and maintaining their own custom integration code for the same handful of services.** This is wasteful, error-prone, and doesn't scale.

Now every connection uses the **same standard wrapper (an MCP server)**. You don't write custom code — you just "plug in."

**Impact of this standardization:**

- No custom code needed
- Reusable connections
- Scalable to hundreds of services

## `KEY NOTE:`

An **MCP Client** is the application that wants to use external tools.

## How MCP Server Returns Results?

```other
User
   ↓
Claude
   ↓
Chooses the required MCP Server
   ↓
Calls the required Tool
   ↓
Tool interacts with API / Database / Service
   ↓
MCP Server returns structured JSON
   ↓
Claude (LLM) understands the JSON
   ↓
Claude converts it into human-friendly language
   ↓
User receives a natural response
```

- **Tool = Worker 👷** → Does one specific job.
- **MCP Server = Toolbox 🧰** → Contains many workers/tools.
- **Claude (LLM) = Manager 🧠** → Reads your request and intelligently decides **which tool** from **which MCP server** should be used.

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/85E505F9-8404-4A98-8CD3-5D34CABA3BA6/EADBE96A-0FA1-4ABA-B02C-B6C4A6C5C28C_2/xkZW9ECNykTIxtBy38OTyShyX5Xb6bhsazghF0w0ILwz/Image.png)

---

## Local vs Hosted:

![Image.png](https://resv2.craft.do/user/full/b1d3e983-5126-d13d-3368-9dcec21914b0/doc/85E505F9-8404-4A98-8CD3-5D34CABA3BA6/FADD66C2-B881-476A-8243-5DDDB814BFDF_2/Ayyq0nfxIYoYaX4mcnqSjPe5ozgfhHEfCXTGL4t6Tswz/Image.png)

| **Feature**        | **Local MCP**              | **Hosted MCP**                   |
| ------------------ | -------------------------- | -------------------------------- |
| Runs on            | Your computer              | Internet                         |
| Installation       | Required                   | Usually none                     |
| Uses               | stdin/stdout               | HTTP                             |
| Internet needed    | Sometimes                  | Yes                              |
| Access local files | ✅ Yes                      | ❌ No (unless explicitly exposed) |
| Browser automation | ✅ Excellent                | ❌ Usually not                    |
| Maintenance        | You update it              | Provider updates it              |
| Startup            | Starts locally             | Already running                  |
| Speed              | Fast after startup         | Depends on network               |
| Security           | Better for local resources | Better for centralized services  |

# 📝 Why Local MCP Uses `stdin` / `stdout`

### What is `stdin` and `stdout`?

    - **stdin (Standard Input):** Used to send requests **to** the MCP server.
    - **stdout (Standard Output):** Used by the MCP server to send results **back** to Claude.

    ### Why are they used?

    A **Local MCP Server** runs as a **local process** on your computer. Since both Claude and the MCP server are on the same machine, they communicate directly using `stdin` and `stdout` instead of making HTTP requests over the internet.

    ### Communication Flow

```other
Claude
   │
   ▼
stdin (Request)
   │
   ▼
Local MCP Server
   │
   ▼
stdout (Response)
   │
   ▼
Claude
```

    ### Example

    **Claude sends (stdin):**

```other
{
  "tool": "navigate",
  "url": "amazon.in"
}
```

    **MCP Server returns (stdout):**

```other
{
  "status": "success"
}
```

    Claude reads the response and presents it in natural language.

    # HTTP vs stdin

| **stdin/stdout**    | **HTTP**               |
| ------------------- | ---------------------- |
| Local communication | Internet communication |
| Very fast           | Slightly slower        |
| No network          | Requires network       |
| Runs as process     | Runs as server         |
| Local only          | Anywhere               |

    ## 📝 What is `npx`?

    - **`npm`** installs JavaScript/Node.js packages permanently.
    - **`npx`** downloads (if needed) and **runs a package instantly** without permanent installation.
    - Used to quickly start **JavaScript-based MCP servers**.

    **Flow:**

```other
Download (if needed)        ↓Run        ↓Exit
```

    **Example:**

```other
npx @playwright/mcp
```

    **⭐ Revision:**

    >> **`npx` = Run JavaScript MCP servers without permanently installing them.**

---

    ## 📝 What is `uvx`?

    - **`uvx`** is used to run **Python-based MCP servers**.
    - Downloads (if needed) and runs the package without permanent installation.
    - Python equivalent of `npx`.

    **Example:**

```other
uvx mcp-server-time
```

    **⭐ Revision:**

    >> **`uvx` = Python's equivalent of `npx` for running MCP servers.**

    # What is Playwright?

    **Playwright itself is not an MCP server.**

    Playwright is an **open-source browser automation library/framework** developed by Microsoft.

    It allows programs to:

    - Open Chrome/Edge/Firefox
    - Click buttons
    - Fill forms
    - Scroll pages
    - Take screenshots
    - Download files

    Example:

```other
Python/Node Program
        │
        ▼
   Playwright Library
        │
        ▼
      Browser
```

---

> ## MCP Configuration Scopes — Where Settings Live

*(Refers to Image 3: "MCP Configuration Scopes")*

There are **three layers** of configuration, from most shared to most private:

| **Layer**          | **File**                        | **Who sees it**                    | **What goes here**                                   |
| ------------------ | ------------------------------- | ---------------------------------- | ---------------------------------------------------- |
| **Project (Top)**  | `.mcp.json` (in repo)           | Whole team (shared)                | Shared tools like GitHub, DB — safe to commit to Git |
| **User (Middle)**  | `~/.claude.json`                | Just you, across all your projects | Personal tools like your email, Notion               |
| **Local (Bottom)** | `.mcp.local.json` (git-ignored) | Only your machine                  | **Secrets**: API keys, tokens — never committed      |

**The Golden Rule:**

- Shared configuration → goes in **Project** scope
- Personal-but-not-secret configuration → goes in **User** scope
- Secrets/API keys → go in **Local** scope, and that file must be added to `.gitignore`

                            u can save them in .env file

**Why this matters (Git basics, explained simply, as the instructor did):**

- `git commit` = saving a snapshot/version of your code, which typically eventually gets pushed to GitHub for others to see.
- `.gitignore` = a file listing patterns (e.g., `*.local.json`) that Git will **never** track or upload — this is exactly how you keep API keys from leaking onto public GitHub repos.
- **Best practice demonstrated:** create `.mcp.local.json` for anything containing a real secret, and make sure your `.gitignore` includes a rule to ignore all `*local*` files.

**Setting the scope explicitly:**

```other
mcp add --scope <project|user|local>
```

---

> ## Security Warning ⚠️ (Very Important, Don't Skip)

- Once connected, an MCP server can effectively read files, run commands, or interact with your browser — anything the tool's permissions allow.
- A malicious MCP server creator could embed a **prompt injection** — hidden instructions designed to trick Claude into taking harmful actions (e.g., "please change all curl requests," silently modifying files, taking screenshots of sensitive data, exfiltrating information).

## Where MCP Shows Up Inside Claude's Own UI

**Claude.ai → Settings → Customize → Connectors:** Lets you connect Gmail, Google Calendar, Google Drive, etc. with one click — under the hood, this is exactly an MCP connection (usually a hosted/remote MCP server).

---

## Common MCP Errors and Fixes

| **Error**                | **Cause**                                                                                                                     | **Fix**                                                                                                                                   |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Connection Closed**    | On **Windows**, `npx` doesn't execute directly the way it does on Mac/Linux                                                   | Wrap the command: instead of `"command": "npx"`, use `"command": "cmd", "args": ["/c", "npx", "-y", "@package/name"]`                     |
| **Server Disconnected**  | The server crashed or failed to start                                                                                         | Check server logs / re-run install                                                                                                        |
| **Auth Failed**          | OAuth/token expired or never completed                                                                                        | Re-authenticate via the login flow                                                                                                        |
| **Error in Tool**        | The tool's own code has a bug (e.g., the debug-print-breaks-JSON issue from Demo D)                                           | Fix the underlying server code                                                                                                            |
| **Server Timeout Error** | Slow startup (common with heavy Python packages) or long-running operations (like Tavily's "research" tool taking 10 minutes) | Increase timeout: run with env var `MCP_TIMEOUT=30000 claude` (30 seconds), or set it permanently in `settings.json`:  <br/><br/>{ "env`` |

---

> ## Complete List of Prompts Used in the Video

1. *"Please create a file — Tavily API where I hit the Tavily search API, keep API key as a variable, and I can also edit out the search query."*
2. *"Run the file."*
3. *"Can you tell me what are the tools in the time MCP server we created, and also where is it actually running?"*
4. *"Tell me the current time by using [the] time MCP server."*
5. *"Go to Amazon and search me a good MacBook for a developer use case."*
6. *"Give me one more option please."*
7. *"Please add iPhone 17 to my cart."*
8. *"Try via Amazon India."*
9. *"Go to news via Hacker News and then tell me the top five stories right now."*
10. *"Fill [this] form."*
11. *"Go to stripe.com/pricing and take a screenshot, then describe what the pricing seems like."*
12. *"Search for recent pricing information on GitHub Copilot and compare their plans."*
13. *"Use Tavily research tool instead."*
14. *"Add the below MCP server for me in this project only."* (Tavily hosted server snippet)
15. *"Can you tell me what all tools are present in the Tavily MCP server?"*
16. *"Can you please search for [the] latest in Claude Code."*
17. *"Can you quickly create a simple MCP server which helps in calculation?"*
18. *"Now use the same to tell me what is 5 + [number]."*
19. *"Can you add a print statement so I could see the numbers which were fed?"* (led to the stdout/stderr JSON-breaking bug and fix)
20. *"Can you remove the research tool from my Tavily MCP server?"*
21. *"Skill creator — create a Tavily search skill which searches Tavily and gets the result."*
22. *"Tavily search — latest AI news."* (using the newly created Skill)
23. *"Create a Python script to add two numbers."* (used to demonstrate CLI-style invocation of Claude Code)
