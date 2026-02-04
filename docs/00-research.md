# 00 - Market Research

> **Agent:** Research Agent
> **Goal:** Gather real market data BEFORE building
> **Priority:** CRITICAL - Execute this phase FIRST

---

## Context

**Product:** TurtlePal
**Domain:** A web application for turtle conservation and education
**Target Audience:** Environmentalists and animal lovers
**Core Entities:** TurtleSpecies, TurtleConservationOrganization

---

## Instructions for Claude Code

This document contains research tasks that YOU (Claude Code) should execute using WebSearch before proceeding with the build phases.

**Why do research first?**
1. Inform pricing decisions with real competitor data
2. Identify gaps in the market to exploit
3. Avoid building features competitors have proven don't work
4. Craft landing page copy that addresses real pain points

---

## Phase 0.1: Competitor Analysis

Use WebSearch to find competitors in the A web application for turtle conservation and education space.

### Search Queries to Execute

```
best A web application for turtle conservation and education apps 2024
```

```
A web application for turtle conservation and education software alternatives comparison
```

```
top A web application for turtle conservation and education tools for turtle_conservation
```

```
A web application for turtle conservation and education app reviews reddit
```

```
G2 A web application for turtle conservation and education software comparison
```

### For Each Competitor Found (aim for 5-8)

Create a file `research/competitors/[name].md` with:

```markdown
# [Competitor Name]

## Overview
- **URL:**
- **Founded:**
- **Tagline:**

## Pricing
| Plan | Price | Key Features |
|------|-------|--------------|
| Free | $0 | ... |
| Pro | $X/mo | ... |
| Enterprise | Custom | ... |

## Key Features
1.
2.
3.

## Strengths (what users love)
-

## Weaknesses (common complaints)
-

## Our Opportunity
How TurtlePal can be better:
-
```

### Summary File

After researching competitors, create `research/competitor-summary.md`:

```markdown
# Competitor Summary for TurtlePal

## Market Leaders
1. [Name] - [Market position / USP]

## Pricing Landscape
| Competitor | Lowest | Mid | Highest |
|------------|--------|-----|---------|
| ... | ... | ... | ... |

## Feature Comparison Matrix
| Feature | Us | Comp 1 | Comp 2 | Comp 3 |
|---------|----|----|----|----|
| auth | Yes | ? | ? | ? |

## Gaps We Can Exploit
1.
2.
3.

## Recommended Positioning
[How should TurtlePal position itself?]
```

---

## Phase 0.2: Market Data

Research the market size and trends for A web application for turtle conservation and education.

### Search Queries to Execute

```
A web application for turtle conservation and education market size 2024 TAM
```

```
A web application for turtle conservation and education industry growth rate CAGR
```

```
turtle_conservation software market trends 2024 2025
```

```
A web application for turtle conservation and education market report statistics
```

### Save to `research/market-data.md`

```markdown
# Market Data for TurtlePal

## Market Size
- **TAM (Total Addressable Market):** $X billion
  - Source: [URL]
- **SAM (Serviceable Addressable Market):** $X billion
- **SOM (Serviceable Obtainable Market):** $X million

## Growth
- **CAGR:** X%
- **Expected market size by 2028:** $X billion

## Key Trends
1. [Trend 1] - [What it means for TurtlePal]
2. [Trend 2] - [What it means for TurtlePal]
3. [Trend 3] - [What it means for TurtlePal]

## Target Segment
- **Primary:** Environmentalists and animal lovers
- **Size:** X million potential users
- **Willingness to pay:** $X-$Y/month

## Sources
- [Source 1](URL)
- [Source 2](URL)
```

---

## Phase 0.3: Pricing Benchmark

Research competitor pricing in detail.

### Search Queries to Execute

```
turtle_conservation software pricing comparison
```

```
turtle_conservation SaaS pricing models
```

```
how much do turtle_conservation tools cost
```

```
turtle_conservation freemium vs paid conversion rates
```

### Save to `research/pricing-strategy.md`

```markdown
# Pricing Strategy for TurtlePal

## Competitor Pricing Analysis

| Competitor | Free Tier | Starter | Pro | Enterprise |
|------------|-----------|---------|-----|------------|
| ... | Yes/No | $X | $Y | Custom |

## Common Pricing Models
- [ ] Per user/seat
- [ ] Per project/workspace
- [ ] Usage-based (API calls, storage, etc.)
- [ ] Feature-based tiers
- [ ] Flat rate

## Pricing Sweet Spot
Based on competitor analysis:
- **Free tier:** [What to include]
- **Starter:** $X-Y/month (comparable to...)
- **Pro:** $X-Y/month (comparable to...)

## Recommended Pricing for TurtlePal

| Plan | Price | Rationale |
|------|-------|-----------|
| Free | $0 | [Why this limit?] |
| Starter | $X/mo | [Competitive with...] |
| Pro | $Y/mo | [Includes...] |

## Monetization Strategy
1. Primary revenue: [subscriptions / usage / one-time]
2. Upsell path: [Free → Starter → Pro]
3. Key conversion trigger: [What makes free users upgrade?]
```

---

## Phase 0.4: User Pain Points

Research what users hate about existing solutions.

### Search Queries to Execute

```
turtle_conservation software frustrations reddit
```

```
turtle_conservation app complaints reviews
```

```
what users hate about turtle_conservation tools
```

```
turtle_conservation software missing features
```

```
auth feature requests turtle_conservation
```

### Save to `research/pain-points.md`

```markdown
# User Pain Points Analysis

## Top 10 Complaints About Existing A web application for turtle conservation and education Tools

| # | Pain Point | Source | Severity | TurtlePal Solution |
|---|------------|--------|----------|------------------------|
| 1 | ... | Reddit/G2/etc | High/Med/Low | ... |
| 2 | ... | ... | ... | ... |

## Feature Requests Users Want
1. [Feature] - [How many mention it?]
2. ...

## Pricing Complaints
- [What users say about competitor pricing]

## UX Frustrations
- [Common UX complaints]

## Support Issues
- [Common support complaints]

## Our Differentiation Opportunities
Based on these pain points, TurtlePal should:
1. [Do this differently]
2. [Focus on this]
3. [Avoid this mistake]
```

---

## Validation Checklist

Before proceeding to `01-setup.md`, verify:

- [ ] 5+ competitors documented in `research/competitors/`
- [ ] `research/competitor-summary.md` created with comparison matrix
- [ ] `research/market-data.md` has TAM/SAM/SOM with sources
- [ ] `research/pricing-strategy.md` has recommended pricing
- [ ] `research/pain-points.md` has top 10 pain points

---

## Using Research in Later Phases

Reference these files in:
- **09-landing.md:** Use competitor positioning for copy
- **Pricing page:** Use pricing-strategy.md recommendations
- **Feature prioritization:** Address top pain points first
- **Marketing:** Reference market size for pitch decks

---

**Next Phase:** `docs/01-setup.md`
