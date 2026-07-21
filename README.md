# patent-gap-supply-chain

通过"专利缺口"反推产业链关系的竞争情报工作流 skill，适用于 WorkBuddy / Codebuddy / Claude Code / Codex。

A competitive intelligence workflow skill that infers supply-chain relationships from patent gaps. Works with WorkBuddy / Codebuddy / Claude Code / Codex.

---

## 中文说明

### 这是什么

当研究科技股 / 硬件 / 制造 / 医药类公司时，发现标的 A 的产品使用了技术 T，但 A 没有 T 的相关专利——这套 skill 引导从专利缺口切入，**先判断缺口是真实外部依赖还是商业秘密/know-how保护**，再激活五条线索并行验证，竞对三角验证增强信号，财务数据交叉验证定论，输出带置信度的产业链关系推断报告。

**v0.3.0 新增**：
- **竞对三角验证**：A 的同行如果也缺相同专利、也指向同一家供应商 → 置信度大幅提升
- **反向工作流**（供应商→客户）：从技术垄断者出发，找下游依赖客户
- **财务数据交叉验证**：供应商/客户集中度、应收账款、关联交易 — 没有财报佐证的推断置信度封顶
- **行业特化参考**：半导体/制药/新能源/消费电子/材料五大行业指南
- **真实案例**：歌尔股份 & Apple VR 供应链完整分析

**v0.2.0**：商业秘密/know-how 评估（6 项指标）

### 核心设计

- **商业秘密先排除再定论**：专利缺口可能是商业秘密保护策略，必须先评估
- **竞对三角验证** (v0.3.0)：多同行同一缺口 → 同一供应商，是最强信号之一
- **财务交叉验证** (v0.3.0)：无财报佐证的推断置信度最多"中等"，需供应商披露/客户集中度/应收账款等硬数据
- **双向工作流** (v0.3.0)：正向（缺口→供应商）+ 反向（专利所有者→下游客户）
- **专利降级为入口线索**：专利缺口只触发验证，不直接定论
- **多线索交叉印证**：≥2 条高置信线索相互印证方可定论
- **替代假设必查**：显式排除交叉授权 / NPE / 第三方模组 / 商业秘密等替代解释
- **反情报对抗必查**：警惕壳公司 / 分散专利掩盖真实供应链

### 安装

    git clone https://github.com/tianzhiceng297-boop/patent-gap-supply-chain.git ~/.workbuddy/skills/patent-gap-supply-chain

或手动复制 `SKILL.md` 和 `references/` 到 `~/.workbuddy/skills/patent-gap-supply-chain/`。

### 兼容性 / Compatibility

| Agent | 加载方式 |
|------|---------|
| WorkBuddy / Codebuddy | 放入 `~/.workbuddy/skills/`，自动加载 + 触发词触发（原生支持） |
| Claude Code | 将 `SKILL.md` 内容追加到项目 `CLAUDE.md`，或放入 `.claude/commands/` 作为自定义命令；`references/` 按需在命令中引用 |
| Codex (OpenAI) | 将 `SKILL.md` 内容追加到项目 `AGENTS.md`；`references/` 按需引用 |
| 其他 agent | 将 `SKILL.md` 作为 system prompt 或项目指令注入；`references/` 按需提供 |

> 本 skill 的指令内容是通用 markdown，任何能读取并执行 markdown 指令的 agent 均可使用。区别仅在于加载机制：WorkBuddy 原生支持自动触发，其他 agent 需手动注入或配置。

### 触发场景

- "研究 XX 科技股"
- "分析 XX 公司的供应链 / 供应商"
- "查一下 XX 技术的专利归属"
- "XX 公司用了某技术但好像没专利"
- "识别 XX 的供应商"
- "XX 公司的下游客户有哪些？" (反向工作流)
- "哪些公司依赖 XX 的技术？" (反向工作流)

### 适用边界

| 标的类型 | 适用性 |
|---------|--------|
| 硬件 / 制造 / 医药 / 材料 | 强适用 |
| 软件 / 互联网 | 弱适用（降低置信度） |
| 商业模式创新 | 不适用 |

---

## English

### What it is

