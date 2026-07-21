---
name: patent-gap-supply-chain
description: 'Competitive intelligence workflow that infers supply-chain
  relationships from patent gaps — with trade secret assessment, competitive
  triangulation, reverse workflow (supplier→client), and financial cross-validation.
  通过"专利缺口"反推产业链关系的竞争情报工作流，集商业秘密评估、竞对三角验证、反向工作流
  （供应商→客户）、财务数据交叉验证于一体。Use when researching
  tech/hardware/manufacturing/pharma stocks, analyzing industry supply chains,
  identifying suppliers or customers, investigating patent gaps, or inferring
  supply-chain relationships from patent ownership. Supports both forward
  (patent gap → supplier) and reverse (technology owner → downstream clients)
  analysis directions. 触发场景：科技股研究、行业分析、产业链推断、供应商识别、客户挖掘、
  专利缺口分析、专利归属查询、技术依赖分析、上下游推断 / Triggers:
  patent gap analysis, supply chain inference, supplier identification,
  customer identification, technology dependency, competitive intelligence,
  tech stock research, industry analysis, trade secret assessment, financial
  cross-validation, reverse supply-chain analysis. The skill starts from a
  patent gap (or known technology owner), evaluates trade secret likelihood,
  activates five verification leads with competitive triangulation, then
  cross-validates against financial disclosures, outputting a
  confidence-scored supply-chain inference report.'
version: 0.3.0
agent_created: true
disable: true
---

# Patent Gap Supply Chain Inference

## Overview

This skill implements a competitive intelligence workflow: "patent gap → supply chain inference." Core insight: when target company A's product uses technology T, but A holds no patents related to T, there may be a technology-licensing or supply-chain dependency. By searching T's patent ownership to identify company B, then activating multiple verification leads, one can infer whether B has entered A's supply chain.

**Critical caveat**: A patent gap can also mean A protects T as a **trade secret / know-how** rather than through patents. Patents require public disclosure; many companies (especially in semiconductor processes, chemical formulations, pharma manufacturing) deliberately choose trade secrets. This skill explicitly evaluates this distinction before concluding supply-chain dependency.

**v0.3.0 new capabilities**:
- **Competitive triangulation**: If A's peers C₁, C₂...Cₙ also lack T's patents and all point to B, inference confidence gets a significant boost. Cross-company corroboration is one of the strongest signals.
- **Reverse workflow** (supplier → clients): Start from technology owner B (who holds monopoly patents in T) and identify downstream companies A₁, A₂...Aₙ that likely depend on B's technology. This is the more common investment research use case.
- **Financial cross-validation**: After inferring a supply-chain relationship, cross-validate against A's top-supplier disclosures, B's customer concentration data, and accounts receivable patterns. Inference without financial corroboration is capped at "medium" confidence.
- **Industry-specific guidance**: See `references/industry-guide.md` for semiconductor, pharma, new energy, consumer electronics, and materials verticals — each has different patent/trade-secret dynamics.

The skill downgrades "patents" from the sole evidence chain to an entry lead, enforcing multi-lead parallel verification, competitive triangulation, financial cross-validation, alternative-hypothesis checks, and counter-intelligence checks to prevent overconfident wrong conclusions.

## When to Use

Trigger when any of the following applies:

**Forward direction** (patent gap → find supplier):
- Researching tech, hardware, manufacturing, pharma, or materials companies where supply chain matters
- Discovering or suspecting a company uses a technology but lacks related patents
- Explicitly asked to do patent ownership analysis, technology dependency analysis, or supply-chain inference

**Reverse direction** (technology owner → find clients) (NEW v0.3.0):
- A company B holds dominant/monopoly patents in a technology T and you want to identify B's downstream customers
- Investment thesis: "B is the sole/dominant supplier of T; companies using T must be B's customers"
- Asked to identify companies that depend on a specific technology or component

Trigger keywords (bilingual): 科技股研究, 行业分析, 产业链, 供应链, 供应商识别, 客户挖掘, 上下游推断, 专利缺口, 专利归属, 技术依赖, patent gap, supply chain, supplier identification, customer identification, technology dependency, competitive intelligence, reverse supply chain.

## Step 0: Applicability Pre-check (mandatory)

Before running the workflow, judge whether the target type is applicable. See `references/industry-guide.md` for industry-specific dynamics:

| Target type | Applicability | Handling |
|-------------|---------------|----------|
| Hardware / manufacturing / pharma / materials | Strong | Proceed. Reference `industry-guide.md` for vertical-specific signals |
| Software / internet | Weak | Warn that patent protection is weak and tech is hidden; suggest code / open-source intelligence; may proceed at reduced confidence |
| Business-model innovation | Not applicable | Stop; suggest financial reports / prospectus / business-model analysis instead |

When not applicable, clearly inform the user and suggest alternatives. Do not force execution.

## Direction Selection (NEW v0.3.0)

Before proceeding, determine the analysis direction:

| Direction | Starting point | Goal | Trigger phrase examples |
|-----------|---------------|------|------------------------|
| **Forward** | Company A uses T but lacks T's patents | Find who supplies T to A | "Identify XX's suppliers", "Who provides XX's technology?" |
| **Reverse** | Company B holds dominant T patents | Find who depends on B's T | "Who are B's customers?", "What downstream companies need this patent?" |

Both directions share the same verification leads, but the entry point and initial search differ. See `references/workflow.md` for detailed reverse-workflow operations.

## Workflow Overview

The full workflow has 10 steps. Detailed operations in `references/workflow.md`:

