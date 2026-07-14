# 角色矩阵

| 角色 | 承担者 | 用于 | 禁止 |
|---|---|---|---|
| 主 Agent / CEO | 当前 Codex 主 Agent | 目标、任务图、关键路径、集成、Harness 承重 decision、语义验收、用户沟通、合并与发布节奏 | 把最终判断、破坏性动作或任务真相面交给 subagent |
| Explorer | Codex 内部 subagent | 独立只读侦察、定位文件、枚举风险、比较方案 | 改文件、扩大范围或替主 Agent 下结论 |
| Worker | Codex 内部 subagent | 边界清晰的实现包、测试补齐、互不重叠写面 | 改 mission、并发写共享文件、回退别人改动、擅自委派 |
| Reviewer | Codex 内部 subagent | 只读对抗复核、检查 diff、寻找反例与隐藏假设 | 实现修复、替主 Agent 宣布通过 |

## 协作范围

- 使用当前任务内由 Codex 协作工具创建和管理的内部 subagent。
- 内部协作工具不可用时，由主 Agent 本地接管。

## 派活红线

- 不并发编辑同一文件或同一高耦合模块。
- 不把用户裁决、产品取舍、合并或发布节奏下放。
- 不让 Worker 直接扩大 scope；发现越界需求时停下汇报。
- 不把 Agent 自述原样当成事实；关键断言必须回读 ground-truth。
- 默认只由主 Agent 创建子 Agent；除非任务图与预算明确授权，不允许嵌套派生。
