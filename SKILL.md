---
name: codex-internal-agent-orchestration
description: Coordinate complex coding and knowledge-work tasks inside a user's target repository with Codex's current main agent and in-task internal subagents. Use for multi-agent task graphs, parallel exploration, disjoint implementation slices, long-running supervision, evidence collection, adversarial review, final integration, and Harness Anything task/decision/fact workflows when the target repository has adopted Harness Anything.
---

# Codex 内部 Agent 编排

## 执行范围

使用当前 Codex 主 Agent，以及当前任务内通过 Codex 协作工具启动的内部 subagent。仓库本地命令、测试和构建用于执行；目标仓库接入 Harness Anything 时，用 `ha` 记录任务、决策、事实与证据。内部 subagent 工具不可用时，由主 Agent 本地完成或向用户说明能力限制。

## 先判定是否派活

依次检查：

1. **任务成立吗？** 先读原始材料或做最小测量，证伪错误前提比执行错误任务更重要。
2. **主 Agent 已能写出确切 diff 吗？** 已知改动且路径短时直接做，避免把委托固定成本包装成管理。
3. **委托总成本更低吗？** 计入 packet、等待、回读、冲突、集成和复验。
4. **是否存在长尾价值？** 大量侦察、试错、测量、上下文卸载或独立复核才值得派活。
5. **边界能独立验收吗？** 每条线必须有清晰语义面、依赖、协调边界和可观察证据。
6. **同一路径已失败两次吗？** 连续两次没有新证据就熔断，由主 Agent 接管、缩小任务或向用户升级。

不为填满并发槽派 Agent。用户指定数量时遵从上位指令，但仍使用最小写面并说明成本或冲突。

## 派活前建立任务图

首个派活前写出最小 orchestration brief：

- Mission：目标、非目标、原始依据、真实使用入口。
- Graph：任务、依赖、关键路径、可并行节点。
- Ownership：每条线唯一 owner、预期写面、冲突/保护面、集成 owner。
- Budget：Agent 数、并发、时间或尝试预算，以及取消、接管和升级条件。
- Acceptance：主 Agent 将亲自回读的 diff、测试、日志、截图或真实入口。

默认按一条连贯语义线切成一个完整包，不预防性切成大量微任务；只有依赖、风险隔离或写面冲突要求时再拆细。读取可以重叠；高耦合代码保持单写者。

每个委托包必须回答五问：**Context**、**Request**、**Output Format**、**Constraints**、**Checkpoint**。Context 至少给 canonical 原文、why/首位使用者、验收者视角、系统地图和真实验收入口。预期文件只是地图，不是信息围栏；Worker 可读全仓理解根因，但不得触碰正在冲突或受保护的写面。模板见 [references/delegation-templates.md](references/delegation-templates.md)。

## 选择内部角色

- **主 Agent / CEO**：目标、关键路径、任务图、用户沟通、集成、语义验收和最终裁决。
- **Explorer**：只读定位、枚举风险、比较方案；提供路径与证据，不改文件。
- **Worker**：完成边界清晰的实现或验证切片；只写授权范围。
- **Reviewer**：独立只读挑战隐藏假设、失败模式和关键 diff；不替主 Agent 验收。

通过当前运行时的 Codex 内部协作工具管理这些角色。完整工具映射、并发规则和失败恢复见 [references/runtime-channels.md](references/runtime-channels.md)，角色红线见 [references/role-matrix.md](references/role-matrix.md)。

## 运行循环

1. 主 Agent 先推进关键路径或当前阻塞项，再派不重叠旁路线。
2. 派出后继续做本地工作，不为等待 Agent 空转。
3. 用消息补充上下文或纠偏；不要为同一工作反复新建 Agent。
4. 优先消费完成事件；只在确有运行中任务时等待或低频检查。
5. 把 Agent 结果先视为 claim；主 Agent 回读 ground-truth 后才成为 evidence。
6. 按依赖顺序集成并运行 fresh verification；各线分别通过不等于整体通过。
7. 完成、需用户裁决或自主面穷尽时，回收/打断遗留 Agent 并一次性收口。

## Reference 路由矩阵

按当前命中的场景读取，不要一次性加载全部 Reference。

| 触发场景 | 必读 Reference | 用它解决什么 |
|---|---|---|
| 准备首个派活包或 orchestration brief | [delegation-templates.md](references/delegation-templates.md) | 五问契约、任务图、Explorer/Worker/Reviewer prompt 和收口格式 |
| 角色、权限、ownership 或写面边界不清 | [role-matrix.md](references/role-matrix.md) + [worker-handbook.md](references/worker-handbook.md) | 谁负责什么、哪些边界不可越、Worker 如何开工和回传 |
| 创建、补充、等待、打断或并发管理内部 Agent | [runtime-channels.md](references/runtime-channels.md) | 协作工具映射、并发预算、共享工作区、监管与失败恢复 |
| 目标仓库启用 Harness Anything，或准备执行任何 `ha` 写操作 | [harness-orchestration.md](references/harness-orchestration.md) | capabilities 驱动的 CLI 发现、原语映射、权责、证据和收口门 |
| 需要相信“已写入”“无发现”“已通过”或“与改动无关” | [ground-truth-discipline.md](references/ground-truth-discipline.md) | 产物回读、全文/全量核验、终态判断、阳性与阴性对照 |
| 集成、review、complete 或最终交付 | [acceptance.md](references/acceptance.md) | 功能、语义、编排、对抗、使用性验收与原始语义勾核 |
| 修改本技能 | [skill-maintenance.md](references/skill-maintenance.md) | 内容边界、更新流程、行为场景和反熵增规则 |

## Harness 投影

本技能用于在**用户自己的目标仓库**中使用 Harness Anything，不携带 Harness Anything 源码仓库的开发流程。检测到 `harness/harness.yaml` 或目标仓治理明确要求时，仅在已有 task、非平凡多协作者工作、跨回合监管或承重证据需要时投影任务图。短小单步任务不因 Harness 存在自动新建 task。

每次通过 `ha capabilities --json`、各原语的 capabilities 与 `ha <kind> --help` 发现当前命令面，不背诵固定参数。主 Agent 自己也必须使用要求 Worker 使用的记录系统；排期、裁决、事实与关系不能只留在聊天里。具体协议见 [references/harness-orchestration.md](references/harness-orchestration.md)。

## 主 Agent 验收

主 Agent 必须亲自完成：

1. 功能复核：实现、测试和声明一致。
2. 语义验收：结果符合原始目标、范围与非目标。
3. 编排复核：任务图已收敛，无遗漏依赖、冲突写面或未回收 Agent。
4. 对抗验证：隐藏假设和高风险异议已被挑战与裁决。
5. 使用证明：真实入口可用，冷启动路径成立。

未验证项必须显式标注。写回执、diff、分页列表和状态快照都不是终态证据；关键结论按 [references/ground-truth-discipline.md](references/ground-truth-discipline.md) 核产物、全文、全量与对照。完整验收见 [references/acceptance.md](references/acceptance.md)，一线纪律见 [references/worker-handbook.md](references/worker-handbook.md)。

## 维护

修改本技能前先读 [references/skill-maintenance.md](references/skill-maintenance.md)。正文只保留高频决策门、执行范围和生命周期；模板、Harness 细节与场景规则下沉到一层 references。
