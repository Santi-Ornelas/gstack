# Santiago's Projects Guide — gstack Playbook

> This file is your personalized map of the entire gstack toolkit.
> It answers: "Which skill do I use, when, and exactly how?"
> Two sections: Synthesis Intelligence and DolarApp KYC Case Study.

---

## SECTION A: Synthesis Intelligence
### AI Research Platform for Mexican Private Markets

**What you're building:** A platform that aggregates private market data in Mexico (M&A deals, private debt, real estate transactions) and structures it into research-ready outputs for financial professionals. The 70-80% of transactions that happen off-exchange, made searchable and analyzable. Starting wedge: connecting buyers and sellers in private deals.

---

### The Sprint Sequence for Synthesis Intelligence

Follow this order. Each step feeds into the next.

```
/office-hours → /autoplan → [build] → /review → /qa → /cso → /ship → /land-and-deploy → /canary
```

---

### Step 0: Before You Write a Single Line of Code

**Skill: `/office-hours`**

This is the most underrated skill in the whole toolkit. Run it before building anything.

It will ask you 6 questions that expose the real shape of your product:
1. **Demand reality** — Who is suffering right now without this? Name specific people.
2. **Status quo** — What do they do today instead? How painful is it exactly?
3. **Specificity** — Is this a market-wide problem or a Mexico-City-banker problem?
4. **Narrowest wedge** — What is the smallest version you can ship tomorrow that someone would pay for?
5. **Observable patterns** — What data exists right now that nobody has aggregated?
6. **Future-fit** — Is this a feature or a platform?

**How to run it for Synthesis Intelligence:**
```
/office-hours

When Claude asks: describe the problem as:
"Mexican bankers and financial professionals spend 15-20 hours/week manually researching
private market transactions. There's no Bloomberg for private Mexican markets. I want to
build a platform that aggregates this data and outputs structured research packs — the
equivalent of what an analyst would produce in 3 days, in 3 minutes."
```

**What you'll get:** A design doc (`DESIGN.md` or similar) with the sharpened vision, narrowest viable wedge, 3 implementation approaches with effort estimates. This file is automatically read by all downstream skills.

---

### Step 1: Challenge Your Own Assumptions

**Skill: `/plan-ceo-review`**

After office-hours, run this. It's a YC partner session — it will push back on your scope.

Four modes you can choose from:
- **Scope Expansion** — "What if this is actually a Bloomberg for all of LatAm?"
- **Selective Expansion** — "Keep the Mexico focus, but add the deal-matching layer now"
- **Hold Scope** — "No. Build the buyer-seller matching only. Prove it works first."
- **Scope Reduction** — "Strip everything except the one thing bankers actually pay for"

**How to run:**
```
/plan-ceo-review
```
Claude will read your design doc and challenge it. Choose "Hold Scope" or "Selective Expansion" mode — you're early, specificity wins.

---

### Step 2: Lock the Architecture

**Skill: `/plan-eng-review`**

After the CEO review approves the scope, this skill becomes your engineering manager. It will produce:
- Data flow diagrams (how data goes from source → ingestion → enrichment → output)
- Database schema decisions
- API design
- Edge cases (what happens when a data source goes down?)
- Test plan

**Critical questions it will answer for Synthesis Intelligence:**
- Where does the private market data come from? (Scraping, partnerships, manual input?)
- How do you verify data reliability? (The key value proposition)
- How does the buyer-seller matching algorithm work?
- What's the output format? (The "PPT boxes" — structured research packs)

**How to run:**
```
/plan-eng-review
```

---

### Step 3: Design the Interface

**Skill: `/design-consultation`**

Financial professionals are used to Bloomberg terminals — dense, information-rich, professional. But you want this to be the tool they actually use, not dread.

This skill will:
- Research what financial data UI looks like (Bloomberg, Capital IQ, Refinitiv)
- Propose typography, color palette, spacing
- Generate font/color preview pages
- Create a `DESIGN.md` with your complete design system

