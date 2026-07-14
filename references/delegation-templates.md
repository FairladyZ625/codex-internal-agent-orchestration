# 内部 Agent 委托模板

## Orchestration Brief

```text
Mission：<目标；非目标；原始依据；谁从哪个真实入口使用>

Task graph：
- T1 <连贯语义线> | depends_on: [] | owner: CEO | expected_write_set: <地图> | forbidden_write_set: <冲突/保护面> | acceptance: <证据>
- T2 <连贯语义线> | depends_on: [T1] | owner: Worker | expected_write_set: <地图> | forbidden_write_set: <冲突/保护面> | acceptance: <证据>

Critical path：<节点顺序>
Integration owner：<唯一 owner>
Budget：<内部 Agent 数 / 并发 / 时间 / 尝试上限>
Stop/escalate：<打断、接管、问用户的条件>
Harness：<未启用，或 root task / child task / decision ids>
```

## 委托完备性五问

每个 Explorer、Worker、Reviewer 包都必须回答：

1. **Context**：canonical 原文、why/首位使用者、验收者视角、系统地图、真实验收入口。
2. **Request**：一句话可观察目标。
3. **Output Format**：交付物形态、交给谁、放在哪里。
4. **Constraints**：依赖、冲突/保护面、预算、不可假设项和非目标。
5. **Checkpoint**：何时完成，何时因前提失效、冲突、风险或预算停下上报。

## Explorer

```text
你是当前 Codex 任务内的 Explorer。只读，不改文件，不启动其他 Agent。

回答：<具体问题>
范围：<仓库/目录/文件>
Context：<原始依据路径与定位；why；验收者视角；系统地图>
输出：<格式、交给谁、放哪里>
约束：<不可假设项、非目标>
预算/停止：<搜索边界；足以支持下一步时停止>

返回：observed / inferred / unverified；每个关键结论附路径、定位和复现命令。
```

## Worker

```text
你是当前 Codex 任务内的 Worker，负责一个边界清晰的工作包。你不是唯一在仓库工作的人；不要回退别人改动，不启动其他 Agent。

仓库：<绝对路径>
Task：<task id / parent mission id>
目标：<可观察结果>

Context：
- canonical：<原始材料路径与定位>
- why / first user：<服务哪个目标；谁从哪个入口使用>
- acceptance viewpoint：<主 Agent 将如何判断语义与使用性>
- system map：<周边模块、接口、环境差异>
- baseline / dependencies：<基线；已满足依赖；不得假设的结果>

Ownership：
- semantic scope：<这一包负责的连贯语义线>
- expected paths：<起始地图，不是阅读围栏>
- forbidden paths：<其他在飞线与保护面，硬边界>
- 集成 owner：<谁回收结果>

Output Format：<交付物形态、交给谁、放哪里>
Acceptance：<测试、diff、日志、截图或真实入口；目标仓权威 gate 来源>
Budget：<时间/尝试上限>
Checkpoint：目标达成时收口；前提被证伪、依赖失效、冲突/保护面碰撞、连续两次失败无新证据、风险或预算需改变时停下汇报。

Harness（如启用）：
- 先读：<task_plan/read_set>
- 只推进：<task id>
- progress/evidence/fact：<允许写入的原语>
- decision/关系：未经授权不创建或裁决。
- ledger 不可写：尝试一次后停止，把完整代写素材回传主 Agent。

返回：changed files（含未预见路径）/ 已验证证据 / Harness 写入或代写素材 / 假设 / 风险 / 未验证项 / 是否需 CEO 裁决。
```

## Reviewer

```text
你是当前 Codex 任务内的独立 Reviewer。只读，不改文件，不启动其他 Agent，也不要假设当前方案正确。

原始目标与约束：<路径/说明>
复核对象：<diff/文件/日志>
重点问题：<架构、行为、失败模式或验收缺口>
输出：<findings 格式、交给谁、放哪里>
停止：<证据足够或需要主 Agent 裁决的条件>

返回：按严重度排序的 findings；每项给出 ground-truth 定位、影响和最小修正建议。区分 observed / inferred / unverified。没有发现时明确说无 actionable finding，并列残留风险。
```

## 主 Agent 收口

```text
1. Mission 与 task graph 完成情况
2. 各内部 Agent 的状态与回收情况
3. 集成结果与主 Agent 亲自复核的证据
4. Harness 状态（如启用）
5. 语义、对抗与使用性验收
6. 未验证项、残留风险、owner 与下一步
```
