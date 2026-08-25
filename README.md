<p align="center">
  <img src="assets/banner.png" alt="yotta-workflow banner" width="100%" />
</p>

<h1 align="center">yotta-workflow · 元序</h1>

<p align="center">一套面向所有 AI 智能体的通用工作流标准：<b>流程全局定、状态就近存；开工必读状态，收工必留锚点</b>。让任何 AI 会话都能无痛接续，避免单会话拉长而失忆。</p>
<p align="center">状态目录统一 <code>.workflow</code>——同一项目所有智能体会话读写同一份状态（一个真相源）；开工读状态恢复上下文、进行中主动落盘流水 / 任务 / 决策、收工生成自包含交接锚点。</p>
<p align="center">纯 Markdown 文本，零依赖、不注入、不锁平台；安装一次，Claude Code / Codex / Cursor / OpenCode 等 78+ 智能体通用。</p>

<p align="center">
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-blue" /></a>
  <a href="https://agentskills.io/"><img alt="Standard: agentskills.io" src="https://img.shields.io/badge/standard-agentskills.io-orange" /></a>
  <a href="https://www.npmjs.com/package/@yottameta/yotta-workflow"><img alt="npm package" src="https://img.shields.io/npm/v/@yottameta/yotta-workflow" /></a>
  <a href="https://github.com/YottaMeta/yotta-workflow"><img alt="GitHub stars" src="https://img.shields.io/github/stars/YottaMeta/yotta-workflow" /></a>
  <a href="https://github.com/YottaMeta/yotta-workflow/commits/main"><img alt="last commit" src="https://img.shields.io/github/last-commit/YottaMeta/yotta-workflow" /></a>
  <a href="https://github.com/YottaMeta/yotta-workflow"><img alt="PRs welcome" src="https://img.shields.io/badge/PRs-welcome-brightgreen" /></a>
</p>

## 这是什么

AI 会话本身是无状态的：每次对话相互独立，聊得越长越容易失忆，换个会话或换个智能体就接不上前文。各平台自带的记忆方案通常只服务单一智能体，不同智能体各记各的，还会产生多个「真相源」。

yotta-workflow 把「跨会话协作」沉淀为一套与智能体无关的协议，回答三个问题：

- **状态放哪里、以什么格式记录**——统一由规则判定，不靠各智能体自由发挥。
- **什么时候读、什么时候写**——开工必读、进行中主动写、收工必留锚点。
- **交接怎么交**——固定模板生成自包含交接锚点，下个会话只凭锚点即可无痛接续。

它不依赖任何特定智能体或平台：状态就是项目目录下的 Markdown 文件，任何智能体、任何工具都能读能写。

## 核心价值

