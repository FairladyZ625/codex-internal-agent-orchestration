# Codex Internal Agent Orchestration

[![skills.sh](https://skills.sh/b/FairladyZ625/codex-internal-agent-orchestration)](https://skills.sh/FairladyZ625/codex-internal-agent-orchestration/codex-internal-agent-orchestration)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

一个面向 Codex 的内部 Agent 编排技能。它在你的目标仓库中组织 Explorer、Worker 和 Reviewer，并让主 Agent 负责目标、关键路径、集成与验收。

它尤其适合已经接入 [Harness Anything](https://github.com/FairladyZ625/harness-anything) 的仓库：把 Agent 工作投影成可追踪的 task、decision、fact、relation、progress 与 evidence，让每次开发都为项目留下可复用记忆。

## 与 Harness Anything 的关系

这个技能**不是用来开发 Harness Anything 源码仓库的**。它的用途是：在你自己的业务、产品或基础设施仓库里，用 Harness Anything 管理 Codex 内部 Agent 的开发工作。

| 组件 | 负责什么 |
|---|---|
| [Harness Anything](https://github.com/FairladyZ625/harness-anything) | 为目标仓库提供任务状态、决策、事实、证据、审查与完成门 |
| 本技能 | 让 Codex 主 Agent 正确拆包、派发内部 Agent、回收证据、写回 Harness 并完成语义验收 |

尚未接入 Harness Anything？先看它的[中文入门文档](https://github.com/FairladyZ625/harness-anything/blob/main/docs-release/start/zh/00-what-is-this.md)，把 Harness 安装到你准备开发的目标仓库，再使用本技能编排工作。

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
使用 $codex-internal-agent-orchestration，在当前业务仓库里读取已有 Harness task，把这次跨模块重构拆成任务图，安排内部 Agent 侦察和实现，并把进展、证据与验收结果写回 Harness。
```

## 适用场景

- 多模块或长时间运行的复杂任务
- 可并行的只读侦察、风险枚举和方案比较
- 写面互不重叠的实现切片
- 独立代码复核和失败模式检查
- 在已接入 Harness Anything 的目标仓库里管理 task、decision、fact、relation、progress 与 evidence
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
- 每个委托包回答 Context、Request、Output Format、Constraints、Checkpoint 五问。
- 高耦合模块保持单写者，互不冲突的任务才并行。
- Agent 返回的是 claim；主 Agent 回读 ground-truth 后才成为 evidence。
- `ha` 命令面通过 capabilities 和 help 当场发现，不冻结易漂移参数。
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
│   ├── ground-truth-discipline.md
│   ├── harness-orchestration.md
│   ├── role-matrix.md
│   ├── runtime-channels.md
│   ├── skill-maintenance.md
│   └── worker-handbook.md
└── LICENSE
```

## License

[MIT](LICENSE) © 2026 FairladyZ625
