# Sample Output

A worked example showing the full report format for a **fictional** case. All companies, patents, and data below are illustrative only.

---

## Input

- **Target A**: NexRouter Inc. (fictional ticker: NXR)
- **Product P**: NexRouter 5G Pro router
- **Technology T**: 5G mmWave antenna technology
- **Background**: NexRouter's 5G Pro uses mmWave antennas, but NexRouter's filings show no mmWave-antenna patents.

## Step 0: Applicability

Hardware / networking equipment → **Strong applicability**. Proceed.

## Step 1: Trigger lead identification

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

## Step 3: Trade secret assessment (NEW v0.2.0)

| Indicator | Assessment | Signal |
|-----------|------------|--------|
| 1. Industry norms | mmWave antenna is RF hardware, not a process/formula; easy to reverse-engineer via teardown | Dependency |
| 2. NDA culture | NexRouter is a standard networking company; no notable NDA litigation | Neutral |
| 3. Patent filing pattern | NexRouter files actively in firmware/SoC but zero in RF — however, no evidence of strategic selectivity | Neutral |
| 4. R&D vs. patent output | NexRouter lists 0 RF engineers and 0 RF R&D budget in filings | Dependency |
| 5. Technology nature | mmWave antenna is a physical hardware component, identifiable in teardown | Dependency |
| 6. Trade secret litigation | No trade secret litigation in mmWave antenna industry; patent litigation is the norm | Dependency |

**Trade secret likelihood**: 0/6 indicators → **Trade secret unlikely**. Proceed with normal workflow.

## Step 4: Multi-lead parallel verification

| Lead | Hit | Detail | Confidence |
|------|-----|--------|------------|
| 1. Patent ownership | Yes | WaveAnt Technologies holds 47 mmWave-antenna patents (CN + US), core filings 2019-2021, before NexRouter 5G Pro launch (2024) | Medium |
| 2. Corporate affiliation | Yes | WaveAnt and NexRouter co-founded a JV "NexWave RF" in 2023 (registered capital 50M, 60/40 split) | High |
| 3. Talent flow | Yes | 3 WaveAnt antenna engineers joined NexRouter in 2023-2024 (LinkedIn / job sites) | Medium |
| 4. Procurement | Yes | NexRouter 2023 annual report lists "mmWave antenna module" as a top-5 procurement category; supplier not named | Medium |
| 5. Product teardown | Yes | TechInsights teardown of 5G Pro (2024) identifies antenna module stamped "WaveAnt WN-5G39" | Very high |

## Step 5: Fusion + confidence

- 5 leads hit, 4 corroborate (patent ownership + JV + talent + teardown all point to WaveAnt).
- Lead 5 (teardown) = very high confidence, direct physical evidence.
- Trade secret assessment: unlikely — no cap on confidence.
- Overall confidence: **High**. Conclusion is supported.

Role determination: Lead 4/5 hit (procurement + teardown) → WaveAnt is a **direct supplier** of mmWave antenna modules to NexRouter.

## Step 6: Alternative-hypothesis check

| Hypothesis | Excluded? | Basis |
|-----------|-----------|-------|
| Gap due to cross-licensing | Yes | No public record; WaveAnt JV suggests commercial supply, not just license |
| WaveAnt is NPE (license only) | Yes | WaveAnt manufactures physical modules (teardown stamp confirms) |
| NexRouter uses third-party module that embeds WaveAnt patent | Partially | Teardown shows WaveAnt-branded module directly, not a third-party rebrand |
| NexRouter-WaveAnt tie is pure financial investment | Yes | JV is operational (NexWave RF), plus direct supply |
| Gap is due to trade secret protection | Yes | 0/6 trade secret indicators; no R&D in RF domain; teardown physical evidence confirms external sourcing |

Unexcluded: none material.

## Step 7: Counter-intelligence check

| Check | Result |
|-------|--------|
| WaveAnt patent ownership abnormally dispersed? | No — 47 patents all under WaveAnt Technologies directly |
| Shell company holding patents? | No — WaveAnt is a real operating company (revenue, employees) |
| NexRouter deliberately not filing to hide tech route? | Possible but unlikely — NexRouter does file aggressively in its core areas |
| Frequent patent transfers? | No |
| Supplier disclosure abnormally vague? | **Flag** — annual report names category but not supplier; common but worth noting |

Counter-intelligence risk: **Low**. One minor flag (vague supplier disclosure).

## Step 8: Output report

### Supply-Chain Inference Report — NexRouter Inc.

**Trade secret assessment**: Unlikely (0/6 indicators). NexRouter has no R&D presence in mmWave antenna; the patent gap reflects true external dependency, not deliberate secrecy.

**Relationship determination**: WaveAnt Technologies has entered NexRouter's supply chain as a **direct supplier** of mmWave antenna modules.

**Evidence chain**:

| # | Lead | Hit | Confidence |
|---|------|-----|------------|
| 1 | Patent ownership | WaveAnt holds 47 mmWave-antenna patents, pre-dating 5G Pro | Medium |
| 2 | Corporate affiliation | Co-founded JV "NexWave RF" (2023) | High |
| 3 | Talent flow | 3 WaveAnt engineers → NexRouter (2023-24) | Medium |
| 4 | Procurement | Annual report lists mmWave antenna module as top-5 procurement | Medium |
| 5 | Product teardown | Teardown identifies "WaveAnt WN-5G39" module | Very high |

**Overall confidence**: **High** (≥2 high-confidence leads corroborating; teardown provides physical confirmation; trade secret assessment negative).

**Alternative hypotheses**: All material alternatives excluded (see Step 6).

**Counter-intelligence risk**: Low. Minor flag on vague supplier disclosure in annual report.

**Follow-up verification suggestions**:
- Confirm WaveAnt revenue exposure to NexRouter via WaveAnt's filings (if listed) or credit report.
- Monitor NexWave RF JV activity for deeper integration signals.
- Track WaveAnt patent filings post-2024 for co-development indicators.
