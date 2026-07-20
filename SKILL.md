---
name: patent-gap-supply-chain
description: 'Competitive intelligence workflow that infers supply-chain
  relationships from patent gaps — while distinguishing between true external
  dependencies and deliberate trade secret / know-how protection. 通过"专利缺口"反推产业链关系
  的竞争情报工作流，区分真实外部依赖与主动商业秘密/know-how保护。Use when researching
  tech/hardware/manufacturing/pharma stocks, analyzing industry supply chains,
  identifying suppliers, investigating patent gaps (a company uses a technology
  but holds no related patents), or inferring supply-chain relationships from
  patent ownership. 触发场景：科技股研究、行业分析、产业链推断、供应商识别、专利缺口分析、
  专利归属查询、技术依赖分析 / Triggers:
  patent gap analysis, supply chain inference, supplier identification,
  technology dependency, competitive intelligence, tech stock research, industry
  analysis, trade secret assessment. The skill starts from a patent gap,
  evaluates whether the gap reflects trade secret protection, then activates
  five parallel verification leads (patent ownership, corporate affiliation,
  talent flow, procurement, product teardown), with built-in
  alternative-hypothesis checks and counter-intelligence checks, outputting a
  confidence-scored supply-chain inference report.'
version: 0.2.0
agent_created: true
disable: true
---

# Patent Gap Supply Chain Inference

## Overview

This skill implements a competitive intelligence workflow: "patent gap → supply chain inference." Core insight: when target company A's product uses technology T, but A holds no patents related to T, there may be a technology-licensing or supply-chain dependency. By searching T's patent ownership to identify company B, then activating multiple verification leads, one can infer whether B has entered A's supply chain.

**Critical caveat**: A patent gap can also mean A protects T as a **trade secret / know-how** rather than through patents. Patents require public disclosure; many companies (especially in semiconductor processes, chemical formulations, pharma manufacturing) deliberately choose trade secrets. This skill explicitly evaluates this distinction before concluding supply-chain dependency.

The skill downgrades "patents" from the sole evidence chain to an entry lead, enforcing multi-lead parallel verification, alternative-hypothesis checks, and counter-intelligence checks to prevent overconfident wrong conclusions.

## When to Use

Trigger when any of the following applies:

- Researching tech, hardware, manufacturing, pharma, or materials companies where supply chain or technology sourcing matters
- Conducting industry analysis, supply-chain mapping, or supplier identification
- Discovering or suspecting a company uses a technology but lacks related patents (patent gap)
- Explicitly asked to do patent ownership analysis, technology dependency analysis, or supply-chain inference

Trigger keywords (bilingual): 科技股研究, 行业分析, 产业链, 供应链, 供应商识别, 专利缺口, 专利归属, 技术依赖, patent gap, supply chain, supplier identification, technology dependency, competitive intelligence.

## Step 0: Applicability Pre-check (mandatory)

Before running the workflow, judge whether the target type is applicable:

| Target type | Applicability | Handling |
|-------------|---------------|----------|
| Hardware / manufacturing / pharma / materials | Strong | Proceed |
| Software / internet | Weak | Warn that patent protection is weak and tech is hidden; suggest code / open-source intelligence; may proceed at reduced confidence |
| Business-model innovation | Not applicable | Stop; suggest financial reports / prospectus / business-model analysis instead |

When not applicable, clearly inform the user and suggest alternatives. Do not force execution.

## Workflow Overview

The full workflow has 8 steps. Detailed operations in `references/workflow.md`:

1. **Trigger lead identification**: Confirm target A, product P, technology T; preliminarily judge whether A lacks T's patents
2. **Patent gap confirmation**: Search A's patent portfolio, compare against T, determine gap validity (exclude cross-licensing / acquisition inheritance / patent pools / open-source)
3. **Trade secret assessment** (NEW v0.2.0): Evaluate whether the patent gap stems from deliberate trade secret / know-how protection rather than external dependency. If trade secret is likely, the gap no longer implies supply-chain dependency — but procurement/teardown evidence can override this judgment.
4. **Multi-lead parallel verification**: Activate 5 leads — patent ownership, corporate affiliation, talent flow, procurement, product teardown
5. **Multi-lead fusion + confidence scoring**: ≥2 high-confidence leads corroborating required to conclude; single lead is only a hypothesis. Trade secret scenarios require procurement/teardown leads to override.
6. **Alternative-hypothesis check**: Explicitly exclude alternative explanations (now including trade secret hypotheses); see `references/checklists.md`
7. **Counter-intelligence check**: Check for abnormally dispersed patent ownership / shell-company patterns; see `references/checklists.md`
8. **Output report**: Relationship determination + evidence chain + confidence + alternative hypotheses + trade secret assessment + counter-intel risk + follow-up verification suggestions

## Input/Output Contract

**Input**:
- Target A (company name / ticker)
- Technology T or product P of interest (optional; may be identified during analysis)
- Known background info (optional)

**Output**: Supply-chain inference report, including:
- Relationship determination: whether B entered A's supply chain + B's role (direct supplier / technology licensor / indirect affiliation / NPE license-only)
- Trade secret assessment result: likelihood + indicator breakdown
- Evidence chain: per-lead hit status + per-lead confidence + corroboration
- Overall confidence: high / medium / low + rationale (including trade secret override if applicable)
- Alternative-hypothesis list: excluded + unexcluded
- Counter-intelligence risk flag: whether patent ownership is abnormally dispersed / shell-company patterns exist
- Follow-up verification suggestions: next evidence to gather

## Tool Integration

Available tools per lead (by availability priority):

| Lead | Tools | Purpose |
|------|-------|---------|
| Patent search | tushare (patent API), PatSnap patsnap-search connector | Search A/B patents, T's patent ownership |
| Corporate affiliation | Tianyancha tyc-mcp, Qichacha qcc-company, Qixinbao qixinbao-mcp | A-B investment / JV / executive overlap |
| Listed-co data | tushare, westock-mcp, westock-tool | Financial report supplier disclosure, prospectus |
| Talent flow | WebSearch, job-site search | B→A technical hires |
| Procurement | WebSearch, gov-procurement portals, tender announcements | A's tender / supplier announcements |
| Product teardown | WebSearch, professional teardown reports | A's product BOM / teardown for B components |
| Trade secret assessment | WebSearch, legal databases, company reports | NDAs, non-competes, trade secret litigation, R&D disclosures |

When a tool is unavailable, fall back to WebSearch for public info and reduce that lead's confidence accordingly.

## Key Principles

- **Patents are an entry lead, not final evidence**: A patent gap triggers verification but does not directly conclude
- **Trade secrets must be ruled out before concluding dependency** (v0.2.0): A patent gap may reflect deliberate trade secret / know-how protection, not external dependency. Always assess trade secret likelihood before proceeding to multi-lead verification.
- **Procurement/teardown leads override trade secret hypothesis**: Even if trade secret protection is likely, physical evidence of external sourcing (leads 4 & 5) can conclusively override the trade secret explanation.
- **Multi-lead corroboration**: ≥2 high-confidence leads corroborating required to conclude
- **Explicit confidence scoring**: Every lead and inference gets a confidence score (high / medium / low)
- **Alternative hypotheses are mandatory**: Every inference must explicitly exclude alternative explanations (including trade secret scenarios)
- **Counter-intelligence check is mandatory**: Watch for targets using shell companies / dispersed patents to hide the real supply chain

## Resources

### references/
- `references/workflow.md`: Detailed 8-step operations, including per-step decision rules, tool invocation, trade secret assessment flow, output format
- `references/checklists.md`: Alternative-hypothesis checklist, trade secret indicators checklist, counter-intelligence checklist, confidence scoring rules

### examples/
- `examples/sample-output.md`: A worked example showing the full report format for a fictional case
