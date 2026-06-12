---
name: ai-design-patterns
description: "Best-practice guidance for designing, building, auditing, or reviewing AI-powered interfaces and human-AI interaction patterns — covering trust-building, transparency, explainability, user control, AI onboarding, confidence and uncertainty communication, error recovery, feedback loops, progressive disclosure of AI capabilities, AI output presentation, and ethical AI design. Use this skill whenever the user is building or reviewing any interface that incorporates AI or ML — chatbots, AI assistants, recommendation systems, AI-generated content interfaces, AI search, copilot features, autocomplete driven by ML, smart suggestions, AI image generation UIs, AI writing tools, or any feature where the system makes predictions, inferences, or decisions on behalf of the user. Trigger on words like AI interface, AI UX, AI design, human-AI interaction, chatbot, conversational UI, chat interface, AI assistant, AI copilot, AI suggestions, smart suggestions, AI recommendations, trust in AI, AI transparency, explainability, XAI, explainable AI, AI confidence, uncertainty communication, AI error, AI onboarding, AI output, AI-generated content, AI writing, generative AI, LLM interface, prompt input, AI design patterns, responsible AI, ethical AI — or when a component uses ML predictions, shows AI-generated content, or requires the user to evaluate, accept, or reject AI output."
metadata:
  tags: ai-ux, human-ai-interaction, trust, transparency, explainability, user-control, ai-onboarding, uncertainty-communication, error-recovery, progressive-disclosure, generative-ai, llm-interface, ethical-ai, responsible-ai, chatbot-ux, copilot-ux
sources:
  - Google PAIR — People + AI Guidebook: https://pair.withgoogle.com/guidebook
  - IF (Projects by IF) — Data Design Patterns Catalogue: https://catalogue.projectsbyif.com/
  - Shape of AI — UX Patterns for Artificial Intelligence: https://www.shapeof.ai/
  - Microsoft Research — Guidelines for Human-AI Interaction (HAI Guidelines, CHI 2019): https://www.microsoft.com/en-us/research/project/guidelines-for-human-ai-interaction/
related-skills:
  - prompt-engineering (prompting is the primary user input in AI interfaces)
  - web-forms-ux (AI input fields and chat inputs are a form of form UX)
---

## When to use

Load this skill when you are:

- Designing or reviewing a chatbot, conversational UI, or AI assistant interface
- Building a copilot feature, smart suggestions, or inline AI assistance
- Adding AI-generated content, AI writing tools, or AI image generation to a product
- Designing any component that surfaces ML predictions, confidence scores, or recommendations
- Evaluating trust, transparency, or explainability in an AI feature
- Onboarding users to a new AI capability or changing AI behavior
- Handling AI errors, low-confidence states, or fallback to human review
- Designing the output presentation layer for LLM or generative AI responses
- Reviewing an AI feature for responsible/ethical design concerns

---

## How to use this skill

1. Identify the interaction phase: **Initially** (first use), **During Interaction** (ongoing), **When Wrong** (error/failure), or **Over Time** (adaptation and trust calibration).
2. Map each surface to the relevant pattern categories: Wayfinders, Inputs, Tuners, Governors, Trust Builders, Identifiers.
3. Apply the core principles from the checklist below, then validate against the anti-patterns list.
4. Use the quick-reference design review at the end before any AI feature ships.

---

## Core concept

AI interfaces differ fundamentally from deterministic software: they produce probabilistic outputs, make mistakes in non-obvious ways, and change their behavior over time. Users cannot easily form accurate mental models of AI capability boundaries. This creates three layered design problems:

**Trust calibration** — Users must neither over-trust (treating AI as infallible) nor under-trust (ignoring genuinely useful AI output). Both failure modes cause harm.

**Transparency and agency** — Users deserve to understand what the AI did, why, and how to override, challenge, or correct it. Removing this agency shifts accountability inappropriately onto users.

**Graceful degradation** — AI systems will be wrong. The design of failure states matters as much as the design of success states.

---

## Core principles

### 1. Make AI capabilities and limits legible from the start

Before a user interacts with an AI feature, they need an accurate model of what it can and cannot do. Interfaces that hide limitations create false confidence, which leads to over-reliance and poor error recovery.

