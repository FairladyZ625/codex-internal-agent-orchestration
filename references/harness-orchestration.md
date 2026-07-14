# 在目标仓库中使用 Harness Anything

## 范围

本协议用于**已经接入 Harness Anything 的用户目标仓库**。它把该仓库的开发任务投影为 Harness 原语，不假设目标仓库是 Harness Anything 的源码仓，也不继承其仓库专属开发约定。

目标仓库存在 `harness/harness.yaml`，或仓库治理明确要求 Harness Anything 时，先判断任务是否需要投影：已有 task、跨回合或多协作者、需要承重 fact/decision/relation、长程监管或仓库强制时启用。可在一个短回合完成的低风险单步任务，不因 Harness 存在自动创建 task。

## 先发现当前命令面

启用后先读目标仓库根 `AGENTS.md`、`harness/AGENTS.md`、`harness/harness.yaml`；已有 task 时再读其 `task_plan.md`、`read_set.md` 和点名材料。

不要把示例命令当永久契约。每次先运行：

```bash
ha capabilities --json
ha task capabilities --json
ha decision capabilities --json
ha fact capabilities --json
ha relation capabilities --json
```

再对即将执行的原语运行 `ha <kind> --help` 或更窄的 capabilities。当前环境只暴露仓库声明的等价入口时，使用该入口。遇到未知参数或返回结构时，停止猜测，重新读取自描述输出。

## 从任务图到 Harness

| 编排概念 | Harness 原语 | 规则 |
|---|---|---|
| Mission / 工作包 | task | 通过当前 CLI 与合适 preset/profile 创建，不手搓目录 |
| 子工作包 | child task | 父子关系表达任务分解，不自动表达执行依赖 |
| 执行依赖 | task `depends-on` relation | 只记录真实依赖，不用父子层级代替依赖 |
| 承重观察 | fact | 记录会被未来决策或跨任务推理引用的观察，不用普通交付证据凑数 |
| 承重选择 | decision | 目标、范围、不可逆选择和长期政策由主 Agent/用户裁决 |
| 语义连接 | relation | 只建立真实的派生、支撑或依赖关系 |
| 执行进展 | progress + evidence | 记录阶段进展、diff、日志、截图、报告与验证证据 |

task review/complete 的 PASS/FAIL 是任务产出裁决，不自动成为 decision。只有出现承重取舍时才新增 decision。

## 启动协议

1. 用当前 `task list/show/tree` 命令面检查已有任务、层级、状态、最近 progress 和 owner。
2. 达到投影门槛且没有适配 task 时，先发现 preset/profile，再按当前 `task create --help` 创建；未达到门槛时不造空壳 task。
3. 在 task plan 写清 objective、non-goals、risk、context、owner、协调边界、depends-on、acceptance、budget 与 stop conditions；不得保留占位模板。
4. 从结构化回执取得新 id，但把回执只视为 claim；立即 `task show` 回读实体，确认字段和关系真实落盘。
5. transition 到 active，并在 progress 声明 owner、工作区、预计语义面和冲突/保护面。
6. 父任务表达 mission 分解；真实执行依赖单独建立 `depends-on`。不要混淆两个概念。

CLI 的串行写入只保证记录不损坏，不保证没有两个人做同一任务。发现重复 owner 或写面碰撞时双方停手，由主 Agent/用户裁决后再继续。

## 权责与记录

- 主 Agent 保留 mission、承重 decision、语义验收、破坏性动作和最终收口裁决。
- Worker 只推进授权 task；用 progress/evidence 回流执行证据，用 fact 记录稳定且承重的观察。
- 主 Agent 自己也使用相同记录系统：排期选择、验收裁决、task↔decision、fact↔decision 的承重关系不能只留在聊天里。
- Worker 可以读全仓理解根因。预期路径是地图，不是信息围栏；冲突/保护面是硬边界，碰撞时立即停手。
- 不手改 coordinator-owned/generated ledger；使用当前 governance/check/rebuild 命令面。
- Harness 写入由协调器提交时，不再补第二个手工 commit。
- 私有 `harness/` 记录与公开实现分仓时，遵守目标仓库的边界，不复制私有 task 内容到公开仓。

若 Worker 所在工作区无法写 Harness ledger，只尝试一次。失败后停止重试，把 progress、evidence、fact 候选和 closeout 素材完整回传，由主 Agent 在正确工作区写入。

## 证据纪律

- progress 只记录已发生的实质进展；“准备执行”不是进展。
- evidence 保存单轮交付证据；fact 只保存会长期参与推理的观察。
- 每次写入后回读实体或目标文件，不只采信 stdout。
- 关键不变量核全文，不只核 diff；列表核全量，不只核默认分页。
- 检测器报告“没有问题”前先做阳性对照；声称失败与本改动无关前先建立有效阴性对照。
- gate 从目标仓库的 preset/profile、治理文档或 CI 配置派生，不凭记忆维护一张静态清单。

完整方法见 [ground-truth-discipline.md](ground-truth-discipline.md)。

## 收口门

进入 review/complete 前至少满足：

1. task graph 的依赖、子任务和状态与真实实现一致。
2. task plan、closeout、review 都是实质内容，不是占位文本。
3. 相关验证已运行；未运行项有 owner 与残留风险。
4. 承重 decision、fact 和 task 间的真实关系已落库。
5. 主 Agent 从原始目标做语义验收，并亲自验证真实使用入口。
6. 按当前 `task review/complete --help` 和目标仓库 resolved preset/profile 的门收口。

Harness Anything 是目标仓库的项目记忆与完成治理面，不替代源码 diff、测试、运行日志和真实入口。
