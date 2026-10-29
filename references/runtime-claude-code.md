# Claude Code 接入

入口：`/suan-qian` 或“帮我算钱”。把完整技能文件夹装入 `~/.claude/skills/suan-qian/`；也可仅在指定项目的 `.claude/skills/suan-qian/` 安装。选择一种范围，避免同名副本版本不同。目录内保留 `SKILL.md`、`references/` 和 `adapters/`，无需转换业务规则；`agents/openai.yaml` 仅供Codex使用。

私有索引使用 `~/.claude/suan-qian/profiles.json`，这属于本技能约定，不是Claude Code内置功能。首次建档默认 `~/.claude/suan-qian/data/<profile_id>/`，已有索引按 `data_root` 读取。写权限不足时请老师指定获准的私有目录；不把学生数据写到公开项目或技能仓库。老师明确要沿用其他宿主台账后，核实身份再登记同一个 `data_root`；不要复制成两套余额，也不要并发写同一台账。

## 浏览器与登录

先检查当前会话是否实际提供浏览器工具。官方Chrome连接可通过 `claude --chrome` 启动、用 `/chrome` 检查连接；需要Claude in Chrome扩展及符合官方要求的登录/套餐。设置以当前官方文档和工具权限为准，不能擅自安装扩展或关闭权限保护。也可使用老师已授权、当前实际提供的其他浏览器MCP工具，先读其说明。

Safari已经登录不等于Claude Code连接的Chrome也登录。让老师在实际连接的浏览器打开CRM并完成验证码/扫码。使用当前工具的观察与交互能力，不能照抄Codex的 `cua` API；不假设工具名称一定存在。

有浏览器但会话过期：保存断点，请老师完成登录，重新核对账号后继续。没有浏览器能力：说明需连接浏览器，也可先审阅老师主动提供的CRM导出/截图，标注日期、已覆盖与未覆盖范围。没有实时来源时不能给出“已全量检查”的结果。始终只读后台，不提交审核或结算。

## 验证边界

此适配已按官方Skill结构和文档编写；未在Claude Code的真实老师账号上跑过CRM全量审阅。首次使用应先用少量学生核对字段、分页与课时，再扩展到全量；测试报告须区分静态检查、合成数据检查和真实CRM验证。

官方依据（2026-10-29核对）：[Skills](https://code.claude.com/docs/en/skills)、[Chrome连接](https://code.claude.com/docs/en/chrome)。
