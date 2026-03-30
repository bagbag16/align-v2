# agent-drift-guard

复杂多轮 AI 协作规则。  
用于约束 agent 在复杂多轮协作中的判断、承接、回查和推进方式。

## 它是什么

`agent-drift-guard` 是一套用于复杂、多轮任务的人机协作规则。  
它的正式形态是一个 skill。

它的目标不是增加流程感，而是：
- 稳住问题
- 保住语义
- 减少漂移
- 在必要时承接关键状态
- 让推进和稳健同时成立

## 它解决什么

`agent-drift-guard` 主要处理这几类常见问题：
- 把暂定假设写成已确认结论
- 把 agent 改写当成用户原意
- 长对话后目标、约束、命名逐渐漂移
- 重要状态只留在聊天里，后续继承失真
- 新信息已经影响旧结论，但没人回查

## 它不是什么

`agent-drift-guard` 不是：
- agent 框架
- 任务管理器
- prompt 集合
- 自动执行系统

它不替用户管理任务，也不替 agent 自动完成协作。  
它做的事是：约束 agent 在复杂任务里如何判断、承接、回查和推进。

## 什么时候该用

以下情况适合使用 `agent-drift-guard`：
- 任务会持续多轮
- 当前讨论对定义、边界、约束、命名敏感
- 后续步骤依赖当前结论
- 当前输入混有目标、修正、约束和新范围
- 当前主要风险是跑偏，不是执行本身

## 什么时候别用

以下情况通常不需要：
- 一次性小任务
- 简单问答
- 指令已经明确的直接执行
- 不需要承接状态的快速修改

简单任务不要做重。  
`agent-drift-guard` 只在复杂度和失真风险上来时才有价值。

## 隐私与泄露边界

`agent-drift-guard` 默认要求最小披露。

- 不把身份证号、手机号、邮箱、住址、账号、密钥、令牌、内部链接等敏感信息直接外发
- 不把含隐私的原始内容原样复制到外部工具、公共仓库、公开文档、群聊或 issue
- 如必须继续处理，优先做脱敏、摘要化、最小摘录，而不是整段转发
- 不因“方便协作”默认扩大可见范围；共享前先确认接收对象和用途
- 当任务目标与隐私保护冲突时，先停在对齐层，明确哪些能发、哪些不能发
- 如果用户没有明确授权，agent 不应把隐私信息当成可公开上下文

## 形态与集成

`agent-drift-guard` 的正式形态是一个 skill。  
它不是插件，不是自动程序，也不是要求用户每次手动复制的 prompt 模板。

这个 skill 包含：
- `SKILL.md`：主规则
- `references/workflow.md`：展开流程
- `references/checklist.md`：检查清单
- `references/update-rules.md`：更新规则
- `examples/`：示例

将整个 `agent-drift-guard` 目录放入支持 skill 的环境中，由 agent 按 skill 机制读取。  
使用时应保留完整目录结构，而不是只复制单个文件。

这个 skill 是否会自动生效，取决于宿主环境的 skill 机制。  
如果宿主会自动读取已安装 skill，agent 可直接按这套规则工作；如果宿主需要显式调用，就应明确按 `agent-drift-guard` 处理当前任务。

这样做的原因是：
- 主规则保持精简
- 展开流程、检查清单、更新边界放在 `references` 中承接
- 用户不需要自己拆分或重组规则

如果你的环境不支持独立 skill，也可以只提炼其核心规则嵌入其他说明文件中；  
但这只是兼容性做法，不是 `agent-drift-guard` 的正式形态。

对用户来说，`agent-drift-guard` 不要求你记住全部规则。  
正常描述任务即可；当任务复杂、多轮、容易漂移时，agent 应主动按这套 skill 工作。

也就是说，`agent-drift-guard` 的主要成本应由 agent 承担，而不是转嫁给用户。

## 首页原则

- 先分清 `已确认 / 暂定假设 / 待确认`，不要把未确认内容写成既定事实
- 不把 agent 的改写默认当成用户原意，尤其是会影响目标、结构、命名和约束时
- 后续会依赖的信息才外化，而且只保留最小必要状态
- 新信息影响旧结论时要回查，而不是只改当前局部
- 默认轻量推进，只有在复杂度和失真风险明显上来时才展开

## 最小状态层

这不是完整记录，只是继续协作时最小够用的状态：

- `current-goal`
- `confirmed-constraints`
- `pending-items`
- `decisions`

## 安装与使用

如果你的宿主支持目录型 skill，把整个 `agent-drift-guard` 目录放进宿主的 skills 目录，并保留完整结构：

- `SKILL.md`
- `references/`
- `examples/`

安装后，宿主应能读取 `SKILL.md` 作为入口规则。

开始使用时，推荐按这个最小流程：

1. 先说清当前任务
2. 区分 `已确认 / 暂定假设 / 待确认 / 当前步不处理`
3. 如果后续还要依赖，就只外化最小状态层
4. 当前步够清楚时，交付最小有用结果

如果你的宿主支持显式调用 skill，可以直接用正式名或简写别名调用：

- `use agent-drift-guard`
- `use adg`
- `请按 agent-drift-guard 处理这个多轮任务`
- `请按 adg 处理这个多轮任务`
- `对这个任务启用 agent-drift-guard`
- `对这个任务启用 adg`

推荐约定：

- `agent-drift-guard` 作为正式名称，用于安装、文档和公开说明
- `adg` 作为日常调用别名，用于对话里快速启用

适合显式调用的场景：

- 这是一个会持续多轮的任务
- 当前讨论对定义、边界、约束或命名很敏感
- 你担心 agent 把暂定假设写成已确认结论
- 后续步骤会依赖当前结论继续推进

如果你的环境不支持独立 skill，也可以把它作为 `CLAUDE.md` 使用。

推荐做法：

1. 新建项目级 `CLAUDE.md`
2. 把 `SKILL.md` 中的主规则复制进去
3. 再把 `references/checklist.md`、`references/workflow.md`、`references/update-rules.md` 中你需要的部分补进去
4. 在 `CLAUDE.md` 开头注明：当前项目默认按 `agent-drift-guard` 规则处理复杂多轮协作

一个最小写法可以是：

```md
# CLAUDE.md

This project uses `agent-drift-guard` for complex multi-turn collaboration.

When tasks are high-impact, multi-step, or easy to drift:
- separate `confirmed`, `temporary assumption`, and `needs confirmation`
- do not treat rewrites as approved user intent
- externalize only the minimum state needed for handoff
- revisit old conclusions when new information changes them
```

如果你只是想在单个项目里长期默认启用，这种做法最直接。

## 示例

用户说：先做方案，不要开始实现。

`agent-drift-guard` 应先区分：
- 已确认：当前轮不进入实现
- 暂定假设：先给一个结构化方案可能有帮助
- 待确认：这个结构是否会成为正式结构
- 当前步不处理：具体实现细节

这样后续不会把暂定结构悄悄升级成既定事实。
