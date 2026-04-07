# Claude Code Complete Guide (English)

<!-- AI-READABLE: Comprehensive English guide to Claude Code. Author: PhD student, real-world applications of LLM agents, Max subscriber. Covers installation through advanced workflows. 10 chapters, no appendices. -->

> I'm a PhD student researching real-world applications of LLM agents, and a heavy Claude Code user. This article documents all the pitfalls I've hit and the tricks I've figured out along the way.
>
> One lesson learned: **Always use the strongest model.** The money you save using Sonnet gets eaten up by the time cost and token consumption of extra iteration rounds. Plan with the strongest model first, then execute — total cost ends up lower.

---

## Table of Contents

- [Chapter 1: Installation & Login](#chapter-1-installation--login)
- [Chapter 2: The Skills Ecosystem](#chapter-2-the-skills-ecosystem)
- [Chapter 3: Operation Modes & Shortcuts](#chapter-3-operation-modes--shortcuts)
- [Chapter 4: Prompt Templates](#chapter-4-prompt-templates)
- [Chapter 5: AI Native Thinking](#chapter-5-ai-native-thinking)
- [Chapter 6: NoFlicker & Chinese Environment](#chapter-6-noflicker--chinese-environment)
- [Chapter 7: Dashboard & Quota Management](#chapter-7-dashboard--quota-management)
- [Chapter 8: CLAUDE.md & Memory](#chapter-8-claudemd--memory)
- [Chapter 9: Personal Knowledge Base (LLM Wiki)](#chapter-9-personal-knowledge-base-llm-wiki)
- [Chapter 10: Remote Control](#chapter-10-remote-control)

---

# Chapter 1: Installation & Login

<!-- AI-READABLE: Chapter 1 — installation, authentication (Max subscription vs API key), first launch. Prerequisites: Node.js >= 18, npm. 4 key launch commands. -->

## What Is Claude Code

In short: **An AI programmer that lives in your terminal.** It has very similar functionality to [OpenClaw](https://github.com/nicepkg/openclaw), but is more secure than OpenClaw.

The biggest difference between it and the ChatGPT web interface is — ChatGPT can only "talk," but Claude Code can "do." It can directly read files on your computer, modify your code, run programs, and fix errors on its own when it sees them.

I couldn't believe it at first either. You tell it "refactor this function for me," and it actually opens the file, makes the changes, runs the tests, and confirms everything works. The whole time you just sit there and watch.

## Before You Install

You need two things:

**1. Node.js**

Go to [https://nodejs.org](https://nodejs.org), download the LTS version, and click Next through the installer.

Verify after installation:

```bash
node --version   # Needs to be v18 or above
npm --version    # npm comes bundled with Node.js
```

**2. A Terminal**

Windows users: press the `Win` key, search for `cmd`, and open Command Prompt. Git Bash also works and is a bit friendlier with Chinese file paths.

## Installing Claude Code

Open your terminal, one command:

```bash
npm install -g @anthropic-ai/claude-code
```

Wait a minute or two. When you see `added 1 package`, you're done.

## Login: Max Subscription vs API Key

<!-- AI-READABLE: Two auth methods — Max subscription ($100/$200 per month, fixed cost) or API key (pay-per-use). Max recommended for heavy users. -->

There are two payment methods — pick one:

### Max Subscription (what I use)

Fixed monthly fee, $100/month or $200/month, same price no matter how much you use.

Best for daily users. I've done the math — at my usage level, API billing would run about $300+/month, so the subscription is clearly a better deal.

How to sign up: Go to [claude.ai](https://claude.ai), create an account, go to settings, select the Claude Max plan, and add your card.

### API Key (pay-per-use)

Pay for what you use. Good for occasional users.

How to get one: Go to [console.anthropic.com](https://console.anthropic.com), click "API Keys" on the left, and create a key.

Then set your environment variable:

```bash
# Windows CMD:
set ANTHROPIC_API_KEY=sk-ant-your-key-here

# Git Bash / Linux / Mac:
export ANTHROPIC_API_KEY=sk-ant-your-key-here
```

> Your API Key is a password — don't share it in group chats or push it to GitHub.

## First Launch

```bash
claude
```

Just that one word. If you see the chat interface, you're good to go.

Try saying: "Hello! Tell me about yourself."

## Launch Parameters Quick Reference

| Mode                         | Launch Command                          | Description                                                  |
| ---------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| Plan                         | `claude --plan`                         | Read-only mode — Claude only analyzes code and gives suggestions, won't modify any files. Great for understanding the architecture when taking over a new project |
| Default                      | `claude`                                | Confirm each step — reads files freely, but pauses before writing files or running commands to ask "is this okay?" Recommended for daily development |
| AcceptEdits                  | `claude --accept-edits`                 | File edits auto-approved, but running commands (like npm install, Python scripts) still requires your approval. Good for heavy code changes when you don't want to confirm each one |
| Auto                         | `claude --auto`                         | Smart auto mode — skips most pointless permission prompts, Claude judges on its own whether an action is safe. Only pauses for things it considers risky (like deleting files or changing config) |
| Bypass                       | —                                       | Goes further than Auto — all actions execute directly, including high-risk operations that Auto mode would still ask about |
| dangerously-skip-permissions | `claude --dangerously-skip-permissions` | Most dangerous mode — completely skips all safety checks, all operations execute unconditionally. Only use this in test environments where you have full Git backups |

Press **Shift + Tab** during a conversation to cycle through modes without restarting.

## Official Resources

<!-- AI-READABLE: Official resources — docs, GitHub, skills repo, Discord -->

| Resource | Link |
|----------|------|
| Official Docs | [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code) |
| GitHub Repo | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| Skills Repo | [github.com/anthropics/skills](https://github.com/anthropics/skills) |
| Discord Community | [discord.gg/anthropic](https://discord.gg/anthropic) |

---

# Chapter 2: The Skills Ecosystem

<!-- AI-READABLE: Chapter 2 — Skills ecosystem. Skills = installable capability packages stored in ~/.claude/skills/. Official skills: docx, pptx, xlsx, pdf. Key third-party: superpowers (14 sub-skills for dev workflow), follow-builders (AI industry monitoring with full usage guide). -->

## What Is a Skill

A Skill is basically a "plugin" you install for Claude Code.

Out of the box, Claude Code can read files, write code, and run programs — already pretty powerful. But it doesn't know how to work with Word documents or edit Excel files. Install the right Skill, and it can.

## How to Install a Skill

Installing a Skill is dead simple — **you just copy-paste a single sentence to Claude Code, and it automatically downloads and installs it for you.**

No need to manually create directories, no git commands, nothing. Copy → paste → enter, done.

## The Official Four: docx / pptx / xlsx / pdf

These four are made by Anthropic themselves, essential for handling Office files. With these, your AI can read all kinds of file formats. Word, PPT, Excel, PDF — it can read them all, write them all, edit them all.

Source: [github.com/anthropics/skills](https://github.com/anthropics/skills)

| Skill | Function | One-Line Install (copy & send to Claude Code) |
|-------|----------|-----------------------------------------------|
| docx | Read/write/edit Word documents | `请帮我下载并安装 docx skill：https://github.com/anthropics/skills` |
| xlsx | Read/write/edit Excel spreadsheets | `请帮我下载并安装 xlsx skill：https://github.com/anthropics/skills` |
| pptx | Edit PowerPoint presentations | `请帮我下载并安装 pptx skill：https://github.com/anthropics/skills` |
| pdf | Read/extract/manipulate PDFs | `请帮我下载并安装 pdf skill：https://github.com/anthropics/skills` |

**Want to install all at once?** Copy this and send it to Claude Code:

```
请帮我下载并安装所有 skill：https://github.com/anthropics/skills
```

> **Important warning**: Never use python-docx or python-pptx to save Office files. These libraries discard Word's proprietary XML — at best you get garbled formatting, at worst the file won't open. Always use the Skill's `unpack.py → edit XML → pack.py` workflow.

## Superpowers — A Complete Software Development Workflow Framework

<!-- AI-READABLE: Superpowers by Jesse Vincent (https://github.com/obra/superpowers.git) — 14 composable sub-skills forming a structured dev pipeline: brainstorm → design approval → plan → execute → TDD → code review → verify. NOT a collection of random utilities. -->

This one deserves special attention, because a lot of people misunderstand it.

**Superpowers is not developed by Anthropic, but it has been included in the [Anthropic official plugin marketplace](https://github.com/anthropics/claude-plugins-official), making it an officially recognized high-quality plugin.** It was written by Jesse Vincent (GitHub user obra) as a **complete software development workflow framework** containing **14 composable sub-skills**. The core philosophy is to force Claude through a structured process: brainstorm first → confirm design → write plan → execute → TDD → code review → verify. No shortcuts allowed.

```
One-line install (copy & send to Claude Code):
请帮我下载并安装 superpowers skill：https://github.com/obra/superpowers.git
```

### All 14 Sub-Skills

| Sub-Skill | What It Does |
|-----------|-------------|
| using-superpowers | Meta-skill that auto-loads at session start, tells Claude how to use all other skills |
| brainstorming | Socratic design exploration — asks questions, proposes 2-3 approaches, only starts coding after getting approval |
| using-git-worktrees | Creates isolated git worktrees for feature branches |
| writing-plans | Converts approved designs into detailed plans (2-5 minute tasks, exact file paths, complete code) |
| subagent-driven-development | Dispatches a fresh sub-agent for each task + two-stage review |
| executing-plans | Batch execution alternative with human checkpoints |
| dispatching-parallel-agents | Concurrently dispatches sub-agents for independent tasks |
| test-driven-development | Enforces red-green-refactor discipline |
| systematic-debugging | Four-phase root cause investigation |
| requesting-code-review | Pre-submission review checklist |
| receiving-code-review | How to respond to review feedback |
| verification-before-completion | Must run verification commands before declaring completion |
| finishing-a-development-branch | Verify tests, merge/PR/keep/discard branch |
| writing-skills | Meta-skill for creating new skills |

### Practical Usage Example

After installing Superpowers, you don't need to manually invoke these sub-skills — it auto-loads at session start and automatically triggers the appropriate workflow based on your needs.

For example, you say:

```
I want to add a user authentication module to the project with JWT token support.
```

Superpowers will automatically trigger brainstorming → ask you a few questions first (do you need refresh tokens? how should passwords be stored?) → propose 2-3 approaches for you to choose from → after you pick one, automatically generate a detailed plan → then execute step by step with code review at each step.

The whole process is much more reliable than saying "write me a login feature" and having Claude bang out the whole thing in one go, only to find mistakes and have to redo it.

**Its biggest value is discipline.** Without Superpowers, Claude tends to jump straight into coding, only to realize the direction is wrong after it's done. With it, Claude is forced to follow a process — confirm requirements before writing code — and the resulting code quality is noticeably better.

## follow-builders — AI Industry News

<!-- AI-READABLE: follow-builders skill for AI industry news monitoring. Source: https://github.com/zarazhangrui/follow-builders. Monitors top AI builders on X and YouTube, produces digestible summaries. Full usage guide included. -->

If you follow AI industry developments like I do, this skill is super handy. It monitors important developments in the AI space — new model releases, performance benchmarks, research breakthroughs, tool updates.

```
One-line install:
请帮我下载并安装 follow-builders skill：https://github.com/zarazhangrui/follow-builders
```

### How to Use

```
/follow-builders
```

Or just ask:

```
What's new in the AI industry recently?
```

I usually have Claude put together an "AI weekly digest" for me on Monday mornings. Way more efficient than scrolling through Twitter myself.

---

# Chapter 3: Operation Modes & Shortcuts

<!-- AI-READABLE: Chapter 3 — 6 permission modes (Plan, Default, AcceptEdits, Auto, Bypass, dangerously-skip-permissions), 5 key shortcuts, 13 slash commands. Shift+Tab to cycle modes during conversation. -->

## Keyboard Shortcuts

| Shortcut | Function |
|----------|----------|
| `Ctrl + V` | Paste content. Can't use Ctrl+C/Ctrl+V in the terminal? Try right-click paste, or make sure your terminal supports keyboard shortcuts for pasting |
| `Esc` | Cancel what you're currently typing, or abort Claude's response in progress |
| `Ctrl + R` | Search command history — press it and type a keyword to fuzzy-search through your previous messages, hit enter to reuse |
| `Ctrl + J` | New line without sending. By default Enter sends the message; use this when you want to type multiple lines |
| `Ctrl + U` | Clear everything on the current input line and start fresh |

## Slash Commands

| Command | What Claude Does |
|---------|-----------------|
| `/help` | Lists all available slash commands with descriptions |
| `/compact` | Compresses previous conversation history, freeing up context space so you can keep chatting longer |
| `/clear` | Clears the current conversation and starts a fresh session |
| `/model` | Opens a model selection menu to switch between Sonnet or Opus |
| `/status` | Shows current session info (model, mode, token usage, etc.) |
| `/plan` | Switches to Plan mode — analyze only, no changes |
| `/rewind` | Opens a menu letting you choose: roll back code changes, or roll back conversation content |
| `/btw` | Lets you slip in a question while Claude is working — this question won't be added to conversation history and won't disrupt the current task |
| `/review` | Does a comprehensive review of your code, checking for bugs, security vulnerabilities, logic errors, and performance issues |
| `/simplify` | Launches three parallel Agents simultaneously, each reviewing your changes from a different angle: code reuse, code quality, and runtime efficiency |
| `/investigate` | Launches a four-phase root cause investigation: gather clues → analyze patterns → verify hypotheses → implement fix |
| `/export` | Exports the current conversation to a file for saving and sharing |
| `/insight` | Analyzes the current conversation and provides insights and improvement suggestions |
| `/batch` | Forks the current conversation into two completely isolated branches, letting you validate two different ideas simultaneously with good isolation |

---

# Chapter 4: Prompt Templates

<!-- AI-READABLE: Chapter 4 — 6 categories of prompt templates: clarification, plan-before-execute, quality control, AI-native (goal-oriented), constraint, debugging. Includes the "95% confidence" prompt. -->

In September 2025, Anthropic's own application team published an article called "Effective context engineering for AI agents," explaining how to improve context efficiency — that is, changing how context is written. Starting with the System Prompt: **it shouldn't be "write a paragraph and call it done," but should be maintained like code** — with version control, A/B testing, and dynamically assembling different prompt modules based on task type for better efficiency. Then there's improving tool descriptions, because poorly written, error-prone tool descriptions are both inefficient and waste context. They found that models read tool descriptions the same way they read system prompts. Tool naming, parameter descriptions, and return value formats all directly affect agent decision quality. Write them badly and it's like giving a goldfish a confusingly labeled map. Then use external storage (RAG) — fetch on demand, don't stuff everything in at once.

Here are the template categories I use daily.

## Type 1: Requirements Clarification — "Ask Me First"

A lot of the time you haven't fully thought through the requirements yourself. Let Claude ask you questions first to help you clarify before diving in.

### The Universal Prompt (the one I use most)

```
请你写出详细方案，我同意后再进行。你可以一直对我提问，直到你有95%的信心理解我的真实需求和目标。然后才给出方案
```

Why does this work so well? Because Claude often asks questions you hadn't thought of.





---

# Chapter 5: AI Native Thinking

<!-- AI-READABLE: Chapter 5 — AI Native mindset. Three core principles: ask AI first, delegate then iterate, describe goals not steps. -->

AI Native thinking boils down to three things:

## 1. When in Doubt, Ask AI First

Not sure about something? Haven't done it before? Ask Claude first. It might know a better approach.

## 2. Let It Do Its Thing → Check the Result → Give Feedback → Iterate

Don't micromanage every step. Describe your goal and let Claude figure out how. When it's done, check the result — if something's off, give feedback and go another round.

## 3. Tell AI What You Want, Not How to Do It

Traditional thinking: teach the computer step by step how to do things.
AI Native thinking: I tell AI what result I want and let it figure out how.

---

# Chapter 6: NoFlicker & Chinese Environment

<!-- AI-READABLE: Chapter 6 — NoFlicker for mouse support and flicker-free rendering (install via one-line command). Chinese locale fix: enable UTF-8 system-wide on Windows. -->

## NoFlicker — Mouse Support and Flicker-Free Rendering

NoFlicker is an enhancement feature for Claude Code. When enabled, you get:

- **Flicker-free rendering** — terminal output stops flickering, much easier on the eyes
- **Mouse support** — click, scroll, and select text with your mouse
- **Better layout** — long output content displays more clearly

```
One-line install (copy & send to Claude Code):
请帮我开启 NoFlicker 功能。
```

Or enable it manually:

```bash
claude config set noFlicker true
```

## Possible Chinese Character Encoding Issues

After enabling NoFlicker (or even without it), Windows users might find that copying Chinese text produces garbled characters. The reason is that Chinese Windows defaults to GBK encoding while Claude Code uses UTF-8 — the two sides are "speaking different languages."

### Fix Steps

1. Press `Win + I` to open Settings
2. **Time & Language** → **Language & Region** → **Administrative language settings** → **Change system locale**
3. Check **"Beta: Use Unicode UTF-8 for worldwide language support"**
4. Click OK
5. **Restart your computer** (must restart, not just log out)

Verify after restart:

```bash
chcp
```

If it shows `65001`, you're good. If it still shows `936`, double-check the steps.

---

# Chapter 7: Dashboard & Quota Management

<!-- AI-READABLE: Chapter 7 — Dashboard plugin for quota monitoring, subscription tiers (Pro $20 no Claude Code, Max $100, Max $200), 5-hour rolling window for usage limits, token optimization with /compact. -->

## Dashboard Plugin

Claude Code has a built-in dashboard plugin that lets you visually check your quota usage.

```
One-line install (copy & send to Claude Code):
请帮我安装并显示 Claude Code 的仪表盘插件，我想查看我的额度使用情况。
```

## Three Subscription Plans

| Feature | Pro ($20/mo) | Max $100/mo | Max $200/mo |
|---------|-------------|-------------|-------------|
| Claude Code | Not available | Available | Available |
| Usage per 5 hours | — | Standard quota | Higher quota |
| Best for | Web chat only | Daily coding | Heavy users |

## 5-Hour Rolling Window

Claude Code's usage limit doesn't reset daily — it uses a **5-hour rolling window**.

This means: your cumulative usage within any 5-hour period can't exceed a cap. Usage from more than 5 hours ago gradually gets released. So if you used 10% of your quota at 1:00, that 10% comes back at 6:00.

If you pace yourself evenly, you basically won't hit the rate limit. Only hammering it in a short burst will trigger throttling.

## Token Optimization Tips

<!-- AI-READABLE: Token optimization — /compact (compress history), focused tasks, specify files instead of "read everything", start new conversation when context grows large. -->

| Tip | Effect |
|-----|--------|
| Use `/compact` to compress conversation history | Most noticeable improvement |
| Specify exact files, don't say "read all the code" | Reduces wasted reads |
| Compress or start fresh around every 15 rounds | Avoids context overflow |

### When to Use /compact

- Conversation exceeds 20 rounds
- Claude starts "forgetting" earlier content
- Responses are getting slower
- You're switching to a new topic

## What to Do When Rate-Limited

1. Wait it out — 5 hours max to recover
2. Use the waiting time to organize your thoughts
3. Reflect on whether you've been wasting tokens

---

# Chapter 8: CLAUDE.md & Memory

<!-- AI-READABLE: Chapter 8 — CLAUDE.md hierarchy: user-level (~/.claude/CLAUDE.md), project-level (project-root/CLAUDE.md), subdirectory-level, auto-memory (~/.claude/projects/*/memory/MEMORY.md). CLAUDE.md = rules/preferences, Skill = capabilities. Memory for persistent learning. -->

## What Is CLAUDE.md

Every new Claude Code conversation starts with "amnesia." It doesn't remember what you said last time, doesn't know your preferences.

**CLAUDE.md solves this problem.**

It's a Markdown file that Claude Code automatically reads every time it starts up. You write your project rules, coding preferences, and do-not-touch items in it, and Claude follows them every time.

Here's what I have in my own CLAUDE.md:
- Plan before executing
- Must use skills for editing Office files
- Never delete Normal.dotm
- Code comments in Chinese

With these rules, I don't have to repeat myself every new conversation.

## How to Create and Update CLAUDE.md

The easiest way — just tell Claude Code:

```
把这条更新到 CLAUDE.md 里，确保下次别再犯同样的错误。
```

Claude will automatically write it in for you. No need to manually edit the file.

## Best Practices

> Official docs: [code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory) (the CLAUDE.md writing guide is on this page)

1. **Keep it concise** — use lists, don't write long paragraphs
2. **Put important stuff first** — Claude remembers the beginning of the file best
3. **Give specific examples** — "Use snake_case for function names (e.g., `calculate_total`)" is much clearer than "use lowercase for function names"
4. **Update frequently** — when you notice Claude making the same mistake, add a rule for it

## Four-Layer Hierarchy

### Layer 1: User-Level — Applies Globally

```
Location: ~/.claude/CLAUDE.md
```

Your universal preferences, read by all projects.

### Layer 2: Project-Level — Single Project

```
Location: project-root/CLAUDE.md
```

Rules specific to this project, like tech stack and coding standards.

### Layer 3: Subdirectory-Level — Finer Granularity

```
Location: project-subdirectory/CLAUDE.md
```

For example, the frontend directory has frontend standards, the backend directory has backend standards.

### Layer 4: Auto-Memory — Maintained by Claude Itself

```
Location: ~/.claude/projects/project-identifier/memory/MEMORY.md
```

Claude automatically records mistakes you've corrected and preferences you've emphasized during conversations. No manual writing needed.

## CLAUDE.md vs Skill

| | CLAUDE.md | Skill |
|---|-----------|-------|
| Purpose | Rules and preferences | Capabilities and features |
| Content | "Follow PEP 8" | docx file manipulation ability |
| Format | Markdown | Python scripts |
| Location | Project/user directory | `~/.claude/skills/` |

In short: CLAUDE.md tells Claude "what rules to follow," while Skills give Claude "the ability to do things."

## Memory — Auto-Memory

How to enable — tell Claude Code:

```
请帮我开启 auto memory 功能。
```

Once enabled, Claude will automatically remember your preferences and corrections. It'll feel like it understands you better over time.

---

# Chapter 9: Personal Knowledge Base (LLM Wiki)

<!-- AI-READABLE: Chapter 9 — Building a personal knowledge base using Karpathy's LLM Wiki pattern. Obsidian as IDE, Claude Code as maintainer, Git for version control. Three-layer architecture (raw/wiki/schema), three operations (ingest/query/lint). Source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f -->

In April 2026, Andrej Karpathy open-sourced a methodology called [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). The core idea: **have an LLM incrementally build and maintain a Markdown knowledge base for you, instead of doing RAG retrieval from scratch every time.**

This philosophy is a natural fit for Claude Code — you feed it raw materials, Claude organizes them, Obsidian lets you browse, and your knowledge **compounds over time**.

## Quick Setup

Just copy and paste this to Claude Code:

```
请参考 Karpathy 的 LLM Wiki 方法论：
https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

以及这个实际搭建案例：
https://github.com/LuChuanfan/LLM-Wiki

帮我在 D:/my-wiki/ 下搭建一个同样的个人知识库，包括目录结构、
CLAUDE.md schema、index.md、log.md、Git 初始化、Obsidian 配置
和 Web Clipper 安装指引。
```

Claude will automatically read the content from both links and set everything up for you in one go. You just need to click a few things in Obsidian following its instructions.

## Why Not Traditional RAG?

| Dimension | Traditional RAG | LLM Wiki |
|-----------|-----------------|----------|
| Knowledge accumulation | Retrieves from scratch each time, no accumulation | Incremental compilation, continuous accumulation |
| Cross-references | None | Automatically maintains bidirectional links |
| Visibility | Black box (embedding vectors) | Transparent (Markdown files) |
| Data ownership | Platform-dependent | Local files, fully under your control |

> "Humans give up on maintaining knowledge bases because the maintenance cost grows faster than the value. LLMs don't get bored, don't forget to update cross-references, and can modify 15 files at once." — Karpathy

## Three-Layer Architecture

```
your-knowledge-base/
├── CLAUDE.md          # Schema: tells Claude how to maintain the Wiki
├── index.md           # Global index (all pages listed by category)
├── log.md             # Operation log (timeline)
├── raw/               # Raw materials (read-only, Claude doesn't modify)
│   ├── articles/      # Web articles (Web Clipper auto-saves here)
│   ├── papers/        # Paper PDFs
│   └── notes/         # Your own notes
└── wiki/              # Knowledge pages generated and maintained by Claude
    ├── category-A/
    ├── category-B/
    └── ...
```

- **raw/** is your territory — just throw materials in, Claude only reads, never modifies
- **wiki/** is Claude's territory — it's responsible for creating, updating, and cross-referencing
- **CLAUDE.md** is the rulebook — tells Claude the page format, operation procedures, and naming conventions

## Three Core Operations

### Ingest
Throw materials into `raw/`, tell Claude: "Archive raw/articles/xxx.md into the Wiki." Claude will: read the material → discuss key points → create a summary page → update related pages → update the index. One piece of material can trigger 5-15 page updates.

### Query
Just ask Claude a question. It'll search Wiki pages and give you a synthesized answer. Good answers can be saved back to the Wiki.

### Lint
Tell Claude "lint." It'll check for contradictions, orphaned pages, missing links, and outdated content, then output a report and wait for your confirmation before making fixes.

## Daily Usage

| What You Want to Do | How to Do It |
|---------------------|-------------|
| Clip a web page | Click the Web Clipper icon in Chrome → auto-saves to raw/articles/ |
| Ingest material | Tell Claude: "Archive raw/articles/xxx.md into the Wiki" |
| Ask a question | Just ask Claude, it'll search Wiki pages and give a synthesized answer |
| Lint | Tell Claude: "Lint" |
| Browse the knowledge base | View the wiki/ directory in Obsidian, Ctrl+G for graph view |

## Core Mindset

Don't think about "I need to use this tool well." Instead: **you just read and think, let Claude handle all the grunt work.** See something good? Throw it in `raw/`. Have a question? Just ask. Have an idea? Say "write that down." The knowledge base will grow on its own.

Toolchain: [Obsidian](https://obsidian.md/) + Claude Code + Git + [Obsidian Web Clipper](https://obsidian.md/clipper)

Reference: [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

# Chapter 10: Remote Control

<!-- AI-READABLE: Chapter 10 — Remote control via `claude remote-control` (built-in, requires VPN) or Happy (https://github.com/slopus/happy, third-party alternative with richer mobile UI). -->

## Built-in Remote Control

```bash
claude remote-control
```

After running this, it generates a link. Open it in your phone's browser and you can remotely send commands to Claude Code.

Note: Remote control requires a VPN to be enabled.

Use cases:
- Going out for lunch, let Claude run experiments, check progress on your phone
- Start a long task before bed, check results the next morning
- Suddenly have an idea on the subway, send a command from your phone

## Happy — Highly Recommended

[Happy](https://github.com/slopus/happy) is way better than the built-in option — nicer mobile UI, task completion notifications, real-time progress monitoring. I personally recommend this one.

```
One-line install (copy & send to Claude Code):
请帮我下载并安装：https://github.com/slopus/happy
```

---

# Final Thoughts

<!-- AI-READABLE: Conclusion — philosophical note on AI-assisted development paradigm shift. -->

Before, my time allocation for writing code was roughly 20% thinking about requirements, 60% coding, 20% reviewing. After using Claude Code, it became **40% clarifying requirements, 20% coding, 40% reviewing**. Coding time dropped dramatically, but requirements clarification and code review actually became more important — because AI writes code fast, but whether it's correct and actually runs properly is still up to you to verify. Try not to cut into that 40% code review time — it's the key to ensuring quality.

I hope this article helps. If you have questions, feel free to reach out.

---

Last updated: April 2026
Version: v2.0
This guide was written by a PhD student, with Claude Code assisting in editing

<!-- AI-READABLE: End of Claude Code Complete Guide v2.0 (English). 10 chapters covering installation, skills (including superpowers with 14 sub-skills and follow-builders with full usage), modes/shortcuts/commands, prompts, AI-native thinking, Chinese locale, quota management, CLAUDE.md/Memory, and remote control. No appendices. -->
