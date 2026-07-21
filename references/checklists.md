# Checklists and Confidence Rules

## Alternative-Hypothesis Checklist

For each inference, check each item and explicitly exclude alternative explanations.

### Alternative explanations for the patent gap
- [ ] Does A's patent gap stem from a cross-licensing agreement? (check announcements / news)
- [ ] Did A inherit T usage rights via an acquisition? (check M&A history)
- [ ] Does T belong to a patent pool? (e.g., Avanci, MPEG-LA, Via Licensing)
- [ ] Is there an open-source implementation of T? (check open-source projects)
- [ ] Does A hold T patents via a subsidiary / affiliate? (check related-entity patents)
- [ ] Does A deliberately protect T as a trade secret / know-how rather than patenting it? (run Trade Secret Indicators checklist)
- [ ] **v0.3.0**: Is the patent gap simply because T is a purchased component that A has no reason to patent? (common in consumer electronics — weak signal)

### Alternative explanations for "B entered the supply chain"
- [ ] Is B an NPE (non-practicing entity)? If so, B licenses only and is not in the supply chain
- [ ] Does A use B's patent indirectly via a third-party module? The third party is the supplier; B is its upstream
- [ ] Is B only a patent licensor with no physical transaction with A?
- [ ] Is the A-B tie purely financial investment, not business cooperation?
- [ ] **v0.3.0**: Is the A-B relationship economically immaterial? (financial cross-validation required to exclude)

## Competitive Triangulation Checklist (NEW v0.3.0)

For each inference involving company A's patent gap, check against peers:

### Peer identification
- [ ] Identified 3-10 direct competitors / industry peers of A
- [ ] Peer selection based on: same industry classification + similar product + similar revenue size
- [ ] Both domestic and international peers considered where relevant

### Gap verification
- [ ] Peer C₁ patent portfolio checked for T → result: [has / partial / none]
- [ ] Peer C₂ patent portfolio checked for T → result: [has / partial / none]
- [ ] Peer Cₙ patent portfolio checked for T → result: [has / partial / none]

### Convergence check
- [ ] For peers with gaps, does T's ownership converge to the same B?
- [ ] Any peers pointing to different B₂, B₃? → flag fragmented supply
- [ ] Any peers with T patents? → A is the outlier, investigate why

### Triangulation boost
- [ ] ≥3 peers same gap → same B: **+2 confidence levels**
- [ ] 2 peers same gap → same B: **+1 confidence level**
- [ ] 1 peer same gap: note in report, no boost
- [ ] Peers diverge: no boost, flag fragmentation
- [ ] Triangulation cannot override trade secret cap or financial cross-validation cap

## Financial Cross-Validation Checklist (NEW v0.3.0)

### Forward direction
- [ ] Checked A's top-5 supplier list in annual report
- [ ] Checked A's supplier concentration ratio (>30% = single-supplier risk)
- [ ] Matched A's procurement categories against B's product lines
- [ ] If B is listed: checked B's top-5 customer list for A-like profile
- [ ] Checked B's customer concentration (>20% from single customer)
- [ ] Checked accounts receivable / payable patterns for correlation
- [ ] Checked related-party transaction disclosures
- [ ] Checked A's procurement growth vs. B's revenue growth (correlation)

### Reverse direction
- [ ] Checked B's top-5 customer list
- [ ] Checked B's revenue by industry segment
- [ ] For each candidate A: checked A's supplier list for B
- [ ] For each candidate A: checked A's related-party disclosures
- [ ] Estimated revenue contribution of each candidate (where data allows)

### Financial corroboration scoring
- [ ] **Strong corroboration**: B named as A's supplier OR A named as B's customer in official filings
- [ ] **Medium corroboration**: Procurement category + revenue scale matches
- [ ] **No corroboration**: No financial data found → **cap overall confidence at "medium"**
- [ ] **Negative corroboration**: Financial data contradicts inference → **reduce confidence by 1 level**

## Counter-Intelligence Checklist

### Patent-ownership side
- [ ] Is B's patent ownership abnormally dispersed? (multiple shell companies / affiliates holding separately)
- [ ] Is there a shell company set up specifically to hold patents? (low registered capital, no real operations, abnormal address)
- [ ] Are patent transfers frequent? (possibly to obscure true ownership)

### A's patent-strategy side
- [ ] Does A deliberately not file patents to hide its tech roadmap?
- [ ] Do A's patent filings mismatch its actual product technology? (decoy filings)

### Anomaly signals
- [ ] Is A's supplier-disclosure abnormally terse or deliberately vague?
- [ ] Are key technical staff employed via labor dispatch / outsourcing rather than directly?
- [ ] Are there related entities with frequent changes of company name / legal representative / shareholders?

