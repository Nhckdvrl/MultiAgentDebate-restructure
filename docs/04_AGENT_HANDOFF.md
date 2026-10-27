# 04 · Local Research Agent Handoff（每次会话/每轮执行前必读）

> **目标**：让接手本仓库的本地 Agent 即使不在原讨论现场，也能以**同一科学问题、同一证据标准、同一学术品味**持续推进，且诚实记录失败。  
> **快照**：2026-10-27；研究方向收敛，尚无冻结的下一代方法与新结果。  
> **本文件是常驻操作约束，不是让你一次性照单实现的任务清单。**

## 0. 每轮的启动协议（必须做）

1. **读完整** `README.md`、`AGENTS.md`、`docs/01_EFD_AND_REVIEW.md`、`docs/02_RELATED_WORK.md`、`docs/03_RESEARCH_BLUEPRINT.md` 和**本文件**；若存在新实验 ledger/manifests/results，检查最后的决策与失败记录。不要只读上次会话摘要。
2. **检查仓库与实际资源事实**：当前代码/数据/日志是否在本仓库或相邻原 EFD 工程？`git status`、目录、可用模型/端点、硬件、预算、数据许可、config。不能从聊天假设已经存了日志/有某个 GPU。
3. 本轮先写 **Research Intent**（可先放实验日志）：要回答哪个科学问题、竞争解释、最低决定性对照、结果怎样改变方法选择。不以“把某个模型跑起来”取代明确问题。
4. 只在必要时写代码/运行实验。先最小验证，再规模化；精度与数据完整性优先于任务数量。
5. 本轮结束留下简洁 **Decision Log**：做了什么；新证据和来源；显著性/置信区间或不能下结论的原因；与中心命题的关系；是否更新路线；下一项最有信息价值的工作。更新文档时只保留稳定新知识，不累积每一轮措辞。

### 当前统一问题（严禁不经讨论改写成更浅的目标）

**Composable Local Reasoning：在分布式、不完整的信息条件下，模型怎样提前完成有价值的局部多步推理/约束约简，同时保留让结果随未知互补伙伴条件正确变化的必要关系，避免局部决策过早升级为全局承诺？**

- “局部完成计算”≠“可以提交最终全局答案”。
- “支持句子完整”≠“关系绑定/局部约简/接收端正确使用完整”。
- “同一消息在两种伙伴世界里都答对”≠“消息贡献了计算”；空消息和简单提取是必要对照。
- EFD/Opinion/Raw 是历史端点，**不是**新论文唯一对手。
- 目前不要求实现重型 DSL、CEGIS、自博弈、自建训练框架；想加新组件，先证明其解决当前未被简单方法处理的瓶颈。

## 1. 两条最高优先级科研纪律

### A. 禁止“防御性瘫痪”（Defensive Paralysis）

**不允许**如下论证循环：
> “思考一个有价值的核心命题 → 搜到 2–4 篇近邻 2025–26 论文 → 说‘这个有相关研究、不能做’ → 缩成微不足道的 prompt 变体。”

2026 年的热点没有“无人区”。近邻工作往往说明 **consensus bottleneck**。正确动作是：
- 指出他们实际做成了什么（目标、训练、对象、实验）；
- 明确该能力哪一部分没被解释或泛化；
- 借用其最强工具，设计比它更有力的机制解释、计算对象或方法；
- 用可区分的反事实、强对照和真实泛化证明我们的进展。
不要为了显得“无争议的新颖”而不断后退。**Novelty check 是磨刀，不是 veto 机器。**

### B. 禁止“现象赌博”（Phenomenon Gambling）

**不允许**把整篇论文压在某个尚未证实的副效应：Hotpot−7.1 vs MuSiQue−0.5 的差异必须被某种认知解释解释；H₀/H_A 一定显示组合短路；某个 3-field prompt 一定胜；GRPO 一定比 SFT 好；Shortlist 一定能涨到 90%+。

优先把论文建在**任务计算结构**与**可验证的新能力**上。机制研究要大胆，但失败时应能转化为边界/竞争解释/新路标。**允许探针否定方法假设，不允许先写漂亮故事再筛选能支持的样本。**

## 2. 学术态度：保持全局观，别局部贪婪

