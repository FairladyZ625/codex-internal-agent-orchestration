# Harness 编排投影

## 启用条件

仓库存在 `harness/harness.yaml`，或治理明确要求 Harness Anything 时，先判断是否需要投影：已有 task、跨回合或多协作者、需要承重 fact/decision/relation、长程监管或仓库明确强制时启用。可在一个短回合完成的低风险单步任务，不因 Harness 存在自动创建 task。

启用后先读仓库根 `AGENTS.md`、`harness/AGENTS.md`、`harness/harness.yaml`；已有 task 时再读 `task_plan.md`、`read_set.md` 和其中点名材料。

不要把示例当永久 CLI 契约。每次先运行：

```bash
ha capabilities --json
ha task capabilities --json
ha decision capabilities --json
ha fact capabilities --json
ha relation capabilities --json
```

拿不准参数时用 `ha <kind> --help`。工作区只暴露 `npx harness-anything` 时使用等价命令；不要使用 stale 的 `harness` / `npx harness`。

## 从任务图到 Harness

| 编排概念 | Harness 原语 | 规则 |
|---|---|---|
| Mission / 工作包 | task | 通过 CLI + 合适 preset 创建，不手搓目录 |
| 子工作包 | child task | 父任务负责 mission；子任务写面和验收必须独立 |
| 执行依赖 | task `depends-on` relation | 只表达真实依赖，不用关系图代替调度 |
| 可复核观察 | fact | 记录实际观察与来源，不把计划写成 fact |
| 承重选择 | decision | 目标、范围、不可逆选择和长期政策由主 Agent/用户裁决 |
| 语义连接 | relation | decision 派生 task、fact 支撑 decision 时建立真实边 |
| 执行进展 | progress + evidence | 只写有工具证据的进展；未验证项明确标注 |

task review/complete 的 PASS/FAIL 是产出裁决，不自动成为 decision。只有暴露战略取舍时才新增 decision。

## 启动协议

1. 用 `ha task list --json`、`ha task show <id> --json`、`ha task tree <id> --json` 检查已有任务、层级、状态与最近 progress。
2. 达到投影门槛且没有适配 task 时，先 `ha preset list`，再按当前 `ha task create --help` 创建；未达到门槛时不造空壳 task。
3. 写清 objective、non-goals、risk、owner、write set、depends-on、acceptance、budget 和 stop conditions。
4. transition 到 active，并在 progress 声明 owner、工作区和预计写面。
5. 有共享包、contract、gate、治理文档或跨线接口时，派活前向其他协作者声明。

当前没有强制 claim/lease 时，CLI 串行写只保证记录不损坏，不保证没有两个人做同一任务。发现重复 owner 或写面碰撞时双方停手，由主 Agent/用户裁决后再继续。

## 执行与证据

- Worker 可以读取全仓理解根因，但只写授权 task/write set。
- Worker 用 progress append 回流阶段性证据；稳定观察用 fact record。
- coordinator-owned/generated ledger 不手改；使用当前 governance/check/rebuild 命令面。
- decision 创建、接受、推翻和承重关系由主 Agent/用户保留，除非 mission 明确授权更窄动作。
- task/fact/decision/relation 经 WriteCoordinator 自动提交时，不补第二个手工 commit。
- 公开实现与私有 `harness/` 记录分仓处理，禁止把私有 task 内容复制进公开仓。

## 收口门

进入 review/complete 前至少满足：

1. task graph 的依赖、子任务和状态与真实实现一致。
2. closeout/review 不是占位文本，且有至少一条真实 fact。
3. 相关验证已运行；未运行项有 owner 与残留风险。
4. 承重 decision、fact 和 task 间的真实关系已落库。
5. 主 Agent 完成语义与使用性验收，再按当前 `ha task review/complete --help` 收口。

Harness 是编排状态与证据的真相面，不替代源码 diff、测试、运行日志和真实入口。