- Show sample outputs or a gallery of representative generations before first use (Shape of AI: Gallery pattern)
- State the scope boundary explicitly: what topics, file types, tasks, or domains the AI handles and which it does not (IF Catalogue: Show capabilities and limitations)
- Communicate processing time and accuracy expectations upfront (IF Catalogue: Show competence)

### 2. Give users meaningful control over automation level

One-size automation is a design mistake. Different users in different contexts want different amounts of AI involvement. The interface should make the automation level visible and adjustable.

- Offer at least three modes: fully automated, suggesting (AI proposes, human approves), and manual/off (IF Catalogue: Control the Level of Automation)
- Let users pause, snooze, or disable AI features at any time without data loss or workflow interruption (IF Catalogue: Stop and Takeover; Shape of AI: Controls pattern)
- When automation is about to take a consequential action, notify the user in advance with enough time to cancel (IF Catalogue: Notice of Upcoming Action)

### 3. Surface confidence and uncertainty — carefully

Showing confidence scores helps users calibrate trust, but confidence displays must be designed with caution: they are subjective, can shift accountability to users in high-stakes contexts, and may create unwarranted certainty.

- Display confidence only when it meaningfully changes the action a user should take (IF Catalogue: Show Confidence)
- Use contextual language over raw percentages where possible ("This is a strong match" vs. "87%")
- Pair low-confidence indicators with clear next steps: what the user should check or do next
- Never suppress uncertainty — showing false confidence is an anti-pattern

### 4. Explain decisions progressively, not all at once

Most users do not need a full audit trail for every AI decision. Progressive disclosure lets users go as deep as they want without overwhelming those who need only the top-level answer.

- Layer explanations: summary first, then supporting evidence on demand, then full model reasoning on further drill-down (IF Catalogue: Progressive Disclosure)
- Provide counterfactual explanations when helpful: "If X were different, the outcome would change" (IF Catalogue: Counterfactual Explanation)
- Always give users a path to question or contest an AI decision, even when the explanation is minimal (IF Catalogue: Query an Event)

### 5. Label AI-generated content — always

Users have a right to know when they are reading, viewing, or acting on AI-generated output. Unmarked AI content erodes trust at both the individual and systemic level.

- Label AI-generated text, images, and summaries at the point of presentation (Shape of AI: Disclosure, Watermark; IF Catalogue: Watermarking)
- For high-stakes domains (health, legal, financial), reinforce AI labeling with a caveat about professional review (Shape of AI: Caveat)
- Provide epistemic disclosure — who trained the model, on what data, when it was last updated — when users are making important decisions based on AI output (IF Catalogue: Epistemic Disclosure)

### 6. Design for the blank-canvas problem in AI inputs

AI input fields (prompts) cause more anxiety than traditional form fields because the space of valid inputs is undefined. Users do not know what to ask, how to ask it, or whether their input is good.

- Provide concrete starter prompts, templates, or suggestions to reduce blank-canvas paralysis (Shape of AI: Suggestions, Templates, Gallery)
- Offer a follow-up mechanism that helps users clarify underdeveloped prompts (Shape of AI: Follow Up)
- Show a "randomize" option for low-stakes, exploratory features — it lowers the activation energy for first interaction (Shape of AI: Randomize)
- Expose prompt enhancement options that help users improve their own prompts without replacing their intent (Shape of AI: Prompt Enhancer)

### 7. Keep humans in the loop for consequential decisions

The more consequential an AI decision, the more human oversight is required. Autonomous AI action is appropriate for low-stakes, reversible, and well-understood domains. Elsewhere, the design must provide meaningful checkpoints.

- Show an action plan before multi-step AI workflows execute, allowing review and modification (Shape of AI: Action Plan pattern)
- Use draft mode for AI outputs that will be published, sent, or committed — treat AI output as a draft by default, not a final action (Shape of AI: Draft Mode)
- Provide branching and versioning for AI-generated content so users can compare alternatives and backtrack (Shape of AI: Branches, Variations)
- Escalate to a human when the system cannot proceed reliably (IF Catalogue: Talk to a Person)

