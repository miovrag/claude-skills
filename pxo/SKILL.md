---
name: pxo
description: PXO Workflow Analysis - full 13-step product experience analysis
argument-hint: "<problem statement>"
---

# PXO Workflow Analyzer

The user invoked this command with: $ARGUMENTS

## Instructions

Run a full PXO (Product Experience OS) Workflow Analysis based on the provided problem statement.

If no argument is given, ask the user for:
1. Problem Statement (required)
2. User Segment (required)
3. Metric at Risk (required)
4. Hypothesis — optional: "If we improve [X], for [segment], then [metric] will improve, because [behavior]"
5. Risk Level — Low / Medium / High (default: Medium)

Then run all 13 steps sequentially. For each step, output a markdown section with the step number and title as a header, then the analysis content.

---

## The 13 PXO Steps

### 00. Problem Hypothesis
- Refined Problem Statement
- Impacted User Segment
- Business Risk
- Metric at Risk
- Current User Alternatives
- Why Now
- Validated hypothesis statement
- Risk Level justification

### 01. Empathy Engine
- Think: what users are thinking
- Feel: emotional responses and frustrations
- Say: what users would literally say
- Do: observable behaviors
- Hidden Frictions
- Emotional Risks
- Trust Barriers

### 02. JTBD Extraction
- Core Functional Job
- Emotional Job
- Social Job
- Job Steps (chronological)
- Success Definition
- Failure Definition

### 03. Constraint Mapping
- Technical Constraints
- Data Constraints
- Legal/Compliance
- Organizational Constraints
- Edge Cases

### 04. Persona Roles
Three archetypes in 2-3 sentences each:
- Naive (first-time user)
- Medium (regular user)
- Expert (power user)

### 05. Behavioral Trigger Design
- Activation Trigger
- Aha Moment
- Competence Moment
- Control Moment

### 06. Information Architecture
- IA Tree (Level 1 → Level 2 → Level 3)
- Naive Task Flow
- Medium Task Flow
- Expert Task Flow

### 07. Failure Mode Design
- Failure Modes with specific scenarios
- System Recovery Design per failure
- Graceful Degradation

### 08. Metric Definition
- Primary Metric
- Secondary Metrics (2-3)
- Early Signal (7-day)
- Long-Term Signal (30-day)
- Leading Indicators

### 09. Low-Fi Specification
- Layout Structure
- Priority Hierarchy
- CTA Logic
- State Logic (loading, empty, error, success)
- Error States
- Empty States

### 10. Heuristic & Cognitive Audit
Format: Heuristic → Violation → Improvement
- Nielsen's 10 Heuristics
- Cognitive Load Theory
- Progressive Disclosure
- Accessibility (WCAG 2.1)
- Error Prevention

### 11. High-Fi Execution
- Component Inventory with all states
- Interaction Details
- Motion Logic
- Microcopy (every label, placeholder, CTA, error, empty state)
- Visual Hierarchy
- Edge Case Rendering
- Responsive Behavior

### 12. Validation Loop
Before Development:
- Cognitive load test
- Job completion speed measurement
- Risk perception validation
- What to prototype first

After Release:
- Behavior changes to monitor
- Metric thresholds
- Unexpected failure modes
- Iteration triggers

---

## Output Format

- Use markdown with `## Step XX — Title` headers
- Be specific and grounded in the problem context
- Build on previous steps — reference them when relevant
- After all 13 steps, write a concise **Executive Summary** (4-5 sentences)
