# 01 · EFD 旧工作、AAAI-27 评审与重构动机

> 状态：**既有研究事实 / 审稿复盘**，不是新项目结果。依据：旧稿《Evidence before Opinions: Evidence-First Deliberation for Multi-Agent Decisions under Distributed Information》与 2026-10-25 提交的五位 reviewer rebuttal。原文见 [archive](../archive/AAAI27_reviews_and_rebuttal.txt)；数据要回溯到实验日志/表格，不能只凭本页重新引用为新的统计结论。

## 1. 旧稿要解决什么，做了什么

**设定。** 全局任务需要组合分散的私有观察 `O_1,...,O_N`。Opinion-first 首先让每个 agent 报出本地答案＋理由；如果本地答案不足以决定全局答案，第一条公共消息可能丢掉后续的桥接证据，同时公开答案会影响后续接收者。

**EFD。** 第一阶段所有 agent 仅传决策无关的事实 memo；汇总 memo 后，第二阶段各 decision agent 独立投票，再做固定多数聚合。保持 observations、模型、随机种子、调用次数 `2N`、汇总规则等尽量匹配，隔离 first-message contract（不是改变辩论拓扑或推出学习算法）。

**原论文主要贡献。** (i) first-message bottleneck 的概念和控制诊断；(ii) 事实优先、延迟公开本地答案的极简协议；(iii) 分布式 QA、HiddenBench、Shortlist、Schedule 上的实证与机制控制。其因果估计针对**整套首消息通信契约**，不能把相对 Opinion 的全部增益都解释为“时间顺序”或“答案标签”。

## 2. 已有实验证据：哪些应该坚决保留

| 证据 | 已观察的结果 | 正确解读 / 限制 |
|---|---|---|
| 原稿 6 模型×2 QA 主网格 | EFD−opinion-first 平均 **+12.7 EM**；对应数据 HotpotQA、MuSiQue，每模型单元 1,000 items、3 seeds | 对特定对照强，**不是**全面优于所有 MAS 方法 |
| HiddenBench 官方 65 任务 | 四个 Qwen 的宏平均 EFD−Opinion **+36.7 pp**；所有报告的 model-seed 比较正向 | Bench 围绕 hidden profile，难度和四角色构造不可忽视 |
| Raw pooling | 原稿 6 模型×2 QA 汇总 EFD−Raw **−0.5 EM**；Raw 用 `N` calls，EFD `2N` | 中央化可行时 Raw 是强对照；它不是真值上界 |
| Gemma3-12B 的 Raw 反例 | Hotpot EFD/Raw **56.5/49.5**；MuSiQue **36.2/29.1** | 局部去噪有时改善接收端，但不是普遍保证 |
| 第一步信息的质量 | Qwen3-14B support recall（H/M）：EFD **64.5/33.7**，Memo+Answer **69.8/40.5**，Fact+Ans **81.1/50.6** | 词汇召回更多而 EM 更差，证明 recall 不是充分解释；不自动证明“答案产生全部下降” |
| Qwen3-14B 一种生成控制 | EFD **52.6/26.5**，Memo+Answer **39.1/19.4**，Fact+Ans **37.5/17.4** | 内容、生成过程、顺序、标签交织；需结合固定 memo |
| 固定报告＋标签 | 3 seeds：事实 memo 隐/显 **52.4→47.4 / 26.9→21.1**；opinion rationale 隐/显 **45.2→33.1 / 16.3→10.0** | Public answer 与报告内容有可观察交互；Hotpot 的 Difference-in-Differences **−7.1 EM**，MuSiQue **−0.5 EM**；机制解释尚待验证 |
| Exact Shortlist | 生成 EFD **50.2**，Opinion **13.9**，Raw **99.5**；固定 3 份精确 shortlist **99.4**，附加局部标签 **86.6** | 消息生成不完整、接收端被局部结论干扰是两类不同可研究失败；固定列表不代表生成器能达到同等质量 |
| Schedule 负例 | Easy EFD **41.0** vs Opinion **47.3**；Medium **32.7** vs **30.7**；Hard 几乎全 0 | 在局部计划本身有计算价值时，“只摘事实”可能不够；Hard 主要为 solver-bound |
| 同质初始输入的 35,787 轨迹审计 | 一致性可能提高而正确候选覆盖下降 | 背景动机，**不是**分布式机制的直接因果证据 |

补充辨析：HotpotQA 的实验分片中第三角色不持有金标支持段落，所以“两 memo 比三 memo 略好”（如 53.8 vs 52.6）不应被当成“相关证据越多越差”的一般规律。支持事实全部或部分缺失与非支持段落造成注意力干扰要分开。

## 3. AAAI-27 五位审稿人：共识在哪里

| Reviewer | 分数 | 最深层担忧 | Rebuttal 状态 |
|---|---:|---|---|
| **6ytG** | 5 | 原稿未体现真正受限 raw；Opinion 可能太弱；简单协议的 novelty | 32/64/128 tokens、长私有文本、thinking Fact+Ans 和 3-seed 重复等已直接回应；**是否因此改分未定** |
| **VHgR** | 4 | novelty / significance；real tool locality；强 debate/routing；内容 vs. visibility | 2×2、tool-local shards、matched debate、InfoNav-style routing 已补；对方法独创性仍是结构问题 |
| **tUa6** | 6 | 非自定 QA 分片、70B 3 seeds、强 debate 与 support coverage | 72B annotation-free native-order thirds、无过滤、3 seeds：H **+5.3**、M **+8.3**，满足其明示的升分实验条件；评分变化未定 |
| **qS8k** | 6 | 原分片用 gold support 人为分散；自然分布；标签顺序混杂；与路由系统比较 | no-gold / tool-local、答案前后/跨题标签、对比 debate/routing、token 统计加强因果与外部有效性 |
| **Satb** | 5 | “先事实”与 shared memory / selective communication 的本质区别；raw 何时不够；机制多因素 | budgeted raw、长视图、2×2与定位说明得到强化；仍需要比协议更深的新贡献 |