- 先问“新工作最终需要独立证明什么”，再决定某个 ablation 是否值得做。禁止因一个小差距反复调 Prompt 却忘记主问题。
- 接受 Gemini/其他模型为思考材料：不全信、不全否；不要把与另一个模型辩论当成果。
- 别滥用“这个定理不是新”“那篇论文做了部分内容”“该机制不一定出现”式防御措辞。只在其**实质影响研究主张或基线选择**时说明；随后明确可推进的科学增量。
- 也不要过度宣传“首次”、通用最优、必然的正/负因果机制。**理论边界与经验边界要同等严谨**。
- 避免把 1985 hidden profile、信息论 index、部分求值、factor graph 当作我们原创证明；把它们作为导出新问题/训练目标/控制设计的基础。
- 不追求同时加入学习、路由、验证器、隐私、agent 拓扑与所有 benchmark。围绕一个主要计算能力提出简洁而深刻的方法。
- 8–9 页主文通常容不下三个各自独立的系统；在 IJCAI/ARR/ICML 不同社区调整讲法，但**不要由会议 deadline 决定科学问题**。
- 研究价值可能来自算法、机制解释或表征本身；RL 不是科研价值的必要条件，使用 GRPO 也不自动使工作“learning-hardcore”。
- 统计上保持冷静：CI/bootstrap/配对对照、预注册停止标准、失败披露；不要把小样本偶然胜利写成结论。

## 3. 关键定义与数学检查（写代码前先过一遍）

### 3.1 Safe local closure

安全的局部推导/排除须有**作用域与完备性保证**。两个基本原型：

- 完整的本地硬约束可行集 `C_i`：`a∉C_i` 是全局合取可行性中的安全排除，但 `a∈C_i` 不是全局可行证明。
- 开放的文档切片：正面证据可建立局部关系 `R(a,b)`，但“我的文档没提到 `R(b,c)`”不是全局否定。不得将“删除了一段支持证据”解释成现实真值变成不可回答。

特殊情况：共享变量不能被两个节点各自消元；输出所需变量不能被消掉；时序、否定与非单调约束可能使上述两个极性原型不足。

### 3.2 Cross-partner response

固定 `Q,O_i,m_i`；伙伴 `c_1,c_2` 必须是合法且真值已验证的互补世界，最好构造 `y_1*≠y_2*`。比较 `(S(Q,m_i,c_1),S(Q,m_i,c_2))` 是否分别等于 `(y_1*,y_2*)`。

`J_pair=1[correct_1]1[correct_2]` 只评价**联合正确响应**；它不能证明消息包含本地有用信息。必须有 `m_i=∅`、simple extraction、simple feasible-set 和/或接收端重算的 matched controls。高阶问题还需同时替换**所有** agent 的消息检验联合成功。

### 3.3 Rescue / Corruption / GRPO 数学陷阱

- 与无消息结果配对：Rescue=从错变对，Corruption=从对变错，`ΔAccuracy=Rescue−Corruption`。
- `R(m,c)−R(∅,c)` 在固定 `c` 的候选组内做相对优势时，空消息常数会抵消。
- 二元 `b(c)` 下 `u=max(0,r−b)−γ max(0,b−r)`，对固定 `c` 仍是正仿射 `r`；GRPO 用组内 std 归一化时，`γ` 可能也被抵消。若要优化跨伙伴风险，**先对每条候选消息聚合多个伙伴 `U(m)`，再对候选组做优化/排序**；需实测非零方差和数值细节。
- 全正确乘积目标能去除单一固定答案但可能奖励稀疏；把它作为第一版评价指标很有价值，作为唯一 RL reward 则需样本效率检验。
- Receiver DPO：同一输入 `x`（证据＋局部标签）对应 chosen/rejected 输出；不要把两个不同 prompt 误配成同一 DPO preference pair。必须有局部标签真正正确的正例。

## 4. 探索性实验建议（可重排；不要机械跑满）

**注意：以下是初步设计，不是已敲定 protocol；具体 prompt、代码、数据管道和完整网格由本地 Agent 独立设计、实现、复核后再提出。**

### E-Inventory：先找对数据

- 定位旧 EFD 的 scripts、raw predictions、memo logs、fixed-report replay、sampling ids、seeds、split manifest、token accounting 与 QA / Shortlist / Schedule evaluators。
- 重建一份 **Assets Manifest**：路径、hash、数据版本、模型版本、prompt hash、生成参数、统计口径、是否漏样本。**不要杜撰旧结果可复现的源文件**。
- 区分已报告旧稿、rebuttal、当前新方法、玩具数学例子四类证据。旧稿论文与 review 文件的本仓库备份见 `archive/`。

### E0：Sender/Receiver 机制诊断（不必训练）

**目的：** 识别固定内容下答案标签效应的异质性；初步判断错误发生于证据组合还是最终答案采用。**不是**为了保证某一“劫持”故事成立。

