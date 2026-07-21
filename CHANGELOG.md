# Changelog

All notable changes to this skill are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/), adheres to [Semantic Versioning](https://semver.org/).

## [0.3.0] - 2026-07-21

### Added
- **Competitive triangulation** (Step 5): Cross-verify A's patent gap against 3-10 industry peers. 3+ peers same gap → same B = +2 confidence boost. Triangulation checklist in `references/checklists.md`.
- **Financial cross-validation** (Step 7): Mandatory financial corroboration. Checks: top-supplier disclosure, customer concentration, AR/AP patterns, related-party transactions. No financial corroboration → confidence capped at "medium". Checklist in `references/checklists.md`.
- **Reverse workflow** (supplier → clients): Start from technology owner B, identify downstream customers. Full appendix in `references/workflow.md` (R1-R5 process).
- **Industry-specific guide** (`references/industry-guide.md`): 5 verticals — semiconductor, pharma, new energy/battery, consumer electronics, materials/chemicals. Each with key signals, verification strategy, and A-share examples.
- **Real case study** (`examples/real-case.md`): Goertek (歌尔股份) & Apple VR supply chain. Demonstrates triangulation across 4 peers, financial cross-validation of 46.7% customer concentration, and ODM business model detection.

### Changed
- Workflow: 8 → 10 steps. Step 5: triangulation, Step 7: financial cross-validation. Subsequent steps renumbered.
- Direction selection added to Step 1 (forward/reverse).
- Step 2 split into 2A (gap) and 2B (patent dominance for reverse).
- Confidence scoring: 3-layer adjustment (trade secret → triangulation boost → financial cap).
- I/O contract, Tool Integration, Key Principles, frontmatter, trigger keywords all expanded.
- README.md rewritten for v0.3.0.
- `examples/sample-output.md` updated with triangulation and financial sections.

## [0.2.0] - 2026-07-20

### Added
- **Trade secret / know-how assessment** (Step 3): New mandatory step after patent gap confirmation that evaluates whether the gap reflects deliberate trade secret protection rather than external dependency
- 6-indicator trade secret assessment framework in `references/workflow.md`: industry norms, company NDA culture, patent filing pattern, R&D-to-patent ratio, technology nature, trade secret litigation history
- Trade secret indicators checklist in `references/checklists.md` with scoring (4-6 = high likelihood, 2-3 = possible, 0-1 = unlikely)
- Trade secret override rules: procurement (Lead 4) or teardown (Lead 5) findings with high-confidence physical evidence can override a trade secret finding
- Trade secret adjustments to overall confidence scoring in `references/checklists.md` (confidence capped at "low" when trade secret likely and no physical sourcing evidence)

### Changed
- Workflow expanded from 7 to 8 steps to accommodate trade secret assessment
- All subsequent step numbers updated (old Step 3→4, Step 4→5, Step 5→6, Step 6→7, Step 7→8)
- Multi-lead fusion rules now incorporate trade secret override logic
- Alternative-hypothesis checklist now explicitly includes trade secret / know-how protection
- Key Principles updated with trade-secret-first rule and procurement/teardown override logic
- Frontmatter description updated to mention trade secret distinction
- README.md Chinese and English sections updated to document new trade secret capability
- Sample output (`examples/sample-output.md`) updated with Step 3 trade secret assessment section

### Design rationale
- Patents require public disclosure; trade secrets do not. Many deep-tech companies (semiconductor processes, chemical formulations, pharma manufacturing) deliberately avoid patents to protect know-how. A patent gap therefore has two possible explanations: (a) true external dependency, or (b) deliberate trade secret strategy. Without explicitly evaluating this distinction, the skill risked false-positive supply-chain inferences.

## [0.1.0] - 2026-07-09

### Added
- Initial release of patent-gap-supply-chain skill
- 7-step workflow: trigger identification → patent gap confirmation → 5-lead parallel verification → fusion + confidence scoring → alternative-hypothesis check → counter-intelligence check → report output
- Bilingual trigger keywords (Chinese + English) in description for cross-locale agent triggering
- Applicability pre-check (Step 0): hardware / manufacturing / pharma / materials = strong; software / internet = weak; business-model innovation = not applicable
- `references/workflow.md`: detailed per-step operations with decision rules and tool invocation
- `references/checklists.md`: alternative-hypothesis checklist, counter-intelligence checklist, confidence scoring rules
- `examples/sample-output.md`: worked example showing the full report format for a fictional case
- Tool integration table covering tushare, PatSnap, Tianyancha, Qichacha, westock, WebSearch
