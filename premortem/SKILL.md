---
name: premortem
description: Premortem risk analysis — assume the project already failed, find out why, fix it before launch
argument-hint: "<feature, decision, or project to premortem>"
---

# Premortem Analyst

The user invoked this command with: $ARGUMENTS

## Instructions

Run a structured Premortem on the provided feature, decision, or project.

If no argument is given, ask the user for:
1. **Subject** — what are we premortemed? (feature, launch, decision, redesign, campaign)
2. **Launch / Decision Date** — when does this go live?
3. **Success Definition** — what does "it worked" look like in numbers or behavior?
4. **Stakeholders** — who depends on this succeeding?
5. **Risk Appetite** — Low / Medium / High (default: Medium)

Then run all 8 steps sequentially. Output each step as a markdown section.

---

## The 8 Premortem Steps

### Step 01 — Failure Scenario

Write the opening line of an incident report dated [launch date + 30 days]:

> "The [subject] has failed. Here is what happened."

Then complete the scenario in 3–5 sentences — be specific and vivid. Name metrics, user behaviors, or business outcomes that collapsed. This primes prospective hindsight.

---

### Step 02 — Silent Brainstorm (Failure Causes)

List every plausible cause of failure across these 6 categories. Be specific — no vague entries like "poor execution":

**User & Behavior**
- Adoption failures, misunderstood value, wrong mental model, friction at key steps

**Product & Design**
- UX failures, missing states, confusing flows, broken edge cases, bad microcopy

**Technical & Infrastructure**
- Performance issues, integration failures, data problems, security gaps, scalability

**Business & Organizational**
- Scope creep, misaligned incentives, resource cuts, ownership gaps, schedule pressure

**Market & External**
- Competitor moves, regulatory changes, macro conditions, timing issues

**Communication & Alignment**
- Stakeholder misalignment, unclear success metrics, poor rollout communication

List at minimum 3 items per category.

---

### Step 03 — Risk Scoring

Score the top 10 risks from Step 02:

| # | Risk | Likelihood (1–5) | Impact (1–5) | Score (L×I) | Owner |
|---|------|-----------------|-------------|-------------|-------|

Sort by Score descending. Flag any risk with Score ≥ 16 as **Critical**.

---

### Step 04 — Deep Dive: Top 3 Risks

For each of the top 3 scored risks:

**Risk:** [name]
- **Root Cause:** why would this actually happen?
- **Early Warning Signal:** what would we see in the first 7 days that indicates this is happening?
- **Cascade Effect:** if this fails, what else breaks downstream?
- **Worst Case:** what does full failure look like?

---

### Step 05 — Blind Spots & Assumptions

List 3–5 assumptions the team is currently making that, if wrong, would invalidate the entire approach.

Format: "We are assuming [X]. If instead [Y], the project fails because [Z]."

Then flag which assumptions are currently untested.

---

### Step 06 — Mitigation Actions

For each Critical or High risk (Score ≥ 12), define:

| Risk | Mitigation Action | When | Who | Cost (Low/Med/High) |
|------|------------------|------|-----|---------------------|

Prioritize actions that are cheap to implement before launch and reduce the highest-scoring risks.

---

### Step 07 — Go / No-Go Signals

Define the criteria that should trigger a pause or kill decision:

**Go signals** (safe to launch if):
- [ ] ...
- [ ] ...

**No-Go signals** (pause or kill if):
- [ ] ...
- [ ] ...

**Kill criteria** (stop immediately if post-launch):
- [ ] ...

Be specific — numbers, user behaviors, system states. No vague thresholds.

---

### Step 08 — Updated Plan

Based on the risks and mitigations identified:

1. **Changes to make before launch** (must-do, ranked)
2. **Monitoring to add** (metrics, alerts, dashboards)
3. **Contingency plan** — if the top risk materializes on day 1, what is the response?
4. **Confidence Rating** — Before premortem: X/10 → After premortem: Y/10
5. **Open Questions** — 2–3 things still unresolved that need answers before launch

---

## Output Format

- Use `## Step XX — Title` headers
- Be specific and grounded in the subject context
- Reference earlier steps when building on them
- End with a **1-paragraph Premortem Summary**: top risk, top mitigation, confidence shift
