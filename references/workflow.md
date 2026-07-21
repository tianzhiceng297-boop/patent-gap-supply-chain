# Detailed Workflow

## Step 1: Trigger lead identification + direction selection

**Goal**: Define the analysis target, determine analysis direction, and identify a potential patent gap.

**Operations**:
1. Confirm target company (full name, ticker)
2. Determine analysis direction based on user intent:
   - **Forward**: user asks "who supplies X?" / "identify suppliers" / "what company provides the tech?" → target is A (the patent-gap company)
   - **Reverse**: user asks "who are B's customers?" / "which downstream companies depend on this?" / "what companies need this technology?" → target is B (the patent owner)
3. Identify the key technology T and/or product P:
   - If not specified by user, extract from product docs, annual report, prospectus, research reports
4. Preliminarily judge patent status:
   - Forward: does A likely lack T's patents?
   - Reverse: does B likely hold dominant/leading T patents?

**Output**: Direction (forward/reverse) + target company + T + P + preliminary judgment

## Step 2A: Patent gap confirmation (forward direction)

**Goal**: Confirm with data whether A truly lacks T-related patents, and rule out legitimate-use scenarios.

**Operations**:
1. Search A's patents using patent search tools (tushare patent API / PatSnap)
   - Dimensions: A's company name + technology T keywords + IPC class
   - Scope: CN patents + US patents + PCT (per A's market coverage)
   - Note: A may hold patents via subsidiaries / affiliates — search A's related entities
2. Determine gap:
   - A (incl. affiliates) has zero T-related patents → gap confirmed
   - A has T-related patents but count significantly below industry average → partial gap
   - A has sufficient T-related patents → no gap, terminate
3. Rule out legitimate-use scenarios (if gap confirmed):
   - Cross-licensing, acquisition inheritance, patent pool, open-source

**Output**: Gap determination + excluded legitimate-use scenarios

## Step 2B: Patent dominance confirmation (reverse direction)

**Goal**: Confirm that B indeed holds a dominant position in T's patent landscape.

**Operations**:
1. Search T's patent landscape comprehensively
2. Rank patent holders by quantity and citation count
3. Determine B's dominance:
   - B holds >30% of T's patents OR top-3 cited in T → dominant position confirmed
   - B holds 10-30% → moderate position
   - B holds <10% → weak position, may not be the key technology gatekeeper
4. Identify B's patent family: core patents + timing + jurisdiction coverage

**Output**: Dominance level (dominant / moderate / weak) + B's core patent list

## Step 3: Trade secret / know-how assessment

**Goal**: Determine whether the patent gap reflects true external dependency or deliberate trade secret / know-how protection.

**Core distinction**:
- **Trade secret**: A has the technology internally, but protects it through secrecy
- **True gap → dependency**: A genuinely lacks the technology and must source it externally

**Trade secret assessment — 6 indicators**:

| # | Indicator | Strong trade secret signal | Strong dependency signal |
|---|-----------|---------------------------|--------------------------|
| 1 | Industry norms | Semiconductor process, chemical formula, pharma manufacturing, metallurgy | Consumer electronics, commoditized components |
| 2 | NDA / confidentiality culture | Aggressive NDAs, non-compete lawsuits, "black box" secrecy | Open publications, transparent R&D |
| 3 | Patent filing pattern | Files patents in some domains, conspicuously avoids T | Files patents evenly or rarely |
| 4 | R&D intensity vs. patent output | High R&D spend + low T patents | Little to no R&D in T's domain |
| 5 | Technology nature | Process/method/formula (hard to reverse-engineer) | Physical component (identifiable in teardown) |
| 6 | Trade secret litigation | Company/industry trade secret theft lawsuits | Patent litigation is the norm |

**Scoring**: 4-6 = high likelihood, 2-3 = possible, 0-1 = unlikely.

**Override rule**: Procurement or teardown evidence can override trade secret finding.

**Note for reverse workflow**: In reverse direction, spend LESS effort on trade secret assessment for each downstream A — the key question is whether B's patents are essential, not whether A could develop internally. Assessment still runs but reduced weight.

**Output**: Trade secret likelihood + indicator breakdown

## Step 4: Multi-lead parallel verification

**Goal**: Activate 5 leads in parallel. Detailed scoring rules in `checklists.md`.

### Lead 1 · Patent ownership (confidence: medium)
- Forward: Search T's patent holders, identify primary owner B
- Reverse: For each candidate A, search whether A's products use T and whether A holds T patents
- Check patent status, timeline, NPE risk

### Lead 2 · Corporate affiliation (confidence: high)
- Search A-B affiliation via Tianyancha / Qichacha / Qixinbao
- Check: mutual investment, JV, executive overlap, historical changes

### Lead 3 · Talent flow (confidence: medium)
- Search B→A technical-hire / job-change records
- Use WebSearch + job sites
- Multiple technical staff → high likelihood

### Lead 4 · Procurement (confidence: high)
- Forward: Search A's tender announcements, supplier disclosures for B
- Reverse: Search B's listed customers, annual report client disclosures
- Directly named → high; category only → medium

### Lead 5 · Product teardown (confidence: very high)
- Physical evidence of B components in A's products
- Teardown reports / BOM analysis / product certifications

**Output**: Per-lead hit status + per-lead confidence

## Step 5: Competitive triangulation (NEW v0.3.0)

**Goal**: Check whether A's industry peers show the same patent gap pattern, providing cross-company corroboration.

**Why this matters**: A single company's patent gap could be random or internal. But if 3-5 industry peers ALL lack T's patents and ALL point to the same technology owner B, the pattern is unlikely to be coincidental — it strongly suggests B controls essential technology for the entire industry.

**Operations**:
1. Identify A's direct competitors / industry peers (C₁, C₂...Cₙ):
   - Same industry classification (SW / CITIC sector)
   - Similar product portfolio and revenue composition
   - Target: 3-10 peer companies
2. For each peer Cᵢ, check patent gap for technology T:
   - Search Cᵢ's patent portfolio for T-related patents
   - Record: has patents / has partial patents / has no patents
3. For peers with gaps, check where T likely comes from:
   - Do they also point to the same B? (patent ownership, procurement hints, teardown)
   - Or do different peers point to different owners?
4. Triangulation scoring:

| Pattern | Interpretation | Triangulation boost |
|---------|---------------|---------------------|
| ≥3 peers have same gap, all point to same B | Industry-wide dependency on B | **+2 levels** (low→high, medium→high) |
| ≥2 peers have same gap, all point to B | Significant corroboration | **+1 level** |
| 1 peer has same gap, points to B | Mild corroboration | No boost, note in report |
| Peers have gaps but point to different owners | Fragmented supply → B's position weaker | **No boost**, flag fragmentation |
| Peers have T patents or different tech paths | A is the outlier → A's gap may be self-inflicted | **No boost**, investigate why A alone lacks |

5. **Reverse workflow triangulation**: If analyzing B→downstream, check whether multiple unrelated companies in different sub-industries all depend on B's T → the wider the customer diversity, the stronger B's technology-gatekeeper position.

**Output**: Peer gap matrix + triangulation boost applied + interpretation

## Step 6: Multi-lead fusion + confidence scoring

**Goal**: Cross-validate and produce an overall determination.

**Fusion rules**:
- ≥2 high-confidence leads corroborating → overall "high", can conclude
- 1 high + 1 medium corroborating → overall "medium"
- Only 1 lead hit, no corroboration → overall "low", hypothesis only
- 0 leads hit → terminate, cannot infer

**Trade secret override rules**:
- Trade secret "high" + no procurement/teardown → cap at "low"
- Trade secret "high" + procurement/teardown confirms → no cap

**Triangulation boost** (v0.3.0, applied after base scoring):
- +2 levels if ≥3 peers confirm same gap → same B
- +1 level if ≥2 peers confirm same gap → same B
- Triangulation boost CANNOT override trade secret cap

**Role determination**:
- Leads 4/5 hit (procurement / teardown) → B is A's direct supplier (forward) / A is B's customer (reverse)
- Only leads 1/2 hit → technology licensor or indirect affiliation
- Lead 1 hit but B is NPE → B licenses only, not in supply chain

**Output**: Overall confidence + triangulation boost applied + B's/A's role

## Step 7: Financial cross-validation (NEW v0.3.0)

**Goal**: Cross-validate the inferred supply-chain relationship against hard financial data. No inference should reach "high" confidence without financial corroboration.

**Why this matters**: Patent gaps, corporate affiliations, and even teardowns can point to a relationship that exists but is economically immaterial. Financial data provides the "size of the bet" — how much does the relationship matter?

**Operations** (select applicable checks based on direction):

### Forward direction (A → find supplier B):
1. **A's supplier concentration** (westock / tushare financial data):
   - Check A's annual report: top-5 supplier list, supplier concentration ratio
   - Is B or a company matching B's profile named?
   - If supplier #1 is an unnamed "Company X" matching B's revenue profile → circumstantial
   - Supplier concentration >30% with unnamed top supplier → worth investigating

2. **A's procurement category matching**:
   - Does A's disclosed procurement categories align with B's products?
   - Check procurement proportion changes over time (growing share = deepening relationship)

3. **B's customer concentration**:
   - If B is listed: check B's top-5 customer list, customer concentration
   - Does a customer match A's revenue contribution profile?
   - If B has a single large customer contributing >20% of revenue → check if it matches A

4. **Accounts receivable / payable patterns**:
   - A's AP aging vs B's AR aging — do patterns correlate?
   - Related-party transaction disclosures

### Reverse direction (B → find customers A):
1. **B's customer concentration**: Who buys from B? Top-5 customer revenue breakdown
2. **B's revenue by industry segment**: Which downstream industries contribute most?
3. **For each candidate A**: Check A's top-5 supplier list for B or B's product category
4. **Related-party disclosures**: Check B and candidates for related-party transactions

### Financial corroboration scoring:

| Finding | Corroboration level | Confidence effect |
|---------|---------------------|-------------------|
| B explicitly named as A's supplier (or A as B's customer) | **Strong** | No cap, confirms relationship |
| Procurement category + revenue size matches | **Medium** | Supports but does not confirm |
| Related-party disclosures confirm | **Strong** | Confirms with financial detail |
| No financial evidence found | **None** | **Cap overall confidence at "medium"** |
| Financial data contradicts inference | **Negative** | **Reduce confidence by 1 level, flag inconsistency** |

