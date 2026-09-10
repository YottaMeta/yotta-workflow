# 更新日志

## v0.4.0 (2026-09-10)

**路径模型澄清**：明确项目根目录、源码目录与工作区根目录的区别，默认采用“项目根 / `.workflow` / 源码目录”标准结构，避免把带 `.git` 的源码目录误当项目根。

- 状态目录唯一位置明确为 `<项目根>\.workflow\`；项目根与源码目录可以重合，也可以嵌套。
- 定位收紧为两种证据：用户明确指定项目根，或向上找到已有 `.workflow`；两者都没有先问，不用 `.git`、`package.json`、`src`、`README`、cwd 或目录结构猜。已有 `.workflow` 永不自动迁移。
- 新增 `references/path-model.md`，补充单仓库、嵌套源码仓库、工作区多项目、从源码子目录开工四类走查。
- README 中英双版、FAQ、复杂场景走查、异常恢复手册同步统一术语。

## v0.3.0 (2026-09-08)

**评测驱动完善**：新增 FAQ、复杂场景走查与异常恢复手册；安装器错误处理与测试补齐。

- 新增 references/faq.md、walkthroughs.md、exception-playbook.md。
- 描述补齐边界：只记项目状态，不写 AI 人格、用户偏好、关系或跨项目通用知识。
- 安装器支持 --help、参数校验、统一退出码与人话错误提示。

## v0.2.6 (2026-08-29)

- 安装方式统一为四方式（对齐发布规范 §3.3.1）：方式一 `npx -y @yottameta/yotta-workflow --agent <name>` / `--dir <dir>`（推荐，走 npm 源）；方式二 `git clone https://github.com/YottaMeta/yotta-workflow.git`；方式三 GitHub Download ZIP；方式四 `bash install.sh --agent/--dir/--list`。移除 `npx skills` 与 `-g` 推荐；中英双 README 安装节同步。
- 版本对齐：package.json / SKILL.md / CHANGELOG / 引擎 VERSION / 测试断言 / README 锚点 = 0.2.6。
- 无功能变更（仅文档与版本同步）。

## v0.2.5 (2026-08-28)

中英双语文档：README.md（英文主文件，作为 GitHub / npm / ClawHub 主页） + 新增 README.zh-CN.md（中文全档）；安装方式统一为三方式（npx -g / --dir、install.sh、手动复制），移除 npx 固定 --agent codex（--agent 仅 install.sh 使用）；npm description 改英文；package.json files 加入 README.zh-CN.md。无功能变更。