### 8. Build trust through behavior, not claims

Claiming an AI is "smart," "accurate," or "safe" in marketing copy builds zero trust. Trust is earned through consistent, transparent, predictable behavior over time.

- Show citations and references for factual AI claims (Shape of AI: Citations, References)
- Maintain a footprints or activity log so users can audit what the AI has done on their behalf (Shape of AI: Footprints; IF Catalogue: Activity Log)
- Provide memory controls — let users see, edit, and delete what the AI remembers about them (Shape of AI: Memory; IF Catalogue: Forget Learning)
- After errors, explain what went wrong, whether it has been corrected, and how to prevent recurrence (Microsoft HAI Guidelines: "When Wrong" phase)

### 9. Design AI error states as first-class experiences

AI errors are not edge cases. They are frequent, often subtle, and can be more damaging than traditional software errors because users may not notice them.

- Design error states that explain what happened without blaming the user
- Give users concrete recovery paths: retry, modify input, escalate to human, report error
- Provide feedback loops so users can signal errors the system may not have detected itself (Microsoft HAI Guidelines: support correction)
- Never silently fail — always tell the user when the AI could not complete a task or when its output may be unreliable

### 10. Respect user data and consent explicitly

AI features often require more data than traditional software — behavioral patterns, preferences, past interactions, personal content. Users must understand what data is used and must be able to control it.

- Request data permissions at the point of use, not at sign-up (IF Catalogue: Just-in-time consent)
- Show concretely how consent has been used and what data is actively shared (IF Catalogue: Show how consent has been used)
- Give users clear data ownership and the right to delete training data derived from their interactions (Shape of AI: Data Ownership)
- Provide incognito/private modes that disable data collection for sensitive sessions (Shape of AI: Incognito Mode)

### 11. Establish a consistent AI identity

AI without a clear identity creates confusion: is this a person? a tool? Users deserve to know what they are talking to, and the AI should maintain a consistent, trustworthy persona.

- Give the AI a distinct name, avatar, and visual identity that signals it is AI, not human (Shape of AI: Name, Avatar, Color, Iconography)
- Maintain a consistent personality and voice across all touchpoints (Shape of AI: Personality)
- Never allow the AI to claim to be human when sincerely asked (responsible AI baseline)

### 12. Adapt over time — but make adaptation visible

AI systems that silently change their behavior undermine trust. Adaptation should be communicated so users understand why recommendations, outputs, or behavior are shifting.

- Notify users when the AI has updated its model, learned from their feedback, or changed its recommendations (Microsoft HAI Guidelines: "Over Time" phase)
- Let users inspect and reset learned preferences (IF Catalogue: Forget Learning; Shape of AI: Memory)
- Surface model provenance when AI behavior changes: version, training date, what changed (IF Catalogue: Model Provenance, Digital Proof for AI)

---

## Pattern guidance

### Wayfinder patterns (onboarding AI input)
Use these when users are encountering an AI feature for the first time or returning after a gap.

| Pattern | When to use |
|---|---|
| Gallery | Show sample outputs before first interaction to calibrate expectations |
| Suggestions | Combat blank-canvas paralysis with curated prompt starters |
| Templates | Provide fillable prompt structures for common use cases |
| Follow Up | Clarify when initial prompts are too vague to produce quality output |
| Randomize | Low-stakes entry point for exploratory or creative features |
| Nudges | Surface AI affordances users have not yet discovered |

### Governor patterns (human oversight)
Use these to keep humans in control of consequential AI actions.

| Pattern | When to use |
|---|---|
| Action Plan | Multi-step autonomous AI workflows — always show plan before execution |
| Draft Mode | AI outputs that will be published or sent — treat as draft by default |
| Branches / Variations | When users need to compare or roll back AI-generated alternatives |
| Stream of Thought | When reasoning transparency builds trust (e.g., complex analysis) |
| Memory | Long-running AI assistants that learn user preferences |
| Cost Estimates | AI actions that consume credits, time, or money — show before execution |
| Sample Response | Allow users to preview AI behavior before committing to a mode |

### Trust builder patterns
Use these to establish and maintain trustworthiness across the user lifecycle.

