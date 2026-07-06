# 📦 Claude Code Plugins

> A **Claude Code Plugin** is an installable package that bundles multiple Claude Code components (agents, skills, hooks, MCP configurations, settings, etc.) into a reusable toolkit that can be shared, versioned, and installed by individuals or teams.

<img width="1138" height="693" alt="Image" src="https://github.com/user-attachments/assets/d46963f9-d1ee-4c91-babd-e43cc680abde" />

# Why Were Plugins Introduced?

Before plugins, if you wanted to share your Claude Code setup with another developer, you had to manually copy many files.

```other
.claude/
│
├── agents/
├── skills/
├── hooks/
├── settings.json
├── CLAUDE.md
└── mcp.json
```

Imagine a new developer joins your company.

You would have to send:

- agents
- skills
- hooks
- CLAUDE.md
- settings.json
- MCP configuration

This is difficult to manage, update, and maintain.

### Problem

❌ Multiple folders to share

❌ Manual setup

❌ Different AI behaviour for everyone

❌ Difficult to maintain versions

This is the problem plugins solve.

---

<img width="1154" height="670" alt="Image" src="https://github.com/user-attachments/assets/972096d5-468e-4ddc-8c60-076aa3567049" />

# What is a Plugin?

Instead of sharing many folders...

Bundle everything into **one installable package**.

```other
QuickTask Plugin
│
├── Agents
├── Skills
├── Hooks
├── Settings
├── MCP Config
└── Metadata
```

Now another developer only installs

```other
QuickTask Plugin
```

Everything is configured automatically.

# Relationship with the `.claude` Folder

Everything you've created throughout the course already exists inside the `.claude` folder.

A plugin is simply a packaged version of this folder.

---

# Plugin Directory Structure

A plugin generally looks like:

```other
my-plugin/
│
├── .claude-plugin/
│      └── claude-plugin.json
│
├── agents/
├── hooks/
├── skills/
├── CLAUDE.md
└── settings.json
```

Notice that the structure is almost identical to the `.claude` folder.

The only major addition is the **plugin manifest**.

---

<img width="1200" height="705" alt="image" src="https://github.com/user-attachments/assets/42635d6c-2379-4f22-abe7-931a536a02ae" />

# Plugin Manifest

The manifest is a configuration file that describes the plugin.

It stores information like

- Plugin Name
- Version
- Description
- Author
- Configuration

Example

```other
{
   "name": "quick-task",
   "version": "1.0",
   "description": "Task Management Toolkit"
  }
```

### Important

The manifest is **optional**.

If it is missing,

Claude automatically discovers

- agents
- hooks
- skills

inside the default locations.

---

## Create Plugins When

- Sharing with teammates
- Sharing across your company
- Publishing to the community
- Maintaining versions
- Combining multiple related components

Plugins become valuable once they are meant to be reused.

---

# Plugin Marketplace

Just like

- VS Code Extensions
- Chrome Extensions

Claude also has plugin marketplaces.

Instead of creating everything yourself,

search

↓

install

↓

use.

Many official plugins already exist.

Examples include

- Context7
- Code Review
- Frontend tools
- Skill Creator

---

# Context7 Plugin

- Context7 is one of the official plugins demonstrated.
- Provide Claude with the latest documentation.

Example

```other
User
↓
Show latest Supabase SDK
↓
Context7
↓
Fetch latest docs
↓
Claude answers
```

This makes responses much more accurate for rapidly changing libraries.

---

# Community Plugins

Besides official plugins,

there are community-created plugins.

Examples

- GitHub repositories
- Third-party marketplaces

### Important

Community plugins are **not reviewed by Anthropic**.

Always

- inspect the code
- test safely
- verify the source

before installing.

---

# Creating Your Own Plugin

Claude can generate a plugin from your existing `.claude` folder.

Example prompt

```other
Create a Claude Code plugin\
from my .claude folder
for team sharing.
```

Claude automatically

- reads your agents
- reads your hooks
- reads your skills
- creates plugin structure
- generates plugin manifest
- prepares installation

Very little manual work is required.

---

# Sharing Plugins with Git

Store plugins inside Git.

```other
Developer
↓
git pull
↓
Latest Plugin
↓
Updated Agents
Updated Hooks
Updated Skills
```

Benefits

- Version control
- Easy updates
- Easy rollback
- Automatic onboarding

New developers simply clone the repository.

---

# LSP Plugin (Language Server Protocol)

One of the most powerful plugin types discussed.

Without LSP

Claude mostly reads source code as plain text.

```other
def create_app():
```

It has to infer relationships itself.

---

With LSP

Claude receives semantic information directly from the language server.

Capabilities

- Go to Definition
- Find References
- Type Checking
- Auto Completion
- Symbol Search
- Hover Documentation
- Detect Missing Imports

Instead of guessing,

Claude actually understands the codebase.

This is especially useful for large Python or TypeScript projects.
