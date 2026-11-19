# AstrBot Codex 命令行配置

为 AstrBot 的 Codex 后台任务提供命令行启动入口、模型配置和任务执行约定。配套插件为 [Codex 后台任务](https://github.com/ahuai114514/astrbot_plugin_codex_tasks)。

## 文件

- `codex-bot` 启动 Codex CLI，并将 `CODEX_HOME` 设置为 `/AstrBot/data/codex-cli/home`。
- `config.toml` 提供模型服务、上下文窗口和执行权限配置。
- `AGENTS.md` 提供项目授权、中文回复和隔离验证约定。
- `SKILL.md` 说明后台任务的提交、状态查询、停止与续接方式。

## 部署

1. 在 AstrBot 容器的 `/AstrBot/data/codex-cli/runtime` 安装 Codex CLI，确保 `node_modules/.bin/codex` 可执行。
2. 将 `config.toml` 和 `AGENTS.md` 放入 `/AstrBot/data/codex-cli/home`，按实际模型服务调整配置并完成认证。
3. 将 `codex-bot` 放到 `/AstrBot/data/codex-cli/codex-bot` 并赋予执行权限。
4. 将后台任务插件的 `cli_path` 设置为 `/AstrBot/data/codex-cli/codex-bot`，按 Bot 的技能加载方式配置 `SKILL.md`。

当前配置使用 `approval_policy = "never"` 和 `sandbox_mode = "danger-full-access"`，面向由管理员授权的 AstrBot 容器执行环境。鉴权数据及任务运行数据保存在部署环境中。

## 版本

当前发布基线为 `v1.0.0`，更新记录见 `change.log`。

MIT License，作者 ahuai。
