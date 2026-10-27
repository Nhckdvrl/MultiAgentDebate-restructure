# 02 · Related Work Map：从分布式证据到可组合局部计算

> **文献快照：2026-10-27。** 本页是研究定位与方法库，不是“论文多所以不能做”的防御性检索。**近邻工作说明问题重要，下一步要识别可继承的技术、尚未解释的计算结构、以及公正的强对照。**
>
> 依据既有多轮检索与本轮对官方出版页/arXiv 的校对。**arXiv 未正式录用的工作标记为 preprint**；若论文版本更新，本地 Agent 应查新版方法细节，不要仅根据摘要下结论。

## 先读：与研究问题最直接相接的 12 篇

| 工作（直达链接） | 已有贡献 | 我们应当继承 / 继续问的问题 |
|---|---|---|
| **[HiddenBench / Systematic Failures in Collective Reasoning](https://proceedings.mlr.press/v306/li26ej.html)** (ICML 2026; earlier [arXiv:2505.11556](https://arxiv.org/abs/2505.11556)) | 官方 65 个 hidden-profile tasks；信息不对称、集体推理失败与结构化沟通 | EFD 旧主战场；新研究不能只复现 hidden-profile“多传私有事实”，要检验局部计算如何传递 |
| **[iAgents / InfoNav](https://papers.nips.cc/paper_files/paper/2024/hash/0534abc9e6db91683d82186ef0d68202-Abstract-Conference.html)** (NeurIPS 2024) | 信息不对称下主动导航、询问与跨代理信息交换；InformativeBench | routing/request 强对照；不能把“让 agent 问缺失事实”单独当创新 |
| **[Neuro-Inspired Inhibitory Message Passing](https://arxiv.org/abs/2610.24718)** (2026-10 preprint) | training-free 按接收者缺失证据选择转发，过滤 sender rationale/answer；节省 token | evidence-only 与去冗余最强近邻之一；研究其对**派生局部计算**（计划/可行域/约简约束）的表达与复用限制 |
| **[MOC: Multi-Order Communication](https://proceedings.mlr.press/v306/guan26i.html)** (ICML 2026) | 多阶证据流、语义—拓扑消息合并、严格 token 约束 | 结构化通信/语义保真强对照；新方法应体现“local work already done”而非仅证据整理 |
| **[CoSIR: Training LLMs for Multi-Agent Collaboration with Verifiable Information Rewards](https://arxiv.org/abs/2610.17893)** (2026-10 preprint) | Bayesian surprisal / verifiable reward，训练分享、请求、使用证据，跨任务迁移 | 信息分享 RL 的重要近邻；跨未来伙伴的**计算结果可用性**与已完成的局部推断是可能的进一步突破点 |
| **[CCPO: Counterfactual Credit Policy Optimization](https://arxiv.org/abs/2603.21563)** (2026-03 preprint) | 通过移除 agent 的反事实轨迹估计个人贡献；训练多代理协作 | 反事实信用分配已有坚实基础；不要仅把 `R(message)-R(empty)` 宣称为新优化原理 |
| **[When Do Multi-Agent Systems Help? An Information Bottleneck Perspective](https://arxiv.org/abs/2607.16133)** (2026-07 preprint) | bounded relay 下减少冗余与损失任务信息的权衡；模型能力影响结果 | 支撑为什么 Raw 对强模型有时更好；需要报告 receiver能力 × 预算 × 保真条件 |
| **[Bayes-Sufficient Compression Is Not Enough](https://arxiv.org/abs/2610.03769)** (2026-09 preprint) | 区分统计充分与有限 decoder 实际利用；消息“到达”后仍可能未被使用 | 支撑 sender representation loss / receiver utilization loss 二分；不能用信息论充分性直接证明 LLM 必能组合 |
| **[Evidence Integration in Large Language Models](https://arxiv.org/abs/2609.04290)** (2026-09 preprint) | 外部候选对接收者答案分布的影响、候选与原先倾向的匹配、验证后仍采用错误候选 | 错误局部标签的强竞争解释；在固定证据下控制 receiver prior、candidate plausibility |
| **[When Upstream Messages Override Correct Answers](https://arxiv.org/abs/2609.36855)** (2026-09 preprint) | 上游消息 Rescue/Corruption 与答案替代的固定下游控制 | 非腐化性评测的重要直接近邻；新研究要**生成**更可组合的局部计算，而不只是门控错误消息 |
| **[BRIDGE: Selective Boundary Repair with Dependency Graph Evidence](https://arxiv.org/abs/2610.17805)** (2026-10 preprint) | 依赖图、来源、handoff 错误定位、候选修复与成本校准 | 修复/图可追溯/反事实 rollout 可借用；别重复造大型 graph controller 当主要创新 |
| **[Composition Collapse](https://aclanthology.org/2026.findings-emnlp.1411/)** (EMNLP 2026 Findings) | 双门控：先确保原子事实和独立子问题正确，再测剩余组合失败 | H₀/H_A 或跨信息使用机制必须防止把“本地没理解”误当“组合失败” |

## A. 领域基础、MAS 与通信协议

1. **[Stasser & Titus (1985), Pooling of Unshared Information](https://doi.org/10.1037/0022-3514.48.6.1467)**。隐藏档案和不共享信息偏差的社会心理学经典；旧 EFD 的社会动机。**边界**：人类讨论偏差不等于自动解释 LLM 固定证据＋标签的接收端错误。
2. **[Du et al., Improving Factuality through Multiagent Debate](https://proceedings.mlr.press/v235/du24e.html)** (ICML 2024)。多轮回答/质疑/修订；旧稿审稿人要求的强 opinion-exchange 家族。
3. **[Debate or Vote](https://arxiv.org/abs/2502.19130)** (ACL Findings 2025)。debate vs independent vote；EFD 旧稿复现其中一种协议并做审计。
4. **[Revisiting Multi-Agent Debate as Test-Time Scaling](https://arxiv.org/abs/2505.22960)** (2025)。强调 debate 的收益依任务难度、模型能力、任务领域变化；提醒别强行声称一个通信格式普胜。
5. **[InfoDelphi: Diverse Evidence, Better Forecasts](https://arxiv.org/abs/2607.01661)** (2026-07 preprint)。设计信息不对称、relevance-aware routing、rationale deliberation、加权聚合；既是强系统候选，也说明 evidence diversity 与 downstream decision 非等价。
6. **[AgentPrune / Cut the Crap](https://proceedings.iclr.cc/paper_files/paper/2025/hash/bbc461518c59a2a8d64e70e2c38c4a0e-Abstract-Conference.html)** (ICLR 2025)。删除空间—时间通信图里的冗余消息；与“消息内容本身的可组合性”区别明确，但应公平比较压缩和成本。
7. **[AGP: Adaptive Graph Pruning](https://arxiv.org/abs/2506.02951)** (ECAI 2025)。agent 数量与边拓扑联合剪枝；不是同一核心问题，可以作为系统级上下游组件。
8. **[SILO-BENCH](https://aclanthology.org/2026.acl-long.1354/)** (ACL 2026)。role-free 分布式计算题与通信复杂度层级，能精确评分；适合检查“真正跨信息的计算”，但不是自然语言 QA 迁移的替代品。

**这一组说明：** 共享/过滤/路由证据很重要；我们的新主张必须转向“哪个局部计算可以安全完成、怎样保留其在未知外部条件下的响应”，并公正对照现有强通信系统。

## B. 压缩、分布式计算与完整成本

1. **[Factor Graphs and the Sum-Product Algorithm](https://doi.org/10.1109/18.910572)** (Kschischang, Frey, Loeliger, 2001)。全球函数通过局部因子和消息传递计算；明确**内部变量 vs 跨组件边界变量**的区别。它是结构工具，而非新论文需要重新证明的定理。
2. **[Functional Compression Through Graph Coloring](https://doi.org/10.1109/TIT.2010.2050835)** (2010)。分布式函数压缩：不同源状态何时必须给不同编码取决于侧信息下的目标；支持“未知伙伴可能区分的关系必须保留”。
3. **[Fundamental Limits of Prompt Compression: A Rate-Distortion Framework](https://proceedings.nips.cc/paper_files/paper/2024/hash/ac8fbba029dadca99d6b8c3f913d3ed6-Abstract-Conference.html)** (NeurIPS 2024)。面向固定黑箱接收者的 query-aware 压缩；我们的补充难题是具体相关键/条件由其他节点握有。
4. **[Bayes-Sufficient Compression Is Not Enough](https://arxiv.org/abs/2610.03769)**。强调统计信息足够 ≠ 接收端能用、协议开销 ≠ 仅信息量；要设计 receiver-aware 实验与“最终采用”控制。
5. **[When Do Multi-Agent Systems Help?](https://arxiv.org/abs/2607.16133)**。对 EFD 的 Gemma 超过 Raw 和部分 Qwen 落后 Raw 提供解释框架；报告模型尺寸、context、预算和实际调用数量。
6. **[Copies or Sources? Measuring How LLM Aggregators Count Restated Evidence](https://arxiv.org/abs/2610.06192)** (2026-10 preprint)。信息来源数 vs 重复表述数；公开标签/重复证据的解释竞争者，注意来源独立性。

**本路线不主推重型 DSL + CEGIS。** 如果可执行约束任务便于构造金标准，可将其作为评测/教师，但别把“把全文转成严格程序”变成整篇论文先决条件。重要的是 **completion of useful local computation**，可以通过自然语言中间结果来实现。

## C. Receiver 如何误用局部结果：机制与验证

1. **[Evidence Integration in LLMs](https://arxiv.org/abs/2609.04290)**：receiver-specific prior 与候选倾向；测试 own vs foreign decoy、局部合法但全局错误、位置/长度控制。
2. **[When Upstream Messages Override Correct Answers](https://arxiv.org/abs/2609.36855)**：对 Rescue 和 Corruption 细分；强调“不帮忙”与“帮倒忙”在评测意义上不同。
3. **[When Structure Is Hidden](https://aclanthology.org/2026.emnlp-main.1792/)** (EMNLP 2026)：局部组件理解正确时仍可能组合失败，结构 hint 的效果随任务变化；支持双门控，反对“多写几个结构字段”当然有效的假定。
4. **[Composition Collapse](https://aclanthology.org/2026.findings-emnlp.1411/)**：原子事实门控＋独立子问题门控后才评价 residual composition；避免不可检验的解释。
5. **[CIGAsk: When and How Should an Agent Clarify?](https://aclanthology.org/2026.findings-emnlp.314/)** (EMNLP 2026 Findings)：何时请求/如何请求信息的 RL，构成主动澄清强近邻；我们应把“提出条件查询”定位为支持局部计算的动作，而非单独卖点。
6. **[BRIDGE](https://arxiv.org/abs/2610.17805)**：handoff trace 依赖破坏的运行时修复，为中间结构 H 的可观测性提供设计参考。

**关于 Hotpot vs MuSiQue −7.1 / −0.5 DiD：** “具体诱饵＋rationale 被放大”与“Unanswerable 传染”是值得做的机制假说，不能因其漂亮就视为因果事实。哪怕该机制不显著，composable local reasoning 的主线仍成立。

## D. 训练与反事实伙伴构造

1. **[CoSIR](https://arxiv.org/abs/2610.17893)**：surprisal 信息奖励，训练共享/请求/使用；要避免将简单的 `information gain RL` 当作主张。
2. **[CCPO](https://arxiv.org/abs/2603.21563)**：反事实去除 agent 的信用分配；借用方法，学习目标应聚焦**同一条局部消息在不同伙伴世界仍然可用**。
3. **[CRiT-QA](https://aclanthology.org/2026.lrec-1.410/)** (LREC 2026)：用反事实链和 distractor traps 排除参数记忆与捷径；对于 `c_swap` 的一致性校验很有参考价值。
4. **[CofCA: A Step-wise Counterfactual Multi-hop QA Benchmark](https://proceedings.iclr.cc/paper_files/paper/2025/hash/2628d4d3b054c2d7ad33ab03435204f4-Abstract-Conference.html)** (ICLR 2025)：反事实多跳＋中间子题核验；可用于设计受控的伙伴世界与 gate。
5. **[CIGAsk](https://aclanthology.org/2026.findings-emnlp.314/)**：asymmetric bonus / counterfactual information gain；可借鉴“什么时候问”的行为建模。
6. **[When Upstream Messages Override Correct Answers](https://arxiv.org/abs/2609.36855)**：风险非对称的客观评测参考，别把 Rescue−Corruption 简单重命名后声称新机制。

训练方面不需要预先冻结 DPO/GRPO/SFT。先将同一 `(Q,O_i)` 的消息固定，再在**合法、真值可核查且不同的伙伴条件**下测试。标准 QA 若不天然具备这些条件，使用可信的反事实链、授权证据可回答性和局部约束任务构造；不能一行替换产生自相矛盾世界。

## E. Benchmark / 系统：谁能支撑主张

| 数据/系统 | 正确用途 | 需警惕 |
|---|---|---|
| **[HotpotQA](https://aclanthology.org/D18-1259/)** (EMNLP 2018) | 原 EFD 实证；支持关系桥接；以 2-hop 为主 | 单 agent 取一句事实上可能只是 extraction，**不必然是局部实质计算** |
| **[MuSiQue](https://aclanthology.org/2022.tacl-1.31/)** (TACL 2022) | 较长依赖链；合理构造局部内部 ≥2-hop 后的跨节点接口 | 分配规则不应只人为制造失败；保留纯原始分片 |
| **[HiddenBench](https://proceedings.mlr.press/v306/li26ej.html)** | 原作 65 题；局部线索和群体条件 | 某个“排除嫌疑人”必须基于已完备硬证据，不能把没见到当不可能 |
| **[InformativeBench / iAgents](https://arxiv.org/abs/2406.14928)** | Schedule 合取约束、局部计划有效的边界 | Schedule Hard 的原始各方法几乎 0，solver ceiling/floor 问题 |
| **Shortlist（EFD 自制）** | 检验完整本地可行集合、合法淘汰和跨节点交集；可严格生成多伙伴世界 | Oracle exact shortlist 不能当成生成模型结果；需另找自然迁移 |
| **[SILO-BENCH](https://aclanthology.org/2026.acl-long.1354/)** | 多节点分布式计算与精确评分；通信—推理 gap | 算法任务不能代替开放自然语言证据 |
| **[SemBench](https://arxiv.org/abs/2511.01716)** | 外部定义的语义查询/多模态抽取、连接与优化 | 与 NLP Agent 论文定位不同；不是第一优先，但能作为严格强比较 |
| **[LOTUS](https://github.com/lotus-data/lotus)** | 已有 sem_filter / sem_join / 提前 projection 等强系统 | 不能把“抽字段再 join”单独写成新方法；如果使用语义抽取，保持相同抽取器与权限 |
| **[CRiT-QA](https://aclanthology.org/2026.lrec-1.410/), [CofCA](https://proceedings.iclr.cc/paper_files/paper/2025/hash/2628d4d3b054c2d7ad33ab03435204f4-Abstract-Conference.html)** | 多跳反事实链 / 世界一致性检查 | 不把不一致替换或 evidence-drop 自动标成新的事实真值 |

## F. 关键概念与最近研究主张的边界

**我们主要关心的不同于**：只发 facts / 删除 answer、压缩消息、选择接收者、奖励信息增益、只做 refutation/validation。那些可以是我们能力的组件，但**新贡献必须是：本地已完成的可靠计算如何保留开放的外部依赖，并对未见伙伴条件继续起作用**。

理想问题公式（完整定义见 [03](03_RESEARCH_BLUEPRINT.md)）：

`m_i=T(Q,O_i)`，下游 `S(Q,m_i,c)` 对多个合法 `c` 保持正确响应，即使真值随 `c` 变化；同时测局部计算增益、非腐化和成本。

**不要误读成以下过强主张：**

- “已有近邻论文研究信息增益，所以我们不可能创新”——错误；近邻说明重要，应主动借用最强方法。
- “同一条消息配两个不同答案的伙伴 world 都答对 ⇒ 消息必然包含局部计算”——错误；伙伴单独已能做对，空消息也可能满分。必须加无消息/简单提取的增量对照。
- “合取 vs 存在完全覆盖自然语言推理”——它们是**教学性极性基例**；复杂场景可能混合量词、时序、否定和不完整信息。
- “基于非对称 Rescue−γ·Corruption 的 GRPO 一定生效”——固定伙伴、组内标准化时正仿射项会抵消；必须先跨伙伴聚合后，再核查优势/偏好构造。
- “外部同一 QA `Q,O_i` 可以无成本取得多个独立真实世界”——需要一致且许可的反事实或结构化任务。
- “闭合是一种永久禁止输出 final answer”——错误；**已确定的局部子目标应该大胆完成，全局承诺必须与证据作用域匹配**。

## G. 推荐阅读顺序与后续维护规则

**优先读全文而不仅摘要：**
1. HiddenBench、原 EFD、Inhibitory Message Passing、MOC（认识旧赛道与 evidence-only 强对照）
2. CoSIR、CCPO、Bayes-Sufficient Compression、IB Perspective（定位训练、receiver 和成本）
3. Evidence Integration、When Upstream Messages Override、Composition Collapse、When Structure Is Hidden（机制控制）
4. CRiT-QA、CofCA、iAgents/InfoNav、SILO-BENCH（设计伙伴反事实与外部验证）
5. 理论工具/编程系统按实验发现的结构再深挖，不为了“避免有人做过”而绕开真正的问题。

每项新增文献只需更新四个字段：**已做什么 / 关键证据 / 我们继承什么 / 由此新增哪个实质问题或强对照**。禁止按时间顺序囤积论文并自动得出“文献距离不够”。维护可查的原文链接；若方法细节未读，只写“abstract-level”，不伪装为全文核查。

---
[返回 README](../README.md) · [研究主线](03_RESEARCH_BLUEPRINT.md) · [Handoff](04_AGENT_HANDOFF.md)