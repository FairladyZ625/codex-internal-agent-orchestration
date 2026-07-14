# Codex Internal Agent Orchestration

[![skills.sh](https://skills.sh/b/FairladyZ625/codex-internal-agent-orchestration)](https://skills.sh/FairladyZ625/codex-internal-agent-orchestration/codex-internal-agent-orchestration)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

一个面向 Codex 的内部 Agent 编排技能。让主 Agent 负责目标、关键路径、集成与验收，并按需组织 Explorer、Worker 和 Reviewer 完成复杂任务。

## 安装

全局安装到 Codex：

```bash
npx skills add FairladyZ625/codex-internal-agent-orchestration -g -a codex -y
```

安装前查看仓库中可用的技能：

```bash
npx skills add FairladyZ625/codex-internal-agent-orchestration --list
```

## 使用

在 Codex 中明确调用：

```text
使用 $codex-internal-agent-orchestration 编排这个任务。
```

也可以直接描述目标，例如：

```text
使用 $codex-internal-agent-orchestration，把这次跨模块重构拆成任务图，安排内部 Agent 并行侦察和实现，最后完成集成验证。
```

## 适用场景

- 多模块或长时间运行的复杂任务
- 可并行的只读侦察、风险枚举和方案比较
- 写面互不重叠的实现切片
- 独立代码复核和失败模式检查
- Harness task、progress、fact 与 evidence 的协同治理
- 多条工作线的证据回收、集成和最终验收

## 内部角色

| 角色 | 职责 |
|---|---|
| 主 Agent / CEO | 明确目标、守住关键路径、集成结果并完成最终验收 |
| Explorer | 只读定位、收集证据、枚举风险和比较方案 |
| Worker | 在唯一 ownership 和明确写面内完成实现或验证 |
| Reviewer | 独立检查关键 diff、隐藏假设、反例和失败模式 |

## 核心原则

- 最小充分团队优先，不为填满并发槽派 Agent。
- 派活前明确任务图、依赖、ownership、预算和验收证据。
- 高耦合模块保持单写者，互不冲突的任务才并行。
- Agent 返回的是 claim；主 Agent 回读 ground-truth 后才成为 evidence。
- 连续两次没有新证据就熔断，由主 Agent 接管、缩小任务或升级问题。
- 各线分别通过不代表整体通过，必须在集成态重新验证。

## 仓库结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── acceptance.md
│   ├── delegation-templates.md
│   ├── harness-orchestration.md
│   ├── role-matrix.md
│   ├── runtime-channels.md
│   ├── skill-maintenance.md
│   └── worker-handbook.md
└── LICENSE
```

## License

[MIT](LICENSE) © 2026 FairladyZ625