建议：
1. 从已存 `Memo+Answer`、Fact+Ans、原 EFD 固定报告生成可配对的 item 记录；核对同一 item 的 question、source、memo、label、seed、一致 parser。
2. 固定 factual content 做 answer hidden/public 与 fact memo/opinion rationale 的 2×2；算每道题/seed 的交互项，报告 Hotpot/MuSiQue DiD 与正确 CI。避免只读均值推断机制。
3. 标签类型：all/local abstention、具体诱饵候选、局部正确且全局正确、无关候选；若按**生成后标签**分层，标注它是探索性异质性结果，注意题目难度与 receiver prior 混杂。
4. 受控中间结构 `H_0` vs `H_A`：Shortlist 可输出候选×部门关系，QA 可输出关键实体链与来源。原子事实及单跳推断**双门控**后测剩余组合失败；缓存 H 做交叉回放 `(H_0,H_A)×(label hidden, label visible)`。H 必须有可检查的约束，不能泄漏正确最终答案。
5. 单独报告 H 的结构准确、最终 EM、错误切换模式、format failures、token 和时延。不要将显式两阶段回放冒充单次模型内部神经机制的完全分解。

**读出分叉：** 若最终阶段仍被错误标签带偏，关注 receiver scope calibration；若 H 已错误，关注组合阶段；若二者均不突出，机制作为边界结果即可，不影响 E1。

### E1：最重要的“local computation 值得传吗？”探针

核心任务：
- **局部多步＋跨角色桥接**：如把 4-hop 关系分为本地 ≥2-hop 的片段和跨节点依赖，使发送方真正在本地完成关系合成；避免总是仅摘一条桥接事实。
- **Shortlist 合取约束**：完整本地名单、局部可行域/违规淘汰、跨部门唯一/多重交集；包含相同局部观察面对不同合法伙伴集。
- **Schedule**：本地完整约束 vs 全局未决时间窗；注意局部 Top-k 可能漏掉全局唯一可行解。
- **真实外部**：QA annotation-free splits、tool-local shard retrieval、HiddenBench、自然长私有文档；不能把新造的所有样本都叫“自然任务”。

建议最小比较：`Raw`、`EFD facts-only`、`Opinion-First`、`Simple Feasible-Set/Local Chain`、`Scoped/Selective Closure concise natural message`，同接收者、同节点分配、同输出 token cap（32/64/128 可为例）、相同工具和 decoder effort。更强 evidence-only filtering/routing 作为第二层强对照。

**不要为了模板效果添加冗长的“三字段”**，允许自适应紧凑输出；必须先测最朴素 local computation baseline。如果它解决了大部分短板，应认真接受这种结果并重新定位新贡献，而不是强行追加复杂协议。

主要评价：单节点本地推断/关系保真、完整可行集的 precision+recall、final EM、同一消息跨伙伴 pair success、Rescue/Corruption、总体 tokens/calls/latency。分析合法闭合的正/负例、错误边界变量、给错局部答案是否干扰。

### E2：跨伙伴后训练（前提：E1 确认瓶颈）

- 先固定 receiver 和推理协议，主要训练 sender；优先从小规模 SFT/DPO 或单次 GRPO 做对照。不要同时训练双方造成 attribution 混杂。
- 对每个固定 `Q,O_i`，用同一伙伴集合 `C_i` 评估每条候选消息：真实互补、授权证据不足（标记为**证据不足**而不是事实无真值）、世界一致的 swap、伙伴已自足/本地只有干扰、局部已完全充分等状态。实际状态由任务结构决定，不强制三态。
- 明确训练 / 选择 / 评测伙伴组合拆分，避免在 eval 中复用训练反事实关系模板或泄漏 `c` 给 sender。
- 比较**同数据、同训练 token、同模型与 compute**的 SFT、普通 outcome GRPO/DPO 与 partner-conditioned 方法。若普通方法一样有效，也可说明这个能力可学，不能硬宣称奖励新颖。
- 若选择 cross-partner non-corruption 风险，先在多个伙伴上汇总再相对比较候选；检查奖励方差、拒答/沉默投机、训练后对“伙伴信息确实有用”的利用能力。

### E3：外部性、强基线与成本

- 外部任务优先选原生信息边界：annotation-free QA、tool-local retrieval、官方 HiddenBench/InformativeBench，以及有本地真计算的外部数据。可评估 SILO-BENCH 等，但注意算法任务 vs 自然文本的差异。
- **同权限中央基线**：允许分批原文读取、检索、缓存、索引和局部工具执行；不要把 Raw 人为限制成“一次性整本塞进 32k”，除非这是明确对应的部署约束。
- 最新 evidence-router、MOC、CoSIR/普通 collaboration RL、InfoNav、matched debate 选择与当前主张真正竞争的组件进行控制。比较同 token cap、calls、工具可见性、训练数据和接收端。
- 全成本：sender thinking/preprocessing、消息、其他 agent、receiver、反例/验证器、retrieval、重复 rollout、latency、存储和缓存复用。Offline training / synthesis 费用单列，报告 amortization 而非隐藏。

