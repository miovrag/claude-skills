---
name: prompt-engineering
description: "Best-practice guidance for writing, refining, and evaluating prompts for large language models — covering role assignment, context setting, instruction clarity, few-shot examples, chain-of-thought prompting, output format specification, system prompts, prompt chaining, and iterative refinement. Use this skill whenever the user is writing or improving prompts for Claude, ChatGPT, GPT-4, Gemini, or any other LLM — including system prompts for AI agents, prompts for AI-powered product features, prompts for generating UI copy or design assets, prompt templates for repeated tasks, or evaluating why a prompt produces inconsistent or wrong output. Trigger on words like prompt, prompt engineering, system prompt, user prompt, LLM prompt, AI prompt, prompt design, prompt writing, prompt refinement, prompt testing, prompt optimization, few-shot, few-shot example, zero-shot, chain of thought, CoT, role prompting, persona prompt, context window, instruction following, prompt template, prompt library, prompt chaining, output format, structured output, JSON output, markdown output, prompt evaluation, prompt iteration, temperature, top-p, model parameters — or complaints like 'the AI doesn't follow instructions', 'responses are too long', 'outputs are inconsistent', 'the AI misunderstands the task', 'how do I make Claude do X'."
metadata:
  tags: prompt, prompt-engineering, llm, ai-prompting, system-prompt, few-shot, chain-of-thought, prompt-design, prompt-testing
sources:
  - Learn Prompting — "Introduction to Prompt Engineering" — https://learnprompting.org/docs/introduction (2024)
  - DAIR.AI — "Prompt Engineering Guide" — https://www.promptingguide.ai/ (2024)
  - V7 Labs — "Prompt Engineering Guide: Techniques, Tips & Best Practices" — https://www.v7labs.com/blog/prompt-engineering-guide (2024)
  - Anthropic — "Prompting Best Practices" (Claude Opus 4.7 / Sonnet 4.6 / Haiku 4.5) — https://platform.claude.com/docs/en/prompt-library/library (2025)
related-skills:
  - ai-design-patterns (prompt input is the primary UI in AI interfaces)
---

## When to use

Load this skill when the user is:

- Writing a system prompt for a product feature, AI agent, or workflow
- Trying to get an LLM to follow instructions more reliably
- Asking why their prompt produces inconsistent, too-long, too-short, or off-topic outputs
- Designing few-shot examples or prompt templates for repeated tasks
- Building prompt chains or agentic pipelines
- Evaluating or testing prompt quality
- Migrating prompts from one model to another (e.g., GPT-4 to Claude, Claude 4.5 to Claude 4.6/4.7)
- Asking "how do I make the AI do X" for any value of X

---

## How to use this skill

1. **Diagnose first.** Before rewriting, identify which failure mode is present (see Anti-patterns below). Different failures need different fixes.
2. **Apply the relevant principle(s).** Use the numbered principles as a decision checklist — most prompts fail on 1–3 of them, not all.
3. **Test against a clear success criterion.** Define what "correct output" looks like before iterating.
4. **Iterate in small steps.** Change one thing at a time so you can attribute improvement to a specific change.
5. **Document what works.** Effective prompts become templates; treat them like reusable design components.

---

## Core concept

A prompt is an instruction contract between you and a model. The model will try to fulfill that contract as literally as it can read it. Every ambiguity in your prompt is a space where the model fills in its own defaults — which may not match yours. Prompt engineering is the practice of reducing that gap: making the contract explicit, grounded in examples, and structured so the model cannot misread it.

The fundamental mental model: treat the model as a new, highly capable collaborator who has never worked with you before and has zero context about your norms, your product, your audience, or your intent. Give them everything they need to do the job exactly right — role, task, context, format, constraints, and examples.

---

## Core principles

### 1. Be explicit, not implicit

