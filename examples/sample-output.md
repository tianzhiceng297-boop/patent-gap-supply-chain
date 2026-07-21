# Sample Output

A worked example showing the full v0.3.0 report format for a **fictional** case with triangulation and financial cross-validation. All companies, patents, and data below are illustrative only.

---

## Input

- **Direction**: Forward
- **Target A**: NexRouter Inc. (fictional ticker: NXR)
- **Product P**: NexRouter 5G Pro router
- **Technology T**: 5G mmWave antenna technology
- **Background**: NexRouter's 5G Pro uses mmWave antennas, but NexRouter's filings show no mmWave-antenna patents.

## Step 0: Applicability

Hardware / networking equipment → **Strong applicability**. Proceed.

## Step 1: Trigger + Direction

- Direction: Forward
- A = NexRouter Inc.
- P = NexRouter 5G Pro
- T = 5G mmWave antenna technology
- Preliminary: NexRouter's main R&D is in router firmware / SoC, not RF antenna design. Gap likely.

## Step 2: Patent gap confirmation

Searched NexRouter + subsidiaries in patent DB (CN + US + PCT) for "mmWave antenna" / IPC H01Q:
- NexRouter patents: 12 total, all in firmware / routing protocols / SoC. **Zero** mmWave-antenna patents.
- Gap status: **Confirmed**.

Excluded legitimate-use scenarios:
- Cross-licensing: no public record found.
- Acquisition: no mmWave-antenna company acquired.
- Patent pool: mmWave antenna not part of Avanci / MPEG-LA pools.
- Open-source: no open-source mmWave-antenna implementation applicable.

## Step 3: Trade secret assessment

| Indicator | Assessment | Signal |
|-----------|------------|--------|
| 1. Industry norms | mmWave antenna is RF hardware, not a process/formula; easy to reverse-engineer | Dependency |
| 2. NDA culture | NexRouter is a standard networking company; no notable NDA litigation | Neutral |
| 3. Patent filing pattern | NexRouter files actively in firmware/SoC but zero in RF — no evidence of strategic selectivity | Neutral |
| 4. R&D vs. patent output | NexRouter lists 0 RF engineers and 0 RF R&D budget in filings | Dependency |
| 5. Technology nature | mmWave antenna is a physical hardware component, identifiable in teardown | Dependency |
| 6. Trade secret litigation | No trade secret litigation in mmWave antenna industry; patent litigation is the norm | Dependency |

**Trade secret likelihood**: 0/6 → **Trade secret unlikely**. Proceed.

## Step 4: Multi-lead parallel verification

| Lead | Hit | Detail | Confidence |
|------|-----|--------|------------|
| 1. Patent ownership | Yes | WaveAnt Technologies holds 47 mmWave-antenna patents (CN + US), core filings 2019-2021, before NexRouter 5G Pro launch (2024) | Medium |
| 2. Corporate affiliation | Yes | WaveAnt and NexRouter co-founded a JV "NexWave RF" in 2023 (registered capital 50M, 60/40 split) | High |
| 3. Talent flow | Yes | 3 WaveAnt antenna engineers joined NexRouter in 2023-2024 (LinkedIn / job sites) | Medium |
| 4. Procurement | Yes | NexRouter 2023 annual report lists "mmWave antenna module" as a top-5 procurement category; supplier not named | Medium |
| 5. Product teardown | Yes | TechInsights teardown of 5G Pro (2024) identifies antenna module stamped "WaveAnt WN-5G39" | Very high |

## Step 5: Competitive triangulation

A's peers in networking equipment:

| Peer | Company | Has mmWave antenna patents? | Points to same B? |
|------|---------|----------------------------|-------------------|
| C₁ | RouteMax Corp | Zero mmWave patents | Yes — also uses WaveAnt (teardown confirms WN-5G37 module) |
| C₂ | NetFusion Ltd | Zero mmWave patents | Yes — procurement category matches |
| C₃ | SpeedLinx Inc | Zero mmWave patents | Yes — JV with WaveAnt (2022) |
| C₄ | TeleCore GmbH | **Has own mmWave patents** | No — internal development |

**Triangulation result**: 3/4 peers all lack T patents, all point to WaveAnt. **Triangulation boost: +2 levels.**

## Step 6: Fusion + confidence (base)