**How to run:**
```
/design-consultation

Tell Claude: "This is a financial research platform for Mexican investment bankers and
private equity professionals. It should feel premium and information-dense, not like
a consumer app. Think Bloomberg meets modern SaaS. Primary users are 35-55 year old
finance professionals who trust dense interfaces."
```

---

### Step 4: Build

Run `/autoplan` before you start implementing each feature. It chains all three plan reviews automatically and only asks you the subjective decisions.

```
/autoplan
```

**As you build, use these skills:**

| Problem | Skill | When |
|---------|-------|------|
| "Something broke and I don't know why" | `/investigate` | Any bug, any time |
| "I finished a feature, let me check it" | `/review` | After each feature |
| "Does the UI look right?" | `/design-review` | After building each screen |
| "Let me see it in the browser" | `/qa https://localhost:3000` | While building |

---

### Step 5: Security Audit (Non-Negotiable for Fintech)

**Skill: `/cso`**

Before you put any real user data in Synthesis Intelligence, run a full security audit.

Financial platforms are high-value targets. The `/cso` skill runs:
- **Secrets archaeology** — checks if any API keys, passwords, or tokens were accidentally committed to git
- **OWASP Top 10** — the 10 most exploited vulnerabilities (SQL injection, XSS, auth failures)
- **STRIDE threat modeling** — who would attack this and how?
- **Supply chain** — are any of your dependencies compromised?
- **LLM/AI security** — if you use Claude in your platform, are there prompt injection risks?

**How to run:**
```
/cso

Run in "comprehensive" mode first (2/10 confidence bar = catches everything, more noise).
Then run daily in "daily" mode (8/10 bar = zero noise, only high-confidence findings).
```

