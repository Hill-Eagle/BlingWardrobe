# OpenSpec 小白入门课程

> **课程依据**：本课程所有内容均来自 OpenSpec 官方仓库与官方文档，无自创教程、无虚构参数。
> 官方仓库：https://github.com/Fission-AI/OpenSpec
> 官方文档目录：https://github.com/Fission-AI/OpenSpec/tree/main/docs
> 官网：https://openspec.dev/

## 什么是 OpenSpec？（先看懂这一段再开始）

**OpenSpec** 是一个「规格驱动开发（Spec-driven Development，简称 SDD）」工具，专门配合 AI 编程助手（如 Claude Code、Cursor、GitHub Copilot、Codex 等三十多种）使用。

用大白话解释：以前你让 AI 写代码，只能发一句模糊的指令（比如"帮我加个暗色模式"），AI 怎么做全靠猜，结果不可预测。OpenSpec 的做法是：**先让 AI 写"计划文档"（规格），你审核确认后，AI 再按计划写代码**。这样结果就可预测、可审查了。

官方强调的设计哲学（引用自官方 README）：
- 流畅而非僵化（fluid not rigid）
- 迭代而非瀑布（iterative not waterfall）
- 简单而非复杂（easy not complex）
- 既支持存量项目也支持新项目（built for brownfield not just greenfield）

---

## ⚠️ 开始前必须搞懂的概念：两种命令，两个地方

这是官方入门文档（`docs/getting-started.md`）中特别强调的、**新手最容易犯的错误**：

| 命令类型 | 长什么样 | 在哪里输入 | 例子 |
|---------|---------|-----------|------|
| **终端 CLI 命令** | `openspec xxx` | 电脑的**终端**（Terminal / 命令行） | `openspec init` |
| **AI 聊天框斜杠命令** | `/opsx:xxx` | **AI 编程助手的聊天输入框**（比如 Claude Code 的对话框、Cursor 的 AI 对话框） | `/opsx:propose add-dark-mode` |

**记忆口诀**：带 `openspec` 前缀的 → 打终端；带 `/opsx:` 前缀的 → 打在 AI 聊天框。在终端里输入 `/opsx:propose` 是不会生效的！

本课程中所有命令都会明确标注所属类型。

---

## 📊 学习进度记录表

> 每学完一个小节，更新状态并在课程结束时提交到 GitHub（方法见附录 A）。

| 章节 | 状态【未开始/进行中/已完成】 | 实操是否通过 | 备注 |
|------|---------------------------|-------------|------|
| 第 1 章 环境准备 | 已完成 | ✅ | Node v22.12.0，高于官方要求的 20.19.0，合格 |
| 第 2 章 安装 OpenSpec | 已完成 | ✅ | npm 安装，openspec --version 验证通过（push 受网络影响，单独处理） |
| 第 3 章 项目初始化 | 进行中 | ❌ | |
| 第 4 章 目录结构讲解 | 未开始 | ❌ | |
| 第 5 章 核心斜杠命令 | 未开始 | ❌ | |
| 第 6 章 完整工作流实操 | 未开始 | ❌ | |
| 第 7 章 常用 CLI 命令 | 未开始 | ❌ | |
| 第 8 章 真实项目案例实操 | 未开始 | ❌ | |
| 第 9 章 常见坑与排错 | 未开始 | ❌ | |

---

# 第 1 章 环境准备

> 📌 本节依据官方文档：`docs/installation.md`（系统要求部分）

## 📖 理论知识点

1. **终端（Terminal）是什么**：一个用文字和电脑对话的窗口。macOS 上按 `Command + 空格` 打开 Spotlight 搜索，输入"终端"或"Terminal"回车即可打开。
2. **Node.js 是什么**：一个让 JavaScript 能脱离浏览器运行的环境。OpenSpec 本身就是一个用 Node.js 运行的程序，所以必须先装好它。
3. **官方硬性要求**：Node.js 版本必须 **≥ 20.19.0**（来源：`docs/installation.md`）。
4. **PATH 是什么**：系统查找命令的"地址簿"。如果装了软件但终端说"command not found"，通常就是它的安装位置没写进 PATH。

## 💻 实操指导

### 步骤 1：打开终端

macOS：`Command + 空格` → 输入 `终端`（或 Terminal）→ 回车。

### 步骤 2：检查 Node.js 是否安装及版本

