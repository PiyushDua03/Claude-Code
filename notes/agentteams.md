#  📦 Claude Code Agent Teams

<img width="2470" height="1484" alt="image" src="https://github.com/user-attachments/assets/fa93024c-08e6-499f-b98f-801e559a5cfd" />

# What Agent Teams changes?

 Agent Teams introduces a lead + teammates structure where: 

1. A task list is created first. 

2. Multiple agents are spawned (each one conceptually like its own Claude Code session). 

3. These agents work on tasks for real, and — critically — they can talk to each other directly, not just report to the lead.

<img width="2650" height="1070" alt="image" src="https://github.com/user-attachments/assets/397905f7-9f88-49fa-b3e6-9db7438ec6da" />

### Prompted directly inside Claude Code: 

> *"Enable agent teams in this folder".*

### What happens: 

- Claude Code creates/updates a **settings.json** file. 
- It may ask whether you want this enabled at the user level or scoped to this project/folder only. 
- Confirming "yes" results in a config update, e.g. a flag like:

    `claude code experimental agent teams: enabled`

# Git Worktree — The Mechanism Behind Safe Parallel Work 

This is presented as a separate,foundational concept — not unique to Claude orAI at all, but a built-in Git feature that Agent Teams (and manual parallel sessions) lean on. 

<img width="2284" height="1268" alt="image" src="https://github.com/user-attachments/assets/1ebdb128-7de3-45bc-833a-855958d1f4d6" />

### What is git worktree ? 

A built-in Git feature that allows you to have multiple branches checked out simultaneously, in different directories, all linked to a single git repository. 

### Why it matters here 

- When multiple agents (teammates) work "together," they are not literally editing the same files in the same folder at the same time (that would guarantee conflicts). Instead: 
- The team lead's directory is the main/source-of-truth copy. Each agent gets its own copy (via worktree) — e.g.: 
    - Agent 1 → its own working copy 
    - Agent 2 → its own working copy 
    - Agent 3 → its own working copy 
- Crucially, this is NOT like manually duplicating the whole project folder three times. A worktree copy still references the same underlying Git repository/objects — it's lightweight, not a full duplicate. 
- Each agent works in isolation in its own checked-out branch/worktree. 
- When work is done, branches are merged back into the main line. 

### Analogy used 

> *Just like teammates don't all work on the same physical laptop — each person has their own machine/context, and when changes are ready, they say "I've made this change, please pull," and it gets merged in.* 

### Key benefit vs. plain branches 

With a simple branch + manual pull, if someone updates the main branch (e.g., fixes a typo) while you're mid-work on your branch, you have to manually go back and pull those changes yourself. 

### With git worktree , the benefit is: 

- Updates can propagate automatically in a more integrated way. 
- It is very safe and very space-efficient — because a worktree mostly just references
