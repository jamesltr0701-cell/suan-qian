# 老师档案、人工名单与持久记录

记录保存在磁盘上的老师私有台账，不能只说“记住了”。不同老师使用不同 `profile_id` 和数据目录；账号/组织/老师ID可得时共同作为身份依据，不能只用姓名跨账号匹配。公共仓库没有预设老师。

## 私有文件

- 本地 `~/.codex/suan-qian/profiles.json`（或 CODEX_HOME 对应路径）：`schema_version`、`profiles` 数组；每项有 `profile_id`、`teacher_name`、可得的 `teacher_id`、`crm_origin`、`data_root`。它只帮助下一次找到数据。
- `<data_root>/teacher-profile.json`：上述身份、`timezone`、`adapter_path`、`confirmed_rules`、`data_root`。已有旧台账可沿用。
- `roster-decisions.json`：人工决定的当前状态与历史事件。
- `project-register.json`：项目进度与已确认事实；`snapshots/`、`reports/`、`run-progress.json` 沿用。
- `settlements/`：原件、识别结果、匹配结果和历史；`reminders.json`：每月结算单提醒是否已给、是否已收到。

首次新建索引或目录受环境权限限制时保留当前工作区数据、明确路径，不能声称跨会话记忆已生效。不能为共享技能创建包含老师隐私的远端同步。

## 决定结构

每位学生用 `(profile_id, student_id)` 定位，包含 `student_name`、`teaching_status`（active / stopped）、`review_scope`（regular / settlement_only / excluded）、`reason`、`confirmed_at`、`confirmed_by`、`user_statement`、`pending_project_keys`、`history`。只保存简短必要的用户原话，不复制整个聊天。

规则：

| 老师明确表达 | 保存与后续行为 |
|---|---|
| 不再带，且已经结清；以后不用查 | stopped + excluded；停止常规扫描，保存旧账 |
| 不再带，但还有待结项目 | stopped + settlement_only；仅跟进这些项目 |
| 只说不再带，未交代是否结清 | 停止常规扫描；已知待结继续跟进，结算状态未知则集中提示一次，不把余额清零 |
| 暂时没上课／本月0课时 | 不改变名单 |
| 恢复检查／又开始带 | active + regular；记录恢复事件，重新核对现有课次 |

老师明确要求完全排除时即使旧账有余额，也可将扫描范围设为 excluded，但保留余额和未解决提示；范围排除与财务结清是两个状态，不能伪造结清。

姓名能唯一对应当前名单可直接执行并回报；重名需问是哪一位，不能排除所有同名者。一次决定长期有效，直到老师更改；不每月重新询问。用户修改规则或名单后重算派生结果，不改CRM事实。

## 每次覆盖与加速

每次先刷新各校区轻量在读/在授名单，加入新生。名单与私有台账去重，应用人工决定。不要仅依赖上一轮缓存导致漏掉新生，也不要因旧在读标记把明确停带者重新加入逐人扫描。若可从轻量列表直接看到明确新授课迹象，与 excluded 冲突则只提醒老师恢复与否，别静默重启完整扫描。

常规范围、仅待结范围、明确排除和未查分别计数。首次或名单有新变化时，报告后可以说“有不再带且已结清的学生，告诉我名字，下次就不查了”；不强制老师逐个确认，不因未回复阻塞报告。

只有可靠的修改时间/增量接口能覆盖补反馈和结算变化时才使用增量读取；否则重读范围内学生的相关列表。逐条课次采集无法定位到ID时保存来源页和完整字段；列表行号只是本次页面定位，不能作为跨月永久课程ID。