- **一个真相源**：同一项目所有智能体会话读写同一份 `.workflow\` 状态目录，不再各建目录、各记各的。
- **状态就近存**：以会话 cwd 为基准判定状态位置，不写死任何默认路径；项目根就近存、工作区按项目名分开存。
- **主动防失忆**：进行中每完成一件事就落盘流水 / 任务 / 决策，不靠对话记忆（上下文会被自动压缩）。
- **自包含交接**：收工生成固定格式交接锚点，下个会话只凭锚点 + 状态文件即可恢复全部上下文。
- **与既有机制兼容**：项目已有自己的交接 / 状态机制时沿用原机制，只需满足两个强制点——开工先读状态、收工更新状态并留锚点。

## 核心优势

| 优势 | 说明 |
|---|---|
| **跨智能体统一** | 符合 Agent Skills 开放标准（agentskills.io），安装一次，78+ 智能体共用同一套状态协议 |
| **一个真相源** | 状态目录统一 `.workflow`，同一项目任何智能体读写同一份状态，杜绝多真相源 |
| **路径判定自动化** | 先取 cwd → 判断是项目根还是工作区 → 就近存或按项目名分开存；全程不写死路径 |
| **主动式落盘** | 进行中即时写流水 / 任务 / 决策，上下文压缩也不丢关键状态 |
| **自包含交接锚点** | 固定模板 + 强制校验（内容必须与状态文件一致），下个会话无痛接续 |
| **轻量零依赖** | 纯 Markdown 文件，无 daemon / 无数据库 / 无注入；任何平台可读可写 |
| **渐进采用** | 已有状态机制的项目可沿用原机制，只需满足两个强制点，迁移成本低 |
| **生态分发** | GitHub + npm 双源同步发布；npx / install.sh / 手动复制三种安装方式，覆盖 17+ 类智能体目录 |

## 协议详解

### 状态文件位置判定（口诀）

**先取 cwd，再看它是不是项目根；是项目根就就近存，是工作区就按项目名分开存；状态目录统一用 `.workflow`，与所用智能体无关；全程不写死任何默认 / 固定路径。**

| base 形态 | 状态目录 |
|---|---|
| 项目根目录（含 `.git`、项目配置，或用户明确指向的单一项目） | `<base>\.workflow\` |
| 工作区根目录（下面并列多个项目子目录） | `<base>\<项目名>\.workflow\` |

> 用户指定的项目目录 / 统一工作区根目录按用户约定作为基准；未指定时以会话开始时的 cwd 为基准。

### 项目状态体系（五类文件）

| 文件 | 内容 |
|---|---|
| `STATE.md` | 当前进度 / 最近决定 / 遗留问题 / 下一步（下个会话恢复的关键） |
| `TASKS.md` | 任务清单（`- [ ]` 待办 / `- [x]` 已完成 / `- [~]` 进行中） |
| `DECISIONS.md` | 决策记录（每条含背景 / 决定 / 理由 / 备选） |
| `ROADMAP.md` | 长期目标 + 下一步计划 |
| `logs\YYYY-MM-DD.md` | 每天一份流水（做了什么 / 产出什么 / 踩了什么坑） |

### 三段式协议

**开工（每次会话开始必做）**：按判定规则定位状态目录 → 存在则完整读取 STATE / TASKS / ROADMAP / DECISIONS 与近期 logs 恢复上下文；不存在则初始化全部文件并向用户确认；一个会话只交付一个里程碑。

**进行中（主动及时写，不靠记忆）**：每完成一件事就追加当天流水；任务状态实时更新 TASKS；方向性决定当场写入 DECISIONS；STATE 的「当前进度」保持最新；关键信息必须已落盘，不能只留在对话里。

**收工（每次会话结束必做）**：更新 STATE / TASKS / ROADMAP → 追加当天流水 → 按模板生成交接锚点，原样输出给用户复制。

### 交接锚点格式

收工时按固定模板输出，锚点必须自包含，内容必须与状态文件一致、不得凭空编写。完整模板见 SKILL.md「五、交接话术模板」，结构要点：

| 段 | 内容 |
|---|---|
| 头部 | 项目名（一句话定位）、项目根目录绝对路径、上次会话结束日期 |
| 进度 | 当前进度、已完成（与 STATE.md 一致） |
| 后续 | 下一步（按优先级）、关键决定、遗留问题 / 注意 |
| 结尾 | 开工请先读取：`.workflow\STATE.md`、TASKS.md、ROADMAP.md |

## 使用示例

**开工**——先读状态，再谈任务：

```text
请先读取 .workflow\STATE.md、TASKS.md、ROADMAP.md，恢复项目上下文。
```

**进行中**——完成一件事，立即落盘：

```text
已完成「xxx」，追加到 logs\2026-08-25.md；勾选 TASKS.md 对应项；更新 STATE.md 当前进度。
```

**收工**——按模板生成交接锚点：

```text
给你的下个会话锚点