- 5 leads hit, 4 corroborate (patent + JV + talent + teardown all point to WaveAnt)
- Lead 5 (teardown) = very high
- Trade secret: unlikely → no cap
- Base confidence: **High**.

**Triangulation boost applied**: +2 levels — already at High, remains High (no ceiling exceeded).

Role determination: Lead 4/5 hit → WaveAnt is a **direct supplier**.

## Step 7: Financial cross-validation

| Check | Result | Corroboration |
|-------|--------|---------------|
| NexRouter top-5 supplier list | Supplier #3: "Antenna module supplier" with 12.4% of procurement — revenue contribution ¥380M matches WaveAnt's reported revenue | **Medium** |
| WaveAnt customer concentration | WaveAnt IPO prospectus (2024) shows Customer #1 = 22% of revenue, profile matches NexRouter | **Medium** |
| Accounts receivable | WaveAnt AR aging shows 60% from one customer with 90-day terms — matches NexRouter's AP profile | **Medium** |

**Financial corroboration**: **Medium** (category + revenue scale matches, but not explicitly named).

**Financial cap**: Medium corroboration → no cap applied.

## Step 8: Alternative-hypothesis check

| Hypothesis | Excluded? | Basis |
|-----------|-----------|-------|
| Gap due to cross-licensing | Yes | No public record |
| WaveAnt is NPE (license only) | Yes | WaveAnt manufactures physical modules (teardown stamp) |
| Third-party module embedding WaveAnt patent | Partially | Teardown shows direct WaveAnt module |
| NexRouter-WaveAnt tie is pure financial investment | Yes | JV is operational (NexWave RF) + direct supply |
| Gap is trade secret protection | Yes | 0/6 indicators; no RF R&D |
| Relationship is economically immaterial | Yes | 12.4% of procurement → material |
| Gap is because mmWave is a purchased component A wouldn't patent | Partially | Possible — but the JV + teardown evidence overrides this |

Unexcluded: none material.

## Step 9: Counter-intelligence check

| Check | Result |
|-------|--------|
| WaveAnt patent ownership abnormally dispersed? | No — 47 patents under WaveAnt directly |
| Shell companies? | No — real operating company |
| NexRouter deliberately not filing? | Unlikely — files aggressively in core areas |
| Frequent patent transfers? | No |
| Supplier disclosure vague? | **Flag** — not explicitly named |

Counter-intelligence risk: **Low**. One minor flag.

## Step 10: Output report

### Supply-Chain Inference Report — NexRouter Inc.

**Direction**: Forward (patent gap → supplier)

**Trade secret assessment**: Unlikely (0/6 indicators). NexRouter has no R&D presence in mmWave antenna.

**Triangulation**: 3/4 industry peers share the same patent gap, all pointing to WaveAnt. Strong cross-company corroboration.

**Relationship determination**: WaveAnt Technologies has entered NexRouter's supply chain as a **direct supplier** of mmWave antenna modules.

**Evidence chain**:

| # | Lead | Hit | Confidence |
|---|------|-----|------------|
| 1 | Patent ownership | WaveAnt holds 47 mmWave-antenna patents, pre-dating 5G Pro | Medium |
| 2 | Corporate affiliation | Co-founded JV "NexWave RF" (2023) | High |
| 3 | Talent flow | 3 WaveAnt engineers → NexRouter (2023-24) | Medium |
| 4 | Procurement | Annual report lists mmWave antenna module as top-5 procurement | Medium |
| 5 | Product teardown | Teardown identifies "WaveAnt WN-5G39" module | Very high |

**Confidence stack**:
- Base fusion: 5/5 leads hit, 4 corroborating → **High**
- Triangulation boost: 3 peers confirm same gap → B → +2 levels → remains **High**
- Financial corroboration: Medium (procurement category + revenue scale match) → no cap
- **Final confidence: High**

**Alternative hypotheses**: All material alternatives excluded.

**Counter-intelligence risk**: Low.

**Follow-up verification**:
- Confirm WaveAnt revenue exposure to NexRouter via WaveAnt's post-IPO filings
- Monitor NexWave RF JV activity for deepening integration
- Track WaveAnt patent filings post-2024 for co-development indicators
- Watch for RouteMax and NetFusion teardown results (triangulation peers)
