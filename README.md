# MultiAgentDebate — Research Reconstruction

> **Snapshot: 2026-10-27 | 阶段：研究问题基本收敛；最终方法、训练目标、实验结论均未冻结。**
>
> **Core question:** How can agents perform useful local computation under distributed information while preserving the ability of that computation to compose with unknown complementary evidence?

这是从 **Evidence before Opinions / Evidence-First Deliberation (EFD)** 出发的下一代研究工作区。它保存既有证据、审稿教训、关键相关工作和当前共识，供研究者和本地 Coding Agent 持续使用。**它不是历史聊天全文、也不是已验证的新论文。**

## 四份核心文档

| 文件 | 要回答的问题 |
|---|---|
| **[01 · EFD 与审稿复盘](docs/01_EFD_AND_REVIEW.md)** | 旧研究实际上证明了什么？AAAI-27 review 的结构性质疑是什么？哪些由 rebuttal 缓解，哪些仍需要新研究？ |
| **[02 · 相关工作地图](docs/02_RELATED_WORK.md)** | 隐藏档案、分布式通信、证据整合、结构化中间结果、训练、benchmark 等哪些工作真正相关？应该继承什么，怎样做强比较？ |
| **[03 · 当前研究蓝图](docs/03_RESEARCH_BLUEPRINT.md)** | 最终收敛的科学问题、结构框架、数学对象、方法候选、决定性评价标准和不确定性是什么？ |
| **[04 · 本地 Agent Handoff](docs/04_AGENT_HANDOFF.md)** | 每轮开始读什么？怎样思考、设计探索实验、汇报证据、避免盲目防御和现象赌博？ |

根目录的 **[AGENTS.md](AGENTS.md)** 是自动化 Agent 的强制入口，指向 [04 Handoff](docs/04_AGENT_HANDOFF.md)。读完四份核心文档后再改设计或启动实验。

## 核心主线（短版）

- **Local closure ≠ global commitment.** 局部子问题可以充分推演并合法结束；其结论不一定可以无条件升级成全局答案。
- **Communicate reusable local computation.** 比起“只发原始事实”或“给局部最佳答案”，研究如何传递已完成的局部计算与仍开放的跨来源依赖。
- **Scope and completeness matter.** 在本地约束完整时可以排除违规候选；在开放世界的文档切片上，局部缺证据不等于全局不存在。
- **Evaluation is multi-dimensional.** 必须同时测局部计算价值、跨伙伴条件下的正确响应、非腐化性及完整成本，而不是单一 EM 或单一压缩率。
- **Hypotheses are not results.** 有关“弃权传染”“诱饵锚定”“rationale 交互”、SER/Prompt/GRPO 改善等都需要验证；整篇研究不应押在某一个事先无法保证的效应上。

## 附件与溯源

- [archive/AAAI27_reviews_and_rebuttal.txt](archive/AAAI27_reviews_and_rebuttal.txt) — 用户提供的审稿与答辩全文，供查数、核对措辞；**正文文件应以提炼过的分析为准**。
- [archive/EFD_original_paper_extracted.txt](archive/EFD_original_paper_extracted.txt) — 用户提供的匿名旧稿 PDF 的提取文本版本（不含图像；原 PDF 为权威原件）。
- 重要外部论文链接统一编入 [02](docs/02_RELATED_WORK.md)，避免来源碎片化。

> **注意：本仓库为 public。** 已按资料提供者授权收录原始材料；归档文件含审稿期内容，之后如有投稿匿名性/共享规则变动，需要由仓库维护者重新评估公开范围。不得把本仓库的讨论结论表述成已经完成的实验结果。

**维护原则：** 以 03 为研究的单一事实来源，04 为 Agent 行动规则；每次新证据更新状态和判断，不把每轮讨论的反复措辞层层叠加。