When researching tech / hardware / manufacturing / pharma companies, you find that target A's product uses technology T, but A holds no patents related to T. This skill starts from the patent gap, **first evaluates whether the gap reflects true external dependency or deliberate trade secret / know-how protection**, then activates five parallel verification leads, competitive triangulation, and financial cross-validation, outputting a confidence-scored supply-chain inference report.

**New in v0.3.0**:
- **Competitive triangulation**: Cross-check A's peers for the same patent gap — multi-company corroboration is one of the strongest signals
- **Reverse workflow** (supplier → clients): Start from a technology monopoly holder and identify downstream customers
- **Financial cross-validation**: Supplier/customer concentration, AR data, related-party disclosures — no inference reaches "high" without financial corroboration
- **Industry-specific guide**: Semiconductor, pharma, new energy, consumer electronics, materials verticals
- **Real case study**: Goertek (歌尔股份) & Apple VR supply chain

**v0.2.0**: Trade secret / know-how assessment (6 indicators)

### Core design

- **Trade secrets ruled out before concluding**: patent gap may reflect trade secret strategy — must assess first
- **Competitive triangulation** (v0.3.0): multi-peer same gap → same supplier is a top-tier signal
- **Financial cross-validation required** (v0.3.0): no "high" confidence without financial corroboration
- **Bidirectional workflow** (v0.3.0): forward (gap → supplier) + reverse (patent owner → downstream clients)
- **Patents demoted to an entry lead**: a patent gap triggers verification but does not directly conclude
- **Multi-lead corroboration**: ≥2 high-confidence leads corroborating required to conclude
- **Alternative hypotheses are mandatory**: explicitly exclude all alternative explanations
- **Counter-intelligence check is mandatory**: watch for shell companies / dispersed patents

### Installation

    git clone https://github.com/tianzhiceng297-boop/patent-gap-supply-chain.git ~/.workbuddy/skills/patent-gap-supply-chain

Or manually copy `SKILL.md` and `references/` to `~/.workbuddy/skills/patent-gap-supply-chain/`.

### Compatibility

| Agent | How to load |
|-------|-------------|
| WorkBuddy / Codebuddy | Drop into `~/.workbuddy/skills/`; auto-loaded + trigger-keyword activation (native) |
| Claude Code | Append `SKILL.md` content to the project `CLAUDE.md`, or place under `.claude/commands/` as a custom command; reference `references/` as needed |
| Codex (OpenAI) | Append `SKILL.md` content to the project `AGENTS.md`; reference `references/` as needed |
| Other agents | Inject `SKILL.md` as a system prompt or project instruction; provide `references/` on demand |

> The skill's instructions are plain markdown — any agent that can read and execute markdown instructions can use it. The only difference is the loading mechanism: WorkBuddy supports auto-triggering natively; other agents require manual injection or configuration.

### Trigger scenarios

- "Research [tech stock]"
- "Analyze [company]'s supply chain / suppliers"
- "Look up patent ownership of [technology]"
- "[Company] uses a technology but seems to have no patents"
- "Identify [company]'s suppliers"
- "Who are [company]'s downstream customers?" (reverse workflow)
- "Which companies depend on [technology]?" (reverse workflow)

### Applicability

| Target type | Applicability |
|-------------|---------------|
| Hardware / manufacturing / pharma / materials | Strong |
| Software / internet | Weak (reduce confidence) |
| Business-model innovation | Not applicable |

---

## File structure

- `SKILL.md` — core: triggers, direction selection, 10-step workflow (triangulation + financial cross-validation), I/O contract, tool integration, key principles
- `references/workflow.md` — detailed 10-step operations + reverse workflow appendix
- `references/checklists.md` — alternative hypotheses, trade secret indicators, triangulation checklist, financial cross-validation checklist, counter-intelligence, confidence scoring rules with 3-layer adjustment
- `references/industry-guide.md` (v0.3.0) — vertical-specific guidance: semiconductor, pharma, new energy, consumer electronics, materials/chemicals
- `examples/sample-output.md` — fictional case showing full 10-step report format
- `examples/real-case.md` (v0.3.0) — real A-share case: Goertek & Apple VR supply chain
- `CHANGELOG.md` — version history

## License

MIT