State your requirements directly. Do not rely on the model to infer what you want from vague cues. If you want structured JSON, say "Return your answer as valid JSON." If you want a two-sentence summary, say "Write a two-sentence summary." If you want the model to take action rather than suggest action, use imperative verbs: "Edit this function" not "Can you suggest how to improve this function."

The Anthropic golden rule: show your prompt to a colleague with zero context on the task and ask them to follow it. If they would be confused, the model will be too.

### 2. Assign a role in the system prompt

Opening your system prompt with a role statement focuses the model's behavior, tone, and knowledge domain. Even a single sentence makes a measurable difference:

- "You are a senior UX writer specializing in SaaS product copy."
- "You are a financial analyst who produces concise, evidence-backed summaries for non-technical executives."
- "You are a helpful coding assistant specializing in Python."

Role assignment works because it activates a coherent persona with consistent defaults, reducing the variance you would otherwise need to specify instruction by instruction.

### 3. Provide context and motivation, not just instructions

Tell the model *why* an instruction exists, not just *what* to do. Context lets the model generalize correctly to edge cases you did not anticipate.

Less effective: `NEVER use ellipses`

More effective: `Your response will be read aloud by a text-to-speech engine, so never use ellipses — the TTS engine cannot pronounce them.`

The second version gives the model enough reasoning to apply the rule correctly in situations you did not explicitly cover.

### 4. Use few-shot examples for format and tone

Examples are the most reliable lever for controlling output format, tone, and structure. Three to five well-chosen examples (few-shot or multishot prompting) will outperform lengthy written instructions for format control.

When adding examples:
- Make them **relevant** — mirror your actual use case closely.
- Make them **diverse** — cover edge cases and intentional variation so the model does not pick up unintended patterns.
- Make them **structured** — wrap examples in `<example>` tags (or `<examples>` for multiple) so the model can cleanly distinguish examples from instructions.
- Ask Claude to evaluate your examples for relevance and diversity, or generate additional ones from your seed set.

### 5. Structure with XML tags for complex prompts

When a prompt mixes instructions, context, examples, and variable inputs, unstructured prose creates ambiguity. XML tags help the model parse each section unambiguously:

```xml
<instructions>
Summarize the following customer feedback in two sentences. Focus on the primary pain point.
</instructions>

<feedback>
{{CUSTOMER_FEEDBACK}}
</feedback>
```

Use consistent, descriptive tag names across your prompts. Nest tags when content has a natural hierarchy (e.g., `<documents>` containing multiple `<document index="n">` tags). For long documents, put the document content *before* the query — this can improve response quality by up to 30% on multi-document tasks.

### 6. Specify output format precisely

Do not leave format to chance. Specify:
- **Structure**: JSON, markdown, plain prose, numbered list, table
- **Length**: "two sentences," "under 150 words," "one paragraph"
- **Voice and tone**: "conversational," "formal," "direct and opinionated"
- **What to omit**: "no preamble," "no bullet points," "respond directly without phrases like 'Here is...'"

Tell the model what to *do*, not just what not to do. Instead of "Do not use markdown," write "Compose your response as flowing prose paragraphs." Positive instructions are parsed more reliably than negative constraints.

### 7. Use chain-of-thought for reasoning-heavy tasks

For tasks requiring multi-step reasoning, math, diagnosis, or analysis, prompt the model to reason before answering. Two approaches:

**With thinking enabled (Claude):** Use `effort: high` or `xhigh` — the model reasons adaptively without requiring explicit CoT instructions.

**Without thinking / any model:** Use structured tags:
```
Think through the problem step by step inside <thinking> tags. Then write your final answer inside <answer> tags.
```

Avoid prescribing the exact reasoning steps — general instructions ("think carefully") frequently outperform hand-written step-by-step plans because the model's reasoning often exceeds what a human would prescribe.

### 8. Iterate in small, testable steps

Treat prompts as hypotheses. Change one element at a time and measure the effect against a defined success criterion before moving on. Common iteration sequence:

