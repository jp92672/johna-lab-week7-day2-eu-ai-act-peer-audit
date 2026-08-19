# Peer Audit Report — Stups

**Auditor:** JohnA
**System audited:** Stups (a personal daily evening-suggestion agent for Hamburg, built on n8n)
**Builder / client:** GordonS
**Materials provided:** README.md, Report_1.md (sample outputs), Stups0.6.md (n8n workflow export), gtm_future_sprints.md, stack_decision_1.md — all in `Peers Project files/`
**Date of review:** 2026-08-19

This report follows the phase structure in `instructions.md`. Work was done independently; clarifying questions were logged in writing (Phase 3) before the classification was finalized.

---

## Phase 1: Read and annotate

Annotated on a first and second read of the materials, per the lab's underline / circle / mark method.

**Underlined — elements affecting risk-tier classification (current build):**
1. Fully automated daily decision, no human in the loop ("a schedule fires... no human involved"; "it decides for me").
2. Single-user, personal use only, with a static, hand-written taste profile — no learning, no other users' data processed.
3. Consumer-facing LLM-generated recommendation content delivered directly to a person via Telegram.

**Underlined — forward-looking / roadmap elements (not built yet, called out separately, not used to set today's tier):**
4. `gtm_future_sprints.md` states clear intent to onboard real outside users (Sprint 0–1).
5. Planned "not tonight → why" feedback loop and persistent memory — where actual behavioral learning/profiling would begin.
6. Planned B2B sale to relocation agencies and employer HR teams (Sprint 3) — introduces a new deployer role and an employment-adjacent distribution context.

**Circled — unclear, resolved via clarifying questions (see Phase 3):**
- Current deployment status (personal-only vs. already shared).
- Whether age or vulnerability screening exists for future users.
- Whether any legal/retention policy exists yet.
- Whether an Article 50 AI-disclosure is already in place.

**Marked — obligation-suggestive elements:**
- Article 50 (transparency for AI-generated content shown to a person) — relevant even at limited-risk tier.
- GDPR — becomes live the moment a second person's data is processed; noted for awareness only per this lab's EU AI Act focus (GDPR is covered in a separate lab).

---

## Phase 2: First-pass classification

| Question | Answer |
|---|---|
| Does this system fall under any prohibited category (Article 5)? | No. No subliminal manipulation, exploitation of vulnerabilities, social scoring, or biometric categorization/emotion inference. |
| Does this system operate in any of the eight Annex III areas? | No. Not employment, education, essential services, law enforcement, migration, justice, biometrics, or critical infrastructure — it is a leisure/lifestyle recommender. |
| If Annex III: significant influence or narrow/preparatory? | N/A — not an Annex III system. |
| Does this system interact with end users or generate content requiring disclosure (Article 50)? | Yes. It is an AI system that generates natural-language content and delivers it directly to a person. |
| **First-pass risk tier** | **Limited risk / transparency (Article 50)** |
| One-sentence justification | The system falls outside Article 5 prohibited practices and outside all eight Annex III high-risk domains, but as an AI system whose output is generated content delivered to a natural person, it triggers the Article 50 transparency obligation. |

**Uncertainty flag:** This tier applies to the *current single-user prototype*. If the GTM roadmap ships as written — real user onboarding, behavioral learning/memory, and B2B distribution through employers/relocation agencies — the tier label likely stays the same (leisure suggestions remain outside Annex III), but the role map and the audience of the Article 50 obligation would expand and should be re-checked at that point.

---

## Phase 3: Clarifying questions log

Questions asked in writing before finalizing the classification, with provisional assumptions and the answers received.

**1. Does the Telegram message currently disclose that the content is AI-generated?**
- Why it matters: directly determines whether Article 50 is met.
- Provisional assumption: No.
- Answer received: **Confirmed no.** No disclosure exists.

**2. What is the actual current user count?**
- Why it matters: determines whether this is internal testing (no third-party data, no wider disclosure obligation) or already live with others.
- Provisional assumption: builder-only.
- Answer received: **Confirmed — 1 user (GordonS, the builder), prototype stage, plans to move from prototyping to testing with friends to small trials.**

**3. Does onboarding collect any special-category data (health, precise location, religion, gender identity, etc.) alongside ordinary preferences?**
- Why it matters: determines whether future data collection stays in ordinary-preference territory or edges into higher-sensitivity processing requiring deliberate handling.
- Provisional assumption: no location data; fields look like ordinary preferences.
- Answer received: **No location data.** However, the onboarding "secret" field is open free text and is logged — a user could plausibly disclose religion, gender identity, or other special-category data if they believe it affects the recommendations they receive. See Finding 2.

**4. Is there any human review of the AI Agent's output before it is sent, or is delivery fully automated end-to-end?**
- Why it matters: confirms the strength of the "no human involved" framing for the audit's autonomy characterization.
- Provisional assumption: fully automated, no pre-send review.
- Answer received: **Confirmed.** Humans review logs after the fact for errors/optimization only; the only feedback loop is the end user's own reaction (attending or not, liking it or not) — not a compliance safeguard.

Parallel note (per lab scope): GDPR obligations become relevant the moment a second person's data is processed, but are intentionally not analyzed in depth here — covered in a separate lab.

---

## Phase 4: Audit report

### Section 1: System summary

Stups is a personal, fully automated evening-suggestion agent built on n8n. Once a day at a fixed time, it fetches live Hamburg weather and event data (via one real API and several scraped sources), feeds these plus a fixed, hand-written taste profile to an LLM agent, and sends one message with two independent suggestions to the builder's phone over Telegram. It currently holds no memory between runs and serves one user only — the builder. The builder's roadmap describes scaling it into a product for other users, with learning, monetization, and eventually B2B distribution through relocation agencies and employer HR teams.

### Section 2: Risk classification

**First-pass tier: Limited risk / transparency (Article 50).**

Justification: the system does not fall under any Article 5 prohibited practice and does not operate in any Annex III high-risk area — it is a leisure recommender, not an employment, education, essential-services, law-enforcement, migration, justice, biometric, or infrastructure system. It does, however, generate AI content delivered directly to a person, which triggers the Article 50 transparency obligation regardless of tier.

Uncertainty: this classification is for the current single-user prototype. The stated roadmap (real users, learning/memory, B2B distribution) does not appear to change the tier itself, but does expand the role map and the audience the transparency obligation applies to — this should be revisited once Sprint 0 ships.

### Section 3: Role map

| Role | Who | Key obligations |
|---|---|---|
| **Provider** | GordonS — designed the taste logic, prompts, and n8n workflow | Under Article 50: ensure AI-generated content is disclosed to users. General duty to document the system, even below high-risk tier. |
| **Deployer** | GordonS (currently same person as provider, running it for personal/prototype use) | At this stage, provider and deployer obligations are held by the same person. Once outside users are onboarded, deployer duties (appropriate use, informing end users) become distinct. |
| **Third-party vendors** | OpenWeatherMap, Ticketmaster, Telegram, DeepSeek, OpenAI, plus scraped sites (hamburg.com, Stadtpark Open Air, Filmraum, Abaton, Kinopolis) | Upstream data/model suppliers, not "providers" of this system under the Act. DeepSeek/OpenAI are themselves regulated model providers whose terms are inherited in part by integration. Scraped sites carry a fragility risk (already self-documented by the builder in `stack_decision_1.md`), not a compliance obligation. |

**Forward-looking note:** if the roadmap proceeds to Sprint 3 (B2B sale to relocation agencies/employer HR teams), a new deployer role appears — the agency or employer distributing Stups to their newcomers/employees — with its own obligation to inform those end users the content is AI-generated.

### Section 4: Compliance findings

> **Finding 1 — No AI-generated content disclosure (Article 50)**
> **Severity:** Significant
> **Description:** Article 50 requires users be informed when interacting with or receiving content from an AI system. Confirmed (not assumed): no disclosure exists in the Telegram messages or bot profile.
> **Recommended action:** Add a lightweight, permanent disclosure (e.g., a fixed line in the message or the bot's profile/description) stating the content is AI-generated, before onboarding any user beyond the builder.
> **Escalation needed?** No — straightforward implementation fix, no legal review required at this scale.

> **Finding 2 — Open free-text field can capture special-category data**
> **Severity:** Significant
> **Description:** The onboarding "secret" field is open-ended by design and is logged. Because it invites the user to share anything that might shape recommendations, it plausibly invites special-category personal data — e.g., religion (to avoid conflicting events, dietary needs) or gender identity (to influence which venues/events feel relevant or safe). This is a predictable use of an open field intended to personalize output, not a remote edge case. No safeguard, minimization, or handling policy exists today.
> **Recommended action:** Before onboarding any outside user, either (a) constrain the field's prompt to steer away from special-category disclosures, or (b) if such input is expected and valuable, treat it deliberately as special-category data with an explicit basis for processing and a retention/deletion policy — rather than letting it happen unintentionally through an open text box.
> **Escalation needed?** Yes — flag to whoever owns data-protection sign-off before Sprint 0, since this shifts from a pure AI Act transparency question toward a data-protection design decision needing deliberate handling.

> **Finding 3 — Fully automated pipeline with no pre-send human oversight**
> **Severity:** Minor
> **Description:** Not a blocking requirement at the current limited-risk tier, but worth noting: no human checks the AI Agent's output before delivery. Oversight is retroactive only (log review, end-user reaction).
> **Recommended action:** No action required at current scale. Revisit if the system scales into contexts with a higher stakes profile (e.g., B2B distribution through employers).
> **Escalation needed?** No.

**Parallel legal issue flagged (not analyzed in depth, per lab scope):** GDPR obligations become relevant the moment a second person's preference data is processed (Sprint 0). Noted for awareness; detailed analysis is out of scope for this lab and belongs to the GDPR-focused lab.

### Section 5: Overall recommendation

**Proceed with conditions.**

The current prototype — single-user, personal use — carries no blocking findings: it is not a prohibited practice and not an Annex III high-risk system, and its limited-risk/transparency classification is manageable as-is. However, two Significant findings (missing Article 50 disclosure; the open free-text field's unmanaged exposure to special-category data) must be resolved before the system moves past prototype into Sprint 0 (onboarding real outside users). Neither finding requires a redesign — both are scoped, implementable fixes — but neither should ship to other people unresolved. Conditions: (1) add an AI-generated content disclosure to the delivered message or bot profile; (2) decide and implement a deliberate policy for the free-text field (constrain it, or treat its contents as special-category data with a basis and retention policy) before any non-builder user is onboarded.

### Section 6: What this report is not

This report is not a legal opinion, not a conformity assessment, and not a certification. It reflects an independent first-pass review based solely on the materials provided (README, sample outputs, workflow export, stack decision, and GTM roadmap) and the clarifying answers given during this audit. Conclusions should be verified with qualified legal counsel before any EU market placement or onboarding of users beyond the current single-user prototype.

---

## Phase 5: Debrief conversation

Debrief held with GordonS, following the order in `instructions.md`: auditor presents → builder responds → compare classifications → compare gap lists → joint closing note.

- **Auditor presents:** Completed — walked GordonS through the audit report uninterrupted.
- **Builder responds:** Completed — GordonS confirmed the findings and added context on the roadmap's future scaling plans.
- **Compare classifications:** No disagreement. Both the self-audit and this peer audit landed on the same tier — Limited risk / transparency (Article 50) — with no dispute on the governing article.
- **Compare gap lists:** Aligned. The self-audit and peer audit identified largely the same findings, with no major gaps caught by one side and missed by the other.
- **Joint closing note (required deliverable):** Auditing your own work and auditing someone else's produced the same conclusions here — both landed on Article 50 and largely the same findings — but the debrief still added value by surfacing future-scaling considerations (prototype → beta → GA) that are easier to spot with a different, external perspective. Agreement on today's findings doesn't mean an outside view stops being useful once the system starts to grow.
