# 算钱

给老师用的月度课时复盘 Skill。对 Codex 或 Claude Code 说一句：

> 帮我算钱。

它会检查你的在授学生，给一份简短清单：本月上了多少课时、哪些项目快到门槛、哪些已经够了，值得看看有没有作品可以送审。教务结算清单出来后，还能再核对一次。

“算钱”默认核对课时与项目，不会在没有课酬单价的情况下估算工资，也不会替你提交审核或操作结算。

核算默认只看 **“作品集课”和“基础课”**，其他课时类目不计入本月课时、未结算余额或距门槛的差额，也不展开反馈检查。基础课计入核算范围，但送审仍需符合对应项目产出规则。科系adapter可按老师明确的规则配置类目范围。

## 三个主要功能

### 每个科系接自己的规则

课时读取和名单管理共用，项目类型由独立的 adapter 提供。电影、设计、建筑等科系可以用各自的消项标准，不用改核心流程。

首次使用，把本系规则文档、图片或文字给当前助手。它会整理成你的私有适配文件，并确认必要口径。仓库里的电影配置是示例，不是各科系通用政策。

### 记住你已经不带的学生

第一轮后可以说：

> 示例同学已经不带了，也全部结清，以后不用查。

下次会跳过该学生。只说“不带了”，则保留旧待结项目的跟进；本月0课时不会自动剔除。以后又开始带，可以说“恢复检查这位学生”。

每位老师的名单分开保存，历史数据仍保留。后续每月刷新轻量名单发现新生，再检查需要关注的人，逐渐减少重复工作。

### 结算清单最后核一遍

每月会提醒一次：教务结项/结算清单出来后，可以发图片、PDF或表格。技能会检查项目、课时和状态是否对得上，并标出看不清或需要确认的地方。

同一单据重复上传、同一条记录在截图中重复出现，不会重复扣课时。“已结项”“已结算”“已付款”分开记录。

## 安装与使用

一个发布包，支持 Codex 和 Claude Code，版本见 `VERSION`。两边共用业务规则，运行入口和浏览器连接各有适配。

### Codex

在 Codex 中发送：

> 帮我从 https://github.com/jamesltr0701-cell/suan-qian 安装这个 Skill。

安装后新开对话，说“帮我算钱”；也可显式输入 `$suan-qian 帮我算钱`。使用当前环境支持的个人技能目录；已有安装原位更新，不再建立第二个同名副本。

### Claude Code

在 Claude Code 中发送：

> 帮我把 https://github.com/jamesltr0701-cell/suan-qian 安装到个人技能目录。如果已经安装，请先核对版本，保留原副本再更新。

首次手动安装也可以在终端执行（需要 Git；目录已存在时不要重复执行）：

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/jamesltr0701-cell/suan-qian.git ~/.claude/skills/suan-qian
```

新开会话后输入 `/suan-qian` 或“帮我算钱”。如只想在一个项目使用，可把完整文件夹放进该项目 `.claude/skills/suan-qian/`；个人安装和项目安装选一种。安装位置和调用方式依据 [Claude Code 官方 Skills 文档](https://code.claude.com/docs/en/skills)。

读取后台还需要浏览器连接。官方路径是安装并连接 Claude in Chrome 扩展，再用 `claude --chrome` 启动，或 `/chrome` 检查连接。账号支持情况与设置见 [官方 Chrome 文档](https://code.claude.com/docs/en/chrome)。Safari里的登录不能自动当成Chrome里的登录。

### 第一次要准备什么

- 在当前工具连接的浏览器登录CRM，验证码或扫码由自己完成。
- 告诉助手检查哪位老师、哪个科系，提供本系消项标准。
- 教务清单出来后再上传图片或表格做最终核对。

没有浏览器工具时，也能先用你提供的导出或截图复盘，但会说明范围不完整。这个CRM此前验证过Codex的Safari读取；**Claude Code适配已完成结构和打包检查，尚未完成真实CRM端到端验证**。安装Skill不会自动获得浏览器权限，每月25日是建议触发日，不自动建立定时任务。

## 示例复盘（虚构数据）

> 截至本月25日，看了3位学生。
>
> 已经够了：示例甲，本月4课时，未结算13。最近在改剪辑，可以看看影片能否按后期制作整理送审。
>
> 差一点：示例乙，本月3课时，未结算10，还差2。
>
> 还早：示例丙，本月2课时，未结算2，还差10。

## 文件结构

```text
SKILL.md                      核心工作流与触发说明
agents/openai.yaml            中文名称和自然语言调用配置
adapters/film-example.json    电影科系示例
adapters/department-template.json  其他科系接入模板
references/adapters.md        适配约定
references/roster.md          老师档案与人工名单
references/settlement.md      结算单核对规则
references/crm-workflow.md    浏览器读取与长列表检查
references/reporting.md       简短报告格式
references/runtime-codex.md   Codex入口和本地索引
references/runtime-claude-code.md Claude Code入口、索引和浏览器连接
VERSION                      发布版本
SOURCE.json                  发布时生成的Source标签、提交与文件校验
```

老师档案、排除名单、公司原始规则、学生数据、结算单和截图存放在老师自己的私有数据目录，不在这个仓库里。Codex索引为 `$CODEX_HOME/suan-qian/profiles.json`（默认 `~/.codex/suan-qian/profiles.json`），Claude Code索引为 `~/.claude/suan-qian/profiles.json`。新用户的数据放在对应状态目录的 `data/<profile_id>/`；已有台账按索引原地接入。换平台要明确选择是否共用台账，不能自动合并两位老师或复制两套余额；同一台账避免并发修改。换电脑时需另行备份和恢复私有数据，安装Skill不会带回旧账。

## 维护方式

唯一维护原稿在 Skill Playground 的 `.agents/skills/suan-qian/`；验证与发布工具位于 `testing/suan-qian/`。流程是修改原稿、验证、提交并打版本标签，再从标签生成安装副本和本公开仓库。`SOURCE.json` 记录发布来源及每个文件的SHA-256，可追溯具体版本。安装目录和GitHub发布包不作为另一个可独立修改的原稿。

老师明确说“暂时停滞”时，会记入暂停观察名单：后续每次复盘先轻量查看新排课或结算信号，再决定是否需要展开。只因本月没课，不会自行停带。暂停、停带都不等于结清。

技能由指令和规则文件组成，不是独立网站或自动发薪系统。成品是否符合消项要求、是否实际送审，由老师判断。需要不同科系规则时，从模板建立自己的适配，不直接修改别人的档案。

显示名称和自动选择元数据参照 [OpenAI 官方技能元数据说明](https://developers.openai.com/plugins/deploy/submission-errors#skill-agent-metadata-errors)。