**What Synthesis Intelligence specifically needs checked:**
- User authentication (financial data must be access-controlled)
- Data source API keys (if you're pulling from paid data providers)
- The matching algorithm output (ensure no PII leaks in API responses)
- If you're using Claude API: prompt injection prevention

---

### Step 6: Test the Browser Experience

**Skill: `/qa` + `/browse`**

```
/qa https://your-staging-url.com

Tell Claude: "Test the main research workflow:
1. User logs in
2. Searches for a company by name
3. Views the research pack
4. Downloads/exports it
Find any broken flows, missing data, or UI issues."
```

For visual polish:
```
/design-review
```

---

### Step 7: Ship

```
/ship
```
This will:
1. Sync main branch
2. Run your tests
3. Bump version number
4. Update CHANGELOG
5. Open a PR

Then when PR is approved:
```
/land-and-deploy
```

After deploy:
```
/canary
```
Watches production for errors for the first hour after deploy.

---

### Step 8: Weekly Rhythm

Every Friday (or end of sprint):
```
/retro
```
Shows you: what shipped, commit velocity, test health, who did what.

---

### Synthesis Intelligence: Skills Priority List

If you can only learn 5 skills right now, start with these:

1. **`/office-hours`** — Sharpen the product thesis (do this TODAY)
2. **`/autoplan`** — Full plan review before every feature
3. **`/review`** — Catch bugs before they reach production
4. **`/cso`** — Security before any real user data
5. **`/ship`** — Automated release workflow

---

---

## SECTION B: DolarApp KYC Case Study
### 60-Hour Case Interview — Overdeliver Strategy

**The problem:** ARQ's KYC funnel pass rate is declining. You have two datasets: `KYC_Details` and `KYC_Summary`. You need to find the root cause and propose solutions. Deliverable: PDF report.

**The opportunity:** This is a case study, but treat it like a real investigation. Overdelivering means:
1. Going deeper than the data shows (root causes, not just symptoms)
2. Providing specific, prioritized, cost-estimated recommendations
3. Making it visually compelling (not just a table of numbers)
4. Thinking like a product + ops + risk person simultaneously

---

### The Investigation Framework (Stolen from `/investigate`)

The `/investigate` skill has an "Iron Law": **never propose a fix without understanding the root cause first.** Apply this framework to the KYC case.

**Phase 1: Investigate — Map the funnel**

Before touching data, map every step:
```
User starts KYC
    ↓
Sees instructions
    ↓
Uploads ID photo
    ↓ ← Where do people drop off?
ID passes/fails verification
    ↓
Takes selfie
    ↓ ← Where do people drop off?
Liveness check passes/fails
    ↓ ← Where do people drop off?
Identity match passes/fails
    ↓ ← Where do people drop off?
Watchlist screening passes/fails
    ↓
KYC complete ✓
```

**Phase 2: Analyze — The KYC_Summary dataset**

Use the `xlsx` skill to analyze `KYC_Summary`:
```
/xlsx

"Analyze this KYC funnel summary data. For each step in the funnel, calculate:
1. Drop-off rate (% of users who started this step but didn't complete)
2. Failure rate (% who completed but failed the check)
3. Abandonment rate (% who left and never came back)
Show me a funnel visualization and rank steps by total user loss."
```

Key metrics to extract:
- **Overall pass rate** (and its trend over time)
- **Step-by-step completion rates** (where is the biggest drop?)
- **Failure type breakdown** (technical failure vs. user error vs. legitimate rejection)
- **Time to complete** (are people taking too long and timing out?)

**Phase 3: Hypothesize — The KYC_Details dataset**

Use `KYC_Details` to test your hypotheses:

| Hypothesis | What to look for in the data |
|-----------|------------------------------|
| "ID photo quality is the problem" | Failure reason codes on ID verification step |
| "Liveness check is too strict" | False positive rate on liveness check |
| "Users are confused by instructions" | Time spent on each step, retry counts |
| "The partner service has degraded" | Error rates over time, especially recent trend |
| "Specific device types are failing" | Failure rates by device/OS/browser |
| "Specific ID types are failing" | Failure rates by document type |
| "Geographic patterns" | Failure rates by region/state |

**Phase 4: Implement — Structure your recommendations**

Borrow the gstack test tier structure for your recommendations:

**Tier 1 — Critical (Fix immediately, high impact):**
- Root cause of the largest funnel drop
- Quick wins that don't require partner changes

**Tier 2 — Medium (Fix in 2-4 weeks):**
- UX improvements to reduce user error
- Retry logic improvements

**Tier 3 — Strategic (Fix in 1-3 months):**
- Partner evaluation (is the external KYC provider underperforming?)
- Alternative verification methods

---

### The DolarApp Deliverable: How to Overdeliver

**What they asked for:** A PDF report or presentation with findings and recommendations.

**What overdelivering looks like (borrow from `/qa-only` report format):**

1. **Executive Summary** (1 page) — KYC health score (like a QA health score), top 3 findings, top 3 recommendations. A busy C-suite should be able to act on this page alone.

2. **Funnel Visualization** — A step-by-step funnel chart showing where users drop off. Not a table — a visual funnel. Most case studies miss this.

3. **Root Cause Analysis** (the `investigate` methodology applied to data):
   - Finding → Evidence → Root Cause → Mechanism
   - "We see 23% drop-off at liveness check" → "Error code data shows 67% are `LIGHTING_POOR`" → "Root cause: users are in dark environments" → "Mechanism: liveness SDK requires minimum lux level"

4. **Prioritized Recommendations** — For each recommendation:
   - What to do (specific, not vague)
   - Expected impact on pass rate (be precise if data allows)
   - Effort estimate (Low/Medium/High)
   - Who owns it (Product, Engineering, Ops, or KYC Partner)

5. **Risk Section** — What happens if you don't fix this? What's the regulatory exposure? (Financial platforms have compliance obligations around KYC.)

6. **Appendix: Data Analysis** — Show your work. Include the actual calculations, charts, raw numbers.

---

### Building the DolarApp Deliverable with gstack Skills

**Step 1: Analyze the data**
```
/xlsx

Attach KYC_Summary.xlsx and KYC_Details.xlsx
"Analyze these KYC funnel datasets. Calculate: overall pass rate trend,
step-by-step drop-off rates, failure reason breakdowns, any time-series trends.
Create charts and produce a structured analysis."
```

**Step 2: Create the presentation**
```
/pptx

"Create a professional case study presentation for a fintech startup (DolarApp/ARQ).
Title: KYC Funnel Analysis — Root Cause & Recommendations
Slides:
1. Executive Summary (health score, top 3 findings, top 3 recs)
2. Funnel visualization (step-by-step drop-off rates)
3-5. Key findings with evidence (one per slide)
6-8. Prioritized recommendations (one per slide, with effort/impact matrix)
9. Risks of inaction
10. Appendix: data methodology

Style: Professional fintech, dark blue/white, data-forward.
Audience: DolarApp founders and product team."
```

**Step 3: Export to PDF**
```
/pdf

Convert the presentation to PDF for submission.
```

---

### KYC-Specific Investigation Checklist

These are the questions your analysis should answer. Check off each one:

**Funnel drop-off:**
- [ ] Which step has the highest absolute user loss?
- [ ] Which step has the highest failure rate (vs. abandonment)?
- [ ] Has the drop-off pattern changed recently? (When did it start declining?)

**ID Verification:**
- [ ] What % fail ID verification?
- [ ] What document types fail most? (Passport vs. INE vs. driver's license)
- [ ] Is it a technical failure or a rejected document?

**Liveness Check:**
- [ ] What % fail liveness?
- [ ] What are the most common failure reasons?
- [ ] Is this step causing users to abandon vs. retry?

**Identity Match:**
- [ ] What % fail the selfie-to-ID match?
- [ ] Is there a demographic pattern?

**Watchlist Screening:**
- [ ] How many users are flagged?
- [ ] False positive rate (if available)?

**Time & UX:**
- [ ] How long does the average user take?
- [ ] Are there users who take >10 minutes and fail?
- [ ] What's the retry behavior? (Do users who fail once usually succeed on retry?)

**Partner / External:**
- [ ] Has the external partner's error rate changed over time?
- [ ] Are there specific error codes that correlate with the decline?

---

### The "Overdeliver" Differentiator

Most case study candidates will do: look at data → find biggest drop → recommend fixing it.

**You will do:**
1. Map the complete funnel mechanistically (not just statistically)
2. Distinguish between **user error**, **technical failure**, and **partner degradation** (three different solutions)
3. Quantify the business impact of the pass rate decline (lost users × lifetime value)
4. Propose an A/B testing framework for validating your recommended fixes
5. Include a "quick win" that DolarApp can implement in 48 hours (shows you think about execution, not just diagnosis)
6. Address the compliance angle (what are the regulatory implications of KYC failures in Mexico?)

---

### Timeline (60 hours)

| Hours | Activity | gstack skill |
|-------|----------|-------------|
| 0-4 | Data loading, initial exploration | `/xlsx` |
| 4-8 | Funnel mapping and drop-off analysis | `/xlsx` |
| 8-12 | Root cause investigation, hypothesis testing | (manual analysis) |
| 12-20 | Recommendation development | `/office-hours` for structuring thinking |
| 20-30 | Presentation creation | `/pptx` |
| 30-36 | Visual polish and design | `/design-review` concept |
| 36-48 | Written analysis and appendix | `/docx` |
| 48-56 | PDF conversion and final review | `/pdf` |
| 56-60 | Buffer for revisions | — |

---

*This guide lives at: `/PROJECTS_GUIDE.md`*
*Dictionary reference: `/DICTIONARY.md`*
*Last updated: 2026-03-27*