> 分数为本轮审稿初始分：**6, 6, 5, 5, 4**。审稿人提出的提分条件被正面实验回应，**不等于已更新分数或已录用**；这个仓库为未定结果独立重构。

### Rebuttal 重要增补（之后新稿应优先核对并按需复用）

- **严格通信预算**：32/64/128-token 消息，12 个模型—数据集—预算单元的 EFD−cap-matched Raw = **+7.9 至 +23.2 EM**；对方 Raw 采用对应限制下的“question-relevant sentences”方式，未来须写清具体生成与长度规则。
- **长观察**：每节点约 12k 私有 tokens，三节点 raw 合计超过 32k 窗口；EFD−Opinion **+11.6/+8.6**，EFD−truncated Raw **+6.4/+2.4†**；`†` 与 CI 需按原日志复核，不能全部称显著。
- **Thinking**：Qwen3 8B/14B thinking 启用后 EFD 对 Fact+Ans 仍领先 **+10.3 至 +15.1 EM**。
- **无标注自然顺序切分**：Qwen2.5-72B 3 seeds、各 1,000 条；Hotpot **58.7 vs 53.4**，MuSiQue **38.1 vs 29.8**；不使用 gold annotation、不做 item filtering。
- **Tool-local retrieval**：按标题哈希分 corpus shards，各 agent 只搜索其 shard、无 gold filtering；14B 3 seeds EFD−Opinion **+11.4/+5.5**。
- **强相邻 baseline**：agent 保留私有段落的 matched-budget debate（2N/3N）及 InfoNav-style request-then-answer（3N）均已与 EFD 比较；需注明是**按论文条件复现的风格基线**，不是宣称无误复现所有原作者实现。
- **公开答案本身的效应**：own-answer vs foreign-answer / 首尾顺序干预；own/foreign 差异支持 answer content 重要，却不证明每种机制的具体神经实现。
- **成本**：14B 的 memo 平均每消息 66/79 tokens、Opinion 102/114；整个问题总 tokens EFD 2.8k/4.1k、Opinion 3.2k/4.5k（H/M）。比较系统成本不能只拿 phase-one 长度当完整成本。

## 4. 哪些宏观问题仍然没有被「补实验」解决

1. **研究对象不够深：** “先发事实”是协议设计原则；相对于隐藏档案、shared memory、routing，它没有独立的学习能力或清楚的计算对象。
2. **存在更强的自然方案：** Raw pooling、中央检索、压缩/路由、事实过滤、可行集结果。以后不可把 Opinion-First 当主要实用对手。
3. **有两类错误未统一：** (A) sender 表达遗漏必要事实/关系；(B) evidence 已存在，receiver 却被局部答案干扰。它们可共存，不能把整套差距归结为一种机制。
4. **EFD 有价值的反例：** Shortlist 需要完整局部可行域，Schedule 需要计算局部约束；“禁止所有局部推断”不是目标。
5. **分布与因果界限：** 构造的 hidden-profile、非自然分片、生成日志 seed 需要在新稿中与更自然的边界条件任务分开；无标注切分仍是人为分配，不等于真实部署。
6. **实际用途/完整成本：** 应研究当本地计算能减少下游工作、而 raw 的传输和接收成本高时，是否改善 accuracy–cost Pareto frontier；不能靠限制 raw baseline 制造优势。
7. **主张过载：** 原稿同质输入审计、EFD 大主表、标签干预、HiddenBench、Schedule 分散注意力；新稿要以单一新能力组织旧实验资产。

## 5. 重构的明确决定

**不再以 “Evidence before Opinions” 作为新论文的终极命题。** 旧稿实证保留，但新研究对象转为：**在局部信息不完备且互补条件未知时，如何提前完成有用的局部计算，同时保持其结果对后续伙伴条件的可组合性（Composable Local Reasoning）**。

不抹掉 EFD：它是只传事实的关键基线；Opinion-First 是过早全局承诺的端点；Raw 是保真但可能昂贵的端点。新的方法是否需要学习、什么表示最好、哪些机制主导，需由有区分力的实验选择。

**已确定的是问题与科学评价原则，不是具体 Prompt、RL 公式、三态构造、论文标题或目标接受率。** 详见 [03](03_RESEARCH_BLUEPRINT.md)；行动约束见 [04](04_AGENT_HANDOFF.md)。

## 6. 数据可信度与待核查

- 本文保留的是用户提供原稿和 rebuttal 的**已报告数值**，不是本仓库重新跑出的结果。新稿必须建立原始 predictions / splits / seeds / configs 的 manifest。
- **98–100% 的精确 shortlist 结果依赖于 oracle-level 中间输入**；不能直接把“加入局部消元后有望接近 99%”写成预期实现结果。
- “Schedule All EFD−Opinion −1.9”在原稿表格 vs caption 存在聚合口径差异，rebuttal 表示另含一组 8B screen；新稿若使用需统一口径，不能混引。
- **事实支持 token recall** ≠ 跨文档关系绑定完整度，也 ≠ 可用的推理量。
- 当前 AAAI 评审进行中；不从 reviewer 条件推定最终结果。

---
**资料定位：** 原稿/原始审稿的精确英文原文保存在 [archive](../archive/AAAI27_reviews_and_rebuttal.txt)；本文是研究选择依据而非替代原件。