以下命令属于 **终端 CLI 命令**（终端中输入）：

```bash
node --version
```

**校验成功标志**：输出类似 `v20.19.0`、`v22.x.x`、`v24.x.x` 等，只要 **主版本号 ≥ 20 且次版本号 ≥ 19（当主版本正好是 20 时）** 即可。例如：

```text
v20.19.0    ✅ 合格
v22.14.0    ✅ 合格
v18.20.0    ❌ 太低，需要升级
command not found ❌ 未安装 Node.js
```

### 常见报错排查

| 报错/现象 | 原因与解决 |
|----------|-----------|
| `command not found` | 未安装 Node.js。去 https://nodejs.org 下载 LTS 版本安装，或使用 nvm 等版本管理器安装后重试 |
| 显示 `v16.x.x` / `v18.x.x` 等低版本 | 版本过低。推荐使用 [nvm](https://github.com/nvm-sh/nvm) 安装并切换新版本：`nvm install 22` 后 `nvm use 22` |
| 装了新版本但还是显示旧版本 | 可能存在多个 Node.js，用 `which node` 查看实际使用的是哪个 |

## ✅ 课后小练习

1. 打开终端，成功运行 `node --version` 并记下你的 Node 版本号。
2. 回答：`/opsx:init` 应该在终端里输入还是在 AI 聊天框输入？（提示：看 `opsx` 这个名字本身——它根本不是 `openspec` 开头的命令）

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 2 章 安装 OpenSpec

> 📌 本节依据官方文档：`docs/installation.md`、官方 README 的 Installation 部分

## 📖 理论知识点

1. **npm 是什么**：Node.js 自带的"软件安装器"（包管理器），装好 Node.js 就自动有 npm。
2. **全局安装（`-g`）是什么意思**：让这个命令在任何目录、任何项目里都能用，而不是只装在某个项目里。OpenSpec 是全局工具，所以要加 `-g`。
3. **`@fission-ai/openspec` 是包名，`@latest` 表示装最新版**。
4. 官方支持多种安装方式：npm、Homebrew、pnpm、yarn、bun、deno、Nix。**小白推荐 npm 或 Homebrew（macOS）**。

## 💻 实操指导

以下均为 **终端 CLI 命令**。

### 方式一（推荐，装了 Node.js 就用这个）：npm 安装

```bash
npm install -g @fission-ai/openspec@latest
```

### 方式二（macOS 用 Homebrew）：

```bash
brew install openspec
```

> 不确定有没有 Homebrew？在终端运行 `brew --version`，有版本号输出就是装了；`command not found` 就先用方式一。

### 步骤 2：验证安装

```bash
openspec --version
```

**校验成功标志**：输出一个版本号（例如 `1.x.x` 形式的数字）。

### 步骤 3（可选但推荐）：关闭遥测数据上报

官方 README 说明：OpenSpec 默认开启匿名遥测，仅收集命令名和版本号（不含参数、路径、内容、个人信息），CI 环境自动禁用。如果你介意，可关闭（官方给出三种方式，任选其一）：

```bash
# 方式 1：用 CLI 配置（推荐）
openspec config set telemetry.enabled false
```

```bash
# 方式 2：环境变量（写入 ~/.zshrc 后 source 生效）
export OPENSPEC_TELEMETRY=0
```

```bash
# 方式 3：另一个标准环境变量
export DO_NOT_TRACK=1
```

### 常见报错排查

| 报错 | 原因与解决（来源：`docs/troubleshooting.md`） |
|------|----------------------------------------------|
| `zsh: command not found: openspec` | 已安装但全局 npm 的 bin 目录不在 PATH。运行 `npm prefix -g` 查看全局目录（macOS/Linux 下可执行文件在其 `bin/` 子目录），把该 `bin/` 路径加入 PATH |
| `EACCES` 权限错误 | npm 全局目录无写权限。可改用 Homebrew 安装，或用 nvm 管理 Node（nvm 的全局目录归用户所有） |
| Yarn 2+ 报错说没有 `global` 命令 | 官方说明：Yarn 2 起移除了 global 命令，改用 npm/pnpm/bun 安装 |

## ✅ 课后小练习

1. 安装成功后，运行 `openspec --version`，把版本号记录下来。
2. 尝试运行 `openspec --help`（这是查看帮助的通用方式），浏览一下输出的命令列表，不用记住。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 3 章 项目初始化

> 📌 本节依据官方文档：`docs/getting-started.md`、`docs/cli.md`（init 命令部分）

## 📖 理论知识点

1. **为什么要初始化**：OpenSpec 需要在一个项目里建立自己的"工作文件夹"（`openspec/`），并给配置好的 AI 工具生成对应的命令/技能文件，AI 才能听懂 `/opsx:` 斜杠命令。
2. **`init` 是交互式的**：运行后会让你用方向键+回车选择要配置哪些 AI 工具。
3. **官方说明（README）**：`init` 会打印出你所选工具的正确命令形式，例如 Cursor / GitHub Copilot 是 `/opsx-propose`，Amazon Q 是 `@opsx-propose`，Codex 是 `$openspec-propose`——**不同工具的斜杠命令写法可能不同，以 init 输出的提示为准**。
4. 初始化后创建的结构（来源：`docs/cli.md`）：`openspec/specs/`、`openspec/changes/`、`openspec/config.yaml`，以及所选工具的技能/命令目录。

## 💻 实操指导

### 步骤 1：准备一个练习项目

用本课程推荐的方式，新建一个空文件夹来练习（**终端 CLI 命令**）：

```bash
mkdir openspec-demo && cd openspec-demo
```

解释：`mkdir` 创建文件夹，`cd` 进入该文件夹。之后的命令都在这个文件夹里执行。

### 步骤 2：初始化（终端 CLI 命令）

```bash
openspec init
```

按提示用方向键和空格选择你正在用的 AI 工具（比如 `claude`、`cursor`、`github-copilot` 等），回车确认。

### 也可以用非交互方式一步到位（终端 CLI 命令）

```bash
openspec init --tools claude,cursor
```

`--tools` 后面跟工具 ID，多个用逗号隔开。常用工具 ID（来源：`docs/cli.md` 完整列表）：`claude`、`cursor`、`codex`、`github-copilot`、`gemini`、`kimi`、`codebuddy`、`qwen`、`trae`、`roocode` 等；`--tools all` 配置所有，`--tools none` 跳过。

### 校验成功标志

1. 终端显示初始化完成的提示（并打印你所用工具的斜杠命令正确形式，**截图/记下它**）。
2. 运行 `ls` 能看到新增了 `openspec/` 目录（以及所选工具对应的隐藏目录，如 `.claude/`、`.cursor/` 等）。
3. 运行 `ls openspec` 能看到 `specs` 和 `changes` 等目录。

### 常见报错排查

| 报错/现象 | 原因与解决（来源：`docs/troubleshooting.md`） |
|----------|----------------------------------------------|
| 提示发现旧文件需要清理但卡住（非交互环境） | 运行 `openspec init --force`（自动清理旧文件并跳过提示） |
| init 后 AI 工具里看不到 `/opsx:` 命令 | ① 重启 AI 工具（多数工具启动时扫描命令文件）；② 在项目根目录运行 `openspec update` 重新生成；③ 确认你在 init 时勾选了正确的工具 |
| 在错误的目录里运行了 init | skills/命令文件按项目写入，必须在你实际打开 AI 助手的那个项目目录里运行 init |

## ✅ 课后小练习

1. 在 `openspec-demo` 项目里完成初始化，并找到 init 打印的"你的工具的正确命令形式"记录下来。
2. 用 `ls openspec` 确认目录已生成。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 4 章 目录结构讲解

> 📌 本节依据官方文档：`docs/getting-started.md`（After Init 目录结构部分）

## 📖 理论知识点

初始化后的完整目录结构（官方原文结构）：

```
openspec/
├── specs/                      # 规格源（描述系统"当前"的行为）
│   └── <domain>/               # 按领域组织，如 auth/、payments/、ui/
│       └── spec.md
├── changes/                    # 提议的变更（每个变更一个文件夹）
│   └── <change-name>/
│       ├── proposal.md         # 提案：为什么做、改什么
│       ├── design.md           # 设计：技术方案与架构决策
│       ├── tasks.md            # 任务清单（带复选框）
│       └── specs/              # 增量规格（描述"变化"的内容）
│           └── <domain>/
│               └── spec.md
└── config.yaml                 # 项目配置（可选）
```

**两个核心目录的角色**（必须理解）：

| 目录 | 作用 | 通俗比喻 |
|------|------|---------|
| `openspec/specs/` | 系统**当前**行为的唯一事实来源，按领域组织 | 房子的"现状图纸" |
| `openspec/changes/` | 每个提议的改动一个文件夹，完成后其中的规格会合并回 `specs/` | "装修方案"，批准后并把结果更新回现状图纸 |

**变更文件夹内的 4 个工件（artifact）及依赖顺序**（来源：`docs/getting-started.md`）：

`proposal ──► specs ──► design ──► tasks ──► implement`

| 文件 | 回答的问题 |
|------|-----------|
| `proposal.md` | 为什么做这个改动？改什么？ |
| `specs/` | 改完之后需求是什么样？（用"需求 + 场景"格式写） |
| `design.md` | 技术上怎么做？ |
| `tasks.md` | 具体拆成哪些步骤？（带 `- [ ]` 复选框） |

实现过程中可以随时回头修订早期工件——这是官方强调的"迭代而非瀑布"。

## 💻 实操指导

### 步骤 1：查看你项目里的实际结构

**终端 CLI 命令**：

```bash
find openspec -type f
```

或用树状查看（macOS 可能需要 `brew install tree`）：

```bash
tree openspec
```

### 步骤 2：打开 `openspec/config.yaml` 看一眼

```bash
cat openspec/config.yaml
```

**校验成功标志**：能列出 specs、changes 目录；config.yaml 存在且内容可读。

### 步骤 3：看官方规格文件的写法格式

官方 README 中的规格示例格式（纯 Markdown，无特殊语法）：

```markdown
## ADDED Requirements

### Requirement: Theme selection
The app SHALL let users switch between light and dark themes,
defaulting to the system preference.

#### Scenario: User toggles dark mode
- **WHEN** the user clicks the theme toggle
- **THEN** the app switches to dark mode and persists the choice
```

现在你的 `specs/` 目录还是空的（因为还没有归档过任何变更），这正常。

## ✅ 课后小练习

1. 画出（或手写）你项目 `openspec/` 目录的树状结构。
2. 用自己的话说一遍：`specs/` 和 `changes/` 的区别是什么？

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 5 章 核心斜杠命令

> 📌 本节依据官方文档：`docs/commands.md`、官方 README

## 📖 理论知识点

### 5.1 命令分两组 profile（配置集）

| Profile | 包含命令 | 说明 |
|---------|---------|------|
| **默认 `core` profile** | `propose`、`explore`、`apply`、`update`、`sync`、`archive` | 全局默认，覆盖完整流程的最短路径 |
| **扩展 profile（expanded）** | `new`、`continue`、`ff`、`verify`、`bulk-archive`、`onboard` | 需手动启用（见 5.4） |

### 5.2 默认 profile 的 6 个命令（来源：`docs/commands.md`）

| 命令 | 作用（一句话） |
|------|---------------|
| `/opsx:explore [主题]` | 无风险"思考伙伴"：读代码、比较方案、把模糊想法变成计划，**不写代码** |
| `/opsx:propose [名称或描述]` | 一步创建变更并生成全部规划工件（proposal、specs、design、tasks），结束即可进入 apply |
| `/opsx:apply [变更名]` | 按 tasks.md 清单逐个实现：写代码、建文件、跑测试、勾选 `[x]`；可中断续做 |
| `/opsx:update [变更名]` | 修订已有规划工件并保持一致，**只改规划工件不改代码** |
| `/opsx:sync [变更名]` | （可选）把变更的增量规格合并进主 specs，但**不归档**变更 |
| `/opsx:archive [变更名]` | 归档完成的变更到 `changes/archive/YYYY-MM-DD-<名称>/`，并合并规格 |

### 5.3 扩展 profile 的 6 个命令

| 命令 | 作用 |
|------|------|
| `/opsx:new [名称]` | 只创建变更脚手架（目录+元数据），后续用 continue/ff 逐个生成工件 |
| `/opsx:continue [变更名]` | 一次创建**下一个**就绪工件，适合想逐个审查的场景 |
| `/opsx:ff [变更名]` | 快进：按依赖顺序一次性生成全部规划工件 |
| `/opsx:verify [变更名]` | 验证实现与工件是否匹配，按 CRITICAL/WARNING/SUGGESTION 分级报告，不阻塞归档 |
| `/opsx:bulk-archive [名称...]` | 批量归档多个变更，自动检测跨变更的规格冲突 |
| `/opsx:onboard` | 交互式教程：用你的**真实代码库**走完整工作流，11 个阶段，约 15–30 分钟 |

### 5.4 如何启用扩展 profile

**终端 CLI 命令**（两步，来源：`docs/commands.md`）：

```bash
openspec config profile   # 交互式选择 workflows
openspec update           # 把新命令写入你的 AI 工具
```

### 5.5 不同 AI 工具的命令写法差异（重要！）

`/opsx:` 是官方文档的规范写法，但各工具注册形式不同（来源：`docs/commands.md`）：

| 工具 | 实际输入形式 |
|------|-------------|
| Claude Code、Gemini CLI、Crush | `/opsx:propose` |
| Cursor、Copilot（IDE）、Trae | `/opsx-propose` |
| Codex CLI | `$openspec-propose` |
| Kimi Code | `/skill:openspec-propose` |
| Amazon Q | `@opsx-propose` |
| Zed Agent、ForgeCode 等 | `/openspec-propose` |

**拿不准就回去看第 3 章 init 输出的提示，以它为准。**

## 💻 实操指导

以下操作在你的 AI 编程助手**聊天框**中进行（**AI 聊天框斜杠命令**），项目目录要处于 `openspec-demo`。

### 步骤 1：试运行 explore

```
/opsx:explore 我想给我的网站加一个留言板功能
```

**校验成功标志**：AI 开始分析、阅读代码、讨论方案，但**不创建任何代码文件**。

### 步骤 2：查看当前有哪些命令生效

在终端确认命令文件已生成（**终端 CLI 命令**，以 Claude Code 为例）：

```bash
ls .claude/skills/
```

应能看到 `openspec-*` 相关文件夹。如果你用的不是 Claude Code，对照 `docs/supported-tools.md` 查看你的工具的文件位置。

### 常见报错排查

| 现象 | 解决（来源：`docs/troubleshooting.md`） |
|------|----------------------------------------|
| 聊天框里 `/opsx:propose` 无响应/不补全 | ① 确认输入在聊天框而非终端；② 终端运行 `openspec update` 后重启 AI 工具；③ 确认 init 时勾选了该工具 |
| 用的工具是 Codex/Amazon Q 等 | 它们不用 `/opsx:` 形式，按 5.5 表格换对应写法 |
| 提示 "Change not found" | 先 `openspec list` 看有哪些变更，或在命令后面显式写上变更名 |

## ✅ 课后小练习

1. 在 AI 聊天框成功运行一次 `/opsx:explore`（主题随意）。
2. 确认你的工具中 `/opsx:` 命令的实际写法并记录。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 6 章 完整工作流实操

> 📌 本节依据官方文档：`docs/getting-started.md`（默认工作流）、`docs/commands.md`

## 📖 理论知识点

### 6.1 官方默认工作流（core profile）

```
/opsx:explore ──► /opsx:propose ──► /opsx:apply ──► /opsx:sync(可选) ──► /opsx:archive
```

官方入门文档的前五分钟流程原文：

```text
TERMINAL  $ npm install -g @fission-ai/openspec@latest   # 全局安装（已做）
TERMINAL  $ cd your-project && openspec init             # 初始化（已做）
AI CHAT   /opsx:explore    （可选：先想清楚要做什么）
AI CHAT   /opsx:propose add-dark-mode   （AI 起草计划，你审核）
AI CHAT   /opsx:apply                   （AI 动手实现）
AI CHAT   /opsx:archive                 （规格更新，变更归档）
```

### 6.2 归档时的合并规则（来源：`docs/getting-started.md`）

- `ADDED` 需求 → 追加到主规格
- `MODIFIED` 需求 → 替换主规格中现有版本
- `REMOVED` 需求 → 从主规格删除
- 变更文件夹整体移入 `openspec/changes/archive/` 留作审计记录

### 6.3 官方使用建议（来源：README）

- **模型选择**：建议使用高推理能力模型。
- **上下文卫生**：实现前清空上下文窗口，会话中保持良好的上下文管理。

## 💻 实操指导

实操环境：`openspec-demo` 项目 + 你配置好的 AI 工具。

### 第 1 步：探索（AI 聊天框斜杠命令）

```
/opsx:explore 给 demo 项目加一个简单的待办事项功能
```

### 第 2 步：创建变更提案（AI 聊天框斜杠命令）

```
/opsx:propose add-todo-list
```

**校验成功标志**：AI 报告创建了 `openspec/changes/add-todo-list/`，包含 `proposal.md`、`specs/`、`design.md`、`tasks.md`。

此时你**必须人工审核**生成的文件——打开 `tasks.md` 和 `proposal.md` 检查内容是否合理，不合理就提修改意见让 AI 改（这正是规格驱动开发的价值：写代码前先把计划审清楚）。

也可以用终端随时查看（**终端 CLI 命令**）：

```bash
openspec list                     # 列出活跃变更
openspec show add-todo-list       # 查看变更详情
openspec validate add-todo-list   # 校验规格格式
```

### 第 3 步：实现（AI 聊天框斜杠命令）

```
/opsx:apply
```

**校验成功标志**：AI 逐个执行任务，完成后 `tasks.md` 中的复选框变为 `- [x]`。

### 第 4 步：归档（AI 聊天框斜杠命令）

```
/opsx:archive
```

**校验成功标志**：
1. AI 报告规格已合并进 `openspec/specs/`；
2. 变更目录被移动到 `openspec/changes/archive/2026-xx-xx-add-todo-list/`（日期为当天）。

终端验证（**终端 CLI 命令**）：

```bash
ls openspec/changes/
ls openspec/specs/
```

### 完整生命周期终端备用命令

```bash
openspec status --change add-todo-list    # 查看工件完成状态
openspec archive add-todo-list --yes      # 终端直接归档（跳过确认）
```

## ✅ 课后小练习

1. 完整走一遍 `explore → propose → apply → archive` 全流程。
2. 归档后查看 `openspec/specs/` 下新生成的规格文件，确认"现状图纸"已被更新。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 7 章 常用 CLI 命令

> 📌 本节依据官方文档：`docs/cli.md`

以下全部是 **终端 CLI 命令**。

## 📖 理论知识点

CLI 命令按官方分类总览：

| 类别 | 命令 | 用途 |
|------|------|------|
| 初始化 | `init`、`update` | 初始化和更新项目中的 OpenSpec |
| 浏览 | `list`、`view`、`show` | 探索变更和规格 |
| 验证 | `validate` | 检查变更和规格的问题 |
| 生命周期 | `archive` | 归档已完成的变更 |
| 工作流 | `new change`、`status`、`instructions`、`schemas` | 工件驱动的工作流支持 |
| 模式 | `schema init / fork / validate / which` | 自定义工作流 |
| 配置 | `config` | 查看和修改设置 |
| 反馈 | `feedback` | 一键创建 GitHub issue（需安装 `gh`） |

全局选项（所有命令通用）：`--version`（-V）、`--no-color`、`--help`（-h）。

## 💻 实操指导（逐个试）

### 浏览类

```bash
openspec list                # 列出活跃变更（默认）
openspec list --specs        # 列出规格
openspec show add-todo-list  # 显示某变更详情
openspec view                # 交互式仪表盘（仅限人类交互使用）
```

### 验证类

```bash
openspec validate                     # 验证单项
openspec validate --all               # 验证全部变更和规格
openspec validate --all --strict      # 严格模式，适合 CI
openspec validate --archived          # 检查归档变更的任务是否全部完成
```

**校验成功标志**：输出无 error；退出码 0。

### 生命周期

```bash
openspec archive add-todo-list --yes   # 归档（--yes 跳过确认；脚本/CI/管道中必填）
```

### 状态与指令

```bash
openspec status --change add-todo-list   # 工件状态；第一个 ready 条目即下一步该写的工件
openspec schemas                          # 列出可用工作流模式（默认 spec-driven）
```

### 配置类

```bash
openspec config path                       # 配置文件位置
openspec config list                       # 所有当前设置
openspec config get telemetry.enabled      # 读取某项
openspec config set telemetry.enabled false  # 关闭遥测
```

### 更新类（升级后必做）

```bash
npm install -g @fission-ai/openspec@latest   # 升级 CLI
openspec update                              # 在每个项目内刷新 AI 指引和斜杠命令
```

### 补全与反馈（可选）

```bash
openspec completion install zsh   # 安装 zsh 补全（支持 bash/zsh/fish/powershell）
openspec feedback "描述你的问题"   # 需要 gh 已登录
```

### 官方"人类专用 vs 代理可用"命令区分

- **仅限人类交互**：`init`、`view`、`workset open`、`config edit`、`feedback`、`completion install`
- **支持 `--json`（可供 AI 代理/脚本使用）**：`list`、`show`、`validate`、`status`、`instructions`、`templates`、`schemas`、所有 `store` 子命令、`new change` 等

## ✅ 课后小练习

1. 运行 `openspec list` 与 `openspec list --specs`，说出两者输出区别。
2. 运行 `openspec validate --all`，确认全部通过。
3. 运行 `openspec view` 体验仪表盘，按提示退出。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 8 章 真实项目案例实操（存量项目 brownfield 采纳）

> 📌 本节依据：官方 README（brownfield 设计哲学）、`docs/getting-started.md`、`docs/commands.md`（verify/onboard）

## 📖 理论知识点

1. 官方 README 明确 OpenSpec "built for brownfield not just greenfield"——**专为存量项目设计**，你现有的代码不用动，只是在项目里多一个 `openspec/` 文件夹。
2. 官方提供 `/opsx:onboard`：用**你的真实代码库**走一遍完整工作流（发现改进点 → 创建变更 → 写工件 → 实现 → 验证 → 归档），共 11 个阶段，每步有解说，耗时约 15–30 分钟。这是官方推荐给新用户的最佳实战入口。
3. 多个变更可并行：不同变更各占一个文件夹，`/opsx:verify` 可在归档前核对实现与规格是否一致（按 CRITICAL/WARNING/SUGGESTION 分级报告，不阻塞归档）。

## 💻 实操指导

以本工作区的真实项目（例如 `BlingMirror`）为例：

### 步骤 1：在真实项目根目录初始化（终端 CLI 命令）

```bash
cd /path/to/你的真实项目
openspec init
```

**校验成功标志**：真实项目里出现 `openspec/` 目录。

### 步骤 2：跑官方交互式教程（AI 聊天框斜杠命令）

```
/opsx:onboard
```

按 AI 的引导走完 11 个阶段。教程创建的变更可以保留也可以丢弃，任选。

### 步骤 3：走一个真实的小需求（AI 聊天框斜杠命令）

```
/opsx:propose 修复一个你观察到的小问题或加一个小功能
```

然后：

```
/opsx:apply
/opsx:verify
/opsx:archive
```

**校验成功标志**：`/opsx:verify` 输出的报告没有 CRITICAL 级问题；归档后 `openspec/changes/archive/` 出现以当天日期命名的新文件夹。

### 建议同步提交到 Git（终端 CLI 命令）

```bash
git add openspec/
git commit -m "chore: adopt OpenSpec spec-driven workflow"
```

> 官方提示（`docs/installation.md` 卸载部分反向印证）：`openspec/specs/` 与 `changes/archive/` 保存了系统行为与变更历史，建议保留在 git 中。

## ✅ 课后小练习

1. 在真实项目里跑完 `/opsx:onboard`（或完整走一个小需求的四条命令）。
2. 用 `openspec list` 确认没有遗留的未归档变更。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 第 9 章 常见坑与排错

> 📌 本节依据官方文档：`docs/troubleshooting.md`

## 📖 理论知识点

排错总原则：先确认**命令类型**（终端 CLI vs AI 聊天斜杠命令），再确认**项目目录**，最后确认**文件是否生成**。

## 💻 实操指导：官方排错速查表

### 坑 1：`openspec: command not found`

```bash
npm install -g @fission-ai/openspec@latest
openspec --version
# 仍找不到：npm prefix -g 查看全局目录，把其 bin/ 加入 PATH
```

### 坑 2：提示 "Requires Node.js 20.19.0 or higher"

```bash
node --version    # 升级 Node 到 20.19.0+
```

### 坑 3：`/opsx:` 命令不出现（官方给出按顺序排查的 6 步）

1. 确认输在 **AI 聊天框**，不是终端；
2. 项目根目录运行 `openspec update` 重新生成命令文件（指令文件来自已安装的 CLI，过时的 CLI 会报"一切最新"却从不写入新命令——update 会检测并提示升级，**接受提示**）；
3. **重启 AI 工具**（多数工具启动时才扫描命令文件）；
4. 确认文件存在（如 Claude Code 看 `.claude/skills/` 里的 `openspec-*` 文件夹）；
5. 确认在该项目目录运行过 `openspec init`；
6. Codex/Amazon Q/Kimi 等工具不用 `/opsx:` 形式（见第 5 章 5.5 表）。

### 坑 4："Change not found"

```bash
openspec list                 # 查看活跃变更
/opsx:apply add-todo-list     # 聊天框中显式指定变更名
```

同时确认你在正确的项目目录中。

### 坑 5："No artifacts ready"

```bash
openspec status --change <变更名>
```

记住依赖顺序：**proposal 启用 specs 和 design；specs 和 design 共同启用 tasks**。先补齐缺失的依赖工件。

### 坑 6：`openspec validate` 报错

- 常见原因：缺少必需部分（如 spec 没有场景）、delta 头格式错误；
- 特殊报错 `MODIFIED "" omits scenario(s)...`：`MODIFIED` 需求会**替换整个需求块**，必须把变更后要保留的所有场景都写进 delta（从 `openspec/specs/<...>/spec.md` 复制回来），而不只是你改的那几条。

### 坑 7："User force closed the prompt with 0 null"

在无法交互的环境（agent 工具调用、CI、stdin 关闭）运行了 `openspec archive`。解决：

```bash
openspec archive <变更名> --yes
```

### 坑 8：`config.yaml` 不生效

三个官方列出的原因：① 文件名必须是 `config.yaml` 不能是 `.yml`；② YAML 语法无效（CLI 会报带行号的错误）；③ 不需要重启，配置立即生效。

### 坑 9：AI 生成的工件不完整

官方给出的 4 个办法：在 `openspec/config.yaml` 添加项目上下文；为工件添加 `rules:` 规则；提案时给更详细描述；用 `/opsx:continue` 逐个生成替代 `/opsx:ff` 一次生成。

### 坑 10：`MODIFIED`/归档警告任务未完成

归档不会**阻止**未完成任务，只会警告。故意保留部分任务可直接继续；否则先完成。delta specs 未同步时归档会提示同步——没有特殊理由建议选"是"。

### 仍无法解决？

- Discord：https://discord.gg/YctCnvvshC
- GitHub Issues：https://github.com/Fission-AI/OpenSpec/issues
- 终端一键反馈：`openspec feedback "出了什么问题"`（自动创建 issue）
- 报问题时附上：`openspec --version`、`node --version`、所用 AI 工具、确切命令和输出。

## ✅ 课后小练习

1. 故意在一个没有初始化的目录输入 `/opsx:propose`（聊天框），观察失败，然后按坑 3 的流程修复它。
2. 整理一份属于你自己的排错卡片（3 条你印象最深的坑）。

**完成后回复【继续】，遇到报错请把报错信息粘贴给我。**

---

# 附录 A：每节课更新进度并记录到 GitHub

学完每一章后，编辑本文件顶部的「学习进度记录表」：

1. 把对应章节的「状态」改为 `已完成`；
2. 把「实操是否通过」改为 ✅；
3. 在「备注」写一句你的收获或踩的坑。

然后在终端提交到 GitHub（**终端 CLI 命令**）：

```bash
git add OpenSpec入门课程.md
git commit -m "docs: 完成 OpenSpec 课程第 X 章（含实操记录）"
git push
```

# 附录 B：卸载 OpenSpec（学完不想用了）

来源：`docs/installation.md`

```bash
# 1. 移除全局包
npm uninstall -g @fission-ai/openspec

# 2.（可选）移除项目中的 openspec 目录
rm -rf openspec/

# 3.（可选）删除各 AI 工具目录下生成的技能/命令文件
#    如 .claude/skills/openspec-*/、.cursor/commands/opsx-* 等
```

> ⚠️ 官方提醒：`openspec/specs/` 和 `openspec/changes/archive/` 保存了系统行为和变更历史，删除前请谨慎考虑。

# 附录 C：官方参考链接汇总

| 内容 | 链接 |
|------|------|
| 官方仓库 | https://github.com/Fission-AI/OpenSpec |
| 官网 | https://openspec.dev/ |
| 入门指南 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md |
| 安装 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/installation.md |
| 斜杠命令 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/commands.md |
| CLI 参考 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/cli.md |
| 排错 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/troubleshooting.md |
| 支持的工具 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/supported-tools.md |
| 概念 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md |
| 工作流 | https://github.com/Fission-AI/OpenSpec/blob/main/docs/workflows.md |
| FAQ | https://github.com/Fission-AI/OpenSpec/blob/main/docs/faq.md |