| Pattern | When to use |
|---|---|
| Disclosure | Label all AI-generated content at point of presentation |
| Citations | Surface sources for factual claims in AI output |
| Caveat | Add professional-review reminders for high-stakes AI output |
| Watermark | Mark AI-generated media (images, audio, video) |
| Consent | Explicit permission before using personal data in AI training |
| Data Ownership | Let users export, delete, or correct data the AI uses about them |
| Footprints / Activity Log | Auditable record of AI actions taken on the user's behalf |
| Incognito Mode | Private session without data collection or learning |

### Confidence and explainability patterns
Use these to communicate how reliable an AI output is and why it was produced.

| Pattern | When to use |
|---|---|
| Show Confidence | When confidence level materially changes user action — use cautiously |
| Progressive Disclosure | Layer explanation depth: summary → evidence → full reasoning |
| Counterfactual Explanation | "If X were different, outcome would change" — actionable and intuitive |
| Epistemic Disclosure | Show model training, data source, funding when stakes are high |
| Model Provenance | Communicate model version, complexity, and update history |

### Control and correction patterns
Use these to give users agency over AI behavior and to handle errors gracefully.

| Pattern | When to use |
|---|---|
| Stop and Takeover | Always — every AI-driven workflow needs a manual override path |
| Control the Level of Automation | When different user segments want different automation depths |
| Notice of Upcoming Action | Before consequential automated actions: alerts with cancel option |
| Query an Event | After automated decisions: let users challenge or investigate |
| Talk to a Person | When AI confidence is low or stakes are too high for autonomous resolution |
| Forget Learning | Let users delete specific interactions from AI memory |

---

## Anti-patterns — never do these

**Dark confidence** — Displaying high confidence scores for AI outputs without surfacing uncertainty, limitations, or failure modes. Users develop false certainty and make poor decisions.

**Silent failure** — The AI cannot complete a task and returns a plausible-looking but incorrect result without flagging the failure. This is the most dangerous anti-pattern in generative AI.

**Hiding the AI** — Presenting AI-generated content as human-authored or as authoritative fact without disclosure. Always label AI output.

**Automation without escape hatch** — Any automated AI workflow that does not have a visible, always-available stop, pause, or override mechanism.

**Consent bundling** — Burying AI data-use consent inside general terms and conditions. Consent for AI training must be separate, specific, and revocable.

**Claiming human identity** — Never let an AI claim to be human in response to a sincere question. This is both unethical and a violation of user trust.

**Exploiting automation bias** — Designing AI output to look authoritative or final in order to reduce user scrutiny. The goal is calibrated trust, not blind acceptance.

**Irreversible AI actions without confirmation** — Any action the AI takes that cannot be undone must have an explicit confirmation step with a plain-language description of what will happen.

**Sycophantic AI personas** — AI assistants designed to always agree with the user, never push back, and optimize for user satisfaction over accuracy. This produces confident-sounding errors.

**Onboarding by omission** — Launching an AI feature without explaining what it does, what data it uses, what its limits are, or how to turn it off. This is how over-trust is manufactured at scale.

**Model updates without communication** — Silently changing AI behavior (recommendations, outputs, personalization) without notifying users. Trust requires predictability.

---

## Do / don't checklist

### Trust and transparency
- [ ] AI-generated content is labeled at every point of presentation
- [ ] The AI's capabilities and limits are communicated before first use
- [ ] Epistemic metadata (training data, model version, date) is accessible when stakes are high
- [ ] The AI does not claim to be human when sincerely asked
- [ ] Citations or references are shown for factual claims in AI output

### User control and agency
- [ ] Users can disable or snooze AI features at any time without workflow disruption
- [ ] An automation level control exists (fully automated / suggesting / off)
- [ ] Consequential automated actions trigger an advance notification with cancel option
- [ ] Users can challenge, question, or contest any AI decision
- [ ] Users can delete specific items from the AI's memory or learning history
- [ ] Every automated workflow has a visible manual override

