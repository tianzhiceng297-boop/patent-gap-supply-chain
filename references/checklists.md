# Checklists and Confidence Rules

## Alternative-Hypothesis Checklist

For each inference, check each item and explicitly exclude alternative explanations. List unexcluded ones in the report's "Alternative hypotheses" section.

### Alternative explanations for the patent gap
- [ ] Does A's patent gap stem from a cross-licensing agreement? (check announcements / news)
- [ ] Did A inherit T usage rights via an acquisition? (check M&A history)
- [ ] Does T belong to a patent pool? (e.g., Avanci, MPEG-LA, Via Licensing)
- [ ] Is there an open-source implementation of T? (check open-source projects)
- [ ] Does A hold T patents via a subsidiary / affiliate? (check related-entity patents)
- [ ] **NEW v0.2.0**: Does A deliberately protect T as a trade secret / know-how rather than patenting it? (run the Trade Secret Indicators checklist below; if ≥3 indicators hit, the gap may be strategic rather than a dependency)

### Alternative explanations for "B entered the supply chain"
- [ ] Is B an NPE (non-practicing entity)? If so, B licenses only and is not in the supply chain
- [ ] Does A use B's patent indirectly via a third-party module? The third party is the supplier; B is its upstream
- [ ] Is B only a patent licensor with no physical transaction with A?
- [ ] Is the A-B tie purely financial investment, not business cooperation?

## Counter-Intelligence Checklist

If the subject has counter-intelligence awareness, it may use these means to hide the real supply chain. Check each:

### Patent-ownership side
- [ ] Is B's patent ownership abnormally dispersed? (multiple shell companies / affiliates holding separately)
- [ ] Is there a shell company set up specifically to hold patents? (low registered capital, no real operations, abnormal address)
- [ ] Are patent transfers frequent? (possibly to obscure true ownership)

### A's patent-strategy side
- [ ] Does A deliberately not file patents to hide its tech roadmap? (the "gap" may be an active choice, not passive dependency)
- [ ] Do A's patent filings mismatch its actual product technology? (decoy filings)

### Anomaly signals
- [ ] Is A's supplier-disclosure abnormally terse or deliberately vague?
- [ ] Are key technical staff employed via labor dispatch / outsourcing rather than directly? (hiding talent flow)
- [ ] Are there related entities with frequent changes of company name / legal representative / shareholders?

If any anomaly exists → flag "counter-intelligence risk" in the report, raise vigilance, and recommend field research or primary-source verification.

## Trade Secret / Know-How Indicators (NEW v0.2.0)

Use this checklist to assess whether the patent gap reflects deliberate trade secret protection rather than external dependency. Count strong signals. See `references/workflow.md` Step 3 for detailed assessment flow.

### Indicator 1 · Industry norms
- [ ] T is in a trade-secret-dominant industry: semiconductor process, chemical formulation, pharma manufacturing process, food/beverage formula, metallurgy, specialty coatings, or advanced materials synthesis
- [ ] The industry is known for "black box" secrecy (e.g., TSMC's process, ASML's optics, DuPont's formulas)

### Indicator 2 · Company NDA / confidentiality culture
- [ ] A has a public record of aggressive NDA enforcement or trade secret litigation
- [ ] A's employee contracts are known to include strict non-compete + confidentiality clauses
- [ ] A's facilities are known for tight access control / "clean room" culture
- [ ] A's technical publications / conference presentations are notably sparse or redacted

### Indicator 3 · Patent filing pattern (strategic selectivity)
- [ ] A files patents across many domains but conspicuously avoids filing in T
- [ ] A's patent portfolio shows carefully scoped filings (protecting what can be reverse-engineered, hiding what cannot)
- [ ] A's patent filings in T's general category exist but intentionally omit key implementation details

### Indicator 4 · R&D intensity vs. patent output
- [ ] A has a significant R&D team / budget in T's domain (check job postings, R&D headcount, R&D expense)
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
- **4-6 indicators checked → High likelihood trade secret**: Patent gap likely strategic; default to "no dependency" unless procurement/teardown evidence overrides
- **2-3 indicators checked → Possible trade secret**: Record as alternative hypothesis; proceed normally
- **0-1 indicators checked → Trade secret unlikely**: Gap likely reflects real dependency; proceed normally

### Override signal
- [ ] Lead 4 (procurement) returns high-confidence evidence of external sourcing → overrides trade secret hypothesis
- [ ] Lead 5 (teardown) physically identifies third-party component → overrides trade secret hypothesis

## Confidence Scoring Rules

### Per-lead confidence

| Lead | High | Medium | Low |
|------|------|--------|-----|
| Patent ownership | B holds T's core patents with valid timeline | B holds T-related patents but timeline is questionable | T patents found but owner unclear |
| Corporate affiliation | JV / mutual investment | Executive overlap | Same address / same legal rep (weak) |
| Talent flow | Multiple B technical staff joined A | Single key person joined | Job postings only mention B background |
| Procurement | B directly named as supplier | B's category mentioned | Category-related but no clear pointer |
| Product teardown | Physically confirmed B component | Teardown report mentions but unconfirmed | No teardown report |

### Overall confidence

- **High**: ≥2 high-confidence leads corroborating
- **Medium**: 1 high + 1 medium corroborating, or multiple medium leads partially corroborating
- **Low**: only 1 lead hit, or leads contradict each other
- **Cannot infer**: 0 leads hit

### Trade secret adjustments to overall confidence (v0.2.0)

- If trade secret likelihood is "high" AND neither procurement nor teardown returns high-confidence evidence → **cap overall confidence at "low"**; conclusion shifts to "likely internal trade secret, no dependency inferred"
- If trade secret likelihood is "high" BUT procurement or teardown confirms external sourcing → **no cap**; the physical evidence overrides the trade secret hypothesis
- If trade secret likelihood is "possible" → **no cap**, but record as an alternative hypothesis
