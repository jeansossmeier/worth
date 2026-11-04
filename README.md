# WORTH: A Framework for Context-Driven Technology Decisions

*A pragmatic approach to cutting through dogma and making engineering choices that deliver real value*

**Version:** 1.2 \
**Author:** Jean Luck Sossmeier \
**Date:** October 2025 (updated November 2026) \
**Repository:** [github.com/jeansossmeier/worth](https://github.com/jeansossmeier/worth)

> **Important boundaries:** WORTH is a decision aid, not legal, financial, security, compliance, or procurement advice. Examples, costs, thresholds, and outcomes are illustrative unless directly cited. Verify current vendor pricing, regional availability, legal obligations, and compliance requirements with qualified professionals before relying on them. Cited sources were last checked in November 2026. AI facts are the most perishable material here: re-score with current facts before relying on any AI scorecard.

---

## WORTH in 30 Seconds

Before adopting any technology, pattern, or practice, score five questions from 1 (clear no) to 5 (strong yes, backed by evidence):

| Dimension | Question |
|-----------|----------|
| **W**eigh the problem | Is the pain concrete, frequent, and business-critical? |
| **O**utcome > overhead | Does the benefit clearly exceed the total cost? |
| **R**ight-sized for the team | Can the current team build and run it confidently? |
| **T**ime-to-value | Does it deliver value within the quarter? |
| **H**orizon flexibility | Can you reverse or adapt the choice later? |

**The rule**: check the hard gates first (legal, security, privacy and data residency, safety, irreversible lock-in, procurement): a failed gate is a no, whatever the total. Then add the five scores: 20-25 is a strong candidate, 18-19 worth doing, 15-17 borderline, 12-14 probably not, below 12 a strong NO. Any dimension at 1 or 2 stops the decision until an **Override:** line says what must change. A benefit nobody has put a number on yet scores O 3 and allows at most a pilot with a kill date. When the case is weak: simplify, re-scope it as a scored pilot, or wait.

**AI decisions**: score one task, never *AI*. Count checking, fixing, and escaped errors as overhead, and add one gate: a named business owner answers for what the AI says and does ([Section 17](#ai-decisions)).

[Run your first scorecard →](#scorecard)

---

## Table of Contents

### Part I: Foundation
1. [Introduction: The Problem with Technology Dogmas](#introduction)
2. [The WORTH Principle: Core Framework](#core-framework)
3. [Decision Scorecard & Methodology](#scorecard)

### Part II: Deep Dive
4. [Expanded Dimensions](#expanded-dimensions)
5. [Real-World Application](#real-world-application)
6. [Practical Examples](#practical-examples)

### Part III: Domain Applications
7. [Code Principles](#code-principles)
8. [Acronym Analysis](#acronym-analysis)
9. [Architecture Decisions](#architecture-decisions)
10. [Common Dogmas Debunked](#dogmas-debunked)

### Part IV: Operationalization
11. [Integration Points](#integration-points)
12. [Anti-Patterns & Failure Modes](#anti-patterns)
13. [Measurement & Validation](#measurement)
14. [Operationalizing the Framework](#operationalizing)

### Part V: Extended Applications
15. [Case Studies](#case-studies)
16. [Blind Spots Addressed](#blind-spots)
17. [AI Decisions](#ai-decisions)

### Appendices
- [A. WORTH Canvas](#appendix-a)
- [B. Decision Tables Consolidated](#appendix-b)
- [C. Additional Dogmas](#appendix-c)
- [D. Glossary & Further Reading](#appendix-d)

### Closing
- [Conclusion](#conclusion)

---

# Part I: Foundation

<a name="introduction"></a>
## 1. Introduction: The Problem with Technology Dogmas

Software engineering is drowning in principles. KISS, DRY, YAGNI, SOLID, STUPID, CUPID; the list grows monthly. Each promises clarity, yet teams still waste months on Kubernetes setups they don't need, microservices that slow them down, and clean code rules that add more ceremony than value.

The problem isn't the principles themselves. The problem is treating them as universal laws rather than context-dependent tools.

### The Missing Question

Behind every architectural debate, every technology choice, every process adoption, one question gets skipped:

**Is it worth it, here, now, for us?**

Not "is it best practice?" Not "what does Big Tech do?" Not "what's trending on Hacker News?"

Worth it. In your context. With your team. On your budget. For your timeline.

### Why Context Matters More Than Dogma

Consider these common scenarios:

**Language Wars**: Developers argue whether Python, Go, or Rust is "best", ignoring that the answer depends entirely on your domain, team skills, hiring market, and performance requirements.

**Clean Code Extremism**: Teams wrap every primitive in a Value Object because a book said so, adding hundreds of lines of boilerplate to a throwaway migration script.

**"You Are Not Netflix"**: A three-person startup burns four weeks setting up Kubernetes because "it's how you scale", never shipping the product that needed to prove market fit first.

**Process Bloat**: A seed-stage company adopts SAFe with two-week sprints and full ceremonies, while the market shifts weekly and competitors move daily.

Each of these decisions might be right somewhere. They're wrong when blindly applied without asking whether they're worth it in your specific situation.

### What WORTH Provides

WORTH is a meta-principle that sits above all other engineering guidelines. It doesn't replace KISS, DRY, or YAGNI; it helps you decide when to apply them.

Think of it as a five-question litmus test you run before adopting any technology, pattern, or practice:

- **W**eigh the problem
- **O**utcome > overhead
- **R**ight-sized for the team
- **T**ime-to-value
- **H**orizon flexibility

If you can't answer "yes" to at least three of these five questions with clear evidence, don't adopt it yet. Even with three yeses, a hard "no" on any single dimension means stop and discuss that dimension first. This yes/no version is only the quick gut check: the hard gates, the full 1-5 scorecard, and the decision bands in Sections 2 and 3 make the call. Revisit when your context changes.

### Who This Framework Serves

**Tech Leads & Staff Engineers**: Battling over-engineering and decision fatigue while trying to ship reliably.

**CTOs & Founders**: Controlling cloud spend, reducing talent churn, and ensuring technology choices support business goals.

**Senior Developers**: Needing guard-rails to push back on dogma without seeming obstinate.

**Teams of Any Size**: From solo developers to hundred-person engineering orgs, context-aware decision-making scales.

### How to Use This Document

This isn't a book you read cover-to-cover and shelve. It's a working framework you return to every time you face a technology decision.

- **Part I** gives you the core principle and scoring methodology
- **Part II** expands the dimensions you need to consider
- **Part III** applies WORTH to specific domains (code, architecture, product)
- **Part IV** shows you how to operationalize it in pull requests, ADRs, and team rituals
- **Part V** provides extended case studies, guidance for specific contexts, and AI decisions (Section 17)

**If you're a solo developer or indie hacker**, skip directly to the [Solo Developer / Indie Hacker Mode](#solo-mode) section (embedded in Section 16) for simplified guidance tailored to one-person projects.

**If you're facing an AI decision** (an AI tool, an AI feature, or an agent), read Sections 2 and 3 first, then [Section 17](#ai-decisions).

Start with the core framework in Section 2. Use the scorecard in Section 3 on your next decision. Refer back to domain-specific chapters as needed.

---

<a name="core-framework"></a>
## 2. The WORTH Principle: Core Framework

WORTH is a pragmatic litmus test for every architectural, process, or tooling choice.

### The Five Questions

| Letter | Question | Signals It's NOT Worth It |
|--------|----------|---------------------------|
| **W – Weigh the problem** | Is the pain concrete, frequent, and business-critical? | Buzz-driven adoption, vague "might need it later" |
| **O – Outcome > overhead** | Will this clearly improve customer or developer outcomes more than it costs in time, money, or complexity? | Long payback period, marginal uplift, hidden ops costs |
| **R – Right-sized for the team** | Can the current team build and run it confidently within working hours? | Reliance on "future hires", hero culture, steep learning curves |
| **T – Time-to-value** | Does it accelerate delivery now or within the quarter you're planning? | Multi-month migrations delaying revenue or feedback |
| **H – Horizon flexibility** | Does it keep sensible options open without locking you into tech or vendors? | Irreversible bets, vendor lock-in, explosive surface area |

### Rule of Thumb

Read the scorecard as a five-dimension profile first and a total second. Three checks, in this order:

1. **Gate check**: every hard gate passes (Section 3, Step 2). A failed gate is a no, whatever the total.
2. **Override check**: any dimension at **1 or 2** stops the decision. Write an **Override:** line naming the dimension and what must change before proceeding.
3. **Total (/25)**, read as zones, not bright lines:

| Total | Verdict | Action |
|-------|---------|--------|
| 20-25 | Strong candidate | Proceed; the gates and the override still apply |
| 18-19 | Worth doing | Proceed with normal caution |
| 15-17 | Borderline | Discuss as a team before deciding |
| 12-14 | Probably not | Find a better-fitting alternative |
| Below 12 | Strong NO | Don't proceed unless an explicit non-WORTH override applies |

**Why the checks matter**:
- The override stops a high total from hiding a fatal weakness: [5,5,5,1,1] = 17 is borderline on the total, but the two 1s stop it
- Neutral on all five, [3,3,3,3,3] = 15, is borderline: no fatal flaw, but no clear win
- A band says how strong the case is, not how fast to roll out: a strong candidate can still start as a pilot with a kill date, and no option goes beyond such a pilot while its O is "break-even until measured"

If a gate fails, fix the cause or drop the option: no pilot starts until every gate is marked Pass. If a 1 or 2 is still unresolved, or the total lands below 15, **don't adopt it yet**. Improve the weak dimension, re-scope it as a smaller, reversible pilot and score that pilot as a new option, or re-evaluate when context changes. A pilot that still scores below 15 runs only under an exception from "When WORTH Doesn't Apply" below, most often as a labeled learning bet.

### Why WORTH Sits Above Other Principles

**KISS** tells you to keep solutions simple once you've decided to build them.

**DRY** avoids duplication inside the chosen solution.

**YAGNI** warns against speculative features.

**WORTH** precedes all of them: it ensures you're even solving the right problem at the right scale before optimizing its internal design.

### W – Weigh the Problem

Start by quantifying the pain you're trying to solve.

**Key Questions**:
- How many people or systems are affected?
- How frequently does this pain occur?
- What's the business impact? (Lost revenue, customer churn, developer hours wasted)
- Is this pain growing or stable?

**Red Flags**:
- "We might need this later" (speculative)
- "Everyone's talking about it" (hype-driven)
- "Best practice" without evidence
- Problem affects one person once

**Good Signals**:
- Daily friction costing measurable hours
- Customer-facing issue impacting retention
- Growing technical debt with clear cost trajectory
- Regulatory or security requirement

**Example**: A team debates adopting GraphQL. The current REST API requires frontend developers to make 5-7 calls to render a dashboard. This happens for every page load across 10,000 daily users. That's real, frequent, measurable pain. ✓

**Counter-example**: A team considers GraphQL because "it's flexible and modern." No evidence of current API causing problems. ✗

### O – Outcome > Overhead

Calculate whether the benefit outweighs the total cost.

**Key Questions**:
- What metric improves? (Speed, quality, cost, developer experience)
- By how much?
- What's the implementation cost? (Engineering hours, dollars, cognitive load)
- What's the ongoing cost? (Maintenance, operations, training)

**Red Flags**:
- Payback period > 1 year in a fast-changing environment
- Marginal improvements (5% faster build when builds take 30 seconds)
- Hidden operational costs not factored in
- "Hard to quantify" benefits without any proxy metric

**Good Signals**:
- Clear ROI calculation
- Total cost of ownership (TCO) fully mapped
- Net improvement in DORA metrics (deployment frequency, change lead time, failed deployment recovery time, change fail rate, deployment rework rate)

**Example**: Migrating to a PaaS costs `$400/month` but saves 20 developer hours/month on infrastructure management. At `$100/hour` blended rate, that's `$2,000` in value for `$400` cost. Clear win. ✓

**Regional Cost Note**: The examples use approximate mid-2020s US market rates. The multipliers below are the author's planning assumptions, not survey data. Adjust for your region:
- **US/Western Europe**: 1.0x (baseline: `$100-150/hour` blended rate)
- **SF Bay Area/NYC**: 1.5x (`$150-225/hour`)
- **Eastern Europe**: 0.5x (`$50-75/hour`)
- **India/South Asia**: 0.3x (`$30-45/hour`)
- **Latin America**: 0.4x (`$40-60/hour`)

Recalculate ROI using your actual rates to ensure accurate WORTH scoring.

**Counter-example**: Adding Kubernetes to reduce deployment time from 5 minutes to 3 minutes, costing 2 weeks of setup and ongoing complexity. Marginal gain, huge overhead. ✗

### R – Right-Sized for the Team

Assess whether your team can realistically build and maintain this.

**Key Questions**:
- Do at least two people on the team already understand this technology?
- Can the team operate it without heroic effort or on-call burnout?
- Does it fit within the team's cognitive load budget?
- Can you hire or train for gaps within your timeline?

**Red Flags**:
- "We'll hire someone who knows this"
- Single expert (bus factor of 1)
- Requires learning 3+ new technologies simultaneously
- Team already maxed out on complexity

**Good Signals**:
- Multiple team members have experience
- Technology fits existing mental models
- Clear documentation and community support
- Graceful learning curve

**Example**: A team of experienced Java developers considers Spring Boot vs. Micronaut. Both fit their skill set and mental models. ✓

**Counter-example**: Same Java team considers rewriting everything in Rust to "learn something new" despite zero Rust experience and aggressive ship dates. ✗

### T – Time-to-Value

Determine if the investment delivers value within your planning horizon.

**Key Questions**:
- When do we see the first benefit?
- Does this accelerate or delay current commitments?
- What's the opportunity cost of not working on other features?
- Can we stage the rollout to get partial value sooner?

**Red Flags**:
- Multi-month migration blocking all feature work
- Benefit only appears after full completion (no incremental wins)
- Faster alternatives exist but are dismissed as "not best practice"
- Timeline keeps extending due to unforeseen complexity

**Good Signals**:
- Value delivered within current quarter
- Can be piloted with subset of users/services
- Improves team velocity starting now
- Clear milestones with measurable progress

**Example**: Adopting feature flags delivers value immediately: first feature uses flags for safe rollout within one sprint. ✓

**Counter-example**: Rewriting the entire app to microservices with no customer value until completion 8 months from now. ✗

### H – Horizon Flexibility

Evaluate whether this choice keeps options open or creates lock-in.

**Key Questions**:
- How easy is it to reverse this decision?
- Does this vendor or technology have a healthy ecosystem?
- Can we migrate away without rewriting everything?
- Does this create an explosion of surface area (new services, configs, dependencies)?

**Red Flags**:
- Proprietary technology with no export path
- Architecture that requires rewrite to change
- Vendor with declining market share or community
- Creates dependencies across entire codebase

**Good Signals**:
- Standards-based approach (SQL, HTTP, JSON)
- Clean interfaces that allow swapping implementations
- Strong community, multiple vendors
- Reversible via feature flags or strangler pattern

**Example**: Using PostgreSQL (open standard, multiple hosts, dump/restore works everywhere) vs. proprietary cloud database with custom query language. PostgreSQL wins on flexibility. ✓

**Counter-example**: Building entire architecture around a startup vendor's beta SDK with no export functionality. ✗

### When WORTH Doesn't Apply

WORTH is built for operational adoption decisions: tools, platforms, architecture, process. Handle these differently:

- **Pure research and deliberate innovation bets**: These score low by design (no proven problem, delayed value). Decide them explicitly as time-boxed bets with their own budget; don't torture the scorecard into approving them. Label each bet with its real score, a budget cap, the question it should answer, and an end date ("this scores 12; we're buying learning"). A bet that drifts into production without a new scorecard is no longer a learning bet.
- **Ethical and values questions**: "Should we sell customer data?" or "May a machine screen out applicants before a person reads them?" aren't WORTH scores. WORTH scores feasibility and return; it doesn't tell you which outcomes to want. Leadership answers values questions first; WORTH then scores what remains.
- **Competitive threats**: A rival justifies overriding a low score only when the threat is existential and you can name the rival's capability and the date it hurts you. Write down why it's existential and what the scorecard said without the exemption. If you can't name them, it's fear, and fear gets no exemption.
- **Emergencies**: During an incident or active security breach, act first. Run WORTH later on the permanent fix.
- **One-way strategic bets**: Company-defining choices (acquisitions, platform pivots) need deeper analysis than a five-question scorecard. WORTH informs the conversation; it doesn't decide it.
- **Trivial, cheap, reversible choices**: If it takes an hour and reverts in a minute, just decide.

---

<a name="scorecard"></a>
## 3. Decision Scorecard & Methodology

### The Six-Step Scorecard

Use this lightweight process for any technology, pattern, or practice decision.

**Step 1: State the Issue (One Sentence)**

Force clarity by writing a single sentence problem statement.

Example: "Release lead-time increased from 3 days to 9 days after adding the reporting service."

**Step 2: Check the Hard Gates (Before Scoring)**

Some constraints are not dimensions. Mark each gate Pass, Fail, or Needs review for every option before you score it. A failed gate is a no, not a low score: never average it into the total.

| Gate | Fails when |
|------|------------|
| Legal / regulatory | It breaks a law, regulation, or license obligation that binds you (HIPAA, GDPR/LGPD, PCI DSS, the EU AI Act, and so on) |
| Security | It adds a vulnerability you can't accept or mitigate |
| Privacy / data residency | Data goes, or is kept, where it isn't permitted |
| Safety | It creates an unacceptable risk to people |
| Irreversible lock-in beyond tolerance | Being wrong would be unrecoverable: the H dimension promoted to a gate when the stakes are existential |
| Procurement viability | It can't actually be bought, approved, or operated in your context |
| Accountability (AI decisions) | No named business owner answers for the AI's output, or an AI claim you make can't be substantiated |

Each gate has a named owner, listed as Gate signers in the ADR template below: legal signs the legal gate, security the security gate, privacy the privacy gate, an accountable executive the safety gate, architecture the lock-in gate, procurement the procurement gate, and the business owner the AI accountability gate. Only the owning function marks a gate Pass, with a name and a date; anyone else marks it *Needs review* and escalates. A scorecard for an option still at *Needs review* shows only its merit case: no total carries an option past its gate.

**For AI decisions**, security signs whenever an AI tool reads untrusted content (customer messages, outsiders' issues, resumes, supplier emails), and AI sign-offs expire: a regulatory date, new vendor terms, or a ruling can flip one, so date every sign-off ([Section 17](#ai-decisions)).

**Step 3: Score Each Dimension (1-5)**

| Dimension | Score | Evidence |
|-----------|-------|----------|
| W – Weigh the problem | 1-5 | How big, frequent, and critical is the pain? |
| O – Outcome > overhead | 1-5 | Does benefit clearly exceed cost? |
| R – Right-sized team | 1-5 | Can we build and run this confidently? |
| T – Time-to-value | 1-5 | Do we see value within current quarter? |
| H – Horizon flexibility | 1-5 | Can we reverse or adapt this choice? |

**Scoring Guide**:
- **5**: Strong yes, clear evidence
- **4**: Probably yes, good indicators
- **3**: Neutral or mixed
- **2**: Probably no, concerns outweigh benefits
- **1**: Clear no, red flags present

**Anchors**: the guide above is the short form; these anchors are the reference. Score each option against them, not against the other options. For unknowns (`U`) and benefits nobody has put a number on yet (O at 3), see Section 5, Step 3.

| Score | W: the problem | O: ROI over 12 months | R: the team | T: value arrives within | H: reversible within |
|-------|----------------|-----------------------|-------------|-------------------------|----------------------|
| 5 | Concrete, frequent, business-critical | Above 300% | Extensive experience, no learning curve | 1 month | 1 week |
| 4 | Concrete and frequent, moderate impact | 100-300% | Relevant experience, 2-3 month learning curve | 3 months | 1 month |
| 3 | Concrete but infrequent, or low impact | 0-100%: roughly break-even | Some experience, 3-6 month learning curve | 6 months | 3 months |
| 2 | Vague or rare, minimal impact | Below 0%: costs exceed benefits | Minimal experience, 6+ month learning curve | 12 months | 6 months |
| 1 | Hypothetical or aesthetic | Large loss: big costs, minimal benefits | No relevant experience; needs hiring or major training | More than 12 months | Effectively irreversible |

**Scoring convention**: when comparing options for the same decision, score **W identically for every option**, including the do-nothing baseline and, for AI decisions, an option without AI. W rates the problem, not the solution; option-specific strengths and weaknesses belong in O, R, T, and H.

**Dimension scoping**: each letter answers one question, and no cost or risk counts twice. Lock-in and exit cost belong to H only, unless the stakes are existential: then lock-in is the Step 2 gate, not a score. When value arrives belongs to T only; O compares benefit with cost, and its 12-month window only sets the period for that comparison. If one fact is lowering two scores, decide which question it answers and score it there once.

**Override line**: any dimension at 1 or 2 gets a sentence starting **Override:** that names the dimension and what must change before proceeding, for example: "Override: R at 2. Proceed only after a second engineer can run it on call." A 1 or 2 is never averaged away. If nothing can fix it, the option is out, or the risk is formally accepted with a named owner and a re-score date.

**Step 4: Calculate Total**

Add the five scores and read the band from the Rule of Thumb in Section 2: 20-25 strong candidate, 18-19 worth doing, 15-17 borderline, 12-14 probably not, below 12 strong NO. The total never overrides a failed gate, a 1 or 2 without its **Override:** line, or an O that is "break-even until measured", which caps the action at a pilot with a kill date.

**Calibration Note**: These bands are author heuristics, not validated benchmarks. They stay the same for every team, so totals stay comparable; context changes which dimensions you scrutinize hardest, never where the bands sit:
- **Startups and small teams**: scrutinize R, T, and H hardest; keep the exit cheap, because you will pivot
- **Large organizations**: demand the most rigor on W and O; longer T horizons are acceptable
- **Regulated domains (fintech, healthcare, government)**: check the hard gates first; H is often decisive; fold compliance and audit costs into O

Never re-score several dimensions after the fact to clear a band; each change needs its own named evidence. Each quarter, check past scores against decision outcomes: if many "proceed" decisions fail, inspect which dimension was over-scored.

**Step 5: Document in ADR**

Record your scoring in an Architecture Decision Record so the rationale is searchable and auditable when context changes.

**Step 6: Set the Review Date and Triggers**

Put a review date on every scorecard: quarterly for fast-moving areas such as AI tooling, 3-6 months for most product and tooling decisions, 6-12 months for infrastructure and architecture. Re-score at once, without waiting for the date, when:
- Scale changes by an order of magnitude (users, traffic, or team size)
- You lose the people who carried the R score
- A vendor is acquired, relicenses, or announces end-of-life for something you depend on
- A new regulation binds you (re-check the hard gates, not just the scores)
- Outcomes miss the success metrics written on the scorecard
- For AI: a model deprecation notice, a price or plan change, a regulatory date, an incident with the same class of tool, or a new general model that might do the task out of the box (Section 17)

Every pilot gets a written success criterion, a go/no-go metric, and a kill date, agreed before the pilot starts.

---

### Worked Examples

*Gates first: in Examples 1-3 every option passes its hard gates, each signed by its owner; Section 17's worked example shows gates at Needs review and Fail.*

#### Example 1: Kubernetes for a 3-Developer Startup

**Problem Statement**: "We need to deploy our app somewhere and want it to scale."

| Dimension | Score | Evidence |
|-----------|-------|----------|
| W – Weigh | 4 | The deployment need is real whatever the tool; the scale need isn't (under 100 users/day on one server) |
| O – Outcome | 2 | Costs exceed benefits: ops overhead buys headroom for 10x traffic we don't have |
| R – Right-sized | 1 | No Kubernetes experience on team; running it needs a hire or major training |
| T – Time-to-value | 2 | Slow to stand up: the MVP slips 3+ weeks, and running it well takes months more; a PaaS ships this week |
| H – Horizon | 4 | Still reasonably reversible: a small app can move off the cluster within a month |

**Total**: 13/25 – Probably not

**Verdict**: ✗ Skip. Use a PaaS (Render, Railway, Heroku) or simple VM with CI/CD.

**Override:** O at 2, R at 1, and T at 2; any one stops Kubernetes here. Re-score at 10,000+ daily active users or when you hire an experienced SRE.

**Review date**: within 6-12 months (Section 3, Step 6, for infrastructure), sooner on either trigger above.

---

#### Example 2: Microservices vs. Modular Monolith (Series B Scale-Up)

**Problem Statement**: "Release collisions between teams slow deployment frequency."

| Dimension | Score | Evidence |
|-----------|-------|----------|
| W – Weigh | 4 | Three teams frequently blocked waiting for others' deploys |
| O – Outcome | 3 | Faster independent deploys, but ops overhead and latency increase |
| R – Right-sized | 3 | Team has two SREs with microservices experience |
| T – Time-to-value | 3 | Migration takes 4-6 months; could start with modules first |
| H – Horizon | 4 | Phased: each extraction can stop or fold back within a month, so no step locks in the rest |

**Total**: 17/25 – Borderline

**Verdict**: Don't migrate yet; discuss as a team. Start with a modular monolith using strong boundaries and module-owned data, and extract 1-2 services only as an evidence-gathering pilot. Measure delivery and operational impact before expanding.

**Kill criteria** (written before the pilot): no rise in deployment frequency for the extracted services by the review date ends the extraction. **Review date**: within 6-12 months (Section 3, Step 6, for architecture).

---

#### Example 3: Custom SQL Abstraction Layer

**Problem Statement**: "We duplicate the same query patterns in five different files."

| Dimension | Score | Evidence |
|-----------|-------|----------|
| W – Weigh | 2 | Annoyance but only affects 2-3 developers occasionally |
| O – Outcome | 3 | Reduces duplication, increases consistency; minimal maintenance |
| R – Right-sized | 5 | Team is SQL-savvy; this is a simple abstraction |
| T – Time-to-value | 5 | Can implement in one day; immediate benefit |
| H – Horizon | 5 | Easy to remove or replace if we change databases |

**Total**: 20/25 – Strong candidate

**Verdict**: ✓ A strong total, but W at 2 still stops it for a discussion first; if it goes ahead, keep it a lightweight pilot. Low cost, raises consistency, reversible in a day.

**Override:** W at 2: the pain is minor, so time-box the work and keep it easy to delete. Drop it the moment it competes with higher-priority work.

**Kill criteria** (written before the pilot): delete it if it takes more than the estimated day or the five files still repeat the same queries. **Review date**: within 3-6 months (Section 3, Step 6).

---

### Embedding the Scorecard in Your Workflow

#### Pull Request Template

Add a "WORTH Assessment" section (the CI check in Section 11 looks for this heading and a filled-in total):

```markdown
## WORTH Assessment (for non-trivial changes)
**Hard gates** (Pass / Fail / Needs review; only the owning function marks Pass, with its name and the date):
Legal/regulatory __ | Security, incl. any AI tool reading untrusted content __ | Privacy/data residency __ | Safety __ | Irreversible lock-in __ | Procurement __ | AI accountability (AI changes only) __
- [ ] New dependency vetted before marking security: still maintained, no known critical vulnerabilities in the pinned version, more than one active maintainer (or an accepted plan if not), compatible license, transitive footprint reviewed

- **W** (__/5) – What problem does this solve? Is it concrete and frequent?
- **O** (__/5) – Does the benefit exceed the complexity added?
- **R** (__/5) – Can the team maintain this without heroic effort?
- **T** (__/5) – Does this accelerate or delay current goals?
- **H** (__/5) – Is this decision reversible or does it create lock-in?

**Total Score**: __/25 → Strong candidate / Worth doing / Borderline / Probably not / Strong NO
**Any score of 1 or 2?** __ If yes, add an **Override:** line: the dimension, what must change before merging (or who owns the accepted risk), and when we re-score.
**Unknowns?** __ Replace unknowns with evidence through a spike or pilot.
```

#### Architecture Decision Record (ADR)

Include WORTH scoring in every ADR:

```markdown
## Context
[Describe the problem]

## Decision Rights
- **Decision owner**: [Person accountable for this ADR]
- **Final decider**: [Person or forum with decision authority]
- **Gate signers**: [Legal, security, privacy, accountable executive (safety), architecture (lock-in), procurement; business owner for AI accountability]

## Hard Gates (before scoring)
| Gate | Status | Signer, date |
|------|--------|--------------|
| Legal / regulatory | Pass / Fail / Needs review | |
| Security | Pass / Fail / Needs review | |
| Privacy / data residency | Pass / Fail / Needs review | |
| Safety | Pass / Fail / Needs review | |
| Irreversible lock-in beyond tolerance | Pass / Fail / Needs review | |
| Procurement viability | Pass / Fail / Needs review | |
| Accountability (AI decisions only) | Pass / Fail / Needs review | |

## WORTH Analysis
| Dimension | Score | Rationale |
|-----------|-------|-----------|
| Weigh | 4 | Daily friction costing 5 dev hours/week |
| Outcome | 4 | Saves about `$2k/mo` in developer time; costs `$2k` setup + `$500/mo` |
| Right-sized | 3 | Two engineers have experience; third will train |
| Time-to-value | 5 | First value in 2 weeks; full ROI in 3 months |
| Horizon | 5 | Standards-based; multiple vendors available |

**Total**: 21/25 → **Strong candidate** (gates passed; no dimension at 1 or 2)

**Override:** [Any 1 or 2: the dimension, what must change before proceeding (or who owns the accepted risk), and when we re-score]

**Options considered**: [At least two alternatives, including do nothing, with their totals]

## Decision
[Record the choice]

**Unresolved dissent**: [Concerns that remain after the decision]

## Consequences
[Expected outcomes]

**Success metrics**: [What we measure, and the result that counts as success]

**Kill criteria** (pilots): [Go/no-go metric and kill date, agreed before the pilot starts]

## Review Date and Triggers
Review on: [Date; see Section 3, Step 6]

Re-score early if: team shrinks below 2 experienced operators, cost increases 2x, or migration takes >4 weeks
```

For AI decisions, append the AI Addendum from [Section 17](#ai-decisions).

#### Quarterly Operations Review

Rotate a "devil's advocate" who re-scores one live component each quarter and proposes simplification if it drops below 15 or any dimension falls to 1 or 2. Section 11 shows the full ritual, including a worked retro example. This catches accumulating complexity and prevents zombie technologies.

---

# Part II: Deep Dive

<a name="expanded-dimensions"></a>
## 4. Expanded Dimensions

The five core WORTH questions are enough for most decisions. For high-stakes choices, expand your evaluation with these additional lenses. Compliance & Security is the exception: its hard gates apply to every decision (Section 3, Step 2).

### Cost of Delay & ROI Horizon

A choice can be "cheap" but still catastrophic if it postpones revenue or feedback.

**Why It Matters**: Every week you delay shipping is lost learning, lost revenue, and lost market position. Quantifying this forces trade-offs into the open.

**How to Measure**:
- Express benefit in dollars per month shipped earlier
- Use a simple approximation: Cost of Delay ≈ (Weekly Value) × (Weeks Delayed); see Reinertsen's *Principles of Product Development Flow* for the rigorous treatment
- Plot cash flow over 12-24 months to visualize payback

**Example**: A feature migration to microservices delays shipping by 3 months. If each month of delay costs `$50k` in lost revenue, the migration carries a hidden `$150k` cost even if engineering time is "free."

**Diagnostic Questions**:
- Can we express benefit in `$/month`?
- What's the opportunity cost of delaying other work?
- Is the ROI horizon longer than our funding runway?

---

### Cognitive Load & Team Topology

High-skill tools are fine until cognitive load crosses what a single team can safely juggle.

**Why It Matters**: Teams have limited mental bandwidth. Each new technology, pattern, or service consumes attention that could go toward delivering features.

**How to Measure**:
- Use Team Topologies' four team types (stream-aligned, platform, enabling, complicated-subsystem) and three interaction modes (collaboration, X-as-a-Service, facilitating) to identify where cognitive load concentrates ([Fowler](https://martinfowler.com/bliki/TeamTopologies.html))
- Count technologies in the stack (languages, frameworks, databases, queues, deployment platforms)
- Survey developers: "Do you feel overloaded?" (1-5 scale)

**Example**: A team already managing React, Node.js, PostgreSQL, Redis, and AWS considers adding Kafka. If they're already near cognitive limits, adding Kafka might tip them into constant firefighting.

**Diagnostic Questions**:
- Would this push the team above its cognitive budget?
- Can we remove something else to make room?
- Is the team already in "survival mode" on current stack?

**Further Reading**:
- *Team Topologies* by Matthew Skelton and Manuel Pais
- Martin Fowler's *Team Topologies* bliki entry, which centers on team cognitive load: [martinfowler.com/bliki/TeamTopologies.html](https://martinfowler.com/bliki/TeamTopologies.html)

---

### Delivery Performance (DORA Metrics)

If a decision lowers deployment frequency or raises change fail rate, beware: you may have traded adaptability for "best practice" theater.

**Why It Matters**: DORA research associates software-delivery performance metrics with organizational outcomes. Use its five current metrics, and check current DORA reports before treating any benchmark as canonical.

**How to Measure**:
- Run an A/B comparison: pilot branch vs. baseline
- Track metrics for 2-4 sprints before and after adoption
- Watch for regression in any of the delivery metrics below

**DORA's Five Delivery Metrics (as of November 2026)**:

| Metric | What It Measures | Example High-Performance Signal |
|--------|------------------|-------------------|
| Deployment Frequency | How often you deploy to production | Frequent, low-friction production deploys |
| Change Lead Time | Time from commit to production | Short, predictable flow from change to release |
| Failed Deployment Recovery Time | Time to recover from a deployment that fails and needs immediate intervention | Fast recovery after a failed change |
| Change Fail Rate | % of deployments causing incidents | Low rate of release-caused incidents |
| Deployment Rework Rate | % of deployments that are unplanned and made because of a production incident | Few unplanned fix-up deployments |

*These replace the traditional Four Keys: in 2023 DORA renamed and redefined Time to Restore Service as Failed Deployment Recovery Time, which counts only recovery from failures caused by a change to production, and in 2024 it added Deployment Rework Rate ([dora.dev](https://dora.dev/insights/dora-metrics-history/)).*

**Note for Non-Continuous Teams**: DORA metrics reflect continuous delivery practices. If your team operates on planned release cycles (quarterly enterprise software, regulated deployments), substitute relevant metrics:
- **Release Quality**: Defects per release, customer-reported issues
- **Feature Velocity**: Story points or features delivered per cycle
- **Customer Satisfaction**: NPS, support ticket volume
- **Incident Rate**: Production issues per release

The principle remains: measure delivery effectiveness in your context.

**AI and delivery (as of November 2026)**: Measure delivery, not belief. DORA's 2024 survey of nearly 3,000 professionals associated each 25% rise in AI adoption with an estimated 1.5% drop in delivery throughput and a 7.2% drop in delivery stability ([2024 report](https://services.google.com/fh/files/misc/2024_final_dora_report.pdf)). In its 2025 survey of 4,867 professionals, 90% used AI and more than 80% believed it had raised their productivity; AI adoption was now linked to higher throughput but still to higher instability, and DORA calls AI an amplifier of an organization's existing strengths and dysfunctions ([2025 report](https://services.google.com/fh/files/misc/2025_state_of_ai_assisted_software_development.pdf)).

Both are correlational surveys, and Google, which co-publishes them, sells AI tools. For an AI tool, pilot against a comparison team and compare throughput (change lead time, deployment frequency, failed deployment recovery time) and stability (change fail rate, deployment rework rate, pull-request size), not self-reported speed.

**Example**: A team adopting microservices sees deployment frequency drop from 5/week to 1/week because coordinated releases require more planning. The architecture might be "better" in theory but worse in practice.

**Diagnostic Questions**:
- How will this affect our DORA metrics?
- Can we pilot on one service and measure before scaling?
- Are we optimizing for scalability we don't need while sacrificing speed?

**Further Reading**:
- *Accelerate* by Nicole Forsgren, Jez Humble, and Gene Kim
- DORA's annual reports at [dora.dev/research](https://dora.dev/research/) (renamed in 2025 from *Accelerate State of DevOps* to *State of AI-assisted Software Development*; the 2025 report is the latest annual edition as of November 2026)

---

### Risk & Uncertainty (Real Options Thinking)

Model upside as options, not certainties. Real Options thinking values flexibility over dogma and lets you stage investments.

**Why It Matters**: Technology decisions under uncertainty are options, not obligations. You can defer commitment, pilot, and learn before full investment.

**How to Apply**:
- Treat decisions as call options with an "exercise price" (cost to fully adopt)
- Value the right to defer until you have more information
- Stage investments: small pilot, partial rollout, full adoption

**Example**: Instead of rewriting the entire app in a new framework, extract one module, ship it, measure results, then decide whether to continue.

**Diagnostic Questions**:
- Can we defer this decision via feature flags or pilot lanes?
- What would we learn from a small experiment first?
- What's the cost of waiting 3 months for more data?

**Further Reading**:
- "Real Options" by Tom Copeland and Vladimir Antikarov
- Chris Matts' work on Real Options in software

---

### Strategic Context (Wardley Mapping)

Wardley Mapping keeps you from reinventing commodities (e.g., bespoke logging) while spotting areas worth in-house mastery.

**Why It Matters**: Not all components deserve equal investment. Commodities should be bought; differentiators should be built and mastered.

**How to Use**:
- Map your value chain from user needs to underlying components
- Place each component on the evolution axis: Genesis → Custom → Product → Commodity
- Invest in differentiators; buy or use open-source commodities

**Example**: Custom logging is a commodity: use Datadog or ELK. Your unique pricing algorithm is a differentiator: build and optimize it.

**Diagnostic Questions**:
- Is this component a differentiator or a utility?
- Are we building what we should buy?
- Are we outsourcing what gives us competitive advantage?

**Further Reading**:
- *Wardley Maps* by Simon Wardley
- [learnwardleymapping.com](https://learnwardleymapping.com)

---

### Bias Check

Sunk-cost fallacy, resume-driven development, or "that's how FAANG does it" can skew scoring. Force a devil's-advocate review to neutralize bias.

**Why It Matters**: Cognitive biases make us defend past investments, chase novelty, or copy others without understanding context.

**Common Biases**:
- **Sunk-cost fallacy**: "We've already spent 2 weeks on this, can't stop now"
- **Resume-driven development**: "I want to learn Rust, so let's use Rust"
- **Conformity bias**: "Google uses it, so we should too"
- **Availability bias**: "I just read about this, so it must be important"
- **Confirmation bias**: collecting only the evidence that supports the favored option
- **Groupthink**: "Nobody objected" (no recorded dissent)
- **Automation bias** (AI decisions): accepting AI output with less scrutiny than a person's work; "everyone feels faster" is not a measurement
- **Fear of falling behind**: "A competitor relaunched AI-first, so every department ships something by Q4"

**How to Counter**:
- Ask: "If another team proposed this, would we still agree?"
- Rotate the "no" voice: assign someone to argue against the decision
- Separate "what I want to learn" from "what the business needs"
- Score independently, then discuss, so the first number spoken doesn't anchor the rest
- Run a pre-mortem: assume it failed a year from now and list why
- Record unresolved dissent on the scorecard, not just the risks the majority accepts

**Diagnostic Questions**:
- What bias might be influencing this decision?
- Would we make this choice if starting from scratch today?
- Are we justifying a conclusion or genuinely evaluating?
- Can we state the problem without naming a technology (or the word AI)?

---

### Compliance & Security

These are hard gates, not an optional lens: check them for every option before scoring (Section 3, Step 2). The obligations below help you see which gates bind you. Regulated domains may hard-block certain stacks. Map legal, security, privacy, and procurement constraints early or your WORTH score is fiction.

**Why It Matters**: Legal requirements trump architecture preferences. Compliance failures carry fines, shutdowns, and reputation damage.

**Common obligations and control frameworks**:
- **HIPAA** (covered health-data contexts): privacy, security, breach-notification, and business-associate obligations where applicable
- **PCI DSS** (payment-card environments): tokenization, network segmentation, logging, and scope control
- **GDPR/LGPD** (personal data): lawful basis, minimization, rights handling, transfer safeguards, and retention controls
- **SOC 2** (audit framework, not a law): controls for security, availability, confidentiality, processing integrity, or privacy depending on scope
- **EU AI Act** (AI systems used in or placed on the EU market): as amended in 2026, prohibitions and AI-literacy duties have applied since February 2, 2025, duties for general-purpose AI model providers since August 2, 2025, and transparency duties since August 2, 2026. New prohibitions apply from December 2, 2026, and high-risk duties from December 2, 2027 for Annex III uses (some hiring tools, for example) and from August 2, 2028 for AI embedded in regulated products ([Article 113](https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-113), as of November 2026)

**Diagnostic Questions**:
- Which regulations apply to our domain?
- Can this technology support the controls we need after proper configuration, contracts, process, and audit review?
- What's the audit burden of this choice?
- Does any AI tool in this option read untrusted content (customer messages, outsiders' issues, resumes, supplier documents)? Then security signs a gate before scoring.

---

### Op-Ex vs. Cap-Ex (Total Cost of Ownership)

TCO often dwarfs initial build effort. Factor on-call load, infra cost curves, licensing, and vendor escalations.

**Why It Matters**: A "free" open-source tool that requires 40 hours/month of maintenance costs `$48k/year` at a `$100/hour` blended rate.

**Components of TCO**:
- **Capital**: Initial development, training, setup
- **Operational**: Infrastructure, licenses, support contracts
- **Maintenance**: Patches, upgrades, bug fixes
- **Opportunity Cost**: Developer time not spent on features

**How to Calculate**:
- Project 24-month cash flow for "run" costs
- Include hidden costs: on-call, debugging, tribal knowledge loss
- Compare against alternatives (build vs. buy)

**Example assumption**: Kubernetes costs zero to download but may require dedicated SRE time. A PaaS may cost more per unit but need far less operations attention. Use your actual compensation, vendor pricing, support, and on-call costs before comparing TCO.

---

### Environmental & Social Impact

Energy footprint or community support can be differentiators in hiring and PR.

**Why It Matters**: Green tech attracts talent and customers. Open-source contributions build community goodwill and reduce vendor lock-in.

**Considerations**:
- Data center energy and grid carbon: in Google Cloud's 2025 region data, grid carbon intensity ran from 11 gCO2eq/kWh (Montréal) to 641 (Johannesburg), and within the US from 98 (Oregon) to 579 (South Carolina) ([Google Cloud region carbon data](https://github.com/GoogleCloudPlatform/region-carbon-info/blob/main/data/yearly/2025.csv))
- AI workloads: a simple text query typically uses less electricity than running a television for the same period, but video generation, reasoning, and agentic tasks can use hundreds or thousands of times more energy per query ([IEA, Key Questions on Energy and AI, April 2026](https://iea.blob.core.windows.net/assets/3179f7f8-01f6-4dd6-bffa-c9f7b73f1dc9/KeyQuestionsonEnergyandAI.pdf))
- Open-source sustainability (are we giving back or just taking?)
- Worker conditions (are gig workers treated fairly?)

**Diagnostic Questions**:
- What's the estimated kWh/month?
- Is the open-source community healthy?
- Does this align with company values?

---

### Market Timing & Competitive Landscape

Early adoption can yield moat-building knowledge or saddle you with a dying ecosystem.

**Why It Matters**: Bleeding-edge technology can become an asset (early expertise) or a liability (abandoned by vendors).

**How to Assess**:
- Check the Gartner Hype Cycle or the Thoughtworks Technology Radar
- Dated example: in January 2026 Gartner said "AI is in the Trough of Disillusionment throughout 2026" ([Gartner](https://www.gartner.com/en/newsroom/press-releases/2026-1-15-gartner-says-worldwide-ai-spending-will-total-2-point-5-trillion-dollars-in-2026)). An analyst placement is context, not evidence for your use case.
- Monitor job posting volume (proxy for ecosystem health)
- Evaluate community momentum (GitHub stars, commits, issues closed)

**Diagnostic Questions**:
- Is this technology in the "trough of disillusionment" or climbing toward mainstream?
- Will we build competitive advantage from early mastery?
- Is the ecosystem growing or dying?

---

### The Extended WORTH Canvas

For high-stakes decisions, use this expanded canvas:

```markdown
## Problem Statement
[One sentence + cost of delay]

## Options Considered
1. Baseline (do nothing or minimal change)
2. Incremental tweak
3. Ambitious leap

## Hard Gates (before scoring; Pass / Fail / Needs review, signed and dated by each gate's owner)
Legal __ | Security __ | Privacy/data residency __ | Safety __ | Irreversible lock-in __ | Procurement __ | AI accountability (AI decisions) __

## Scoring Table
| Dimension | Option 1 | Option 2 | Option 3 | Scrutiny |
|-----------|----------|----------|----------|----------|
| W – Weigh | 4 | 4 | 4 | High |
| O – Outcome | 2 | 4 | 3 | High |
| R – Right-sized | 5 | 4 | 2 | High |
| T – Time-to-value | 5 | 3 | 1 | High |
| H – Horizon | 4 | 4 | 2 | Medium |
| **Core WORTH** | **20** | **19** | **12** | Unweighted |
| **Band** | Strong candidate | Worth doing | Probably not | |
| Cost of Delay | 4 | 3 | 1 | Discussion lens |
| Cognitive Load | 5 | 3 | 1 | Discussion lens |
| Delivery Impact | 3 | 4 | 2 | Discussion lens |
| Risk | 5 | 3 | 2 | Discussion lens |
| Strategic Fit | 3 | 4 | 5 | Discussion lens |

*W is identical across options: it rates the problem, not the solution. Scrutiny levels here are illustrative: set them for your context (Section 3, Calibration Note).*

**Override:** Option 1's O at 2; Option 3's R at 2, T at 1, and H at 2. Name what must change for each, or drop the option.

## Bias Check
- Option 1: Low risk might mean stagnation
- Option 2: Balanced but requires discipline
- Option 3: Exciting but high failure risk
- Unresolved dissent: [Concerns that remain after the decision]

## Decision
[Chosen option + rationale]

## Success Metrics
[What we measure, and the result that counts as success]

## Kill Criteria
Revert if: [Specific measurable conditions]

## Review Date
[Per Section 3, Step 6; auto-remind]
```

---

<a name="real-world-application"></a>
## 5. Real-World Application

### A Cookbook Workflow

This process works for a one-page ADR or a 50-team program.

#### Step 1: Snapshot the Pain

Write one sentence. Make it numeric if possible.

**Example**: "Release lead-time ballooned from 3 days to 9 days after adding the reporting service."

#### Step 2: Generate Three Options Minimum

Always consider baseline, incremental, and ambitious approaches.

| Label | Description | Estimated Effort |
|-------|-------------|------------------|
| A – Baseline | Keep monolith, add read-replica database | 2 weeks |
| B – Incremental | Extract reports into separate process + queue | 6 weeks |
| C – Ambitious | Full microservice split + Kubernetes autoscaling | 12 weeks |

#### Step 3: Score Every Option Across WORTH + Extended Lenses

Check the hard gates for each option first (Section 3, Step 2). Then score each option against the 1-5 anchors (Section 3, Step 3). Mark unknowns as `U`, not as a number. A `U` means "not enough evidence for adoption"; run a spike or pilot to replace it with evidence. Score O on your own cost and benefit numbers, estimates included: a 5 needs ROI above 300% on a measurable benefit and well-defined costs. When a benefit is plausible but nobody has put a number on it yet, or the evidence points both ways, score O at 3 ("break-even until measured") and pilot with a kill date. Keep `U` for facts nobody can even estimate yet.

**Gates**: all three options pass every gate, each signed by its owner, so the scores below decide.

| Dimension | Option A | Option B | Option C |
|-----------|----------|----------|----------|
| W – Weigh | 4 | 4 | 4 |
| O – Outcome | 2 | 4 | 3 |
| R – Right-sized | 5 | 4 | 2 |
| T – Time-to-value | 5 | 4 | 4 |
| H – Horizon | 5 | 4 | 2 |
| **Core Total** | **21** | **20** | **15** |
| **Band** | Strong candidate | Strong candidate | Borderline |

#### Step 4: Highlight Biggest Sensitivity

Flag critical assumptions.

**Example**: Option C only becomes viable if we hire two SREs in the next 30 days (unlikely). Mark as red flag.

#### Step 5: Decide + Set Kill Switch

Make the call and define failure conditions.

**Decision**: Pick Option B (extract reports into separate process). B (20/25, strong candidate) is the highest-scoring option that actually addresses the problem; start it as a pilot with the kill switch below.

**Override:** A's O at 2. A totals higher (21/25), but a read replica speeds queries without decoupling releases, and the stated pain is release lead time, not query speed; nothing fixes that, so A is out. C's R at 2 and H at 2 stop it too.

**Kill Switch**: If change lead time stays above 5 days after 2 sprints, fold the reports back into the monolith and revisit.

#### Step 6: Calendar a Review

Set an auto-reminder in your ADR system for the review date (Section 3, Step 6: 6-12 months for architecture) to revisit the score with new data.

---

<a name="practical-examples"></a>
## 6. Practical Examples

*Except where a specific company is named and publicly documented (such as Amazon Prime Video below), the cases and figures in this document are illustrative composites of common patterns, not measurements from a single named organization.*

### Case 1: Three-Developer Bootstrapped SaaS Debates Kubernetes

*This revisits Example 1 from Section 3, now with the business context attached.*

**Context**:
- €300/month budget
- No SRE on team
- Goal: Revenue in 90 days

**WORTH Outcome**: Not worth it

**Rationale**:
- **W = 4**: The deployment need is real (revenue in 90 days); the scale need isn't (< 100 users/day)
- **O = 2**: Costs exceed benefits: ops overhead eats the budget for scale we don't need
- **R = 1**: No k8s experience; running it needs a hire or major training
- **T = 2**: Slow to stand up: shipping slips a month, and running it well takes months more
- **H = 4**: Still reasonably reversible: the app can move off within a month

**Total**: 13/25 – Probably not

**Verdict**: ✗ Skip. Use a PaaS (Railway, Render, Heroku). k8s adds 4+ weeks with zero customer value.

**Override:** O at 2, R at 1, and T at 2; any one stops Kubernetes here. Re-evaluate at 10,000+ DAU or when hiring an experienced SRE.

**Gates and review date**: as in Example 1.

**Illustrative path**: Deploy on a PaaS, ship the MVP quickly, and revisit infrastructure only after deployment, reliability, or cost pain becomes concrete.

---

### Case 2: Fintech Scale-Up (Series C) Considers Microservices

**Context**:
- 40 developers across 4 teams
- PCI compliance requirements
- Release collisions weekly
- Current: Modular monolith

**WORTH Outcome**: Full migration is borderline, and its R and T stop it; pilot the staged extraction instead

**Gates** (before scoring): both paths pass every gate, PCI DSS included, each signed by its owner.

**Rationale** (scoring the full 10+ service migration):
- **W = 4**: Real pain, teams blocking each other
- **O = 3**: Faster scaling, but ops overhead increases
- **R = 2**: Two SREs on staff, but running 10+ services exceeds today's platform capacity
- **T = 2**: Full migration takes 8-12 months and disrupts every team along the way
- **H = 4**: Phased: each extraction can stop or fold back within a month, so no step locks in the rest

**Total**: 15/25 – Borderline

**Verdict**: ✗ for full microservices. But the pain is real (W=4), so pilot the hybrid path: extract the strongest bounded context first, measure delivery and operational impact, and expand only with evidence. (Case Study 2 in Section 15 scores this same composite option-by-option; the staged extraction lands at 20/25, a strong candidate that still starts as a pilot.)

**Override:** R at 2 and T at 2. Full migration waits until the platform can run 10+ services and value can arrive in stages; the staged extraction below is that path.

**Kill criteria** (written before the pilot): no deployment-frequency or lead-time gain after the payments service's 3-month run, or a sustained rise in change fail rate, ends the extraction. **Review date**: within 6-12 months (Section 3, Step 6, for architecture).

**Illustrative pilot path**:
1. Extracted payments service (high isolation, PCI boundary)
2. Ran for 3 months, measured deployment frequency (+30%)
3. Extracted auth service
4. Kept reporting and admin in monolith (low change rate, high coupling)

**Result**: Hybrid architecture. Teams ship independently where it matters. Avoided microservices sprawl.

---

### Case 3: Amazon Prime Video's Targeted Service Consolidation

**Context**: Amazon reported that one Prime Video audio/video monitoring workload reduced infrastructure cost by over 90% after consolidating a distributed serverless design into a single-process service running on EC2/ECS. The original Prime Video Tech post is no longer online; InfoQ's May 2023 summary is a reliable secondary reference ([InfoQ](https://www.infoq.com/news/2023/05/prime-ec2-ecs-saves-costs/)).

**Why It's a Key Example**: This was **not** an abandonment of microservices. It illustrates that even at massive scale, architectural choices are component-specific, not all-or-nothing dogmas.

**Original Problem**: Needed to scale quality monitoring across thousands of streams.

**Initial Distributed Approach**:
- Split monitoring into multiple distributed components
- Used AWS Step Functions for orchestration
- Hit scaling limits and cost overruns for this specific use case

**WORTH Re-evaluation** (scores are the author's retrospective interpretation of the public write-up; W is the same for both designs because both address the same problem):

| Dimension | Distributed design | Consolidated single process |
|-----------|--------------------|-----------------------------|
| W – Weigh | 4: monitoring every stream for quality issues is a real, business-critical problem | 4: same problem |
| O – Outcome | 2: its cost and operational complexity outweighed the benefit | 5: dramatically lower cost, same capability |
| R – Right-sized | 3: the team could operate it, but painfully | 5: simpler to operate and debug |
| T – Time-to-value | 2: it handled only a small fraction of the expected load, so full-scale value had to wait for a redesign | 5: it addressed the scaling limits, so the full value arrived as soon as it ran |
| H – Horizon | 3: could change, but AWS deeply embedded | 4: still cloud-based, but simpler to migrate |
| **Total** | **14/25 – Probably not** | **23/25 – Strong candidate** |

**Override:** O and T at 2 for the distributed design. It could go on only without the orchestration overhead, which is what the consolidation removed.

**Reported change**: The monitoring workload was consolidated into a single process while keeping logical module boundaries. Amazon reported infrastructure cost reduction of over 90%.

**Lesson**: **Revisit WORTH scores regularly** (on the review date set in Section 3, Step 6, and at once when a trigger fires). Scale and cost assumptions can flip, making previously "correct" architecture wrong. Also: architectural decisions are component-specific, not all-or-nothing. You can consolidate one service while keeping others distributed.

**Further Reading**:
- Prime Video engineering write-up, "Scaling up the Prime Video audio/video monitoring service and reducing costs by 90%" (2023). The original post is no longer online; a stable secondary summary: [InfoQ, "Prime Video Switched from Serverless to EC2 and ECS to Save Costs" (May 2023)](https://www.infoq.com/news/2023/05/prime-ec2-ecs-saves-costs/)

---

# Part III: Domain Applications

<a name="code-principles"></a>
## 7. Code Principles: Applying WORTH to Clean Code, SOLID, and Object Calisthenics

WORTH is a meta-filter. Before adopting any coding rule (Uncle Bob's Clean Code, Jeff Bay's Object Calisthenics, SOLID principles), run the five questions.

### How WORTH Sits Above Code Principles

| **If the rule says…** | **Ask with WORTH…** | **Worth it when…** | **Not worth it when…** |
|--------------------|-------------------|-------------------|---------------------|
| "Wrap every primitive in a Value Object" (Object Calisthenics #3) | **O – Outcome > overhead**: Does the extra class reveal real behavior or validation? | Domain-heavy codebases (money, dates, IDs) where bugs are expensive | One-off scripts, data processing, throwaway ETL |
| "Functions should do one thing" (Clean Code) | **R – Right-sized**: Do we have time to refactor, and will it help juniors reason faster? | Core modules touched daily, high change rate | Legacy subsystem slated for decommission in 3 months |
| "No 'else' statements" (Object Calisthenics #2) | **H – Horizon flexibility**: Does banning else keep options open, or push us into contorted polymorphism? | Code that thrives on polymorphic behavior (strategy engines) | Simple data flows where a guard clause reads clearer |
| "Depend upon abstractions, not concretions" (SOLID-D) | **W – Weigh the problem**: Are we actually swapping implementations? | Plugin systems, multi-cloud adapters | One cloud, one database, stable for years |

### Key Point

The original rule books give general guidance. WORTH makes sure they're cost-effective in your context, today.

---

### Practical Integration

#### Design Doc Header

```markdown
## Which Principle Are We Invoking?
Clean Code: "Functions should do one thing"

## WORTH Score
- [x] W – Weigh: Functions currently 200+ lines, causing 3 bugs/week
- [x] O – Outcome: Refactor saves 5 hours/week debugging
- [x] R – Right-sized: Team comfortable with Extract Method pattern
- [x] T – Time-to-value: Complete in 3 days
- [ ] H – Horizon: Easier to test and modify going forward (doesn't say how we'd undo it)

**Clear yeses**: 4/5 → quick check passed; confirm with the hard gates, the full 1-5 scorecard, and the Override check before adoption
```

#### Pull Request Bot

If any letter scores 1 or 2, or the total is 17 or below (borderline or worse), tag `#possible-over-engineering` for reviewer attention.

#### Retro Audit

Quarterly, pick one principle adopted last quarter. Re-run WORTH. If score dropped (team changed, scope shifted), schedule removal or simplification.

---

### Outcome: Balanced Engineering

Using WORTH with clean code rules prevents two extremes:

1. **Dogma-driven engineering**: Blindly applying every rule because "the book says so"
2. **Chaos-driven shortcuts**: Rejecting discipline because "we're moving fast"

Instead, every guideline must pay rent in the form of clearer code, faster delivery, or lower defects that outweigh its cognitive and calendar cost.

---

<a name="acronym-analysis"></a>
## 8. Acronym Analysis: Running WORTH Against Common Principles

### STUPID – Six Code Smells to Avoid

**What It Is**: Singleton, Tight-coupling, Untestability, Premature-optimization, Indescriptive naming, Duplication

**WORTH Check**: Outcome > overhead – Will refactoring pay back quickly in fewer bugs or faster changes?

**Worth It**: Core modules touched daily; every hour saved in debugging repays fast

**Not Worth It**: Throwaway migration script that dies next sprint

---

### CUPID – Joyful Code Properties

**What It Is**: Composability, Unix-philosophy, Predictability, Idiomatic, Domain-based

**WORTH Check**: Right-sized for team – Can your devs design tiny, composable units without paralysis?

**Worth It**: Green-field services where micro-features ship independently

**Not Worth It**: Legacy blob where carving out composable pieces freezes delivery

---

### FIRST – Test Quality Principles

**What It Is**: Fast, Independent, Repeatable, Self-validating, Timely

**WORTH Check**: Time-to-value – Do cleaner tests shorten feedback loops enough to justify extra mocks and fixtures?

**Worth It**: High-change domains (pricing engines, rules) where hours matter

**Not Worth It**: Stable ETL job touched twice a year; quick smoke test is cheaper

---

### INVEST – User Story Criteria

**What It Is**: Independent, Negotiable, Valuable, Estimable, Small, Testable

**WORTH Check**: Weigh the problem – Is story churn causing rework or developer idle time?

**Worth It**: Distributed teams sprinting in parallel needing clear boundaries

**Not Worth It**: Solo developer on hobby app; sticky notes beat fine-grained backlog

---

### SMART – Goal-Setting Framework

**What It Is**: Specific, Measurable, Achievable, Relevant, Time-bound

**WORTH Check**: Horizon flexibility – Does locking target and date help focus, or stifle exploration?

**Worth It**: Delivery roadmaps where dates affect marketing or compliance

**Not Worth It**: Early R&D spikes where discovery is the whole point

---

### ACID vs. BASE – Database Guarantees

**What It Is**: ACID (Atomicity, Consistency, Isolation, Durability) vs. BASE (Basically-Available, Soft-state, Eventually-consistent)

**WORTH Check**: Outcome > overhead – Which failure hurts worse: stale reads or blocked writes?

**ACID Worth It**: Banking ledger behind a queue

**BASE Worth It**: Feed counters, analytics dashboards

---

### CAP Theorem – Distributed Systems Trade-offs

**What It Is**: Consistency, Availability, Partition tolerance; under a network partition, you must choose between Consistency and Availability

**WORTH Check**: Risk & uncertainty – Which guarantee keeps revenue safest under network splits?

**Choose AP (Availability + Partition)**: Global SaaS where an up-to-date cart is less critical than staying online

**Choose CP (Consistency + Partition)**: Stock trading engine; tolerate brief downtime for correctness

---

### Using This Table

1. **Name the acronym** you're tempted to enforce
2. **Run WORTH questions** (+ risk lens if stakes high)
3. **If < 3 clear "yes" answers, or any hard "no"**, don't enforce it yet. Either way, the full scorecard (Section 3) makes the call: the yes count is only the quick gut check

**Result**: Every principle earns its keep instead of bloating rituals or code.

---

<a name="architecture-decisions"></a>
## 9. Architecture Decisions: Monolith, Microservices, Infrastructure

### Monolith vs. Microservices

This is the most contentious architecture debate. WORTH cuts through the noise.

#### When Monolith Wins

**WORTH Profile**:
- **W = 2-3**: Single team, low coordination overhead
- **O = 5**: Simple deployment, low ops cost
- **R = 5**: Fits most team skill sets
- **T = 5**: Ships fast, refactors easily
- **H = 4**: Can extract services later via strangler pattern

**Override:** W at 2 holds for every option, since W rates the problem: the coordination pain is too small to justify re-architecting, so the simple monolith stays. Re-score when teams start blocking each other's releases.

**Signals**:
- Team size < 10 developers
- Single deployment cadence acceptable
- Domain boundaries still emerging
- Database transactions simplify consistency

**Example**: Early-stage SaaS, internal tools, MVP validation

#### When Microservices Win

**WORTH Profile**:
- **W = 4-5**: Multiple teams, release collision pain
- **O = 3-4**: Ops overhead justified by independent scaling
- **R = 4-5**: Team has SRE/DevOps expertise
- **T = 3**: Migration has a staged path that creates measurable value before full extraction
- **H = 4**: Phased: each extraction can stop or fold back within a month, though reverting gets expensive once many services exist

Even this best-case profile tops out around 21/25, a strong candidate on the total; microservices still earn adoption through a measured pilot, not through the scorecard alone.

**Signals**:
- 3+ teams needing independent deploy cadence
- Clear bounded contexts (payments, auth, catalog, etc.)
- Different scaling needs per service
- Regulatory boundaries (PCI isolation)

**Example**: Large organizations with many teams and clear domain boundaries, such as 150+ developers across 15 teams. Below about 50 developers, a monolith or modular monolith usually scores higher.

#### The Hybrid Middle Ground: Modular Monolith

**WORTH Profile**: Often scores well because it preserves delivery speed while adding boundaries

**Characteristics**:
- Logical modules with strong boundaries
- Module-owned data boundaries; separate schemas can help, but are not required
- Single deployment artifact
- Can extract services later when justified

**When It's Best**: Often useful for teams that need clearer boundaries but do not yet need independent deployment for every domain.

---

### Infrastructure Decisions

#### Bare VM vs. PaaS vs. Kubernetes

| Option | W (Problem) | O (Outcome) | R (Team) | T (Time) | H (Horizon) | Total |
|--------|-------------|-------------|----------|----------|-------------|-------|
| **Bare VM** | 4 (same problem) | 4 (cheap; scaling is manual) | 5 (everyone knows) | 5 (deploy today) | 4 (standard stack; moves within weeks) | **22** (strong candidate) |
| **PaaS** | 4 (same problem) | 4 (ops hours saved exceed the fee) | 5 (no ops needed) | 5 (deploy today) | 4 (vendor lock manageable) | **22** (strong candidate) |
| **Kubernetes** | 4 (same problem) | 3 (powerful but heavy) | 2 (need expertise) | 2 (weeks to set up, months to run well) | 4 (portable, but leaving takes weeks) | **15** (borderline) |

*Illustrative scores for a small-to-mid product team. W is identical across options: it rates the deployment problem, not the platform.*

**Override:** Kubernetes' R at 2 and T at 2. It waits until the team has dedicated operators and a need simpler platforms can't meet (see below).

**Default for many small-to-mid product teams without special infrastructure constraints**: Start with PaaS or a simple VM. Treat vendor names as examples, not endorsements. Vendor status moves: in February 2026 Heroku said it was "transitioning to a sustaining engineering model focused on stability, security, reliability, and support" and stopped offering Enterprise Account contracts to new customers ([Heroku, February 6, 2026](https://www.heroku.com/blog/an-update-on-heroku/)). Score a vendor's shift away from new features under H; if your procurement requires an enterprise contract, the contract change is a procurement-viability gate, not a score.

**When k8s may become worth evaluating**:
- Many services need orchestration, isolation, and scheduling that simpler platforms cannot provide
- Multi-cloud or portability requirements are concrete, including data, IAM, networking, and operations
- Measured infrastructure cost or utilization justifies the platform investment
- Team includes dedicated operators or platform engineers

---

<a name="dogmas-debunked"></a>
## 10. Common Dogmas Debunked

### Dogma 1: "X Is the Best Language"

**Why People Repeat It**: Tribal pride, blog benchmarks, echo chambers

**Reality Check**: Match runtime, ecosystem, hiring market, and performance envelope to your needs. Switching costs eclipse elegance.

**WORTH Analysis**:

| Situation | Language Choice | Why |
|-----------|----------------|-----|
| ML/Data Science | Python | Ecosystem (NumPy, PyTorch, scikit-learn) |
| Network Services | Go | Concurrency, deployment simplicity |
| Systems Programming | Rust/C++ | Performance; memory safety (Rust) |
| Web Backends | Node.js, Python, Ruby, Go | Team skill + ecosystem fit |

**When It Pays Off**: Domains with clear incumbent (Python for ML, Go for network daemons)

**When It Hurts**: Polyglot hobby projects that quietly become production: ops chaos, talent shortage, fragmented tooling

**WORTH Question**: Does this language choice improve outcomes more than the cost of switching, hiring, and maintaining two stacks?

---

### Dogma 2: "Code Must Follow Every Clean Code Rule"

**Why People Repeat It**: Books sell certainty; lint rules give easy metrics

**Reality Check**: Each rule adds lines, indirection, review time. Measure gains in defect rate or comprehension, not aesthetic purity.

**Spectrum**:
- **Too Rigid**: 300-line class for a 10-line feature because "SRP demands it"
- **Too Loose**: 2000-line God class doing everything
- **Middle Ground**: Modules with clear responsibilities, reasonable cohesion, pragmatic extraction when pain hits

**WORTH Question**: Does this clean code practice reduce bugs or speed changes more than the boilerplate costs?

**When It Pays Off**: Core libraries touched daily where bugs are costly

**When It Hurts**: Short-lived scripts, migration throwaway code, experiments racing deadlines

---

### Dogma 3: "You Need Kubernetes and Microservices from Day One"

**Why People Repeat It**: Copying Big Tech success stories without context

**Reality Check**: k8s ⇒ steep cognitive load; microservices ⇒ extra latency, ops, observability. Ask: "Will a single VM + CI keep us shipping faster for the next 12 months?"

**The "You Are Not Netflix" Principle**:
- Netflix: a large engineering organization, over 325 million paid memberships at the end of 2025 ([Q4 2025 shareholder letter](https://www.sec.gov/Archives/edgar/data/1065280/000106528026000033/ex991_q425.htm)), polyglot services
- You: 3 engineers, 100 users, single product

**WORTH Question**: Does the complexity pay off before we run out of runway?

**When It Pays Off**: Multiple cross-functional teams needing independent deploys, strict uptime SLOs

**When It Hurts**: Three-dev startup chasing product-market fit with < `$1k/month` budget

**What to Do Instead**:
1. Start with PaaS or single server
2. Add modules/boundaries in monolith
3. Extract 1-2 services when pain justifies it
4. Scale infrastructure when revenue supports it

---

### Dogma 4: "Every Team Needs Heavyweight Process"

**Why People Repeat It**: Comfort for managers, audit trails for enterprises

**Reality Check**: Process overhead ∝ team size × coupling. Bureaucracy kills flow in small squads.

**Spectrum**:
- **Too Rigid**: SAFe for 5-person startup (planning ceremonies take longer than building)
- **Too Loose**: No standups, no retrospectives, chaos
- **Middle Ground**: Lightweight rituals scaled to team size

**WORTH Question**: Does this process improve delivery speed or just create overhead?

| Team Size | Appropriate Process |
|-----------|-------------------|
| 1-3 devs | Kanban board, weekly sync, monthly retro |
| 4-10 devs | Scrum-lite: standups, sprints, retros |
| 10-50 devs | Scrum + architecture review + quarterly planning |
| 50+ devs | Shape Up, SAFe, or custom scaled framework |

**When It Pays Off**: Regulated scale-ups (fintech, health) where compliance beats velocity

**When It Hurts**: Seed-stage ventures where market changes faster than sprint review cycle

---

# Part IV: Operationalization

<a name="integration-points"></a>
## 11. Integration Points: Embedding WORTH in Daily Workflow

### Pull Request Template

Add a WORTH delta checkbox for non-trivial changes:

```markdown
## WORTH Delta (for changes adding complexity)

Does this change raise our cognitive load score?
- [ ] Adds new technology/framework
- [ ] Increases coupling between modules
- [ ] Requires new operational expertise

If yes to any, fill in the WORTH Assessment from Section 3, including the hard gates and any **Override:** line.

**WORTH Score**: __/25 (if applicable)
```

---

### CI Pipeline Integration

Fail architecture, infrastructure, and dependency pull requests that skip the WORTH Assessment section from Section 3 or leave its total blank:

<!-- {% raw %} -->
```yaml
# .github/workflows/worth-check.yml
name: worth-check

on:
  pull_request:
    types: [opened, edited, synchronize, labeled, unlabeled]

permissions: {}

jobs:
  verify-worth:
    if: >-
      contains(github.event.pull_request.labels.*.name, 'architecture') ||
      contains(github.event.pull_request.labels.*.name, 'infrastructure') ||
      contains(github.event.pull_request.labels.*.name, 'dependency')
    runs-on: ubuntu-latest
    steps:
      - name: Require a WORTH Assessment section
        env:
          PR_BODY: ${{ github.event.pull_request.body }} # untrusted input: read it only as "$PR_BODY"
        run: |
          if ! grep -qi '^## WORTH Assessment' <<< "$PR_BODY"; then
            echo "::error::Architecture, infrastructure, and dependency PRs need a '## WORTH Assessment' section."
            exit 1
          fi
          if ! grep -qiE '^\*\*Total Score\*\*: *[0-9]+/25' <<< "$PR_BODY"; then
            echo "::error::Fill in the WORTH Assessment: its Total Score line still reads __/25."
            exit 1
          fi
```
<!-- {% endraw %} -->

Keep delivery-metric diffs as a reviewer habit rather than a CI job: post them in the PR description so reviewers see immediate delivery impact.

**Visible Metrics**:
- Build time change
- Test duration change
- Estimated deployment frequency impact

---

### Backlog Refinement Rule

Every story that adds durable complexity should link to its parent WORTH-scored ADR. Size alone is not enough; use this for new technologies, service boundaries, irreversible vendor choices, security/compliance impact, or material ongoing cost.

**Example**:
```markdown
## Story: Add GraphQL endpoint for dashboard

**Parent ADR**: #147 (GraphQL Migration - WORTH Score 20/25, strong candidate)

**Justification**: Reduces frontend API calls from 7 to 1; expected to improve load time by roughly 40%
```

---

### Quarterly Operations Review

Rotate a "devil's advocate" who:
1. Selects one live component or technology
2. Re-scores it with current context
3. Proposes kill/simplify plan if score < 15 or any dimension falls to 1 or 2

**Example Retro**:
- **Q1 2024**: Piloted, then adopted, Redis for caching (initial score: 19/25, worth doing)
- **Q4 2024**: Team size halved, cache hit rate 12%, ops burden high
- **Re-score**: 11/25 (strong NO)
- **Action**: Remove Redis, use in-memory cache, save `$200/month` + 5 hours/week ops

---

<a name="anti-patterns"></a>
## 12. Anti-Patterns & Failure Modes

### Anti-Pattern 1: Resume-Driven Development

**Symptom**: Technology choice justified by "industry standard" or "I want to learn this"

**Why It Happens**: Individual career goals override business needs

**Guard-rail**: Bias check: mandatory counter-proposal from someone outside the team who argues for simpler alternative

**Example**:
- **Proposed**: Rewrite in Rust because "it's the future"
- **Counter**: Keep Python, invest saved time in user research
- **WORTH Score**: Rust = 9/25 (strong NO), Python = 21/25 (strong candidate)
- **Decision**: Stay with Python

---

### Anti-Pattern 2: Cargo-Cult Microservices

**Symptom**: Dozens of tiny services, yet releases still tied together; change lead time unchanged or worse

**Why It Happens**: Copying Big Tech patterns without understanding context

**Guard-rail**: Check change lead time quarterly. If unchanged after 6 months, merge modules back

**Example**:
- Split monolith into 15 services
- Deployment frequency: Before 5/week → After 1/week
- Lead time: Before 2 days → After 5 days
- **Action**: Consolidated 10 services back into 3 larger modules
- **Result**: Deployment frequency back to 4/week

---

### Anti-Pattern 3: Hero Ops

**Symptom**: One expert owns critical infrastructure (k8s, database); vacations freeze deploys

**Why It Happens**: Hiring/training gaps; over-reliance on individual

**Guard-rail**: Treat a "Right-sized" score below 3 as a hero-ops red flag; its **Override:** line must require at least two on-call capable developers

**Example**:
- k8s managed by single SRE
- SRE leaves company
- **6 weeks** of deployment freeze while team ramps up
- **Prevention**: Require pair rotation, documentation, training budget

---

### Anti-Pattern 4: Analysis Paralysis

**Symptom**: Extended WORTH scoring sessions lasting days; no decision made

**Why It Happens**: Perfectionism, fear of commitment, unclear decision-maker

**Guard-rail**: Time-box WORTH canvas to 2 working days maximum. Default to lowest-cost prototype if still undecided.

**Example**:
- Team debates database choice for 3 weeks
- **Intervention**: "We'll pilot PostgreSQL for 2 sprints. If it doesn't work, we switch. Decision made, moving on."

---

### Anti-Pattern 5: Gaming the Score

**Symptom**: Scores are artificially inflated or manipulated to justify a predetermined outcome. This nullifies the framework's purpose, turning it into a tool for validating biases rather than challenging them.

**How Teams Game the Score**:
1.  **Inflating scores**: Marking unknowns or subjective opinions as 5/5 without evidence.
2.  **Cherry-picking evidence**: Ignoring contradictory data that would lower a score.
3.  **Skipping the bias check**: Not having a neutral party or "devil's advocate" review the scoring.
4.  **Ignoring re-scoring**: Never revisiting past decisions to validate the original score against real-world outcomes.

**Guard-Rails**:
- Require concrete evidence for any score of 4 or 5.
- Rotate the "devil's advocate" or "no" voice for bias checks.
- Post scorecards publicly within the engineering team for peer review.
- Calendar auto-reminders for each decision's review date (Section 3, Step 6).

---

### Anti-Pattern 6: The AI Mandate

**Symptom**: "Every department ships something AI by Q4." Each proposal leads with a product, a vendor, and a deadline, and every scorecard clears the bar.

**Why It Happens**: AI arrives tool-first, and fear of falling behind fills the space where a problem should be

**Guard-rail**: Split the mandate into tasks. Each gets its own scorecard, a named verifier, gate sign-offs, and a kill date (see Section 17)

**Example** (composite):
- **Proposed**: Seven AI launches by Q4, including coding agents with production credentials and a resume screener
- **Counter**: A portfolio of task-level scorecards, an approved-tools policy for the AI already in use, and a capped, labeled learning budget
- **WORTH Score**: Mandate = 12/25 (probably not) plus a failed security gate; portfolio = 19/25 (worth doing)
- **Decision**: Fund the portfolio; nothing ships without its own task, verifier, and scorecard

---

<a name="measurement"></a>
## 13. Measurement & Validation: Learning Whether WORTH Helps

### Metrics That Matter

Track these to learn whether WORTH-driven decisions age well in your context:

#### 1. Software Delivery Metrics

| Metric | How WORTH May Help |
|--------|----------------|
| Deployment Frequency | T (Time-to-value) ensures choices accelerate shipping |
| Change Lead Time | O (Outcome) prevents overhead that slows delivery |
| Failed Deployment Recovery Time | R (Right-sized) ensures team can operate without heroics |
| Change Fail Rate | W (Weigh) focuses on real problems, not speculative fixes |

Track DORA's fifth metric, Deployment Rework Rate, alongside Change Fail Rate (see Section 4).

**How to Measure**: Use production deploy, incident, and change history over comparable windows. PR-local CI data can provide useful proxies, but it is not the same as DORA measurement.

---

#### 2. Cognitive Load Surveys

Quarterly developer survey:

```markdown
Rate agreement 1-5 (1 = strongly disagree, 5 = strongly agree):

1. I feel overwhelmed by the number of technologies I need to know
2. I often need help to debug production issues
3. Large parts of our systems are unclear to me
4. I have no time to learn new skills

Score < 12 = Healthy

Score 12-16 = Warning

Score > 16 = Overloaded (simplify stack)
```

---

#### 3. Decision Cycle Time

Track time from "proposal" to "decision made":

- **Before WORTH**: Median time from proposal to decision: \_\_\_\_\_\_
- **After WORTH**: Median time with scorecard: \_\_\_\_\_\_

**Goal**: Reduce decision paralysis while maintaining quality. Fill in your own baseline; this document does not claim a universal improvement.

---

#### 4. Rollback Rate

What percentage of decisions get reversed within 6 months?

- Track reversals with reasons and decision age.
- High reversals may mean optimistic scoring, changing context, or healthy learning.
- Very low reversals may mean good decisions, excessive conservatism, irreversibility, or under-reporting.

---

#### 5. Cost Metrics

Track infrastructure and operational costs quarterly:

```markdown
## Q3 2024 Infra Costs

| Category | Cost | Change vs. Q2 |
|----------|------|---------------|
| Cloud (AWS) | `$8,200` | -15% (removed unused k8s cluster) |
| SaaS Tools | `$3,400` | +5% (added monitoring) |
| On-call Hours | 40 hrs | -30% (simplified stack) |

**WORTH Impact**: Killed over-engineered service mesh (score dropped to 9/25, strong NO). Saved `$1,500/month` + reduced on-call burden.
```

---

### Example: Tracking One Decision

**Decision**: Migrate from REST to GraphQL for dashboard API

**Initial WORTH Score**: 20/25 (strong candidate)
- W = 4 (7 API calls per page load)
- O = 4 (expect 40% faster loads)
- R = 3 (team learning GraphQL)
- T = 4 (2-sprint migration; full rollout by week 8)
- H = 5 (can revert via feature flag)

**Measurement Plan**:
1. **Week 0**: Baseline page load time (2.1s), API call count (7), developer velocity (8 stories/sprint)
2. **Week 4**: Pilot on one dashboard
3. **Week 8**: Full rollout
4. **Week 12**: Re-measure

**Results**:
- Page load: 1.3s (38% faster) ✓
- API calls: 1 (86% reduction) ✓
- Developer velocity: 6 stories/sprint (25% slower) ✗
- On-call incidents: +2 (query complexity bugs) ✗

**Re-score WORTH**: 18/25, worth doing on the total (dropped from 20), but R at 2 stops rollout beyond the dashboards
- O = 3 (speed gain offset by velocity loss)
- R = 2 (team still struggling: +2 on-call incidents from query complexity bugs)

**Action**: Pause rollout beyond the dashboards. **Override:** R at 2. Keep GraphQL on the existing dashboards only, and expand after the team upskills and R re-scores at 3 or more. Re-evaluate in 6 months.

---

<a name="operationalizing"></a>
## 14. Operationalizing the Framework

To make WORTH a team-wide habit, it needs to be embedded in the daily rituals of software development. The goal is to make the five questions an automatic, low-friction part of decision-making.

### Start Small

1.  **Pick one pending decision** (technology, architecture, process).
2.  **Check the hard gates, then fill out the WORTH scorecard** (about 30 minutes for a major decision: gather the evidence, have each person score independently, then compare and discuss; with practice, routine calls take about 5 minutes).
3.  **Share the result** and make a call with explicit kill criteria.
4.  **Set a review date** (Section 3, Step 6) to re-score the decision with real data.

Repeat this process. After 20-30 scored decisions, the five questions tend to become the default first response to any new proposal, long before anyone books a meeting about it.

---

# Part V: Extended Applications

<a name="case-studies"></a>
## 15. Case Studies: WORTH in Practice

*These case studies are illustrative composites of common patterns; the figures are representative rather than measured from a single named company.*

### Case Study 1: Bootstrapped SaaS – Railway Over Kubernetes

**Context**:
- 3 developers (full-stack)
- €300/month runway
- MVP targeting small businesses
- Goal: Revenue within 90 days

**Pain Point**: Need to deploy app; considering infrastructure options.

**Options**:

| Option | Setup Time | Monthly Cost | Ops Burden |
|--------|-----------|--------------|------------|
| A: DigitalOcean VM | 1 day | €20 | Medium (manual deploys) |
| B: Railway PaaS | 2 hours | €20-40 | Minimal |
| C: Kubernetes (EKS) | 2-3 weeks | €150+ | High |

**Gates** (before scoring): all three options pass every gate, each signed by its owner.

**WORTH Scores**:

| Dimension | VM | Railway | k8s |
|-----------|-----|---------|-----|
| W – Weigh | 4 | 4 | 4 |
| O – Outcome | 4 | 5 | 2 |
| R – Right-sized | 4 | 5 | 1 |
| T – Time-to-value | 5 | 5 | 2 |
| H – Horizon | 5 | 5 | 4 |
| **Total** | **22** | **24** | **13** |
| **Band** | Strong candidate | Strong candidate | Probably not |

*W is scored identically across options: the deployment problem is the same whichever platform solves it. A small MVP moves off a VM or a PaaS within a week, so both score H 5. Railway's O is 5: excellent ROI on a small, known bill (€20-40 a month), with almost no ops work to absorb. The k8s column matches the scorecard for this same scenario in Sections 3 and 6.*

**Decision**: Railway (24/25, strong candidate)

**Override:** Kubernetes' O at 2, R at 1, and T at 2 rule it out here.

**Review date**: within 6-12 months (Section 3, Step 6, for infrastructure), sooner past 10,000 daily active users.

**Rationale**: Maximize time-to-value. Can ship MVP this week. Budget-friendly. Easy migration path if growth demands it.

**Implementation**:
- Deployed in 3 hours
- Auto-deploy on git push
- First customer within 2 weeks

**Composite outcome shape**:
- Product ships before infrastructure becomes the bottleneck
- Infrastructure cost remains small enough not to drive the roadmap
- Re-score early when deployment, reliability, workload scheduling, or cost constraints become concrete

**Lessons Learned**:
- PaaS removed nearly all infrastructure distraction
- Delayed optimization was correct call
- Now approaching scale where k8s might be justified (re-evaluate past 10,000+ daily active users)

---

### Case Study 2: Fintech Scale-Up – Hybrid Microservices

**Context**:
- 40 developers, 4 teams
- Series C funded
- PCI DSS compliance required
- Modular monolith causing deployment conflicts

**Pain Point**: Teams blocking each other 2-3 times/week waiting for deployment windows. Lead time increased from 3 days to 7 days.

**Options**:

| Option | Migration Time | Complexity | Team Impact |
|--------|---------------|------------|-------------|
| A: Tighten module boundaries | 1 month | Low | Minimal |
| B: Extract 2-3 services (payments, auth) | 3-4 months | Medium | 2 teams affected |
| C: Full microservices (10+ services) | 8-12 months | High | All teams disrupted |

**Gates** (before scoring): all three options pass every gate, PCI DSS included, each signed by its owner.

**WORTH Scores**:

| Dimension | A: Modules | B: Hybrid | C: Full |
|-----------|------------|-----------|---------|
| W – Weigh | 4 | 4 | 4 |
| O – Outcome | 2 | 4 | 3 |
| R – Right-sized | 5 | 4 | 2 |
| T – Time-to-value | 5 | 4 | 2 |
| H – Horizon | 4 | 4 | 4 |
| **Total** | **20** | **20** | **15** |
| **Band** | Strong candidate | Strong candidate | Borderline |

*W is identical across options (the deployment-collision problem is the same regardless of solution). Option A's O reflects that tightening an already-modular monolith has limited headroom against release coupling. B's first extraction lands within 3 months (T 4), and extracted services take more than a week, but under a month, to fold back (H 4). C's H 4 assumes it is phased too: each extraction can stop or fold back within a month; its T 2 reflects 8-12 months of disrupting every team.*

**Decision**: Option B (Hybrid). It ties A at 20/25, but A's O of 2 trips the override: tightening an already-modular monolith can't remove the release coupling, so the tie breaks on the profile, not the sum. B starts as a staged pilot.

**Override:** A's O at 2: nothing inside the monolith fixes the release coupling, so A is out. C's R at 2 and T at 2: full migration waits until the team can run 10+ services and value can arrive in stages.

**Kill criteria** (written before the pilot): no deployment-frequency or lead-time gain for the extracted domain by month 3, or a sustained rise in change fail rate, ends the extraction.

**Rationale**:
- PCI compliance easier with separate payments service
- Auth used by all teams, high coupling risk
- Keep reporting, admin, backoffice in monolith (low change frequency)

**Implementation**:
- **Month 1-2**: Extract payments service (clear PCI boundary)
- **Month 3**: Measure: deployment frequency +30%, lead time -1 day
- **Month 4-5**: Extract auth service
- **Month 6**: Stabilize, measure

**Composite outcome shape**:
- Deployment conflicts reduce for the extracted domains
- Lead time improves where service ownership is clear
- Change fail rate may rise during transition, so the pilot needs close monitoring
- Infrastructure and on-call cost increase, but stay acceptable if the W and O assumptions were true

**WORTH Re-score**: Revisit after the pilot using measured delivery, incident, cost, and audit data, and again by the review date, within 6-12 months (Section 3, Step 6).

**Lessons Learned**:
- Hybrid architecture was sweet spot
- Didn't extract everything; kept stable domains in monolith
- PCI isolation may reduce audit scope or review effort, depending on the environment
- Now template for future service extraction

---

<a name="blind-spots"></a>
## 16. Blind Spots Addressed

This section tackles objections and contexts the main flow doesn't address head-on: four dogmas with strong followings, additional decision lenses, and a dedicated mode for one-person projects. Where Section 10 debunks dogma broadly, these entries drill into specific high-stakes cases.

### Dogmas Revisited

#### Dogma: "TDD Is Mandatory or You're Reckless"

**Why It's a Dogma**: TDD can add indirection and slow exploratory work; in the 2014 "Is TDD Dead?" conversations, Kent Beck, Martin Fowler, and David Heinemeier Hansson debated where it pays off ([martinfowler.com](https://martinfowler.com/articles/is-tdd-dead/)).

**Reality Check**: TDD shines for:
- Safety-critical code (billing engines, medical devices)
- Long-lived domains with clear requirements
- Refactoring existing code

TDD struggles for:
- Green-field spikes exploring problem space
- Data science notebooks
- One-off migration scripts

**WORTH Application**:

| Context | W | O | R | T | H | Total | Verdict |
|---------|---|---|---|---|---|-------|---------|
| Billing engine | 5 | 5 | 4 | 3 | 5 | 22 | ✓ Strong candidate: TDD worth it |
| One-off migration | 2 | 1 | 3 | 1 | 3 | 10 | ✗ Strong NO: post-hoc tests sufficient |

**Override:** for the one-off migration, W at 2, and O and T at 1: TDD comes back only if the script turns into long-lived code.

---

#### Dogma: "Go All-In on Serverless"

**Why It's a Dogma**: "Never think about servers again!" marketing obscures trade-offs.

**Reality Check**:
- **Wins**: Spiky traffic and reduced idle compute cost, while remembering that API gateways, queues, databases, observability, provisioned concurrency, and networking can still create baseline costs
- **Loses**: Cold-start latency, opaque debugging, vendor lock-in

**WORTH Application**:

| Scenario | Cold Start Acceptable? | Debugging Needs | Lock-In Risk | Verdict |
|----------|----------------------|----------------|--------------|---------|
| Image resize API | Yes (async) | Low | Moderate (logic portable) | ✓ Lambda |
| Real-time chat | No | High | High | ✗ PaaS/VM |

**Illustrative rollback story**: A startup moved to Lambda for all APIs. Debugging production issues took roughly 5x longer (no SSH, CloudWatch delays). After a few months, it moved the critical path back to EC2 and kept async jobs on Lambda. Hybrid won.

---

#### Dogma: "Event-Driven Architecture Is Always Better"

**Why It's a Dogma**: Loose coupling sounds great in theory.

**Reality Check**:
- **Wins**: Multiple consumers, async workflows, audit trails
- **Loses**: Traceability nightmares, idempotency complexity, eventual consistency headaches

**WORTH Decision Matrix**:

| Use Case | Consistency Needs | Traceability | Latency | Verdict |
|----------|------------------|--------------|---------|---------|
| Order processing | Strong | High | Low | ✗ Request/response + DB transaction |
| Analytics events | Eventual OK | Medium | High OK | ✓ Event stream |

---

#### Dogma: "GraphQL Fixes Every API Problem"

**Why It's a Dogma**: Flexible queries sound universally better than REST.

**Reality Check**:
- **Wins**: Rich clients querying complex object graphs, mobile apps with bandwidth constraints
- **Loses**: N+1 query problems, complex caching, resolver-level authorization, query-depth limits, data exposure risk, and overkill for CRUD

**WORTH Sidebar**:

| API Pattern | When to Use |
|------------|-------------|
| REST | Simple CRUD, public APIs, need HTTP caching |
| gRPC | Internal services, performance-critical, typed contracts |
| GraphQL | Rich client queries, reducing round trips, mobile apps |

---

### Other Lenses

#### Security & Compliance Guard-Rails

**Problem**: WORTH can score "yes" but legal requirements veto the choice.

**Solution**: Check the hard gates (Section 3, Step 2) before scoring. In regulated domains, start from this checklist:

```markdown
## Compliance Gate (complete BEFORE WORTH scoring)

- [ ] PCI DSS requirements (if handling card payments)
- [ ] HIPAA requirements (if handling US health data as a covered entity or business associate)
- [ ] GDPR/LGPD (if handling EU/Brazil personal data)
- [ ] SOC 2 controls (if selling to enterprises)
- [ ] Threat model reviewed (if internet-facing)
- [ ] AI rules checked wherever the AI use reaches, such as the EU AI Act, and local laws such as New York City's Local Law 144 for hiring tools (if any AI is involved; legal signs)
- [ ] A named business owner answers for the AI's output, and every AI claim we make can be substantiated (if AI output reaches customers, staff, or decisions)
- [ ] No AI session combines untrusted content, private data or sensitive systems, and external communication or state change, or security has signed that a successful prompt injection is tolerable (if an AI tool reads untrusted content)

Mark each item Pass / Fail / Needs review.
Only the owning function (legal, compliance, security, privacy, procurement, or the business owner for AI accountability) marks Pass; anyone else marks Needs review and escalates.
Any Fail puts the option outside the adoption set, whatever its total.
Date every AI sign-off: AI passes expire when laws, vendor terms, or rulings change (Section 17).
```

---

#### Data Modeling & Storage

**Why It Matters**: SQL vs. NoSQL wars rival language wars in heat/lack of nuance.

**WORTH Application**:

| Storage Type | When It Wins | When It Loses |
|--------------|--------------|---------------|
| **PostgreSQL (relational)** | Relational data, strong transactional invariants, complex queries | Schemaless flexibility, certain horizontal-scaling patterns |
| **MongoDB (document)** | Flexible document model, denormalized reads, tunable consistency | Complex joins or invariants if the model is poorly designed |
| **Redis** | Caching, sessions, pub/sub | Primary data store, durability critical |
| **NewSQL (CockroachDB)** | Need both ACID + horizontal scale | Low-write workloads, budget-constrained |

**WORTH Question**: Does our data access pattern justify the operational complexity of this database?

---

#### Observability: How Much Is Enough?

**Dogma**: "Full OTEL stack or you're flying blind"

**Reality**: Observability has costs (infrastructure, cognitive load, vendor bills). Size it by SLOs, blast radius, architecture complexity, and MTTR goals rather than user count alone.

**WORTH Spectrum**:

| Context | Logging | Metrics | Tracing | APM | Cost Shape |
|---------|---------|---------|---------|-----|------------|
| Low criticality / simple architecture | Structured logs | Basic uptime/errors | None or sampled | None | Low |
| Customer-facing / moderate complexity | Centralized logs | RED metrics | Critical paths | Optional | Medium |
| High criticality / distributed architecture | Standardized telemetry | SLO dashboards | Distributed tracing | APM where useful | Higher |

**WORTH Question**: Does the observability investment reduce MTTR enough to justify the cost?

---

#### People Dogmas

**Dogma 1**: "Only hire seniors"

**Reality**: Balanced teams (seniors + mid-level + juniors) often outperform all-senior teams due to:
- Cost efficiency
- Diverse perspectives
- Knowledge transfer culture
- Hunger/growth mindset

**WORTH Question**: Does hiring exclusively seniors improve delivery more than the higher salary cost?

---

**Dogma 2**: "10x engineer myth"

**Reality**: However strong one engineer is, a team that depends on that person has a bus factor of one, which Section 2 lists as an R red flag.

**WORTH Question**: Are we building for individual heroics or team resilience?

---

**AI and junior roles**: Stopping junior backfills because AI drafts the work is an AI decision, scored or not. In ADP payroll data through June 2026, employment of 22-25-year-olds in AI-exposed occupations stood 19% below where it would be had it kept pace with less-exposed peers; the authors of this unreviewed working paper call it descriptive, not causal ([Stanford Digital Economy Lab](https://digitaleconomy.stanford.edu/app/uploads/2026/08/Canaries_August2026.pdf)).

**WORTH Question**: If nobody learns the work now, who'll check the AI's work later? Score who checks today under R, and the cost of rebuilding the skill under H.

---

<a name="solo-mode"></a>
#### Solo Developer / Indie Hacker Mode

**Blind Spot**: Most books ignore one-person projects.

**WORTH Solo Mode**:

**Default Answers for Solo Devs**:
- **W**: Only solve pains you feel daily
- **O**: Optimize for shipping, not elegance
- **R**: Stick to technologies you already know
- **T**: MVP in days/weeks, not months
- **H**: Avoid vendor lock-in (you can't negotiate)

**One-Page Solo Canvas**:

```markdown
## Solo Dev WORTH Checklist

- [ ] It passes the hard gates: no law or security rule broken, and no client data sent where it isn't allowed (for example, a free consumer AI chatbot). If this one fails, skip it whatever the others say.

- [ ] Do I actually need this, or is it "best practice" theater?
- [ ] Can I build and run this alone in < 1 week?
- [ ] Will this help me ship faster or just add complexity?
- [ ] Can I reverse this in a weekend if it doesn't work?

If < 3 of these four checks pass, skip it. Even with three passes, a clear "no" on the remaining check means fix that first or skip it.

For an AI tool, measure first: for two weeks, time three recurring tasks both with and without it, checking included, and keep it only where time falls and errors don't rise.
```

---

<a name="ai-decisions"></a>
## 17. AI Decisions: Cheap to Generate, Expensive to Trust

AI collapsed the cost of producing work. It did not collapse the cost of checking it, owning it, or undoing it.

Every AI decision is therefore a decision about verification, accountability, and reversibility. You don't need a new framework for that: WORTH's gates and scores already cover all three. You do need to score honestly, because AI distorts each dimension in a predictable direction.

**Score a task, never *AI*.** *Should we adopt AI?* names no problem for W to weigh, so it can't be scored. *Should AI draft first replies to tier-1 refund tickets?* names a task, a volume, a cost, and an owner, so it can.

**The verification tax** is the time, skill, and risk cost of checking, fixing, and owning AI output before it can be relied on. Demos show the generation; your budget pays for the checking:
- In GDPval, a benchmark that OpenAI built and ran, the paper's GPT-5 estimates started at 90x faster and 474x cheaper than unaided experts. Once expert review and fixing were counted, they shrank to about 1.1-1.4x faster and 1.2-1.6x cheaper: still a gain, by a much thinner margin ([GDPval, ICLR 2026](https://proceedings.iclr.cc/paper_files/paper/2026/hash/290c2430f91912204f30bbcc990fff1d-Abstract-Conference.html)).
- METR ran an agent on 18 real issues from two mature repositories. It passed the tests on 38% of issues on average, yet none of the 15 pull requests humans reviewed was mergeable as-is ([METR, 2025](https://metr.org/blog/2025-08-12-research-update-towards-reconciling-slowdown-with-time-horizons/)).

The tax isn't a reason to avoid AI. It's a cost: put a number on it before you score anything.

*AI facts are the most perishable in this document. Figures, vendor policies, and laws below are as of November 2026; re-score with current facts before you decide.*

### The Five AI Distortions

Each distortion starts from something true: AI is general, pleasant to use, generous to beginners, fast to demo, and easy to start. Each truth inflates one score. The questions below feed the Section 3 scoring guide; they never replace it.

| Dimension | What the pitch says | The distortion | The correction |
|-----------|---------------------|----------------|----------------|
| **W** | "We need an AI strategy." | The tool-shaped problem | Score the task, not the tool |
| **O** | "Everyone feels faster." | The perception gap and the verification tax | Measure outcomes, and price verification and usage as overhead |
| **R** | "A human reviews every draft." | The verification gap | Right-skilled to verify, with oversight designed in |
| **T** | "The demo works; launch next month." | The demo mirage | Score time to reliable value, not time to first demo |
| **H** | "It's a monthly contract." | The two-way horizon | Hedge lock-in and obsolescence, and protect skills |

- **W**: Can you state the problem without the word AI? If the sentence collapses, you're weighing a tool. Capability is jagged, so a verdict on *AI* tells you nothing about your task: on one task designed to sit outside GPT-4's frontier, 84.5% of BCG consultants without AI got it right, against 60% and 70% in the two AI arms ([Dell'Acqua et al.](https://mitsloan.mit.edu/sites/default/files/2023-10/SSRN-id4573321.pdf)). Keep W identical across options, including doing nothing and a fix without AI.
- **O**: Feeling faster isn't being faster. In METR's 2025 randomized trial (an unreviewed preprint), experienced developers on their own mature repositories took 19% longer with early-2025 AI tools, yet believed AI had sped them up by 20% ([METR, 2025](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)). A 2026 follow-up leaned toward a speedup, but METR calls its data only very weak evidence of how large any speedup now is ([METR, 2026](https://metr.org/blog/2026-02-24-uplift-update/)). Benefit is a measured outcome against a comparison group. Overhead is the usage bill at realistic volume, priced per task rather than per token, plus checking and fixing time, plus the expected cost of errors that get through.
- **R**: The person who gains most from AI can be the person least able to catch its errors, and reviewers tend to defer to the machine. In a mammography experiment, inexperienced readers rated 79.7% of mammograms correctly when the AI suggestion was correct and only 19.8% when it was wrong; for very experienced readers the figures were 82.3% and 45.5% ([Dratsch et al., Radiology](https://doi.org/10.1148/radiol.222176)). Score the verifier's skill, not the user's enthusiasm, and design the check in: people decide before they see the AI's answer, AI routes cases, or AI checks the human. Treat a near-zero overrule rate as a question, not as proof of quality.
- **T**: A demo takes days; reliable production takes months. LinkedIn's engineers reached 80% of the basic experience they wanted in the first month of building a generative-AI product, then spent four more months trying to get past 95% of the full experience ([LinkedIn Engineering](https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product)). Score the time to clear a numeric quality bar on cases nobody hand-picked, count workflow redesign in the timeline, and write the success criterion and kill date before the demo.
- **H**: H has three risks: lock-in, obsolescence, and skills that fade while the tool does the work. Score only one and H looks better than it is.
  - **Retirement**: as of November 2026, OpenAI gives at least 6 months' notice before retiring generally available models, but previews may be retired with much shorter notice, such as 2 weeks, and safety or compliance concerns can shorten any timeline ([OpenAI](https://developers.openai.com/api/docs/deprecations)); Anthropic gives at least 60 days for publicly released models ([Anthropic](https://platform.claude.com/docs/en/about-claude/model-deprecations)).
  - **Being overtaken**: in an independent evaluation, general-purpose GPT-4 beat the finance-specific BloombergGPT's reported scores on 4 of the 5 public benchmarks where both had results ([Li et al., EMNLP 2023](https://aclanthology.org/2023.emnlp-industry.39/)).
  - **Fading skills**: high-school students given unrestricted GPT-4 scored 17% lower on a later exam without AI than students who never had it, while a guardrailed tutor avoided the harm ([Bastani et al., PNAS](https://doi.org/10.1073/pnas.2422633122)).
  - **Hedge both ways**: test what a general model already does before you build, and climb from prompting to retrieval to fine-tuning only when your own evals show the rung below falls short. Keep the layer thin behind an abstraction, invest in what's portable (your data and your eval suite), and count rebuilding unaided skill as exit cost.

### AI Hard Gates

Gates come before any score (Section 3, Step 2). AI sharpens the legal, privacy, security, and safety gates and adds one: **you own what your AI says and does.** The irreversible lock-in and procurement gates apply unchanged. Laws are as of November 2026; check them with counsel.

| Gate | AI-specific test | Who signs |
|------|------------------|-----------|
| **Legal / regulatory** | Which AI rules apply wherever this use reaches, now and before your next review? Is any use prohibited? | Legal |
| **Accountability** | When the output is wrong, who answers for it? Can every AI claim you make be substantiated? | Business owner |
| **Privacy / confidentiality** | Where does each input go, under which terms, for how long? Which tools are people already using? | Privacy and security |
| **Security** | Does one session combine untrusted content, access to private data or sensitive systems, and the power to communicate externally or change state? Is a successful injection tolerable? | Security |
| **Safety / harm to people** | Could an error cost someone a job, credit, care, or safety? Can the overseer override or stop it? | Accountable executive |

- **A failed gate is a no, not a low score.** Only the owner in the third column can mark a gate Pass. Anyone else marks it *Needs review* and escalates. No total carries an option past its gate.
- **With AI, passes expire.** A regulatory date, new vendor terms, or a ruling can flip one, so date every sign-off. No vendor can sign for you.
- **When an AI tool reads untrusted content, security signs.** Customer messages, outsiders' issues, resumes, and supplier emails and invoices can all carry hidden instructions. The UK NCSC says prompt injection may never be fully mitigated ([NCSC](https://www.ncsc.gov.uk/blog-post/prompt-injection-is-not-sql-injection)). Private data, untrusted content, and external communication in one agent make what Simon Willison calls the lethal trifecta ([Willison](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)), and Meta's Agents Rule of Two says one session should hold at most two of the security test's three properties ([Meta](https://ai.meta.com/blog/practical-ai-agent-security/)).
- **Accountability is not theoretical.** A British Columbia tribunal held Air Canada liable for its website chatbot's wrong fare advice ([Moffatt v. Air Canada](https://www.mccarthy.ca/en/insights/blogs/techlex/moffatt-v-air-canada-misrepresentation-ai-chatbot)).
- **Legal dates move.** The EU AI Act phases in its duties from February 2, 2025 to August 2, 2028; Compliance & Security in Section 4 lists the dates as of November 2026 ([AI Act, Article 113](https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-113)).

### Agents: Scoring Autonomy

An agent is a system in which a model directs its own steps and tool use, instead of following predefined code paths ([Anthropic](https://www.anthropic.com/research/building-effective-agents)). It repeats its task all day, so judge it on consistency, not the average. τ-bench measures this with pass^k, the chance of succeeding on all k repeated tries of the same task: the best agent τ-bench tested in 2024, gpt-4o, succeeded on 61.2% of retail tasks on average, but its retail pass^8 fell below 25% ([τ-bench, ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/1b126cc38b8638e07bef37e7b2bb72bf-Abstract-Conference.html)).

**Autonomy is an H decision.** With an agent you're wrong one action at a time, and its permissions decide which mistakes can be undone. List every tool it holds and what it could do with each one without asking anybody:

| Action scope | Reversible? | Who approves |
|--------------|-------------|--------------|
| Read records (invoices, purchase orders, history) | Yes: nothing changes | The data owner, once, when granting access |
| Draft a reply or a recommendation | Yes: a draft can be discarded | Nobody per action; a person decides what to use |
| Send a message outside the company | No: an email can't be unsent | A named person, per message |
| Approve a payment | No: money paid out is costly to recover | A named approver, every time |
| Delete records, change permissions, touch production | No | Never the agent |

**Score an agent's H on the worst action it can take alone.** A session that holds untrusted input, private data, and the power to act externally goes back through the security gate with a design that removes one of the three. An approver who clicks through every draft is a rubber stamp, not a check. The builders' own advice is conservative: Anthropic recommends "finding the simplest solution possible, and only increasing complexity when needed. This might mean not building agentic systems at all." ([Anthropic](https://www.anthropic.com/research/building-effective-agents))

### Worked Example: AI Coding Tools for a Review-Bound Team

**Problem Statement**: "Changes to our invoice-matching service wait days for review; the queue has grown for two quarters while throughput stayed flat." The sentence needs no AI, and the pain is measured weekly, so W = 4 for every option.

**Gates first, then scores**:

| Option | Gate check | W | O | R | T | H | Total | Verdict |
|--------|-----------|---|---|---|---|---|-------|---------|
| 1. Status quo: informal individual use | *Needs review*: security hasn't vetted which tools see the code | 4 | 2 | 3 | 5 | 4 | 18 | Worth doing, on paper only |
| 2. Sanctioned assistants, measured pilot | Pass once security signs an approved-tools list and the team lead, as business owner, signs for accountability | 4 | 3 | 4 | 4 | 5 | 20 | ✓ Strong candidate, as a pilot with a kill date |
| 3. Autonomous agents with merge rights and production credentials | **Fail: security.** Reads outsiders' issues (untrusted content), holds the private repository and production credentials (private data), merges with network access (external communication) | 4 | 2 | 3 | 3 | 1 | 13 | ✗ Probably not, and moot: the security gate said no |

**Override:** Option 1's O at 2. Keep it only until someone can say what the tools deliver.

**Override:** Option 3's H at 1 until permissions are scoped: no production credentials, no merge rights, one repository per session with least-privilege tokens, and a human who merges. Scoped that way, Option 3 becomes a bounded agent inside Option 2's pilot.

**Override:** Option 3's O at 2 until the team measures how many agent pull requests its own reviewers would merge, net of fixing time.

Option 3's failure mode has been demonstrated: in a security vendor's proof of concept, a malicious public issue made an agent leak private-repository data into a public pull request ([Invariant Labs](https://invariantlabs.ai/blog/mcp-github-vulnerability)).

**Option 2 in detail**:

| Dimension | Score | Evidence |
|-----------|-------|----------|
| W – Weigh | 4 | Review waits of days; the queue has grown for two quarters |
| O – Outcome | 3 | Randomized company rollouts counted more completed pull requests, but their one quality measure, build success rate, showed no negative effect at Microsoft and was negative at Accenture ([Cui et al.](https://economics.mit.edu/sites/default/files/inline-files/draft_copilot_experiments.pdf)). Break-even until measured: the bottleneck is review, where more generated code adds load |
| R – Right-sized | 4 | The team can verify its own code: small pull requests, authors own every line, a senior reviews juniors' changes |
| T – Time-to-value | 4 | A month-three verdict, late enough to see whether early velocity fades, as it did after about two months in a peer-reviewed study of Cursor adoption ([He et al., MSR '26](https://doi.org/10.1145/3793302.3793349)) |
| H – Horizon | 5 | Monthly seats, nothing tied to one vendor, no code left behind that nobody understands |

**Total**: 20/25 – Strong candidate, as a pilot with a kill date

**Caveats behind these scores**:
- The often-quoted 55.8% speedup comes from one controlled experiment: a single small JavaScript task built on a provided skeleton, counting only developers who completed it, in an unreviewed preprint whose authors include GitHub and Microsoft staff ([Peng et al.](https://arxiv.org/abs/2302.06590)).
- In a Stanford user study, people with an AI coding assistant wrote less secure code yet more often believed it was secure ([Perry et al., ACM CCS 2023](https://doi.org/10.1145/3576915.3623157)). Option 2's R of 4 assumes authors who could write the code they accept, plus a review of generated code.
- **Seat vs. API**: Option 2's H of 5 belongs to cancellable editor seats. Wiring a model's API into your product or internal tools scores H lower: models are retired on the provider's schedule, prompts and evals tuned for one model don't transfer for free, and switching providers is a migration. Put an interface between your code and any model API, and budget usage-based pricing as a variable cost, like an infrastructure bill.

**Verdict**: ✓ Pilot Option 2 against a sibling team. Compare throughput (change lead time, deployment frequency, failed deployment recovery time) and stability (change fail rate, deployment rework rate, pull-request size), and check quality debt (static-analysis warnings and complexity) at month three. Confirm each new dependency is the package you meant, from a publisher you expect: on average at least 5.2% of the packages recommended by the three commercial models tested (all OpenAI GPT models) did not exist (21.7% for open-source models), and an attacker can register an invented name ([Spracklen et al., USENIX Security 2025](https://www.usenix.org/conference/usenixsecurity25/presentation/spracklen)).

**Kill criteria** (written before the pilot): no lead-time gain by month three, or worse stability.

**Re-score when**: the month-three review, a vendor model or pricing change, any proposed agent permission, any incident traced to generated code or a hallucinated dependency, and each quarter.

### When WORTH Misleads on AI

- **"A competitor went AI-first" is not a problem statement.** Name the competitor's capability and the date it hurts you, then score it as a problem with a deadline. If you can't, it's fear, and fear gets no exemption. In IBM's 2025 survey of 2,000 CEOs, 64% acknowledged that the risk of falling behind drives investment in some technologies before they have a clear understanding of the value ([IBM](https://newsroom.ibm.com/2025-05-06-ibm-study-ceos-double-down-on-ai-while-navigating-enterprise-hurdles)).
- **Doing nothing is not free.** For AI, the status quo isn't AI-free; it's AI nobody manages. In MIT NANDA's non-representative survey, workers at over 90% of the companies used personal AI tools for work, but only 40% of the companies said they had bought an official LLM subscription ([MIT NANDA, 2025, third-party copy](https://www.artificialintelligence-news.com/wp-content/uploads/2025/08/ai_report_2025.pdf)). Put the free consumer chatbot one browser tab away on the do-nothing scorecard, and run it through the privacy gate.
- **Learning bets and values questions** follow "When WORTH Doesn't Apply" in Section 2: label a learning bet with its real score, a budget cap, and an end date, and let leadership answer values questions before WORTH scores what remains.

### Keeping AI Scorecards Honest

With AI, a scorecard can go stale within months. **Re-score quarterly, and at once on a trigger**: a deprecation notice, a price or plan change, a regulatory date, an incident (yours or with the same class of tool), or a new general model that might do the task out of the box.

**Measure against a comparison group, not self-report.** An AI rollout can usually have one:
1. **Fix the success metric before rollout**: correct refunds, reopened tickets, time to resolution. Not hours saved.
2. **Randomize** by ticket, week, or team start, and enroll people before rollout.
3. **Log the verification tax** from systems, not surveys: checking minutes, escalations, overrule rate.
4. **Discount self-report**, and agree before rollout whether new work the tool creates counts as value.
5. **Re-measure** after two or three months and after every trigger.

If an AI judge grades your evals, keep checking it against human grades.

**Record the bet** in an addendum to your ADR (Section 3):

```markdown
## AI Addendum
- **Task**: [One task, stated without the word AI]
- **Frontier evidence**: [Results on your own unpicked cases]
- **Verifier and verification method**: [Who checks; could they do it unaided; decide-first, routing, or AI-checks-human]
- **Error cost**: [Cost of one undetected error; how often one gets through]
- **Eval suite**: [Location, size, last run, human check of any AI judge]
- **Model and version pinned**: [Exact model ID and settings; provider's notice policy]
- **Fallback and exit path**: [What runs if the model is retired; time to switch]
- **Data classification and terms**: [Highest data class sent; commercial or consumer terms]
- **Gate sign-offs**: [Each gate: Pass / Fail / Needs review, signer, date]
- **Autonomy scope**: [Actions allowed without a human; irreversible ones blocked]
- **Review date**: [Next quarterly re-score; triggers watched]
```

### The AI WORTH Card

**Gates first**, each pass/fail and signed by its owner:
- Legal and regulatory (legal)
- Accountability for AI output (business owner)
- Privacy and confidentiality (privacy and security)
- Security, including prompt injection (security)
- Safety and harm to people (accountable executive)
- Irreversible lock-in and procurement (Section 3, Step 2)

| Dimension | AI question | Red flag | Green flag |
|-----------|-------------|----------|------------|
| **W** – Weigh | Can you name the task and its cost without saying AI? | Tool first, problems bundled | One owned task, measured cost, same W across options |
| **O** – Outcome | What moves against a comparison group, net of the verification tax? | Felt speed, per-token pricing | Beats the comparison, and cost per task includes checking and errors |
| **R** – Right-sized | Who checks, and can they catch its errors at today's volume? | Least-skilled user checks alone, overrules near zero | Checker stronger than the AI here, oversight designed in |
| **T** – Time-to-value | When is it reliable on unpicked cases? | Launch date set by a demo, no kill date | Quality bar and kill date in writing, redesign in the plan |
| **H** – Horizon | If the model is retired or overtaken, how fast can you switch? If you exit, what does rebuilding unaided skill cost? | Prompts tuned to one model, no evals, skills fading | Thin layer, portable evals, second provider tested |

The scale, the bands, and the override are unchanged. **Score AI one task at a time, like every other decision in this framework.**

---

# Appendices

<a name="appendix-a"></a>
## Appendix A: WORTH Canvas (One-Page Template)

```markdown
# WORTH Decision Canvas

**Date**: ___________  **Decision owner**: ___________  **Final decider**: ___________  **Reviewer**: ___________

## Problem Statement (one sentence)
[What pain are we solving?]

**Cost of Delay**: $______/week OR _______ hours/week

---

## Options Considered

| Option | Description | Estimated Effort |
|--------|-------------|------------------|
| A (Baseline) | | |
| B (Incremental) | | |
| C (Ambitious) | | |

---

## Hard Gates (before scoring; Pass / Fail / Needs review)

Only the gate's owner marks Pass; anyone else marks Needs review. A Fail takes the option out, whatever its total.

| Gate | A | B | C | Signer, date |
|------|---|---|---|--------------|
| Legal / regulatory | | | | |
| Security (incl. prompt injection if an AI tool reads untrusted content) | | | | |
| Privacy / data residency | | | | |
| Safety | | | | |
| Irreversible lock-in beyond tolerance | | | | |
| Procurement viability | | | | |
| Accountability (AI decisions only) | | | | |

---

## WORTH Scorecard (1-5 scale, unknown = U)

| Dimension | A | B | C | Scrutiny (High / Medium / Low) |
|-----------|---|---|---|----------|
| **W** – Weigh the problem | | | | |
| **O** – Outcome > overhead | | | | |
| **R** – Right-sized team | | | | |
| **T** – Time-to-value | | | | |
| **H** – Horizon flexibility | | | | |
| **CORE TOTAL** | | | | |
| **Band** | | | | |
| **Any 1 or 2? Override:** | | | | |

Set scrutiny for your context (Section 3, Calibration Note): startups scrutinize R, T, and H hardest, large organizations W and O, and in regulated domains H is often decisive. Scrutiny is a focus, not a weight: it sets where the team digs and how strictly it scores, but it never adds points or multiplies a score.

### Extended Lenses (optional, high-stakes only)

| Dimension | A | B | C |
|-----------|---|---|---|
| Cost of Delay | | | |
| Cognitive Load | | | |
| Delivery Impact | | | |

---

## Bias Check

**Potential biases influencing this decision**:
- [ ] Sunk-cost fallacy
- [ ] Resume-driven development
- [ ] Conformity ("everyone's doing it")
- [ ] Availability bias
- [ ] Confirmation bias
- [ ] Groupthink (no recorded dissent)
- [ ] Automation bias (AI decisions)
- [ ] Fear of falling behind ("we need an AI strategy")

**Counter-argument for preferred option**: ___________

**Unresolved dissent**: ___________

---

## Decision

**Chosen**: Option ____

**Rationale**: ___________

**Success metrics**: ___________

**Kill Criteria** (revert if):
1. ___________
2. ___________

**Review Date**: ___________ (per Section 3, Step 6: quarterly for AI and other fast-moving tools, 3-6 months for most decisions, 6-12 months for infrastructure and architecture)

**Re-score early if**: ___________

---

## Signatures

**Approved**: ___________ (Final decider)

**Gates signed**: see Hard Gates (name and date per gate)

**Reviewed**: ___________ (Devil's Advocate)
```

---

<a name="appendix-b"></a>
## Appendix B: Decision Tables Consolidated

### Table 1: Common Dogmas vs. Reality

| Dogma | Reality Check | Pays Off When | Hurts When |
|-------|--------------|---------------|------------|
| "X is the best language" | Match runtime, ecosystem, hiring market to needs | Domain has clear incumbent (Python-ML, Go-network) | Polyglot chaos; ops fragmentation |
| "Follow every Clean Code rule" | Weigh defects saved vs. boilerplate added | Core libraries touched daily | Throwaway migration scripts |
| "Start with k8s & microservices" | Cognitive load check; outcome > overhead | Multi-team, strict SLO scale-ups | 3-dev startup pre-PMF |
| "Heavyweight process for all" | Time-to-value; process ∝ team size | Regulated enterprises | Seed-stage pivoting weekly |
| "TDD everywhere" | Test value vs. exploration speed | Safety-critical long-lived code | Green-field spikes, notebooks |
| "Serverless cures ops" | Cold-start vs. observability trade-off | Spiky traffic, budget tied to usage | Steady low-latency workloads |
| "EDA > request/response" | Traceability vs. loose coupling | Multi-consumer async workflows | Single-team ACID transactions |
| "GraphQL over REST" | Query flexibility vs. caching complexity | Rich client queries, mobile | Simple CRUD, public APIs |

---

### Table 2: Acronyms Through WORTH Lens

| Acronym | Core Idea | WORTH Check | Worth It | Not Worth It |
|---------|-----------|-------------|----------|--------------|
| **STUPID** | Six code smells to avoid | Outcome > overhead | Daily-touched modules | One-off scripts |
| **CUPID** | Joyful code properties | Right-sized team | Green-field micro-features | Legacy blob refactor |
| **FIRST** | Fast, independent tests | Time-to-value | High-change domains | Yearly ETL jobs |
| **INVEST** | Story slicing criteria | Weigh the problem | Parallel distributed teams | Solo dev hobby projects |
| **SMART** | Goal-setting framework | Horizon flexibility | Compliance-driven roadmaps | R&D exploration spikes |
| **ACID/BASE** | DB guarantee trade-offs | Outcome > overhead | Banking (ACID), Social feeds (BASE) | - |
| **CAP** | Distributed systems trade | Risk lens | Trading (CP), SaaS cart (AP) | - |

---

### Table 3: Architecture Decision Matrix

| Pattern | When It Wins | WORTH Red Flags |
|---------|--------------|----------------|
| **Monolith** | Single team, emerging domain, rapid iteration | Team > 15, deployment conflicts, different scaling needs |
| **Modular Monolith** | 20-60 devs, clear modules, shared DB acceptable | Need polyglot, PCI isolation, independent scaling |
| **Microservices** | 50+ devs across multiple teams (strongest around 150+), clear bounded contexts | < 50 devs, single team, shared transactions common |
| **Serverless** | Spiky traffic, async jobs, cost ∝ usage | Real-time requirements, deep debugging needs |

---

### Table 4: Infrastructure Choice Matrix

| Option | Setup Time | Monthly Cost | Ops Burden | Scaling Headroom | Best For |
|--------|-----------|--------------|------------|-------------|----------|
| **Bare VM** | 1 day | `$20-100` | Medium | Low | Solo dev, learning, static sites |
| **PaaS** | 2 hours | `$20-500` | Minimal | Medium | Startups, small teams, rapid iteration |
| **Kubernetes** | Weeks to stand up, 2-3 months to run well | `$200+` | High | Very High | 50+ engineers, multiple services, dedicated DevOps/SRE team, multi-cloud needs |

---

<a name="appendix-c"></a>
## Appendix C: Additional Dogmas Deep-Dive

### Infrastructure as Code (IaC)

**Dogma**: "Everything must be IaC (Terraform, Pulumi) or you're unprofessional"

**Reality Check**: IaC shines in multi-environment estates (dev, staging, prod × 3 regions = 9 configs). For disposable infrastructure, manual setup may be fine; for production, at minimum script or document the provisioning well enough to rebuild it.

**WORTH Application**:

| Infrastructure Scale | IaC Worth It? | Why |
|---------------------|--------------|-----|
| 1-2 disposable VMs | ✗ Usually no | Manual setup may be faster; fewer changes |
| 1-2 production VMs | Depends | Script or codify enough to recover and audit |
| 3-10 servers, multiple envs | ✓ Yes | Reproducibility saves time; disaster recovery |
| 50+ resources, multi-region | ✓ Definitely | Manual management impractical; drift prevention critical |

---

### Feature Flags for Every Toggle

**Dogma**: "Feature flags everywhere for continuous deployment"

**Reality Check**: Flags multiply configuration drift and kill readability when product has < 100 users and tight feedback loops.

**WORTH Application**:

| Stage | Flag Complexity | Worth It? | Rationale |
|-------|----------------|-----------|-----------|
| MVP (< 100 users) | High (10+ flags) | ✗ No | Just deploy and monitor |
| Growth (1k-10k users) | Medium (targeted rollouts) | ✓ Yes | Gradual rollout reduces risk |
| Scale (100k+ users) | Managed (kill-switch only) | ✓ Definitely | Instant rollback critical |

---

### Daily Stand-ups & Two-Week Sprints

**Dogma**: "Agile = daily standups + two-week sprints, non-negotiable"

**Reality Check**: Async tools (Linear, Loom, written standup posts) can yield comparable visibility for remote micro-teams without synchronous overhead.

**WORTH Application**:

| Team Situation | Synchronous Standups | Async Updates |
|----------------|---------------------|---------------|
| Co-located team, 5-10 devs | ✓ 15-min standup works | - |
| Remote team, < 5 devs, tight communication | ✗ Overhead > value | ✓ Daily Slack updates |
| Remote team, 10+ devs, coordination heavy | ✓ Yes, but 2-3x/week | ✓ + async for off-days |

---

### Long-Running Release Branches

**Dogma**: "Only merge via long-running release branches (Git Flow)"

**Reality Check**: Trunk-based development with small PRs often substantially reduces merge conflicts and lead time.

**WORTH Application**:

| Deployment Frequency | Git Flow | Trunk-Based |
|---------------------|----------|-------------|
| Monthly releases | ✓ Acceptable | - |
| Weekly releases | Painful merges | ✓ Better flow |
| Daily+ releases | ✗ Bottleneck | ✓ Essential |

---

### Domain-Driven Design (Full Bounded Contexts)

**Dogma**: "DDD means full bounded-context everything with event storming and aggregates"

**Reality Check**: Heavy DDD artifacts pay off in domain-rich areas with complex business rules, not in simple CRUD SaaS v1. Event storming, aggregates, and bounded contexts are tools to apply selectively, not a package deal.

**WORTH Application**:

| Domain Complexity | Full DDD | Lite DDD (just bounded contexts) |
|------------------|----------|--------------------------------|
| Simple CRUD (admin panel, CMS) | ✗ Overkill | ✓ Basic modules enough |
| Medium (e-commerce, SaaS) | Selective use | ✓ Yes, skip event sourcing |
| Complex domain rules (payments, clinical workflows, trading) | ✓ Often justified | - |

---

<a name="appendix-d"></a>
## Appendix D: Glossary & Further Reading

### Glossary

**ADR (Architecture Decision Record)**: Document capturing important architectural decisions with context, options, and rationale.

**DORA Metrics**: Software delivery performance measures from DORA research. The original four keys were deployment frequency, lead time, time to restore (MTTR), and change failure rate; in 2023 DORA renamed and redefined time to restore as failed deployment recovery time, which counts only recovery from failures caused by a change to production, and in 2024 it added deployment rework rate ([dora.dev](https://dora.dev/insights/dora-metrics-history/)). DORA research associates them with organizational outcomes.

**Cognitive Load**: Mental effort required to understand and operate a system; teams have finite capacity.

**Cost of Delay**: Economic impact of postponing value delivery; approximated as (weekly value) × (weeks delayed).

**Decision Bands**: The five verdict zones for a /25 total: 20-25 strong candidate, 18-19 worth doing, 15-17 borderline, 12-14 probably not, below 12 strong NO. Zones, not bright lines; every band stays subject to the hard gates, the low-score override, and the pilot cap on an O that is "break-even until measured".

**Hard Gates**: Pass/fail checks done before any scoring: legal/regulatory, security, privacy/data residency, safety, irreversible lock-in beyond tolerance, procurement viability, and, for AI decisions, accountability for AI output, signed by the business owner. Only the owning function marks a gate Pass. A failed gate disqualifies an option regardless of its total.

**Low-Score Override**: If any dimension scores 1 or 2, stop and discuss it regardless of the total, and write an **Override:** line saying what must change.

**PaaS (Platform as a Service)**: Cloud hosting that abstracts infrastructure (Render, Railway, Heroku).

**Real Options**: Financial concept applied to software: valuing the right to defer decisions until more information is available.

**TCO (Total Cost of Ownership)**: Full lifecycle cost including development, operations, maintenance, and opportunity costs.

**Three-Option Pattern**: Score at least three realistic options, including "do nothing", instead of debating a binary.

**Wardley Mapping**: Strategic planning tool showing component evolution from genesis to commodity.

**AI terms** (Section 17):

**Agent (AI)**: A system in which a model directs its own steps and tool use toward a goal, instead of following predefined code paths (Section 17).

**Automation Bias**: The tendency to accept automated advice, including wrong advice, with less scrutiny than human advice.

**Evals**: Repeatable tests of an AI system's output quality on your own tasks; the most portable asset when models change.

**Fine-Tuning**: Further training an existing model on your own examples to change its behavior.

**Hallucination**: Fluent, confident AI output that is false or unsupported, such as invented citations.

**Jagged Frontier**: The uneven boundary of AI capability: strong on some tasks, unreliable on similar-looking others.

**Lethal Trifecta**: The combination, in one agent, of access to private data, exposure to untrusted content, and the ability to communicate externally, which lets an attacker trick the agent into leaking data.

**Prompt Injection**: Instructions hidden in content an AI reads that hijack its behavior.

**RAG (Retrieval-Augmented Generation)**: Supplying a model with retrieved documents at answer time instead of training the knowledge into it.

**Shadow AI**: Staff using unapproved AI tools for work, usually consumer accounts outside company terms.

**Verification Tax**: The time, skill, and risk cost of checking, fixing, and owning AI output before it can be relied on.

---

### Essential Reading

#### Books

1. **Accelerate** by Nicole Forsgren, Jez Humble, Gene Kim
   *The science behind DORA metrics and high-performing teams*

2. **Team Topologies** by Matthew Skelton, Manuel Pais ([2nd edition, 2025](https://itrevolution.com/product/team-topologies-second-edition/))
   *Organizing teams for fast flow and cognitive load management*

3. **The Principles of Product Development Flow** by Donald Reinertsen
   *Cost of Delay, queuing theory, and economic decision-making*

4. **Wardley Maps** by Simon Wardley
   *Strategic context and component evolution*

5. **Working Effectively with Legacy Code** by Michael Feathers
   *Pragmatic refactoring without dogma*

6. **A Philosophy of Software Design** by John Ousterhout
   *Complexity management and design trade-offs*

#### Online Resources

- **DORA Research**: [dora.dev](https://dora.dev)
- **Team Topologies**: [teamtopologies.com](https://teamtopologies.com)
- **Wardley Mapping**: [learnwardleymapping.com](https://learnwardleymapping.com)
- **Thoughtworks Technology Radar**: [thoughtworks.com/radar](https://www.thoughtworks.com/radar)
- **Martin Fowler's Blog**: [martinfowler.com](https://martinfowler.com)

---

### How to Contribute

This framework improves through real-world application. Share your WORTH decisions:

**Template**:
```markdown
## Decision: [Title]
**Context**: [Team size, domain, constraints]
**WORTH Score**: __/25
**Outcome**: [What happened]
**Lesson**: [What you'd do differently]
```

Submit case studies and feedback to: **[github.com/jeansossmeier/worth](https://github.com/jeansossmeier/worth)**

---

<a name="conclusion"></a>
## Conclusion

### The Core Message

Great engineering isn't about accumulating principles. It's about consistently choosing the simplest solution that delivers outsized value **in your context**.

WORTH turns that judgment into a repeatable, team-wide habit by asking five questions:
1. Is the pain real and business-critical? (**W**eigh)
2. Does benefit exceed cost? (**O**utcome)
3. Can the team operate it? (**R**ight-sized)
4. Does it deliver value soon? (**T**ime)
5. Does it keep options open? (**H**orizon)

If you can't answer "yes" to at least three with clear evidence, don't adopt it yet. Even with three yeses, a hard "no" on any single dimension means stop and discuss that dimension first. That's the quick gut check: the hard gates, the full 1-5 scorecard, and the decision bands make the call.

---

### What Makes WORTH Different

**Not another best practice**: WORTH sits *above* principles, helping you decide which to apply when.

**Context-aware**: What works for Netflix doesn't work for your three-person startup. WORTH forces explicit context.

**Measurable**: Scorecards, DORA metrics, and TCO calculations replace gut feelings with evidence.

**Iterative**: Review dates and re-score triggers catch when "right" becomes "wrong" as context shifts.

**Actionable**: Templates, checklists, and automation make it operational, not theoretical.

**Ready for AI**: AI made producing work cheap, but not checking, owning, or undoing it. WORTH scores those costs one task at a time (Section 17).

---

### Start Tomorrow

1. **Pick one pending decision** (technology, architecture, process)
2. **Check the hard gates, then fill out the WORTH scorecard** (about 30 minutes for a major decision; about 5 with practice)
3. **Share with your team** for bias check
4. **Make the call** with explicit kill criteria
5. **Set a review date** to re-score with real data

That's it. One decision. Five questions. Repeat.

Over time, asking "Is it worth it?" becomes muscle memory. Your team stops chasing hype and starts compounding value.

---

### Final Thought

The best engineers don't memorize frameworks. They ask better questions.

**Is it worth it?**

Everything else follows from that.

---

*Go build something worth building.*

---