1. Write a minimal prompt and note failure modes.
2. Add role assignment.
3. Add context/motivation.
4. Add examples.
5. Add explicit format constraints.
6. Add negative prompts (what to exclude).
7. Test edge cases.

Do not rewrite the entire prompt after a single bad output — diagnose which principle is being violated first.

### 9. Use negative prompts strategically

Specify what to *exclude* as well as what to include, but be precise. "Avoid jargon" is vague. "Do not use terms the user did not introduce; if a technical term is unavoidable, define it on first use" is actionable. Negative prompts are particularly useful for:

- Tone exclusions ("do not use exclamation marks")
- Content exclusions ("do not include competitor names")
- Format exclusions ("do not wrap code in a prose introduction")
- Behavioral exclusions ("do not ask clarifying questions; make the most reasonable assumption and state it")

### 10. Control verbosity and length explicitly

Models will fill available space if you do not set expectations. If outputs are too long:
- "Provide a concise, focused response. Skip non-essential context and keep examples minimal."
- "Answer in no more than three sentences."
- "Use bullet points only when listing truly discrete items. Otherwise write in prose."

If outputs are too short or superficial:
- "Go beyond the basics to create a fully-featured implementation."
- "Include as many relevant considerations as possible."
- Raise the effort level (Claude) or add explicit instruction to be thorough.

### 11. Prompt chain for multi-stage tasks

Break complex workflows into sequential prompts where each stage takes the output of the previous. This is useful when:
- You need to inspect or validate intermediate outputs before continuing.
- Different stages require different roles or formats.
- You want auditability at each step.

Most common pattern: **draft → review → refine**. Generate a draft in call 1, pass it to call 2 with a review rubric, pass the review to call 3 for targeted revision. Each call is a separate API request with its own system prompt optimized for that stage's job.

### 12. Test prompts against a defined rubric before deploying

Before shipping a prompt, define 3–5 representative test cases including at least one edge case. For each, define what "correct" output looks like. Use this rubric for:
- Evaluating whether a new prompt version is better or worse than the previous.
- Catching regressions when switching models.
- Measuring recall/precision for classification or extraction tasks.

For Claude specifically: if you're evaluating a prompt change, run it against a subset of test cases and measure F1, recall, or task success rate — not just subjective impression.

---

## Pattern guidance

### System prompt vs. user prompt
The system prompt sets persistent behavior, role, tone, and constraints for the entire session. The user prompt contains the specific task or query for a single turn. In product contexts, put all invariant rules (persona, output format, safety guardrails, brand voice) in the system prompt. Reserve the user prompt for dynamic, per-request content.

### Long context prompting (20k+ tokens)
- Put long documents and inputs at the **top** of the prompt, above instructions and query.
- Wrap each document in `<document index="n">` tags with `<source>` and `<document_content>` subtags.
- Ask the model to quote relevant passages before synthesizing — this focuses attention and reduces hallucination.
- End with the query/instruction, not the documents.

### Zero-shot vs. few-shot selection
- Use **zero-shot** when the task is simple, the format is standard, or you are testing the model's default behavior.
- Use **few-shot** when you need a specific format, tone, or style that deviates from the model's defaults. Three to five examples is the practical sweet spot.
- Use **one-shot** as a quick baseline when you have one good example and want to establish format without full few-shot investment.

### Agentic and tool-use prompts
For AI agents (Claude Code, AI-powered features that call tools):
- Be explicit about *when* to use each tool, not just that the tool exists.
- Specify reversibility expectations: "For destructive operations, confirm with the user before proceeding."
- Define the scope of autonomy: "Take local, reversible actions freely; ask before affecting shared systems."
- Tell the agent to investigate before answering: "Never speculate about code you have not read. Read the relevant file before answering."

