# workflow-standard · 跨会话/跨项目工作流标准

> 一套面向所有 AI 智能体的通用工作流标准：开工必读状态、状态就近存、进行中自动记流水/任务/决策、收工必留交接锚点。
> 通过「流程全局定、状态就近存」，让任何 AI 会话都能无痛接续，避免单会话拉长而失忆。

## 这是什么

`workflow-standard` 定义了一套跨会话、跨项目的协作协议。它规定项目状态放在哪里、以什么格式记录，并要求智能体在会话开始时读取状态、进行中及时落盘、结束时生成交接锚点。核心原则：**流程全局定，状态就近存；开工必读状态，收工必留锚点。**

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
npx -y @yottameta/workflow-standard -g
npx -y @yottameta/workflow-standard --dir <你的技能目录>   # 任意智能体：指定目录安装
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
把整个 `workflow-standard` 文件夹复制到目标智能体的 skills 目录。常见位置（用户级；Windows 用 `%USERPROFILE%`，Linux/macOS 用 `~`）：

| 智能体 | 用户级目录 | 项目级目录 |
|---|---|---|
| Codex | `%USERPROFILE%\.codex\skills\workflow-standard\` | `.codex\skills\` |
| Claude Code | `%USERPROFILE%\.claude\skills\workflow-standard\` | `.claude\skills\` |
| Cursor | `%USERPROFILE%\.cursor\skills\workflow-standard\` | `.cursor\skills\` |
| Windsurf | `%USERPROFILE%\.codeium\windsurf\skills\workflow-standard\` | `.windsurf\skills\` |
| opencode | `%USERPROFILE%\.config\opencode\skills\workflow-standard\` | `.opencode\skills\` |
| Gemini | `%USERPROFILE%\.gemini\skills\workflow-standard\` | `.gemini\skills\` |
| Goose | `%USERPROFILE%\.config\goose\skills\workflow-standard\` | `.goose\skills\` |
| Amp | `%USERPROFILE%\.config\agents\skills\workflow-standard\` | `.agents\skills\` |
| Kiro | `%USERPROFILE%\.kiro\skills\workflow-standard\` | `.kiro\skills\` |
| WorkBuddy | `%USERPROFILE%\.workbuddy\skills\workflow-standard\` | `.workbuddy\skills\` |
| Trae Code CLI | `%USERPROFILE%\.traecli\skills\workflow-standard\` | `.traecli\skills\` |
| Trae IDE（国内） | `%USERPROFILE%\.trae-cn\skills\workflow-standard\` | `.trae\skills\` |
| Qwen Code | `%USERPROFILE%\.qwen\skills\workflow-standard\` | `.qwen\skills\` |
| Comate 文心快码 | `%USERPROFILE%\.comate\skills\workflow-standard\` | `.comate\skills\` |
| CodeBuddy Code | `%USERPROFILE%\.codebuddy\skills\workflow-standard\` | `.codebuddy\skills\` |
| Kimi Code CLI | `%USERPROFILE%\.kimi\skills\workflow-standard\` | `.kimi\skills\` |

> 通用约定：`.agents/skills` 并非所有智能体都读取（Claude Code 与 Codex 默认不读），仅为 OpenCode / Cursor / Cline / Amp / Kimi / Gemini CLI 等智能体识别。已修改默认目录的智能体，请用 `--dir` 指定实际路径。

## 使用示例

开始一个会话时，读取项目状态：

```text
请先读取 .codex\STATE.md、TASKS.md、ROADMAP.md，恢复项目上下文。
```

收工时，按本技能格式生成交接锚点，并原样输出给用户复制，供下个会话使用。

## 开发与校验

本项目内运行：`python tools/validate-skill.py workflow-standard`。

## 许可证

MIT © YottaMeta