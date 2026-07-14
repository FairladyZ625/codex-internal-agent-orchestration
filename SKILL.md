---
name: codex-internal-agent-orchestration
description: Coordinate complex coding and knowledge-work tasks with Codex's current main agent and in-task internal subagents. Use for multi-agent task graphs, parallel read-only exploration, disjoint implementation slices, long-running supervision, Harness task packages, evidence collection, adversarial review, and final integration.
---

# Codex 内部 Agent 编排

## 执行范围

使用当前 Codex 主 Agent，以及当前任务内通过 Codex 协作工具启动的内部 subagent。仓库本地命令、测试、构建和 Harness 用于执行与记录证据。内部 subagent 工具不可用时，由主 Agent 本地完成或向用户说明能力限制。

## 先判定是否派活

依次检查：

1. **任务成立吗？** 先读原始材料或做最小测量。
2. **委托总成本更低吗？** 计入写 packet、等待、回读、冲突、集成和复验；主 Agent 更快就直接做。
3. **旁路有独立价值吗？** 只为可并行切片、上下文卸载、专项侦察或独立复核派活。
4. **边界能独立验收吗？** 写任务必须有唯一 ownership、明确依赖和可观察证据。
5. **同一路径已失败两次吗？** 连续两次没有新证据就熔断，由主 Agent 接管、缩小任务或向用户升级。

不为填满并发槽派 Agent。用户指定数量时遵从上位指令，但仍使用最小写面并说明成本或冲突。

## 派活前建立任务图

首个派活前写出最小 orchestration brief：

- Mission：目标、非目标、原始依据、真实使用入口。
- Graph：任务、依赖、关键路径、可并行节点。
- Ownership：每条线唯一 owner、write set、共享面、集成 owner。
- Budget：Agent 数、并发、时间或尝试预算，以及取消、接管和升级条件。
- Acceptance：主 Agent 将亲自回读的 diff、测试、日志、截图或真实入口。

没有独立写面就不要并行写。读取可以重叠；高耦合生产代码保持单写者。模板见 [references/delegation-templates.md](references/delegation-templates.md)。

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

## Harness 投影

检测到 `harness/harness.yaml` 或仓库明确要求 Harness 时，仅在已有 task、非平凡多协作者工作、跨回合监管或承重证据需要时投影任务图。短小单步任务不因 Harness 存在自动新建 task。具体规则见 [references/harness-orchestration.md](references/harness-orchestration.md)。

## 主 Agent 验收

主 Agent 必须亲自完成：

1. 功能复核：实现、测试和声明一致。
2. 语义验收：结果符合原始目标、范围与非目标。
3. 编排复核：任务图已收敛，无遗漏依赖、冲突写面或未回收 Agent。
4. 对抗验证：隐藏假设和高风险异议已被挑战与裁决。
5. 使用证明：真实入口可用，冷启动路径成立。

未验证项必须显式标注。详见 [references/acceptance.md](references/acceptance.md)，一线纪律见 [references/worker-handbook.md](references/worker-handbook.md)。

## 维护

修改本技能前先读 [references/skill-maintenance.md](references/skill-maintenance.md)。正文只保留高频决策门、执行范围和生命周期；模板、Harness 细节与场景规则下沉到一层 references。
