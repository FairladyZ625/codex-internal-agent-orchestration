# Skill 维护

## 内容边界

只收录换仓库仍成立的内部 Codex 编排方法。

- `SKILL.md`：成本门、任务图、内部执行边界、生命周期和验收原则。
- `role-matrix.md`：角色责任与禁止项。
- `runtime-channels.md`：内部工具、并发预算、监管和失败恢复。
- `delegation-templates.md`：brief 与 prompt 契约。
- `harness-orchestration.md`：在用户目标仓库中使用 Harness Anything 的条件性投影与 CLI 适配。
- `ground-truth-discipline.md`：回执、全文、全量、终态与对照纪律。
- `acceptance.md`：ground-truth、编排完整性和使用证明。
- `worker-handbook.md`：一线 Worker 纪律。

不要新增 CHANGELOG 或 QUICK_REFERENCE。

## 不变量

- 使用当前 Codex 主 Agent 与当前任务内的内部 subagent。
- 内部工具不可用时，由主 Agent 本地接管或向用户说明。
- 新增角色或通道时，保持当前任务内的协作边界。

## 更新流程

1. 为实际缺口构造至少 3 个行为场景。
2. 只增加修复已观察失败的最小规则；优先改写既有段落。
3. 用新 Codex 内部 Agent 做正向测试时，只传技能与真实任务，不泄露预期答案。
4. 运行 `quick_validate.py`，检查 frontmatter、链接、正文长度和 `agents/openai.yaml` 一致性。
5. 全文搜索绝对路径、死链和重复规则。

## 行为验收场景

至少覆盖：

1. 低风险单文件短任务：主 Agent 直接做，不为仪式派 Agent。
2. 三条线共享核心模块：建立依赖图，保持单写者，不盲目全并行。
3. Worker 声称通过但关键验证未运行：主 Agent 不宣布完成，先 fresh verification。
4. 内部 Agent 两次失败：熔断并由主 Agent 本地接管。
5. Harness 多协作者无 claim：查 task/progress、声明写面，碰撞时停手升级。
6. 内部 Agent 工具不可用：由主 Agent 串行完成或说明限制。
7. 用户目标仓库升级 `ha` 命令面：应通过 capabilities/help 重新发现，不沿用冻结参数。
8. Harness Anything 自身存在仓库专属流程：不得把这些约定复制到本技能。