**Critical rule**: If overall confidence after Step 6 (fusion + triangulation) is "high" but financial corroboration is "none" → **cap at "medium"**. The inference is promising but unverified financially. Only procurement/teardown physical evidence can maintain "high" without financial data.

**Output**: Financial corroboration level + specific evidence + confidence cap applied

## Step 8: Alternative-hypothesis check

See `checklists.md`. Explicitly exclude alternative explanations for each inference.

## Step 9: Counter-intelligence check

See `checklists.md`. Check for shell companies / dispersed patents / decoy filings.

## Step 10: Output report

Organize per the I/O contract in SKILL.md. Must include:
- Analysis direction (forward / reverse)
- Relationship determination (incl. role)
- Trade secret assessment result
- Evidence chain (5-lead hit table + confidence)
- **Triangulation result** (peer gap matrix + boost applied)
- **Financial corroboration** (evidence + corroboration level)
- Overall confidence + rationale (triangulation boost + financial cap transparent)
- Alternative-hypothesis list
- Counter-intelligence risk flag
- Follow-up verification suggestions

---

# Appendix: Reverse Workflow (Supplier → Clients)

## When to use

Trigger when the user provides a technology-owning company B and wants to identify downstream customers. Common scenarios:

- "台积电的先进封装客户有哪些？"
- "Qualcomm's 5G modem customers besides Apple?"
- "哪些车企用了宁德时代的麒麟电池？"
- "What companies depend on ASML's EUV lithography?"

## Process

### R1: Confirm B's patent dominance
- Run Step 2B to validate B's position in T
- If B is not dominant → warn and proceed at reduced confidence

### R2: Identify technology T and its necessity
- What exactly does B's patented technology enable?
- Is T a must-have (essential standard) or a nice-to-have (one of many options)?
- Essential standard (e.g., 5G SEP, HDMI) → downstream dependency is automatic
- Non-essential → need product-level evidence for each candidate A

### R3: Candidate screening
- Identify all companies whose products plausibly need T:
  - Industry classification search
  - Product announcements mentioning T or B
  - Trade show / conference participation (companies showcasing products using B's T)
  - Teardown reports naming B components
- Build candidate list: A₁, A₂...Aₙ

### R4: For each high-priority candidate, run abbreviated forward workflow
- Step 2A: Confirm Aᵢ lacks T patents
- Step 3: Quick trade secret check (reduced weight)
- Step 4: Run 5 leads for Aᵢ→B
- Step 7: Financial cross-validation (check B's customer list for Aᵢ)

### R5: Aggregate report
- List all confirmed customers with per-customer confidence
- Map B's customer concentration risk
- Flag any unnatural customer concentration (>30% from single customer)