【会话交接锚点】
项目：<项目名>（<一句话定位>）
路径：<项目根目录绝对路径>
上次会话结束于：<日期>
当前进度：…
下一步（按优先级）：…
开工请先读取：.workflow\STATE.md、TASKS.md、ROADMAP.md
```

## 触发方式

在以下场景使用本技能：

- 开始或结束一个工作会话，或恢复一个项目时。
- 项目状态发生变化时（完成任务 / 记录决策 / 更新路线图）。
- 需要给下一个会话留下自包含交接锚点，或读取已有交接锚点时。

针对一次性的只读提问（如「这个函数什么意思」）不必触发本技能。

## 安装

三种方式任选其一，技能文件统一从 **npm** 获取（GitHub 无代理时较慢，npm 可配国内镜像加速）。

### 方式一：npm（推荐，一行安装）
```bash
# 国内加速（可选）：npm config set registry https://registry.npmmirror.com
npx -y @yottameta/yotta-workflow -g
npx -y @yottameta/yotta-workflow --dir <你的技能目录>   # 任意智能体：指定目录安装
```
> 智能体不在预置列表里？用 `--dir` 指定它的 skills 目录，或手动复制（方式三）。`--list` 可查看各智能体对应的默认目录。

### 方式二：install.sh 一键安装
获取技能文件夹后（`npm pack` 解包或 `git clone`），进入技能文件夹：
```bash
bash install.sh -g    # 用户级；bash install.sh --list 查看全部目录
bash install.sh --agent codex   # 指定智能体（--list 可查看可用项）
bash install.sh       # 项目级：自动检测已存在的 .claude/.cursor/.codex 等 skills 目录
bash install.sh --dir /path/to/skills
```
> 覆盖 17 类智能体，含国内 Trae / Qwen / Comate / CodeBuddy / Kimi。Windows 用户：装有 Git Bash 即可用；否则用方式三手动复制。

### 方式三：手动复制
把整个 `yotta-workflow` 文件夹复制到目标智能体的 skills 目录。常见位置（用户级；Windows 用 `%USERPROFILE%`，Linux/macOS 用 `~`）：

| 智能体 | 用户级目录 | 项目级目录 |
|---|---|---|
| Codex | `%USERPROFILE%\.codex\skills\yotta-workflow\` | `.codex\skills\` |
| Claude Code | `%USERPROFILE%\.claude\skills\yotta-workflow\` | `.claude\skills\` |
| Cursor | `%USERPROFILE%\.cursor\skills\yotta-workflow\` | `.cursor\skills\` |
| Windsurf | `%USERPROFILE%\.codeium\windsurf\skills\yotta-workflow\` | `.windsurf\skills\` |
| opencode | `%USERPROFILE%\.config\opencode\skills\yotta-workflow\` | `.opencode\skills\` |
| Gemini | `%USERPROFILE%\.gemini\skills\yotta-workflow\` | `.gemini\skills\` |
| Goose | `%USERPROFILE%\.config\goose\skills\yotta-workflow\` | `.goose\skills\` |
| Amp | `%USERPROFILE%\.config\agents\skills\yotta-workflow\` | `.agents\skills\` |
| Kiro | `%USERPROFILE%\.kiro\skills\yotta-workflow\` | `.kiro\skills\` |
| WorkBuddy | `%USERPROFILE%\.workbuddy\skills\yotta-workflow\` | `.workbuddy\skills\` |
| Trae Code CLI | `%USERPROFILE%\.traecli\skills\yotta-workflow\` | `.traecli\skills\` |
| Trae IDE（国内） | `%USERPROFILE%\.trae-cn\skills\yotta-workflow\` | `.trae\skills\` |
| Qwen Code | `%USERPROFILE%\.qwen\skills\yotta-workflow\` | `.qwen\skills\` |
| Comate 文心快码 | `%USERPROFILE%\.comate\skills\yotta-workflow\` | `.comate\skills\` |
| CodeBuddy Code | `%USERPROFILE%\.codebuddy\skills\yotta-workflow\` | `.codebuddy\skills\` |
| Kimi Code CLI | `%USERPROFILE%\.kimi\skills\yotta-workflow\` | `.kimi\skills\` |

> 通用约定：`.agents/skills` 并非所有智能体都读取（Claude Code 与 Codex 默认不读），仅为 OpenCode / Cursor / Cline / Amp / Kimi / Gemini CLI 等智能体识别。已修改默认目录的智能体，请用 `--dir` 指定实际路径。

## 升级 / 卸载

- **升级**：重新安装最新版覆盖即可——`npx -y @yottameta/yotta-workflow -g` 或重跑 `bash install.sh -g`。技能目录内的旧文件会被覆盖；项目里的状态文件（`.workflow\`）不受影响。
- **卸载**：删除目标智能体 skills 目录下的 `yotta-workflow` 文件夹（各智能体目录见上表）。卸载不影响已写入项目的状态文件。

## 常见问题

- **状态目录在哪？** 先看项目下是否存在 `.workflow\`；不存在时按「状态文件位置判定」规则以会话 cwd 为基准定位。
- **多个智能体状态不同步？** 确认它们指向同一项目目录（同一份 `.workflow\`）。本技能设计为共享一份状态；若各自建了 `.workflow`，说明项目目录不一致。
- **项目已有自己的交接机制？** 沿用原机制即可，只需满足两个强制点：开工先读状态、收工更新状态并留锚点。

## 开发与校验

本项目内运行：`python tools/validate-skill.py yotta-workflow`。

## 许可证

MIT © YottaMeta