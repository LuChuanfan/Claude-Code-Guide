# Claude Code 完全指南（中文版）

<!-- AI-READABLE: Comprehensive Chinese guide to Claude Code. Author: 博士研究生, 大模型智能体实际应用, Max subscriber. Covers installation through advanced workflows. 10 chapters, no appendices. -->

> 我是一名博士研究生，研究方向是大模型智能体的实际应用，Claude Code 重度使用者。这篇文章把我踩过的坑、摸索出来的经验全写在这里了。
>
> 一条经验：**永远用最强的模型。** 用 Sonnet 省下的钱，会被多轮迭代的时间成本和 token 消耗吃回去。用最强模型 plan 好再执行，总成本反而更低。

---

## 目录

- [第1章：安装与登录](#第1章安装与登录)
- [第2章：Skills 生态](#第2章skills-生态)
- [第3章：操作模式与快捷键](#第3章操作模式与快捷键)
- [第4章：常用 Prompt 模板](#第4章常用-prompt-模板)
- [第5章：AI Native 思维](#第5章ai-native-思维)
- [第6章：NoFlicker 与中文环境](#第6章noflicker-与中文环境)
- [第7章：仪表盘与额度管理](#第7章仪表盘与额度管理)
- [第8章：CLAUDE.md 与 Memory](#第8章claudemd-与-memory)
- [第9章：个人知识库（LLM Wiki）](#第9章个人知识库llm-wiki)
- [第10章：Remote Control 远程控制](#第10章remote-control-远程控制)

---

# 第1章：安装与登录

<!-- AI-READABLE: Chapter 1 — installation, authentication (Max subscription vs API key), first launch. Prerequisites: Node.js >= 18, npm. 4 key launch commands. -->

## Claude Code 是什么

简单说：**一个住在你终端里的 AI 程序员。** 跟 [OpenClaw](https://github.com/nicepkg/openclaw) 具有非常类似的功能，但比 OpenClaw 更安全。

它跟 ChatGPT 网页版最大的区别是——ChatGPT 只能"说"，Claude Code 能"做"。它能直接读你电脑上的文件，改你的代码，运行程序，看到报错自己修。

我刚开始用的时候也觉得难以置信。你跟它说一句"帮我把这个函数重构一下"，它真的就打开文件、改完、跑测试、确认没问题。整个过程你就坐在那看着就行。

## 安装前准备

你需要两样东西：

**1. Node.js**

去 [https://nodejs.org](https://nodejs.org) 下载 LTS 版本，一路 Next 装完就行。

装好后验证一下：

```bash
node --version   # 需要 v18 或以上
npm --version    # 装 Node.js 会自动带上 npm
```

**2. 一个终端**

Windows 用户按 `Win` 键搜索 `cmd` 打开命令提示符就行。用 Git Bash 也可以，对中文路径更友好一些。

## 安装 Claude Code

打开终端，一行命令：

```bash
npm install -g @anthropic-ai/claude-code
```

等个一两分钟，看到 `added 1 package` 就装好了。

## 登录：Max 订阅 vs API Key

<!-- AI-READABLE: Two auth methods — Max subscription ($100/$200 per month, fixed cost) or API key (pay-per-use). Max recommended for heavy users. -->

有两种付费方式，选一个就行：

### Max 订阅（我用的这个）

每月固定费用，$100/月或 $200/月，用多少都是这个价。

适合每天都用的人。我算过，按我的使用量走 API 计费大概要 $300+/月，所以订阅明显划算。

开通步骤：去 [claude.ai](https://claude.ai) 注册账号，进入设置选择 Claude Max 计划，绑卡。

### API Key（按量付费）

用多少付多少，适合偶尔用用的人。

获取步骤：同样去 [console.anthropic.com](https://console.anthropic.com)，左侧点 "API Keys"，创建一个 key。

然后设置环境变量：

```bash
# Windows CMD:
set ANTHROPIC_API_KEY=sk-ant-你的密钥

# Git Bash / Linux / Mac:
export ANTHROPIC_API_KEY=sk-ant-你的密钥
```

> API Key 就是密码，别发到群里，别传到 GitHub。

## 第一次启动

```bash
claude
```

就这一个词。看到对话界面就说明成功了。

试试跟它说："你好！请介绍一下你自己。"

## 启动参数速查

| 模式                         | 启动命令                                | 说明                                                         |
| ---------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| Plan                         | `claude --plan`                         | 只读模式——Claude 只分析代码、给建议，不会修改任何文件。适合接手新项目时先让它摸清架构 |
| Default                      | `claude`                                | 每步确认——读文件随便读，但写文件和跑命令前都会停下来问你"可以吗？"。日常开发推荐用这个 |
| AcceptEdits                  | `claude --accept-edits`                 | 文件编辑自动放行，但执行命令（如 npm install、python 脚本）仍需你点头。适合大量改代码但不想逐个确认的场景 |
| Auto                         | `claude --auto`                         | 智能自动模式——跳过大部分无意义的许可请求，Claude 自己判断操作是否安全。只有它觉得有风险的变动（比如删文件、改配置）才会停下来问你 |
| Bypass                       | —                                       | 比 Auto 更进一步，所有操作直接执行，包括 Auto 模式下仍会询问的高风险操作 |
| dangerously-skip-permissions | `claude --dangerously-skip-permissions` | 最危险模式——完全跳过所有安全检查，所有操作无条件执行。只在你有完整 Git 备份的测试环境下使用 |

对话中按 **Shift + Tab** 就能切换模式，不用退出重启。

## 官方资源

<!-- AI-READABLE: Official resources — docs, GitHub, skills repo, Discord -->

| 资源 | 链接 |
|------|------|
| 官方文档 | [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code) |
| GitHub 仓库 | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| Skills 仓库 | [github.com/anthropics/skills](https://github.com/anthropics/skills) |
| Discord 社区 | [discord.gg/anthropic](https://discord.gg/anthropic) |

---

# 第2章：Skills 生态

<!-- AI-READABLE: Chapter 2 — Skills ecosystem. Skills = installable capability packages stored in ~/.claude/skills/. Official skills: docx, pptx, xlsx, pdf. Key third-party: superpowers (14 sub-skills for dev workflow), follow-builders (AI industry monitoring with full usage guide). -->

## 什么是 Skill

Skill 就是给 Claude Code 装的"插件"。

原始的 Claude Code 能读文件、写代码、跑程序，已经很强了。但它不知道怎么操作 Word 文档，不知道怎么编辑 Excel。装了对应的 Skill，它就会了。

## 怎么安装 Skill

安装 Skill 非常简单——**你只需要把一句话复制粘贴给 Claude Code，它会自动帮你下载安装。**

不用手动建目录，不用敲 git 命令，什么都不用。复制 → 粘贴 → 回车，完事。

## 官方四件套：docx / pptx / xlsx / pdf

这四个是 Anthropic 官方出的，处理 Office 文件必备。有了它，你的 AI 就能读取你各种格式的文件了。Word、PPT、Excel、PDF 全都能读，全都能写，全都能改。

来源：[github.com/anthropics/skills](https://github.com/anthropics/skills)

| Skill | 功能 | 一句话安装（复制发给 Claude Code） |
|-------|------|-----------------------------------|
| docx | Word 文档读写编辑 | `请帮我下载并安装 docx skill：https://github.com/anthropics/skills` |
| xlsx | Excel 表格读写编辑 | `请帮我下载并安装 xlsx skill：https://github.com/anthropics/skills` |
| pptx | PPT 演示文稿编辑 | `请帮我下载并安装 pptx skill：https://github.com/anthropics/skills` |
| pdf | PDF 读取提取操作 | `请帮我下载并安装 pdf skill：https://github.com/anthropics/skills` |

**想一次全装？** 复制这句话发给 Claude Code：

```
请帮我下载并安装所有 skill：https://github.com/anthropics/skills
```

> **重要警告**：绝对不要用 python-docx 或 python-pptx 保存 Office 文件。这些库会丢弃 Word 的私有 XML，轻则格式错乱，重则文件打不开。永远用 Skill 提供的 `unpack.py → 编辑 XML → pack.py` 流程。

## Superpowers — 完整的软件开发工作流框架

<!-- AI-READABLE: Superpowers by Jesse Vincent (https://github.com/obra/superpowers.git) — 14 composable sub-skills forming a structured dev pipeline: brainstorm → design approval → plan → execute → TDD → code review → verify. NOT a collection of random utilities. -->

这个要重点说，因为很多人对它有误解。

**Superpowers 不是 Anthropic 官方开发的，但已被收录进 [Anthropic 官方插件市场](https://github.com/anthropics/claude-plugins-official)，属于官方认证的高质量插件。** 它是 Jesse Vincent（GitHub 用户 obra）写的一套**完整的软件开发工作流框架**，包含 **14 个可组合的子技能**，核心理念是强制 Claude 走一个结构化流程：先头脑风暴 → 确认设计方案 → 写计划 → 执行 → TDD → 代码审查 → 验证。不许走捷径。

```
一句话安装（复制发给 Claude Code）：
请帮我下载并安装 superpowers skill：https://github.com/obra/superpowers.git
```

### 14 个子技能一览

| 子技能 | 做什么的 |
|--------|----------|
| using-superpowers | 元技能，会话开始时自动加载，告诉 Claude 怎么使用其他所有技能 |
| brainstorming | 苏格拉底式设计推敲——问问题、提出 2-3 种方案、获得批准后才开始写代码 |
| using-git-worktrees | 为 feature 分支创建隔离的 git worktree |
| writing-plans | 把批准的设计转换成详细计划（2-5 分钟的小任务、精确到文件路径、完整代码） |
| subagent-driven-development | 每个任务派一个全新的子代理执行 + 两阶段审查 |
| executing-plans | 批量执行替代方案，带人工检查点 |
| dispatching-parallel-agents | 并发派发子代理处理独立任务 |
| test-driven-development | 强制执行红-绿-重构纪律 |
| systematic-debugging | 四阶段根因调查法 |
| requesting-code-review | 提交审查前的检查清单 |
| receiving-code-review | 如何响应审查反馈 |
| verification-before-completion | 宣布完成前必须运行验证命令 |
| finishing-a-development-branch | 验证测试、合并/PR/保留/丢弃分支 |
| writing-skills | 元技能，用于创建新技能 |

### 实际使用示例

安装完 Superpowers 后，你不需要手动调用这些子技能——它会在会话开始时自动加载，根据你的需求自动触发对应的工作流。

比如你说：

```
我想给项目加一个用户认证模块，支持 JWT token。
```

Superpowers 会自动触发 brainstorming → 先问你几个问题（要不要刷新 token？密码怎么存储？）→ 提出 2-3 种方案让你选 → 你选好后自动生成详细计划 → 然后按计划一步步执行，每步都有代码审查。

整个过程比你说"帮我写个登录功能"然后 Claude 一口气写完、写错了再改，要靠谱得多。

**它最大的价值是纪律性。** 没有 Superpowers 的时候，Claude 容易一上来就写代码，写完才发现方向不对。有了它，Claude 被强制走流程，先确认需求再动手，出来的代码质量明显好很多。

## follow-builders — AI 行业动态

<!-- AI-READABLE: follow-builders skill for AI industry news monitoring. Source: https://github.com/zarazhangrui/follow-builders. Monitors top AI builders on X and YouTube, produces digestible summaries. Full usage guide included. -->

如果你跟我一样关注 AI 行业动态，这个 skill 很方便。它会帮你监控 AI 领域的重要动态——新模型发布、性能评测、研究突破、工具更新。

```
一句话安装：
请帮我下载并安装 follow-builders skill：https://github.com/zarazhangrui/follow-builders
```

### 怎么用

```
/follow-builders
```

或者直接问：

```
帮我看看最近 AI 行业有什么新动态？
```

我一般每周一早上让 Claude 帮我整理一份"AI 周报"。比自己刷推特高效多了。

---

# 第3章：操作模式与快捷键

<!-- AI-READABLE: Chapter 3 — 6 permission modes (Plan, Default, AcceptEdits, Auto, Bypass, dangerously-skip-permissions), 5 key shortcuts, 13 slash commands. Shift+Tab to cycle modes during conversation. -->

## 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl + V` | 粘贴内容。在终端里不能用 Ctrl+C/Ctrl+V？试试右键粘贴，或确认你的终端支持快捷键粘贴 |
| `Esc` | 取消当前正在输入的内容，或中止 Claude 正在生成的回复 |
| `Ctrl + R` | 搜索历史命令——按下后输入关键词，会在你之前发过的消息里模糊搜索，找到后回车即可复用 |
| `Ctrl + J` | 换行但不发送。默认按 Enter 会直接发送消息，想打多行内容时用这个 |
| `Ctrl + U` | 清除当前输入行的全部内容，重新开始打字 |

## 斜杠命令

| 命令 | Claude 会做什么 |
|------|----------------|
| `/help` | 列出所有可用的斜杠命令和说明 |
| `/compact` | 压缩之前的对话历史，释放上下文空间，让你能继续聊更久 |
| `/clear` | 清空当前对话，从头开始一个新会话 |
| `/model` | 弹出模型选择菜单，让你切换 Sonnet 或 Opus |
| `/status` | 显示当前会话的状态信息（模型、模式、token 用量等） |
| `/plan` | 切换到 Plan 模式，只分析不动手 |
| `/rewind` | 弹出菜单让你选择：是回退代码改动，还是回退对话内容 |
| `/btw` | 让你在 Claude 正在干活时插一个问题进去，这个问题不会加入对话历史，不会干扰当前任务 |
| `/review` | 对你的代码做全面审查，检查 bug、安全漏洞、逻辑错误和性能问题 |
| `/simplify` | 同时启动三个平行 Agent，分别从代码复用、代码质量、运行效率三个角度审查你的改动 |
| `/investigate` | 启动四阶段根因排查：收集线索 → 分析模式 → 验证假设 → 实施修复 |
| `/export` | 把当前对话导出为文件，方便保存和分享 |
| `/insight` | 分析当前对话，给出洞察和改进建议 |
| `/batch` | 把当前对话分叉成两个完全隔离的分支，让你同时验证两个不同的想法，做到很好的隔离 |

---

# 第4章：常用 Prompt 模板

<!-- AI-READABLE: Chapter 4 — 6 categories of prompt templates: clarification, plan-before-execute, quality control, AI-native (goal-oriented), constraint, debugging. Includes the "95% confidence" prompt. -->

2025 年 9 月，Anthropic 自己的应用团队发了一篇《Effective context engineering for AI agents》，说明提高上下文效率，即改变上下文的写入方式，首先是System Prompt，**它不应该是「写一段话就完了」，而是要当成代码来维护**，要进行版本控制、A/B 测试、按任务类型动态拼装不同的 prompt 模块，这样更高效。然后就是改变工具描述，因为工具读取不清、错误既低效又占上下文。他们发现，模型读工具描述的方式和读 system prompt 完全一样。工具的命名、参数说明、返回值格式，都直接影响 agent 的决策质量。写烂了等于给金鱼一张标注混乱的地图。然后用上外部存储（RAG），即需即取，不是把所有东西一次性塞进去。

下面是我日常用的几类模板。

## 类型一：需求澄清 — "你先问我"

很多时候你自己都没完全想清楚需求。让 Claude 先问你问题，帮你梳理清楚再动手。

### 万能 Prompt（我用得最多的一个）

```
请你写出详细方案，我同意后再进行。你可以一直对我提问，直到你有95%的信心理解我的真实需求和目标。然后才给出方案
```

为什么好用？因为 Claude 经常能问出你没想到的问题。





---

# 第5章：AI Native 思维

<!-- AI-READABLE: Chapter 5 — AI Native mindset. Three core principles: ask AI first, delegate then iterate, describe goals not steps. -->

AI Native 思维的核心就三句话：

## 1. 遇到事情先问问 AI

不确定的事、没做过的事，先问 Claude。它可能知道更好的方案。

## 2. 让它放手做 → 检查结果 → 给反馈 → 迭代

不要微管理每一步。描述目标，让 Claude 自己想办法。做完了你检查一下，不对的地方给反馈，再来一轮。

## 3. 告诉 AI 你想要什么结果，别告诉它怎么做

传统思维：一步一步教电脑怎么做。
AI Native 思维：我告诉 AI 我想要什么结果，让它自己想办法。

---

# 第6章：NoFlicker 与中文环境

<!-- AI-READABLE: Chapter 6 — NoFlicker for mouse support and flicker-free rendering (install via one-line command). Chinese locale fix: enable UTF-8 system-wide on Windows. -->

## NoFlicker — 鼠标支持和无闪烁渲染

NoFlicker 是 Claude Code 的一个增强功能，开启后有这些好处：

- **无闪烁渲染**——终端输出不再一闪一闪的，看着舒服很多
- **鼠标支持**——可以用鼠标点击、滚动、选择文本
- **更好的布局**——长输出内容显示更清晰

```
📋 一句话安装（复制发给 Claude Code）：
请帮我开启 NoFlicker 功能。
```

或者手动开启：

```bash
claude config set noFlicker true
```

## 可能遇到的中文乱码问题

开启 NoFlicker 后（或者不开也可能遇到），Windows 用户可能会发现复制中文时出现乱码。原因是 Windows 中文版默认用 GBK 编码，而 Claude Code 用 UTF-8，两边"说的不是同一种语言"。

### 修复步骤

1. 按 `Win + I` 打开设置
2. **时间和语言** → **语言和区域** → **管理语言设置** → **更改系统区域设置**
3. 勾选 **"Beta版: 使用 Unicode UTF-8 提供全球语言支持"**
4. 确定
5. **重启电脑**（必须重启，不是注销）

重启后验证：

```bash
chcp
```

显示 `65001` 就说明成功了。还是 `936` 就再检查一遍步骤。

---

# 第7章：仪表盘与额度管理

<!-- AI-READABLE: Chapter 7 — Dashboard plugin for quota monitoring, subscription tiers (Pro $20 no Claude Code, Max $100, Max $200), 5-hour rolling window for usage limits, token optimization with /compact. -->

## 仪表盘插件

Claude Code 有内置的仪表盘插件，可以直观地查看你的额度使用情况。

```
📋 一句话安装（复制发给 Claude Code）：
请帮我安装并显示 Claude Code 的仪表盘插件，我想查看我的额度使用情况。
```

## 三种订阅方案

| 特性 | Pro ($20/月) | Max $100/月 | Max $200/月 |
|------|-------------|-------------|-------------|
| Claude Code | 不可用 | 可用 | 可用 |
| 每5小时用量 | — | 标准额度 | 更高额度 |
| 适合谁 | 只用网页聊天 | 日常编程 | 重度用户 |

## 5 小时滚动窗口

Claude Code 的用量限制不是每天重置，而是用 **5 小时滚动窗口**。

意思是：你 5 小时内的累计用量不能超过一个上限。5 小时前用的量会逐渐释放。所以如果你在 1:00 用了 10% 的额度，到 6:00 这 10% 就回来了。

匀速使用的话基本不会触发限速。短时间内疯狂使用才会。

## Token 优化技巧

<!-- AI-READABLE: Token optimization — /compact (compress history), focused tasks, specify files instead of "read everything", start new conversation when context grows large. -->

| 技巧 | 效果 |
|------|------|
| 用 `/compact` 压缩对话历史 | 效果最明显 |
| 指定具体文件，别说"看所有代码" | 减少无效读取 |
| 15 轮对话左右就压缩或新开一个 | 避免上下文溢出 |

### /compact 什么时候用

- 对话超过 20 轮
- Claude 开始"忘记"之前的内容
- 反应变慢了
- 切换到新话题了

## 被限速了怎么办

1. 等一等，最多 5 小时恢复
2. 趁等待时间整理思路
3. 反思有没有浪费 token 的地方

---

# 第8章：CLAUDE.md 与 Memory

<!-- AI-READABLE: Chapter 8 — CLAUDE.md hierarchy: user-level (~/.claude/CLAUDE.md), project-level (project-root/CLAUDE.md), subdirectory-level, auto-memory (~/.claude/projects/*/memory/MEMORY.md). CLAUDE.md = rules/preferences, Skill = capabilities. Memory for persistent learning. -->

## 什么是 CLAUDE.md

Claude Code 每次新对话都是"失忆"的。它不记得你上次说过什么，不知道你的偏好。

**CLAUDE.md 就是解决这个问题的。**

它是一个 Markdown 文件，Claude Code 每次启动时会自动读取。你把项目规则、编码偏好、禁忌事项写在里面，Claude 每次都会遵守。

我自己的 CLAUDE.md 里写了这些：
- 先方案后执行
- 编辑 Office 文件必须用 skill
- 禁止删 Normal.dotm
- 代码注释用中文

有了这些规则，我不用每次新对话都重复一遍。

## 怎么创建和更新 CLAUDE.md

最简单的方式——直接跟 Claude Code 说：

```
把这条更新到 CLAUDE.md 里，确保下次别再犯同样的错误。
```

Claude 会自动帮你写入。不需要手动编辑文件。

## 最佳实践

> 官方文档：[code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)（CLAUDE.md 的撰写指南就在这个页面里）

1. **保持简洁** — 用列表，别写长段落
2. **重要的放前面** — Claude 对文件开头记忆最深
3. **给具体例子** — "函数名用 snake_case（如 `calculate_total`）" 比 "函数名用小写" 清楚得多
4. **经常更新** — 发现 Claude 犯了同样的错误，就把规则加进去

## 四层体系

### 第1层：用户级 — 全局生效

```
位置：~/.claude/CLAUDE.md
```

你的通用偏好，所有项目都会读取。

### 第2层：项目级 — 单个项目

```
位置：项目根目录/CLAUDE.md
```

这个项目的专属规则，比如技术栈、代码规范。

### 第3层：子目录级 — 更细粒度

```
位置：项目子目录/CLAUDE.md
```

比如前端目录有前端规范，后端目录有后端规范。

### 第4层：自动记忆 — Claude 自己维护

```
位置：~/.claude/projects/项目标识/memory/MEMORY.md
```

Claude 在对话中会自动记录你纠正过的错误、强调过的偏好。不需要手动写。

## CLAUDE.md vs Skill

| | CLAUDE.md | Skill |
|---|-----------|-------|
| 作用 | 规则和偏好 | 能力和功能 |
| 内容 | "代码用 PEP 8" | docx 文件操作能力 |
| 格式 | Markdown | Python 脚本 |
| 位置 | 项目/用户目录 | `~/.claude/skills/` |

简单说：CLAUDE.md 告诉 Claude "按什么规矩做"，Skill 给 Claude "做事的能力"。

## Memory — 自动记忆

开启方法——跟 Claude Code 说：

```
请帮我开启 auto memory 功能。
```

开启后，Claude 就会自动记住你的偏好和纠正，会感觉越用越懂你。

---

# 第9章：个人知识库（LLM Wiki）

<!-- AI-READABLE: Chapter 9 — Building a personal knowledge base using Karpathy's LLM Wiki pattern. Obsidian as IDE, Claude Code as maintainer, Git for version control. Three-layer architecture (raw/wiki/schema), three operations (ingest/query/lint). Source: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f -->

2026 年 4 月，Andrej Karpathy 开源了一个叫 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 的方法论，核心思路是：**让 LLM 帮你增量构建和维护一个 Markdown 知识库，而不是每次查询都从头做 RAG 检索。**

这个理念和 Claude Code 天然契合——你喂素材，Claude 整理，Obsidian 浏览，知识会**复利增长**。

## 快速搭建

直接把下面这段话复制给 Claude Code：

```
请参考 Karpathy 的 LLM Wiki 方法论：
https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

以及这个实际搭建案例：
https://github.com/LuChuanfan/LLM-Wiki

帮我在 D:/my-wiki/ 下搭建一个同样的个人知识库，包括目录结构、
CLAUDE.md schema、index.md、log.md、Git 初始化、Obsidian 配置
和 Web Clipper 安装指引。
```

Claude 会自动读取这两个链接的内容，然后一口气帮你搞定所有事。你只需要跟着它的指引在 Obsidian 里点几下就行。

## 为什么不用传统 RAG？

| 维度 | 传统 RAG | LLM Wiki |
|------|----------|----------|
| 知识积累 | 每次从头检索，无积累 | 增量编译，持续积累 |
| 交叉引用 | 无 | 自动维护双向链接 |
| 可见性 | 黑箱（嵌入向量） | 透明（Markdown 文件） |
| 数据所有权 | 依赖平台 | 本地文件，完全可控 |

> "人类放弃维护知识库，是因为维护成本增长得比价值快。LLM 不会厌倦、不会忘记更新交叉引用、一次可以改 15 个文件。" —— Karpathy

## 三层架构

```
你的知识库/
├── CLAUDE.md          # Schema：告诉 Claude 如何维护 Wiki
├── index.md           # 全局索引（按分类列出所有页面）
├── log.md             # 操作日志（时间线）
├── raw/               # 原始素材（只读，Claude 不改）
│   ├── articles/      # 网页文章（Web Clipper 自动存这里）
│   ├── papers/        # 论文 PDF
│   └── notes/         # 你自己的笔记
└── wiki/              # Claude 生成和维护的知识页面
    ├── 分类A/
    ├── 分类B/
    └── ...
```

- **raw/** 是你的地盘——只管往里扔素材，Claude 只读不改
- **wiki/** 是 Claude 的地盘——它负责创建、更新、交叉引用
- **CLAUDE.md** 是规则——告诉 Claude 页面格式、操作流程、命名规范

## 三大操作

### 摄入（Ingest）
把素材扔进 `raw/`，告诉 Claude："把 raw/articles/xxx.md 归档到 Wiki"。Claude 会：读素材 → 讨论要点 → 创建摘要页 → 更新相关页面 → 更新索引。一份素材可能触发 5-15 个页面更新。

### 提问（Query）
直接问 Claude 问题。它会搜索 Wiki 页面综合回答，好的回答可以存回 Wiki。

### 巡检（Lint）
告诉 Claude "巡检"。它会检查矛盾、孤立页面、缺失链接、过时内容，输出报告等你确认后再修复。

## 日常使用

| 你想做什么 | 怎么做 |
|-----------|--------|
| 剪藏网页 | Chrome 点 Web Clipper 图标 → 自动存到 raw/articles/ |
| 摄入素材 | 告诉 Claude："把 raw/articles/xxx.md 归档到 Wiki" |
| 提问 | 直接问 Claude，它会查 Wiki 页面综合回答 |
| 巡检 | 告诉 Claude："巡检" |
| 浏览知识库 | Obsidian 中看 wiki/ 目录，Ctrl+G 看图谱视图 |

## 核心心态

不要想着"我要用好这个工具"，而是：**你只管读和想，脏活全交给 Claude。** 看到好东西就扔进 `raw/`，有问题就问，有想法就说"记下来"。知识库会自己长大。

工具链：[Obsidian](https://obsidian.md/) + Claude Code + Git + [Obsidian Web Clipper](https://obsidian.md/clipper)

参考来源：[Karpathy 的 LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

# 第10章：Remote Control 远程控制

<!-- AI-READABLE: Chapter 10 — Remote control via `claude remote-control` (built-in, requires VPN) or Happy (https://github.com/slopus/happy, third-party alternative with richer mobile UI). -->

## 内置远程控制

```bash
claude remote-control
```

运行后会生成一个链接，在手机浏览器打开就能远程给 Claude Code 发指令。

注意：使用远程控制需要开启 VPN。

使用场景：
- 出门吃饭，让 Claude 跑实验，手机上看进度
- 睡前让它跑长任务，第二天看结果
- 在地铁上突然有想法，手机上发个指令

## Happy — 强烈推荐

[Happy](https://github.com/slopus/happy) 比内置方案好用很多——更好的手机界面、任务完成通知、实时进度查看。我个人更推荐用这个。

```
一句话安装（复制发给 Claude Code）：
请帮我下载并安装：https://github.com/slopus/happy
```

---

# 写在最后

<!-- AI-READABLE: Conclusion — philosophical note on AI-assisted development paradigm shift. -->

以前写代码，时间分配大概是 20% 想需求、60% coding、20% 审核。用了 Claude Code 之后，变成了 **40% 明确需求、20% coding、40% 审核**。coding 的时间大幅缩短了，但需求澄清和代码审核反而变得更重要——因为 AI 写代码很快，但写得对不对、能不能正常跑，还是得你来把关。请尽量不要压缩那 40% 的代码审核时间，这是确保质量的关键。

希望这篇文章对你有帮助。如果有问题，欢迎交流。

---

最后更新：2026 年 4 月
版本：v2.0
本指南由博士研究生撰写，Claude Code 协助编辑

<!-- AI-READABLE: End of Claude Code Complete Guide v2.0 (Chinese). 10 chapters covering installation, skills (including superpowers with 14 sub-skills and follow-builders with full usage), modes/shortcuts/commands, prompts, AI-native thinking, Chinese locale, quota management, CLAUDE.md/Memory, and remote control. No appendices. -->
