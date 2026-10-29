# Codex 接入

入口：自然语言“帮我算钱”，显式 `$suan-qian`。保留 `agents/openai.yaml` 的中文显示名称。采用当前环境实际的 Skill 安装目录；维护原稿在 Skill Playground，安装副本不独立修改。

私有索引沿用 `$CODEX_HOME/suan-qian/profiles.json`，未设置时用 `~/.codex/suan-qian/profiles.json`。已有索引的 `data_root` 原地使用。首次无档案时，在同一 `suan-qian` 状态目录的 `data/<profile_id>/` 建档；这与技能目录分离。写权限不足时请用户指定获准的私有目录，不把学生数据默认写入公开仓库，也不假装已持久保存。

优先使用当前提供且获准的浏览器/Computer Use工具。原生Safari曾用于这个CRM：仅当工具文档确实支持时可用 `cua.getApp("com.apple.Safari")`。该API不是Claude Code通用能力。内置浏览器是否可用须现场检查，不重复依赖旧失败结论。账号与验证码由老师在同一已连接浏览器中完成。

切到Claude Code时，不自动复制索引。老师明确要沿用台账后，核实身份与 `data_root`，再在Claude侧索引登记同一路径；不能同时用两个会话写同一台账。
