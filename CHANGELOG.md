# 更新日志

## v0.2.6 (2026-08-29)

- 安装方式统一为四方式（对齐发布规范 §3.3.1）：方式一 `npx -y @yottameta/yotta-workflow --agent <name>` / `--dir <dir>`（推荐，走 npm 源）；方式二 `git clone https://github.com/YottaMeta/yotta-workflow.git`；方式三 GitHub Download ZIP；方式四 `bash install.sh --agent/--dir/--list`。移除 `npx skills` 与 `-g` 推荐；中英双 README 安装节同步。
- 版本对齐：package.json / SKILL.md / CHANGELOG / 引擎 VERSION / 测试断言 / README 锚点 = 0.2.6。
- 无功能变更（仅文档与版本同步）。

## v0.2.5 (2026-08-28)

中英双语文档：README.md（英文主文件，作为 GitHub / npm / ClawHub 主页） + 新增 README.zh-CN.md（中文全档）；安装方式统一为三方式（npx -g / --dir、install.sh、手动复制），移除 npx 固定 --agent codex（--agent 仅 install.sh 使用）；npm description 改英文；package.json files 加入 README.zh-CN.md。无功能变更。
