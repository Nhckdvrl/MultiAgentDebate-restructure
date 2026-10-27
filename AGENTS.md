# AGENTS.md — Required instructions for every local research agent

**This repository's goal is research, not a preselected experiment or a predetermined positive result.**

## Mandatory per-session reading (do not skip)

Read the following files **in full**, in order, **at the beginning of each new agent session/round** and again after substantive changes:

1. [README.md](README.md)
2. [docs/01_EFD_AND_REVIEW.md](docs/01_EFD_AND_REVIEW.md)
3. [docs/02_RELATED_WORK.md](docs/02_RELATED_WORK.md)
4. [docs/03_RESEARCH_BLUEPRINT.md](docs/03_RESEARCH_BLUEPRINT.md)
5. **[docs/04_AGENT_HANDOFF.md](docs/04_AGENT_HANDOFF.md)** — operational contract; especially §1 highest-priority research disciplines, §3 mathematical cautions, §4 experimental probes, §5 preregistration.

If an active experiment registry / Decision Log exists, read the most recent decisions before writing code. Do not assume stored EFD logs or GPU hardware until you have inspected them.

## Non-negotiables

- **Keep the global question:** *How can local reasoning be completed usefully while preserving its composability with unseen complementary information?*
- **No defensive paralysis:** A paper on the same general problem is an invitation to improve mechanism/algorithm/evidence, not an automatic veto.
- **No phenomenon gambling:** Do not stake the paper on HotpotQA's −7.1 interaction, specific anchoring story, SER prompt gains, or a particular GRPO reward.
- **No novelty theater:** Avoid overclaiming novelty or overqualifying every statement with “not new”. Specify the concrete additional capability and proper strong controls.
- **Do not conflate:** raw-evidence retention, local computation, receiver adoption, joint sufficiency, partner-sensitive response, and non-corruption.
- **Fair baselines:** matched budget/permissions/decoder/splits; include simple local feasible-set, raw/central, and strong evidence-only filtering/routing or training rivals as applicable.
- **No leaked labels:** sender messages must not observe held-out partner worlds or global gold labels.
- **Distinguish data states:** already reported EFD/rebuttal vs new measured results vs toy mathematical checks vs hypotheses.
- **No silent scope creep:** do not introduce a heavy DSL/CEGIS/full self-play system without a documented problem and lighter baseline indicating it is needed.
- **Scientific honesty:** report failures, seeds, CI, resource usage; run commands and code artifacts should be reproducible.

## What to do when unsure

Do not default to endless criticism or a request for guidance on routine implementation. Instead, explain the competing interpretations, identify the smallest decisive probe, and propose its evaluation and fallback branches. Capture this in a dated Decision Log.

**Source-of-truth priority:** 03 (research problem and evaluation) > 04 (operational policy) > 02 (bibliography) > 01 (historical evidence). If you find a contradiction, document it and correct it transparently.

This short file is only an entry point. **The full standing instructions are in 04; reading only AGENTS.md is not compliant.**