1. **Trigger lead identification**: Confirm target company, product, technology; determine forward or reverse direction
2. **Patent gap confirmation** (forward) / **Patent dominance confirmation** (reverse): Search patent portfolio, determine gap validity or market dominance
3. **Trade secret assessment**: Evaluate whether gap reflects deliberate trade secret / know-how protection
4. **Multi-lead parallel verification**: Activate 5 leads — patent ownership, corporate affiliation, talent flow, procurement, product teardown
5. **Competitive triangulation** (NEW v0.3.0): Check A's peers/competitors for the same patent gap. If ≥2 peers also lack T's patents and all point to B, boost confidence.
6. **Multi-lead fusion + confidence scoring**: ≥2 high-confidence leads corroborating required; triangulation provides confidence multiplier
7. **Financial cross-validation** (NEW v0.3.0): Cross-validate inferred relationship against A's supplier disclosures / B's customer concentration / accounts receivable. Inference without financial corroboration is capped.
8. **Alternative-hypothesis check**: Explicitly exclude alternative explanations; see `references/checklists.md`
9. **Counter-intelligence check**: Check for shell companies / dispersed patents; see `references/checklists.md`
10. **Output report**: Relationship determination + evidence chain + triangulation result + financial corroboration + confidence + alternative hypotheses + trade secret assessment + counter-intel risk + follow-up verification

## Input/Output Contract

**Input**:
- Target company (A for forward, B for reverse) — name / ticker
- Technology T or product P of interest (optional; may be identified during analysis)
- Analysis direction: forward or reverse (default: forward if user mentions "supplier", reverse if user mentions "customer" or "downstream")
- Known background info (optional)

**Output**: Supply-chain inference report, including:
- **Direction**: forward or reverse analysis
- **Relationship determination**: whether a supply-chain relationship exists + role (direct supplier / technology licensor / customer / indirect affiliation / NPE license-only)
- **Trade secret assessment**: likelihood + indicator breakdown
- **Evidence chain**: per-lead hit status + per-lead confidence + corroboration
- **Triangulation result** (v0.3.0): which peers confirmed the same gap + triangulation boost applied
- **Financial corroboration** (v0.3.0): supplier disclosure hits, customer concentration data, AR patterns, corroboration status
- **Overall confidence**: high / medium / low + rationale (including triangulation boost and financial cap if applicable)
- **Alternative-hypothesis list**: excluded + unexcluded
- **Counter-intelligence risk flag**: shell companies / dispersed patents
- **Follow-up verification suggestions**: next evidence to gather

## Tool Integration

Available tools per lead (by availability priority):

| Lead | Tools | Purpose |
|------|-------|---------|
| Patent search | tushare (patent API), PatSnap patsnap-search connector | Search A/B/peers patents, T's patent ownership |
| Corporate affiliation | Tianyancha tyc-mcp, Qichacha qcc-company, Qixinbao qixinbao-mcp | A-B investment / JV / executive overlap |
| Listed-co data | tushare, westock-mcp, westock-tool | Financial report supplier disclosure, prospectus |
| Talent flow | WebSearch, job-site search | B→A technical hires |
| Procurement | WebSearch, gov-procurement portals, tender announcements | A's tender / supplier announcements |
| Product teardown | WebSearch, professional teardown reports | A's product BOM / teardown for B components |
| Trade secret assessment | WebSearch, legal databases, company reports | NDAs, non-competes, trade secret litigation, R&D disclosures |
| Competitive triangulation (v0.3.0) | tushare, WebSearch, industry reports | A's peer patent portfolios, cross-company gap confirmation |
| Financial cross-validation (v0.3.0) | westock-mcp, tushare, annual report filings | Supplier concentration, customer concentration, AR aging, related-party transactions |
| Industry guidance (v0.3.0) | `references/industry-guide.md` | Vertical-specific patent/trade-secret dynamics and verification strategies |

When a tool is unavailable, fall back to WebSearch for public info and reduce that lead's confidence accordingly.

## Key Principles

- **Patents are an entry lead, not final evidence**: A patent gap triggers verification but does not directly conclude
- **Trade secrets must be ruled out before concluding dependency**: A patent gap may reflect deliberate trade secret / know-how protection
- **Procurement/teardown leads override trade secret hypothesis**: Physical evidence trumps trade secret likelihood
- **Competitive triangulation strengthens inference** (v0.3.0): Cross-company corroboration is one of the strongest signals — a gap shared by multiple peers pointing to the same supplier is much harder to dismiss
- **Financial cross-validation is required for high-confidence conclusions** (v0.3.0): No supply-chain inference reaches "high" without at least one form of financial corroboration (supplier disclosure, customer concentration, or AR evidence)
- **Multi-lead corroboration**: ≥2 high-confidence leads corroborating required to conclude
- **Explicit confidence scoring**: Every lead and inference gets a confidence score (high / medium / low)
- **Alternative hypotheses are mandatory**: Every inference must explicitly exclude alternative explanations
- **Counter-intelligence check is mandatory**: Watch for shell companies / dispersed patents hiding the real supply chain

## Resources

### references/
- `references/workflow.md`: Detailed 10-step operations + reverse workflow section
- `references/checklists.md`: All checklists — alternative hypotheses, trade secret indicators, triangulation, financial cross-validation, counter-intelligence, confidence scoring rules
- `references/industry-guide.md` (NEW v0.3.0): Vertical-specific guidance for semiconductor, pharma, new energy, consumer electronics, and materials

### examples/
- `examples/sample-output.md`: Fictional case showing the full 10-step report format
- `examples/real-case.md` (NEW v0.3.0): A real Chinese A-share case study with actual company names and data