If any anomaly exists → flag "counter-intelligence risk" in the report.

## Trade Secret / Know-How Indicators

### Indicator 1 · Industry norms
- [ ] T is in a trade-secret-dominant industry: semiconductor process, chemical formulation, pharma manufacturing process, food/beverage formula, metallurgy, specialty coatings, advanced materials synthesis
- [ ] Industry is known for "black box" secrecy (e.g., TSMC process, ASML optics, DuPont formulas)

### Indicator 2 · Company NDA / confidentiality culture
- [ ] A has public record of aggressive NDA enforcement or trade secret litigation
- [ ] A's employee contracts include strict non-compete + confidentiality clauses
- [ ] A's facilities known for tight access control / "clean room" culture
- [ ] A's technical publications / conference presentations notably sparse or redacted

### Indicator 3 · Patent filing pattern (strategic selectivity)
- [ ] A files patents across many domains but conspicuously avoids filing in T
- [ ] A's patent portfolio shows carefully scoped filings (protecting what can be reverse-engineered, hiding what cannot)
- [ ] A's patent filings in T's general category exist but intentionally omit key implementation details

### Indicator 4 · R&D intensity vs. patent output
- [ ] A has significant R&D team / budget in T's domain (check job postings, R&D headcount, R&D expense)
- [ ] A produces few or zero patents in T despite the R&D investment
- [ ] A's R&D-to-patent ratio in T is significantly lower than industry peers

### Indicator 5 · Technology nature (protectability)
- [ ] T is a process / method / formula / manufacturing technique (hard to reverse-engineer → good for trade secret)
- [ ] T is a software algorithm embedded in a black-box system (source code hidden → trade secret feasible)
- [ ] T would be difficult to prove infringement even if patented (→ low incentive to patent)

### Indicator 6 · Trade secret litigation history
- [ ] A has sued others for trade secret theft in T's domain
- [ ] A's peers in the industry have active trade secret litigation
- [ ] Industry news reports trade secret disputes related to T

### Scoring
- **4-6 indicators checked → High likelihood trade secret**: Patent gap likely strategic; default to "no dependency" unless procurement/teardown overrides
- **2-3 indicators checked → Possible trade secret**: Record as alternative hypothesis; proceed normally
- **0-1 indicators checked → Trade secret unlikely**: Gap likely reflects real dependency; proceed normally

### Override signal
- [ ] Lead 4 (procurement) returns high-confidence evidence of external sourcing → overrides trade secret hypothesis
- [ ] Lead 5 (teardown) physically identifies third-party component → overrides trade secret hypothesis

## Confidence Scoring Rules

### Per-lead confidence

| Lead | High | Medium | Low |
|------|------|--------|-----|
| Patent ownership | B holds T's core patents with valid timeline | B holds T-related patents but timeline questionable | T patents found but owner unclear |
| Corporate affiliation | JV / mutual investment | Executive overlap | Same address / same legal rep (weak) |
| Talent flow | Multiple B technical staff joined A | Single key person joined | Job postings only mention B background |
| Procurement | B directly named as supplier | B's category mentioned | Category-related but no clear pointer |
| Product teardown | Physically confirmed B component | Teardown report mentions but unconfirmed | No teardown report |

### Overall confidence (base)

- **High**: ≥2 high-confidence leads corroborating
- **Medium**: 1 high + 1 medium corroborating, or multiple medium leads partially corroborating
- **Low**: only 1 lead hit, or leads contradict each other
- **Cannot infer**: 0 leads hit

### Adjustment layers (apply in order, each CAN cap or boost):

**Layer 1 — Trade secret adjustment** (v0.2.0):
- Trade secret "high" + no procurement/teardown → cap at "low"; shift conclusion to "likely internal trade secret"
- Trade secret "high" + procurement/teardown confirms external sourcing → no cap
- Trade secret "possible" → no cap, record as alternative hypothesis

**Layer 2 — Triangulation boost** (v0.3.0):
- ≥3 peers same gap → same B: **boost +2 levels** (low→high, medium→high)
- 2 peers same gap → same B: **boost +1 level**
- Cannot override Layer 1 trade secret cap

**Layer 3 — Financial cross-validation cap** (v0.3.0):
- Strong financial corroboration → no cap
- Medium financial corroboration → no cap
- No financial corroboration → **cap at "medium"** (exception: procurement/teardown physical evidence can maintain "high" without financial data)
- Negative corroboration → **reduce by 1 level**

### Final confidence = min(base_confidence ± adjustments, Layer 3 cap)
