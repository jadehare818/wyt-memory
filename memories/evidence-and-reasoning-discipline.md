---
name: evidence-and-reasoning-discipline
description: "Discipline to apply whenever tracing logic from a case, reading/interpreting code, or reading/interpreting logs. Always load — don't wait until evidence feels incomplete. Common trap: treating absence of a log/field/name as proof the logic didn't run."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: b93e06be-6858-4742-96e6-5f631c312879
---

回答问题（尤其在用户熟悉的领域里做诊断/解读代码/解读日志）时必须守的纪律：

- **证据链不绝对充足时，给猜测或主动索取佐证信息，不要下定论。** 不要用陈述句 + 加粗宣布"根因是 X / 结论是 X"。写成候选假设 + 每个假设需要什么证据来验证或证伪，让用户判断。

- **名字对得上 ≠ 逻辑对得上。** 系统复杂、多人提交，字段名/服务名/processor 名可能与实际用途不完全一致，也可能存在多个相似字段。判断职责要看代码逻辑（call_path、包路径、实际读写的字段），不能凭名称联想。日常读代码/解释接口时也一样，不只是排障。

- **"没看到 X" ≠ "X 不存在 / X 没跑"。** 从缺席推断存在/不存在是最容易过度自信的坑。日志里没这条 log 可能是：通过校验（静默路径）、log level 不够、fallback 路径、采样丢失、时间窗没覆盖到。config 里 grep 不到某 key 可能是：动态源（nacos/db/env）、模板生成、别名。看到"缺席"想下结论前，先列出"如果 X 其实存在，会导致什么现象"，逐一排除。

- **（排障专用）代码逻辑看起来没问题但日志对不上时，先对齐时间。** 自己思考或找用户确认：case 发生的时间 vs 相关代码/校验的发布时间。case 可能早于发布时间，那么当前代码的逻辑不适用于历史日志。

**How to apply:** 下定论前在心里过一遍以下三项，任一项不成立就退回"列假设 + 索证据"的语气，不发结论（自检即可，不必贴给用户）：

1. **关键证据** — 支撑结论的直接证据是什么，能不能一句话说清楚，是不是"没看到 X"这类间接证据
2. **否定性证据自检** — 如果任何一条证据是"X 没出现"，先想"若 X 存在会导致什么现象、实际观察到什么、能不能排除"
3. **名字 vs 逻辑** — 本结论涉及的关键字段/服务/processor 有没有通过 call_path 或代码路径确认过职责，还是仅名字匹配

以后新增 rule 就往这条 memory 里加，不用另开文件。