### Confidence and uncertainty
- [ ] Confidence is shown only where it materially changes user action
- [ ] Low-confidence states include a clear next-step recommendation
- [ ] The AI does not suppress uncertainty or display false confidence
- [ ] Explanations are progressively disclosed (summary first, depth on demand)
- [ ] Counterfactual explanations are available for decision-affecting outputs

### Onboarding and input
- [ ] The blank-canvas problem is addressed: starter suggestions, templates, or gallery exist
- [ ] Follow-up clarification is offered for underdeveloped prompts
- [ ] The AI's purpose and scope are communicated at first launch
- [ ] Users know what data the AI uses and can access consent controls immediately

### Error handling
- [ ] Error states explain what happened without blaming the user
- [ ] Every error provides a concrete recovery path
- [ ] Users can report AI errors the system may not have detected
- [ ] Silent failure is impossible — the AI always signals when it cannot proceed reliably
- [ ] Post-error: explanation of what went wrong and whether it has been corrected

### Data and consent
- [ ] AI data-use consent is separate from general ToS consent
- [ ] Consent is revocable at any time with clear UI affordance
- [ ] Users can export or delete their data used by the AI
- [ ] A private/incognito mode exists for sensitive sessions
- [ ] Active data sharing is surfaced in a visible audit log

### Long-term adaptation
- [ ] Users are notified when the AI updates its model or behavior
- [ ] Model version, training date, and update history are accessible
- [ ] Personalization changes are visible and reversible
- [ ] Activity/footprints log covers all consequential AI actions

---

## Quick reference: the 30-second design review

Before shipping any AI feature, check all five of these. If any answer is "no" or "I don't know," fix it before launch.

1. **Labeled?** — Is AI-generated content visibly labeled at the point the user sees it? Can users always tell they are looking at AI output?

2. **Controllable?** — Can the user disable, pause, or override this AI feature right now, without navigating away or contacting support?

3. **Honest about failure?** — What happens when the AI is wrong or uncertain? Does the UI surface that — or does it silently serve a plausible-looking wrong answer?

4. **Consented?** — Has the user given explicit, specific, revocable consent for the data this AI feature uses? Is it separate from general ToS acceptance?

5. **Recoverable?** — If the AI takes a consequential action (sends, publishes, deletes, transacts), can the user undo it — or were they warned before it happened?

---

## Source deep dives

### Google PAIR Guidebook (pair.withgoogle.com/guidebook)
PAIR (People + AI Research) is Google's multidisciplinary team exploring the human dimensions of AI. Their guidebook provides practical best practices for developing useful, responsible AI applications. Core themes: user mental models, calibrated trust, graceful failure, and participatory design. The guidebook emphasizes that for ML to achieve positive potential, it must be guided by diverse stakeholders and involve the communities it affects.

### IF Data Design Patterns Catalogue (catalogue.projectsbyif.com)
IF is a design and research studio focused on data rights and trustworthy digital services. Their catalogue documents 65+ design patterns across six categories, with particular depth in AI-specific patterns: Show Confidence, Epistemic Disclosure, Progressive Disclosure, Counterfactual Explanation, Stop and Takeover, Control the Level of Automation, Notice of Upcoming Action, Query an Event, Watermarking, and Forget Learning. Licensed Creative Commons Attribution 4.0.

### Shape of AI (shapeof.ai)
A pattern library organized into six functional categories — Wayfinders, Inputs, Tuners, Governors, Trust Builders, Identifiers — covering the full UX surface of generative AI interfaces. Particularly strong on the blank-canvas problem (Suggestions, Templates, Gallery, Follow Up), human-in-the-loop governance (Action Plan, Draft Mode, Branches, Stream of Thought), and trust infrastructure (Disclosure, Citations, Caveat, Data Ownership, Footprints, Incognito Mode).

### Microsoft Research — Guidelines for Human-AI Interaction (HAI Guidelines, CHI 2019)
18 guidelines developed by Microsoft Aether, Microsoft Research, and Office teams. Validated through a four-step process including a user study with 49 design practitioners. Organized into four temporal interaction phases: Initially (first encounter), During Regular Interaction, When Wrong (error and failure), and Over Time (adaptation and long-term trust). Represents synthesis of 20+ years of human-AI interaction research. The CHI 2019 paper received an Honorable Mention Award.
