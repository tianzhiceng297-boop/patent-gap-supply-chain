# Detailed Workflow

## Step 1: Trigger lead identification

**Goal**: Define the analysis target and identify a potential patent gap.

**Operations**:
1. Confirm target A (full company name, ticker)
2. Identify A's product P and the key technology T it uses
   - If T is not specified by the user, extract core technologies from A's product docs, annual report, prospectus, research reports
3. Preliminarily judge whether A may lack T's patents
   - If A's product uses T but A's core business / R&D direction does not overlap with T, gap likelihood is high

**Output**: A, P, T triple + preliminary gap judgment

## Step 2: Patent gap confirmation

**Goal**: Confirm with data whether A truly lacks T-related patents, and rule out legitimate-use scenarios.

**Operations**:
1. Search A's patents using patent search tools (tushare patent API / PatSnap)
   - Dimensions: A's company name + technology T keywords + IPC class
   - Scope: CN patents + US patents + PCT (per A's market coverage)
   - Note: A may hold patents via subsidiaries / affiliates — search A's related entities
2. Determine gap:
   - A (incl. affiliates) has zero T-related patents → gap confirmed
   - A has T-related patents but count is significantly below industry average → partial gap
   - A has sufficient T-related patents → no gap, terminate
3. Rule out legitimate-use scenarios (if gap confirmed):
   - Cross-licensing: does A have a cross-licensing agreement with anyone? (check announcements / news)
   - Acquisition inheritance: did A acquire T usage rights via M&A?
   - Patent pool: does T belong to a pool (e.g., Avanci, MPEG-LA)?
   - Open-source: is there an open-source implementation of T?

**Output**: Gap determination (confirmed / partial / none) + excluded legitimate-use scenarios

## Step 3: Trade secret / know-how assessment (NEW v0.2.0)

**Goal**: Determine whether the patent gap reflects true external dependency or deliberate trade secret / know-how protection. This step is critical because a patent gap does NOT automatically mean A lacks the technology — it may mean A has it but chooses not to disclose.

**Core distinction**:
- **Trade secret**: A has the technology internally, but protects it through secrecy (NDAs, non-competes, confidentiality policies) rather than patents. No external dependency. The "gap" is strategic.
- **True gap → dependency**: A genuinely lacks the technology and must source it externally (licensing, supplier, JV).

**Trade secret assessment — evaluate the following 6 indicators**:

| # | Indicator | Strong trade secret signal | Neutral | Strong dependency signal |
|---|-----------|---------------------------|----------|--------------------------|
| 1 | Industry norms | T is in semiconductor process, chemical formulation, pharma manufacturing, food/beverage formula, or metallurgy — domains known for trade secret dominance | General hardware / manufacturing | T is in consumer electronics, mechanical devices, or commoditized components where reverse-engineering is easy |
| 2 | Company NDA / confidentiality culture | A has a documented history of aggressive NDAs, non-compete lawsuits, or employee confidentiality policies; known for "black box" secrecy (e.g., TSMC, ASML) | Standard corporate confidentiality | A openly publishes technical papers, participates in open standards, has a transparent R&D culture |
| 3 | Patent filing pattern | A files patents aggressively in some domains but conspicuously avoids T — strategic selectivity suggests deliberate trade secret choice | A files patents across all domains evenly | A rarely files patents at all, or files in unrelated domains only |
| 4 | R&D intensity vs. patent output | A has high R&D spend and a large R&D team in T's domain, but produces few or zero patents in T → likely trade secret | Moderate R&D with moderate patent output | Little to no R&D in T's domain → A probably doesn't have the tech |
| 5 | Technology nature | T is a process, method, formula, or manufacturing technique (harder to detect via reverse engineering) | T is a mix of process and product | T is a physical component, device, or material that can be physically identified in teardown |
| 6 | Trade secret litigation history | A or its peers have sued/been sued for trade secret theft in T's domain | No litigation history | A or the industry has a history of patent litigation (suggesting patents are the norm) |

**Trade secret likelihood scoring** (count strong signals):
- **4-6 indicators → "High likelihood trade secret"**: The gap is most likely deliberate protection. DEFAULT starting assumption: A has the tech internally, no external dependency. Multi-lead verification still runs, but only leads 4 (procurement) or 5 (teardown) with high-confidence physical evidence can override this.
- **2-3 indicators → "Possible trade secret"**: Trade secret is a plausible alternative explanation. Record it as an alternative hypothesis. Proceed with normal verification but flag this scenario.
- **0-1 indicators → "Trade secret unlikely"**: The gap is most likely a real dependency. Proceed with normal workflow.

**Critical override rule**: Regardless of trade secret likelihood, IF leads 4 (procurement) or 5 (teardown) later return high-confidence evidence that A physically sources T from an external party B, the trade secret hypothesis is **overridden** — physical evidence of external sourcing trumps trade secret assessment.

**Output**: Trade secret likelihood (high/possible/unlikely) + indicator breakdown + whether procurement/teardown evidence is required to conclude dependency.

## Step 4: Multi-lead parallel verification

**Goal**: Activate 5 leads in parallel, each with an independent confidence score. Detailed scoring rules in `checklists.md`.

### Lead 1 · Patent ownership (confidence: medium)
- Search technology T's patent holders, identify primary owner B
- Check B's patent status: granted / under examination / lapsed / transferred
- Timeline check: B's patent filing date vs A's product launch date (A's product must postdate B's filing)
- Note: B may be an NPE (non-practicing entity), licensing only

### Lead 2 · Corporate affiliation (confidence: high)
- Search A-B affiliation via Tianyancha / Qichacha
- Check: mutual investment, joint ventures, executive overlap, historical changes
- Strong tie (JV / investment) → high confidence; weak tie (executive overlap) → medium

### Lead 3 · Talent flow (confidence: medium)
- Search B→A technical-hire / job-change records
- Use WebSearch + job sites + LinkedIn-type sources
- Multiple B technical staff joining A → high likelihood of technology dependency

### Lead 4 · Procurement (confidence: high)
- Search A's tender announcements, supplier-day disclosures, annual-report supplier info
- Whether B is directly named, or B's product category is mentioned
- Directly names B → high confidence; category only → medium

### Lead 5 · Product teardown (confidence: very high)
- Search teardown reports / BOM analysis of A's product P
- Whether it physically contains B's components / chips
- Physical evidence → very high confidence (strongest evidence)

**Output**: Per-lead hit status + per-lead confidence

## Step 5: Multi-lead fusion + confidence scoring

**Goal**: Cross-validate and produce an overall determination.

**Fusion rules**:
- ≥2 high-confidence leads corroborating → overall "high", can conclude
- 1 high + 1 medium corroborating → overall "medium"
- Only 1 lead hit, no corroboration → overall "low", hypothesis only
- 0 leads hit → terminate, cannot infer

**Trade secret override rules** (v0.2.0):
- If trade secret likelihood is "high" AND neither Lead 4 (procurement) nor Lead 5 (teardown) returns high-confidence external sourcing evidence → overall confidence is capped at "low". Conclusion: "A likely uses internal trade-secret-protected technology; no supply-chain dependency inferred."
- If trade secret likelihood is "high" BUT Lead 4 or 5 confirms external sourcing with high confidence → trade secret hypothesis is overridden. Proceed with normal fusion rules.
- If trade secret likelihood is "possible" → record as an alternative hypothesis; no cap on overall confidence.

**Role determination**:
- Leads 4/5 hit (procurement / teardown) → B is A's direct supplier
- Only leads 1/2 hit (patent / corporate) → B may be technology licensor or indirect affiliation
- Lead 1 hit but B is NPE → B licenses only, not in supply chain

**Output**: Overall confidence + B's role determination

## Step 6: Alternative-hypothesis check

See the alternative-hypothesis checklist in `checklists.md`. Explicitly exclude alternative explanations for each inference; list unexcluded ones in the report.

## Step 7: Counter-intelligence check

See the counter-intelligence checklist in `checklists.md`. Check whether the subject uses counter-intelligence means to hide the real supply chain.

## Step 8: Output report

Organize the report per the I/O contract in SKILL.md. The report must include:
- Relationship determination (incl. B's role)
- Trade secret assessment result (likelihood + indicator breakdown)
- Evidence chain (5-lead hit table + confidence)
- Overall confidence + rationale (including trade secret override if applicable)
- Alternative-hypothesis list (excluded / unexcluded)
- Counter-intelligence risk flag
- Follow-up verification suggestions