### Output format for structured data
For JSON, YAML, or other structured output: do not rely on prefilled responses (deprecated in Claude 4.6+). Instead:
1. First try: "Return your answer as valid JSON matching this schema: `{...}`" — modern models follow this reliably.
2. If inconsistent: use structured outputs / tool calling with an enum or schema constraint.
3. Post-processing fallback: strip preamble text in code if the occasional introductory sentence slips through.

### Prompt templates for repeated tasks
Identify any prompt you write more than twice and templatize it with `{{VARIABLE}}` placeholders. Document:
- What each variable expects (type, length, examples)
- Which parts of the prompt should never change
- Known edge cases and how the template handles them

Treat prompt templates as design components — version them, test them, and review them when switching models.

---

## Anti-patterns — never do these

1. **Vague instructions with no success criterion.** "Make this better" — better how? For whom? By what measure? Always specify what good output looks like.

2. **Expecting the model to infer unstated context.** The model does not know your product, your audience, your brand voice, or your prior conversations unless you tell it. State all relevant context explicitly.

3. **Writing prohibitions without providing alternatives.** "Don't use jargon" leaves the model guessing. "Use plain language a non-technical reader can understand without glossary lookups" gives it a target.

4. **Changing multiple things between tests.** If you alter role, context, format constraints, and examples simultaneously, you cannot know which change caused the improvement or regression. Iterate one variable at a time.

5. **Using ALL CAPS, excessive bolding, or emotional language for emphasis.** "CRITICAL: You MUST always..." in a system prompt causes over-triggering on modern models. Normal declarative instructions ("Use this tool when...") work better.

6. **Writing instructions in the negative only.** "Do not use bullet points" is harder for models to follow consistently than "Write in flowing prose paragraphs." Lead with positive specifications.

7. **Assuming a prompt that worked on one model works on another.** Models differ significantly in default verbosity, tone, instruction-following literalism, and tool-use triggering. Re-evaluate prompts when switching models.

8. **Over-prompting to compensate for an underspecified task.** If you find yourself adding 20 rules to get correct output, the prompt architecture may be wrong. Consider whether the task needs to be decomposed into a chain, or whether role assignment and a few examples would do the work of all 20 rules.

9. **Using subjective quality terms without definition.** Words like "high-quality," "professional," "natural," and "concise" mean different things to different people and models. Operationalize them: "professional" might mean "no contractions, active voice, no first-person opinion statements."

10. **Testing only the happy path.** A prompt that works on your single representative example may fail on edge cases, empty inputs, adversarial inputs, or inputs with unusual formatting. Always test at least one edge case before deploying.

11. **Neglecting to specify what to do when the model is uncertain.** If you do not tell the model what to do when it lacks information, it will fill the gap — often with a hallucination or a vague hedge. Specify: "If you are uncertain, say so explicitly and explain what information would resolve the uncertainty."

12. **Letting the system prompt grow without pruning.** Every instruction added has a cost — cognitive load on the model, and eventually contradictions between rules. Audit prompts regularly and remove instructions that no longer reflect actual requirements.

---

## Do / don't checklist

| Category | Do | Don't |
|---|---|---|
| Role | Open the system prompt with a clear, specific role statement | Leave the model roleless for complex or domain-specific tasks |
| Instructions | Use imperative verbs ("Summarize," "Return," "List") | Use hedged requests ("Could you try to maybe...") |
| Context | Explain *why* constraints exist, not just *what* they are | Give rules without rationale |
| Format | Specify format, length, and structure explicitly | Leave format undefined and hope for the best |
| Examples | Include 3–5 diverse, relevant examples wrapped in tags | Use examples that are too similar to each other |
| Long docs | Put documents before the query/instruction | Put the query before long context blocks |
| Iteration | Change one element per iteration | Rewrite the whole prompt after one bad output |
| Negative prompts | Pair every prohibition with a positive alternative | Write only prohibitions |
| Model switching | Re-evaluate every prompt when changing models | Assume parity across models |
| Agentic prompts | Define tool-use scope and reversibility expectations | Leave agent autonomy and safety bounds ambiguous |
| Testing | Test on 3–5 cases including at least one edge case | Test on one representative example only |
| Structured output | Use explicit schema instructions or tool calling | Rely on prefill to force format (deprecated on Claude 4.6+) |
| Verbosity | Set explicit length expectations | Let the model choose response length without guidance |
| Chain-of-thought | Ask for reasoning when the task is complex or multi-step | Skip CoT prompting for diagnosis, math, or multi-step analysis |
| Template reuse | Parameterize prompts used more than twice | Copy-paste the same prompt with manual edits each time |

