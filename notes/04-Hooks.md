# What is a Hook?

A **Hook** is a piece of code (usually a Python script, Bash script, or command) that automatically runs when a specific event occurs.

Think of it as:

```other
Event Occurs
      ↓
Hook Runs
      ↓
Action Happens Automatically
```

### Simple Example

Event:

```other
A Python file is created
```

Hook:

```other
auto-format.py
```

Action:

```other
Format the Python code automatically
```

You do not need to manually run the formatter. The hook does it for you.

<img width="625" height="373" alt="image" src="https://github.com/user-attachments/assets/01e42089-f3a4-427d-b563-c5bb19a3cc2f" />

# Why are Hooks Independent of LLMs?

Hooks are simply:

```other
Event + Script + Action
```

They do NOT require:

- Claude
- GPT
- Gemini
- Any other AI model

to function.

That is why hooks are called **LLM-independent**.

hooks are not good at:

```other
✗ Understand business requirements
✗ Design software architecture
✗ Explain concepts
✗ Review code intelligently
✗ Generate proposals
```

Those tasks require reasoning.

Reasoning is where Claude, GPT, or other LLMs are used.

# Why Hooks Use Zero Tokens

Hooks are normal scripts running on your computer.

```other
Token Usage = 0
```

### Why?

Because tokens are only consumed when an LLM processes text.

# When Can a Hook Use Tokens?

However, a hook CAN use tokens if the hook itself calls an LLM.

Example:

```other
Event
      ↓
Hook Runs
      ↓
Hook sends request to Claude API
      ↓
Claude responds
```

Now tokens are used because an AI model was invoked.

---

# Why Hooks Exist

Hooks are mainly used for:

### 1. Automation

Automatically perform tasks.

Examples:

```other
File created
→ Format code

Code changed
→ Run tests

Task completed
→ Update changelog
```

---

### 2. Logging

Record everything happening in the project.

Examples:

```other
Claude edited file X

Claude executed command Y

Claude created folder Z
```

Useful for:

        - Auditing
        - Debugging
        - Team environments

---

    ### 3. Guardrails

        Prevent dangerous actions.

        Examples:

```other
Block rm -rf

Prevent deleting production files

Prevent API key exposure

Prevent pushing secrets to GitHub
```

        Think:

```other
Hook = Safety Net
```

---

# Hook Lifecycle in Claude Code

A typical flow:

```other
You Type Prompt
        ↓
Submit
        ↓
PreTool Hook
        ↓
Tool Executes
        ↓
PostTool Hook
        ↓
Success / Failure
        ↓
Stop Hook
```

Hooks act as checkpoints around Claude's actions

<img width="399" height="890" alt="image" src="https://github.com/user-attachments/assets/b2d037b5-57f8-4ee4-84de-b1044a3d55fd" />

# Major Hook Events

## 0. Submit Hook

Runs when user submits prompt.

Purpose:

```other
Validate prompt
Log prompt
Modify prompt
```

Example:

```other
Record all user requests
```

---

## 1. PreTool Hook

Runs BEFORE Claude uses a tool.

Most powerful hook.

Purpose:

```other
Validate
Control
Block
```

Example:

Claude wants to run:

```other
rm -rf project/
```

Hook intercepts:

```other
PreTool
↓
Check command
↓
Dangerous?
↓
BLOCK
```

Tool never runs.

---

## 2. Tool Execution

Claude performs:

```other
Read
Write
Edit
Bash
Search
```

This is where actual work happens.

---

## 3. PostTool Hook

Runs AFTER successful execution.

Purpose:

```other
Format
Validate
Clean up
```

Example:

Claude creates:

```other
app.py
```

PostTool:

```other
auto-format.py
```

Result:

```other
Code automatically formatted
```

---

## 4. Fail Hook

Runs when tool execution fails.

Purpose:

```other
Log errors
Alert user
Retry operation
```

Example:

```other
npm install failed
```

Hook:

```other
Log failure details
```

---

## 5. Stop Hook

Runs when Claude finishes.

Purpose:

```other
Notifications
Logging
Cleanup
```

Example:

```other
Task completed
↓
Send desktop notification
```



---

# Hook Exit Codes

Hooks return status codes.

These codes tell Claude what to do.

<img width="836" height="505" alt="image" src="https://github.com/user-attachments/assets/ae7d6d5c-82ac-48a4-a1e8-2ab5fb9819f1" />

## Exit 0 (Allow)

Meaning:

```other
Everything is okay
Continue
```

Example:

```other
Formatting successful
```

Result:

```other
Action allowed
```

---

## Exit 1 (Warning)

Meaning:

```other
Something went wrong
BUT
Do not block
```

Example:

```other
Formatter missing
```

Result:

```other
Warning shown
Continue execution
```

---

## Exit 2 (Block)

Most important.

Meaning:

```other
STOP IMMEDIATELY
```

Think:

```other
Emergency Brake
```

Example:

```other
rm -rf important-folder
```

Hook:

```other
Detected dangerous command
Exit 2
```

Result:

```other
Command blocked
```

Nothing executes.

---

# Complete Hook System Example

Imagine these four hooks:

## Security Hook

PreTool

```other
Block dangerous commands

rm -rf
sudo operations
secret exposure
```

Role:

```other
Security Guard
```

## Format Hook

PostTool

```other
Run Black
Run Prettier
Run ESLint
```

Role:

```other
Code Cleaner
```

## Notify Hook

Stop

```other
Task complete
Send notification
```

Role:

```other
Personal Assistant
```

## Log Hook

Async

```other
Record every action
```

Role:

```other
Auditor
```

<img width="787" height="494" alt="image" src="https://github.com/user-attachments/assets/a741be93-6492-45f0-8afd-9904dc01f08e" />

---

# Example Hooks:

- # block-rm.sh

<img width="1261" height="419" alt="image" src="https://github.com/user-attachments/assets/82116ac0-5cb5-42dc-85cd-6590ba102804" />

    Your instructor demonstrated a hook similar to:

```other
block-rm.sh
```

    Purpose:

```other
Protect important folders from deletion
```

    ### Workflow

    Event:

```other
rm -rf important-folder
```

    User attempts to delete a folder.

    Hook intercepts the command.

    Checks:

```other
Is this folder protected?
```

    If yes:

```other
Deletion blocked
```

    Output:

```other
ERROR:
Protected folder cannot be deleted.
```

    ### Why is this useful?

    Without hook:

```other
Accidental deletion
      ↓
Project lost
```

    With hook:

```other
Accidental deletion
      ↓
Hook catches it
      ↓
Operation blocked
```

    No AI is required.

    No tokens are used.

- # auto-format.py

    Purpose:

```other
Automatically format Python files
```

    ### Workflow

    Event:

```other
Claude creates a Python file
```

    Hook:

```other
auto-format.py
```

    Action:

```other
Run formatter
Fix indentation
Fix spacing
Apply coding style
```

    Result:

```other
Clean and consistent code
```

    ### Why is this useful?

    Without hook:

```other
Create file
Manually format code
```

    With hook:

```other
Create file
↓
Hook runs automatically
↓
Code formatted
```

    Again:

```other
No AI
No Tokens
```

---

# Hook vs Skill

## Hook

Answers:

```other
WHEN should something happen automatically?
```

Example:

```other
After file creation
→ Format code
```

## Skill

Answers:

```other
HOW should a task be performed?
```

Example:

```other
Code Review Skill
AI Audit Skill
SEO Audit Skill
```

---

<img width="1049" height="605" alt="image" src="https://github.com/user-attachments/assets/700c022a-9c32-4c87-ac13-adaa3bdc735e" />