## 5. 高质量实验：预注册和统计规范

每项主实验先写：
- `Question`：什么科学命题与竞争解释？
- `Design`：item 数据分配、训练/验证/测试切分、预算、模型版本、推理温度、种子、随机化；严格防止训练消息看到隐藏 partner/world。
- `Primary metric`：一个最主要指标、次指标、最小有意义增益（不是事后选最好看的）。
- `Failure modes`：何种结果说明假说不成立/该转线？有何替代解释？
- `Analysis`：paired item bootstrap、适合时的 McNemar/sign、训练种子的稳定性、CI/效应量及交互项 CI；不能用 item bootstrap 当作训练 seed variance。
- `Compute`：大样本前做小样本 power/sanity；必要时以 ~500 items 的 power run 检验，不能把少量题目的随机胜利当结论。

可按模型能力/任务难度做条件分析；对自然任务保证失败样本保留、no malformed retry、无基于标签/输出进行事后保留（除非明确作为受控条件，另外报告覆盖率）。

## 6. 结果出来后怎样判断研究前途

- **E1 简单局部集合/两跳消息已足够好：** 不要复杂化以制造新方法；进一步挖掘它何时失败以及 receiver 如何使用，考虑机制＋极简方法的一条扎实路线。
- **E1 关系边界经常错：** 主攻 learnable composable message；逐步引入可信伙伴变化的训练，而非先造大控制器。
- **E0 仅某个标签机制成立：** 写成解释性规律，不更改整篇中心；如深入到可迁移 receiver 机制，也可以成为独立 paper。
- **训练无法超越 simple SFT/outcome RL：** 坚决报告；检查消息表示、数据构造与任务本身，不靠在 loss 上无限加项“救火”。
- **所有方法在自然任务一样好：** 看完整成本；可能本地 computation 并非瓶颈。这是科学边界，需调整论文意义，而非只留强正向合成结果。
- **发现近邻比我们强：** 解读它具体解决了我们的哪一层；借鉴、融合、寻求底层解释、提出更有意义的新主张。**不自动停止研究**，也不伪装文献不存在。

## 7. 每轮 Agent 的最低输出模板

```md
# Research Decision Log — YYYY-MM-DD
## Core question touched
## What was actually inspected/run (commit, data, model, scripts, seeds)
## Result (numerical evidence, CI, costs, and failures) vs interpretation
## Strongest alternative explanation / baseline
## What changed in the overall research map (not just local prompt)
## Next highest-value question / test and why
## Files updated
```

研究结果应以文件、命令、commit、配置 hash、真实输出和失败日志为凭证；不写“已验证”但没有数据的结论。不要在每轮总结中复述全部过去讨论；引用 [03](03_RESEARCH_BLUEPRINT.md) 作为主线。

建议按需新增（**不是要求立刻创建**）：
- `experiments/README.md`：路径、版本和运行规范；
- `experiments/registry.csv` 或 YAML：实验定义/状态；
- `experiments/logs/YYYY-MM-DD_*.md`：结果与决策；
- `data/MANIFEST.md`：样本来源、split、hash、许可；
- `notes/open_questions.md`：未解决高价值问题，更新删除过期项。

## 8. 权限、资料与科研纪律

- 此仓库是公开的，用户已允许收录旧稿和 review；对进一步的个人信息、密钥、API token、未授权第三方数据绝不上传。检查 `git diff` 再提交。
- 当前 AAAI-27 讨论尚未最终确定；不能写成已拒稿/已录用。若会议规则涉及公开匿名材料，再由仓库维护者决定是否调整 archive。
- 要调查源论文时读最新版主文/方法/附录，核对是否被接受、官方出版信息；不要只看搜索摘要。引用准确 URL。
- 自己能完成的编码/分析要完成，但不臆造不存在的文件/算力/实验日志；被阻塞时清楚说明、给出已完成的可用结果。
- **拒绝“为了发论文”制造效果，拒绝无止境挑刺，拒绝无止境乐观。** 研究目标是找到可复用的科学规律与清楚的算法/能力增量。

---
**一句话交班：** 从旧 EFD 的“别在共享证据前公布本地答案”，升级到“让局部计算尽量有价值且正确，在外部条件未到齐时仍可组合”。把它作为唯一中心；不为近邻论文缩题，不为漂亮机制赌博，不为方法复杂度牺牲研究可证伪性。

[旧稿与审稿](01_EFD_AND_REVIEW.md) · [相关工作](02_RELATED_WORK.md) · [研究蓝图](03_RESEARCH_BLUEPRINT.md)