---

## Quick reference: the 10-second prompt review

Before sending or deploying any prompt, run through this checklist. If any answer is "no," fix it first.

1. **Role** — Does the system prompt open with a clear role statement?
2. **Task** — Is the task stated as an explicit imperative, not a suggestion?
3. **Context** — Does the prompt include the "why" behind key constraints?
4. **Format** — Is the output format (structure, length, voice) specified explicitly?
5. **Examples** — For non-standard format or tone: are there 3–5 diverse examples?
6. **Success criterion** — Can you tell, without guessing, whether the output is correct?
7. **Edge case** — Have you tested at least one input that differs from your primary example?
8. **Positive framing** — Are prohibitions paired with positive alternatives?
9. **Model fit** — Was this prompt written for or validated against the model you are deploying to?
10. **Scope** — Is every rule in this prompt still necessary, or has scope creep added dead weight?

---

## Technique reference

| Technique | When to use | Key signal you need it |
|---|---|---|
| Zero-shot | Simple, standard tasks | Default model behavior is close enough |
| Few-shot (3–5 examples) | Non-standard format, tone, or style | Model defaults don't match your target output |
| Chain-of-thought | Reasoning, diagnosis, math, multi-step analysis | Outputs are correct in intent but wrong in conclusion |
| Role assignment | Any task with a specific domain, audience, or tone | Outputs feel generic or off-voice |
| XML structure | Complex prompts mixing instructions, context, examples | Model confuses one section for another |
| Prompt chaining | Multi-stage tasks requiring validation at each step | Single-prompt approach produces one kind of error at each stage |
| Negative prompts | Recurring unwanted patterns in output | "Don't do X" plus "instead do Y" |
| Output schema / structured output | JSON, YAML, classification, extraction | Inconsistent structure across runs |
| Self-correction loop | High-stakes or low-tolerance tasks | First-pass accuracy insufficient; human review too costly |
| Meta-prompting | Prompt design itself | Ask the model to evaluate or improve your prompt |

---

## Model-specific notes (Claude)

These notes reflect Claude Opus 4.7 / Sonnet 4.6 behavior as of May 2026. Re-evaluate if the model changes.

- **Literalism**: Claude 4.7 follows instructions more literally than prior models. If you want a rule applied broadly, state the scope explicitly: "Apply this formatting to every section, not just the first."
- **Verbosity**: Claude 4.7 calibrates length to task complexity. If you need a fixed verbosity level, specify it explicitly.
- **Effort levels**: Use `effort: xhigh` for coding and agentic tasks; `high` for most intelligence-sensitive tasks; `medium` or `low` for cost/latency-sensitive workloads.
- **Tool use**: If the model uses tools less than expected, raise effort or explicitly describe when and why to use each tool.
- **Design defaults**: Claude 4.7 has a default visual style (warm cream, serif type, terracotta accents). For dashboards, dev tools, or enterprise UIs, specify the visual direction explicitly rather than using generic negations.
- **Prefill**: Prefilled responses on the last assistant turn are not supported in Claude 4.6+. Use structured output, tool calling, or direct instructions instead.
- **Thinking**: Use `thinking: {type: "adaptive"}` with `effort` rather than `budget_tokens` (deprecated). For simple queries, the model skips thinking automatically; for complex ones, it reasons deeply.
