---
name: voice-ux
description: "Best-practice guidance for designing, building, auditing, or reviewing voice user interfaces and conversational UX — covering voice persona design, conversation flow, error handling in voice, multi-modal design (voice + screen), wake words, voice feedback, voice accessibility, VUI patterns, voice search, and voice command design. Use this skill whenever the user is working on any voice interface — Alexa skills, Google Assistant actions, Siri shortcuts, voice search, voice navigation, speech input, voice commands, IVR systems, voice chatbots, or any interface where the primary input modality is speech. Trigger on words like voice UX, voice UI, VUI, voice interface, voice design, voice assistant, voice search, speech input, speech recognition, voice command, conversational UI, conversational UX, conversation design, dialogue design, wake word, trigger phrase, voice error, voice feedback, voice persona, voice brand, voice accessibility, IVR, interactive voice response, multimodal, voice and screen, text-to-speech, TTS, speech-to-text, STT, natural language, utterance, intent, slot, voice flow, conversation flow — or when a design involves microphone input, spoken commands, or audio output from a system."
metadata:
  tags: voice, VUI, conversational-UX, voice-assistant, dialogue-design, persona, error-handling, multimodal, accessibility, IVR, speech-input, TTS, STT, natural-language, conversation-flow
sources:
  - https://voiceprinciples.com/ — Voice Principles (aggregated principles from Amazon Alexa, Google, Apple Siri, Microsoft Cortana, IBM, Cathy Pearl, Grice, BBC, Fjord, and others)
  - https://uxdesign.cc/voice-user-experience-design-and-prototyping-for-mere-mortals-ef080c843640 — "Voice User Experience Design and Prototyping for Mere Mortals" (UX Collective / UX Design)
  - https://medium.com/ibm-watson/best-practices-designing-a-persona-for-your-assistant-c2a58666f3c — "Best Practices: Designing a Persona for Your Assistant" (IBM Watson / Medium)
  - https://system.design.orange.com/0c1af118d/p/520a6e-voice-ui — Orange Design System: Voice UI
related-skills:
  - ai-design-patterns (voice assistants are a form of AI interface)
  - web-forms-ux (voice input replaces or supplements typed form inputs)
---

## When to use

Load this skill when the user is:
- Designing or auditing a voice assistant, Alexa skill, Google Action, Siri shortcut, or IVR system
- Working on conversation flow, dialogue scripts, or sample utterances
- Designing error handling, no-input, and misrecognition recovery paths
- Creating or evaluating a voice persona or brand voice for an assistant
- Building a multimodal interface that combines voice with a screen
- Evaluating voice accessibility for hearing, speech, or motor-impaired users
- Reviewing VUI copy, prompt wording, or TTS output quality
- Prototyping a voice flow or sample dialogue script
- Working on voice search, voice navigation, or voice-driven form input

---

## How to use this skill

1. **Identify the modality scope** — pure voice (no screen), voice + screen (multimodal), or voice as supplement to a GUI.
2. **Identify the platform** — Alexa, Google Assistant, Siri, custom IVR, web speech API, mobile STT, etc. — platform constraints differ.
3. **Identify the design stage** — early (persona, flow structure), mid (dialogue scripts, error paths), or late (copy review, audio QA, accessibility audit).
4. **Apply the relevant section** — use Core Principles for any stage, Pattern Guidance for specific flows, Anti-Patterns for audits, and the Quick Review for sign-off.
5. **Always listen, not read** — run all dialogue through TTS or read it aloud before shipping. What sounds right when read silently often sounds wrong when spoken.

---

## Core concept

Voice UX is conversation design, not command design. Users do not read menus — they speak naturally, imprecisely, and contextually. The system's job is to understand intent, not to enforce syntax. Every VUI is a two-turn dialogue: the system speaks, the user responds, the system must interpret that response gracefully — including when the user says nothing, says the wrong thing, or says something partially correct.

The three inescapable constraints of voice:
- **No visual affordance** — users cannot see what is possible; the system must speak possibilities.
- **No persistent state** — users cannot scroll back; the system must restate context when needed.
- **No tolerance for long output** — audio is linear and time-bound; long responses are abandoned.

---

## Core principles

### 1. Brevity is mandatory
Keep every system turn as short as possible. Respect that users cannot scan audio — they must listen in real time. Remove every word that does not carry meaning. Provide the key information first, elaboration last (inverted pyramid for audio). Google's rule: "Keep it short. Respect users' time."

### 2. Natural language, not command syntax
Users speak in natural sentences, not "press-1-for" commands. Design for how people actually talk — with contractions, discourse markers ("okay," "so," "anyway"), ellipsis, and pronoun reference. Grice's Maxims apply: be truthful (Quality), be as informative as needed and no more (Quantity), be relevant (Relation), and be clear and brief (Manner).

### 3. Design the persona first, write dialogue second
A voice assistant has a character whether you design it or not — users infer personality from every word choice, response time, and error message. Define the persona on explicit spectrums (formal/informal, warm/cool, expert/novice, enthusiastic/calm, empathic/distant) before writing a single line of dialogue. Inconsistency across interactions produces a "split personality" that erodes trust. Align the persona with brand identity and the emotional context of the task.

### 4. Set expectations honestly and early
Tell users what the system can and cannot do. Users who understand system scope make better requests and tolerate errors more gracefully. Never imply capabilities the system does not have. Apple's guideline: "Ensure accuracy and relevance." Voysis: "Set expectations and build trust through honest handling of limitations."

### 5. Confirmations must match risk level
Use explicit confirmation ("I'll delete your account — is that right?") for high-stakes, irreversible, or financial actions. Use implicit confirmation (restating the understood intent in the next prompt) for low-stakes, easily reversible actions. Avoid confirming trivial actions — it adds friction without value. Avoid skipping confirmation for destructive actions — it causes irreversible errors.

### 6. Error recovery is the core skill
Errors are not edge cases — they are central to voice design. Plan three error levels: (1) rapid re-prompt with a non-specific recovery phrase mimicking natural conversation ("What was that?", "Sorry, I missed that"), (2) escalating guidance that gets more specific with each failure ("I heard something about your account — did you want to check your balance or recent transactions?"), (3) graceful exit or handoff to a human when repeated failures occur. Never blame the user. Never terminate unilaterally. Never loop the same error message more than twice.

### 7. Context is memory
Voice systems must remember what was said earlier in the conversation and apply it. Users use pronouns and ellipsis: "What about tomorrow?" means something only if the system remembers the preceding topic. Maintain conversational state. Use context to reduce repetition. IBM guideline: relay conversation history and current state. Cathy Pearl: "Maintain contextual awareness (pronouns, references)."

### 8. Design for the no-input case
Silence is a user response. Design explicit no-input handlers at every prompt that awaits a response. After one silence: re-prompt gently with a shorter, simpler version of the question. After two silences: offer to continue later or connect to help. After three silences: exit gracefully. Signal when the system is waiting for input with an audio or visual cue.

### 9. Accommodate novice and expert users differently
First-time users need onboarding: capabilities overview, example utterances, and gentle guidance. Return users find the same onboarding irritating. Design progressive disclosure — start with guided prompts for new users, shift to open-ended prompts as familiarity grows. Microsoft Cortana: "Design around the most common use case. Make tasks feel quick and easy."

### 10. Avoid the uncanny valley
Design assistants to feel natural without attempting to be human. Claiming to be human, or creating interactions that are so life-like they surprise users with their limits, destroys trust. IBM's CASA research shows minimal cues create social presence — full humanization is neither needed nor desirable. Be transparent about being an AI while maintaining warmth and personality.

### 11. Provide universal navigation commands
Every voice interface must support: repeat (replay last system turn), main menu / home (return to top level), help (explain current options), operator / human (escalate to a person), and goodbye / cancel (exit cleanly). These are safety nets users reach for when lost — their absence causes abandonment.

### 12. Use conversational markers and acknowledgers
Signal that the system heard and understood: "Got it," "Okay," "Sure," "I see." Randomize these acknowledgers to avoid robotic repetition. Use discourse markers to signal transitions: "Now, for your address..." Use parallel language structure when listing options. Cathy Pearl: "Use conversational markers for orientation."

### 13. Limit choices to three or fewer
Human working memory in audio is severely limited. Offering more than three options forces users to hold and compare auditory information they cannot see. Provide no more than three options per turn. If more options exist, guide the user through a decision tree across turns rather than dumping all options at once.

### 14. GUI parity is not the goal
A voice interface is not a spoken version of a screen. Attempting to replicate all GUI features in voice produces overwhelming, unusable interactions. Identify the tasks that benefit most from voice (hands-free, eyes-free, speed-sensitive, ambient) and design only those. Cheryl Platz: "GUI parity is not the goal."

### 15. Signal delays and processing time
If the system needs more than 1–2 seconds to process a request, signal this explicitly with an audio cue or brief message ("Let me check on that..."). Unexplained silence reads as a system failure. Users will repeat their utterance, interrupt, or abandon.

### 16. Privacy and social context are first-class concerns
Users are often not alone. Voice output may be heard by others. Sensitive information (account numbers, health data, financial balances) must never be read aloud in a context where it could be overheard without user consent. Offer to send sensitive data to the screen or a notification instead. CX Partners: "Address privacy and social context concerns."

### 17. Write for the ear, not the eye
Voice copy obeys different rules than screen copy. Use spoken contractions. Avoid parenthetical asides and nested clauses. Front-load the most important word in each sentence — the ear hears the beginning most clearly. Read every line aloud before finalizing. Cheryl Platz: "Test dialogues in audio form before deployment."

### 18. Earcons and audio icons: use sparingly
Non-speech audio cues (earcons) can signal state changes, confirmations, or errors without words. Use them to mark the start and end of listening windows, to confirm successful actions, and to signal errors. Work with a sound designer. Do not create novel earcons without user testing — unfamiliar sounds are distracting or alarming.

---

## Pattern guidance

### Onboarding a new user
Open with a brief, warm introduction that names the assistant and states one or two things it can do. Offer an example utterance — not a list of commands, but a natural sentence. Do not deliver a full capability manifest on first use. Onboard progressively across the first few sessions.

### Opening prompts
Keep the opening prompt short. Do not ask two questions in one turn. End the prompt with a clear signal that the system is listening. For open-ended systems, use an open question ("What can I help you with?"). For task-specific systems, offer a constrained prompt ("Would you like to check your balance or make a payment?").

### Confirmation patterns
- **Explicit confirmation**: "I'm going to transfer $500 to Sarah. Shall I go ahead?" — use for irreversible, high-stakes, or financial actions.
- **Implicit confirmation**: Restate the understood intent in the next step — "Okay, checking your balance..." — use for low-stakes, reversible actions.
- **No confirmation**: For trivial, instantaneous actions with easy undo, skip confirmation entirely to maintain flow.

### Error recovery ladder
1. **First error — non-specific re-prompt**: "Sorry, I didn't catch that. Could you try again?" (Short. Natural. No blame.)
2. **Second error — guided re-prompt**: "I'm having trouble understanding. Are you asking about your balance or a recent transaction?" (Narrow the space.)
3. **Third error — escalate or exit**: "I'm not able to understand right now. I can connect you to a team member, or you can try again later." (Exit gracefully. Never loop a third time.)

### No-input handler
1. First silence: Gentle re-prompt with a shorter version of the question.
2. Second silence: "Still there? Just say yes to continue or nothing to end the session."
3. Third silence: "I'll let you go. Goodbye." — exit cleanly.

### Multi-turn conversation management
Use questions to structure multi-turn flows — each turn should end in a question that naturally advances the conversation. Carry context across turns using pronouns and state. Summarize progress for long flows ("So far I have your name and email — now I just need your address."). Offer an escape at every step.

### Multimodal (voice + screen) design
When a screen is available: use it for lists, confirmations, maps, images, and dense information that would be unwieldy in audio. Keep voice output brief and direct users to look at the screen ("I've shown your options on screen"). Never require the user to look at the screen — design for eyes-free use as the baseline and treat the screen as enhancement. Orange Design System: treat voice and screen as complementary, not duplicative channels.

### Voice search
For voice search, immediately confirm what was understood ("Searching for 'flights to Paris'...") before presenting results. For ambiguous queries, ask a single clarifying question rather than presenting multiple result sets. Keep result summaries to three items maximum in audio; show full results on screen.

### IVR (Interactive Voice Response)
Keep menu depth to two levels maximum. Offer a "repeat" option at every menu. State the choice options before the key/phrase to activate them — "For billing, press 1" not "Press 1 for billing." Offer a human agent option at every level. Never trap users in a menu loop.

---

## Anti-patterns — never do these

- **Blame the user**: "I didn't understand you" or "You need to speak more clearly" — always accept fault, not the user's.
- **Terminate unilaterally**: Ending the session without warning or graceful exit destroys trust.
- **Read back sensitive information aloud**: Never say account numbers, passwords, SSNs, or health data in voice output without confirming the user is in a private context.
- **Loop the same error message**: Repeating the identical error prompt three times is a dead end — escalate or exit.
- **More than three options per turn**: Listing four or more options in a single audio turn overloads working memory.
- **Two questions in one turn**: "Would you like to order pizza or check the menu, and what size are you thinking?" — users can only process one question.
- **Command-style prompts**: "Say 'ORDER' to place an order" — design for natural utterances, not keyword commands.
- **Unexplained silence**: Any processing pause over 2 seconds without an audio signal reads as a failure.
- **GUI parity**: Attempting to voice-narrate an entire screen interface — design a voice-native flow instead.
- **Inconsistent persona**: Friendly tone for most interactions, blunt or corporate tone for errors or rejections — persona must hold across all emotional registers.
- **Claiming to be human**: Deceiving users about the assistant's nature when directly asked is an ethical and trust violation.
- **No universal commands**: Failing to support repeat, help, main menu, and goodbye at every point in the flow.
- **Long opening statements**: Opening with a paragraph of welcome text before asking what the user needs — users will interrupt or abandon.

---

## Do / don't checklist

### Persona
- [x] Define the persona on explicit spectrums before writing any dialogue
- [x] Align persona to brand identity and task emotional context
- [x] Test persona consistency across error states, edge cases, and rejections
- [ ] Do not let the persona shift between friendly and corporate registers
- [ ] Do not claim to be human when directly asked

### Conversation flow
- [x] Each system turn ends in one clear question or action signal
- [x] Context (pronouns, references) carried across turns
- [x] Onboarding introduces capabilities with example utterances
- [x] Universal commands (repeat, help, home, goodbye, human) supported at every step
- [ ] Do not ask two questions in one turn
- [ ] Do not offer more than three options per turn

### Language and copy
- [x] Use contractions and natural spoken language
- [x] Front-load the key information in each sentence
- [x] Use conversational acknowledgers (varied, not robotic)
- [x] Read every line aloud before finalizing
- [ ] Do not write in formal or written register
- [ ] Do not use technical jargon or command-style syntax

### Error handling
- [x] Three-level error ladder: non-specific, guided, escalate/exit
- [x] No-input handler at every listening prompt
- [x] Error messages never blame the user
- [x] Never loop the same error message more than twice
- [ ] Do not terminate unilaterally without warning

### Confirmations
- [x] Explicit confirmation for high-stakes, irreversible, financial actions
- [x] Implicit confirmation (restatement) for low-stakes actions
- [ ] Do not confirm trivial, easily reversible actions (adds friction)
- [ ] Do not skip confirmation for destructive actions

### Multimodal
- [x] Design for eyes-free baseline; screen is enhancement
- [x] Direct user to screen for lists, maps, dense data
- [x] Keep audio brief when screen is available
- [ ] Do not require screen interaction as the only path to complete a task

### Accessibility
- [x] Design for hearing impairment: visual equivalents of all audio output
- [x] Design for speech impairment: alternative text input path always available
- [x] Design for motor impairment: voice as the primary path, not a secondary option
- [x] Test with users who have relevant disabilities

### Performance and privacy
- [x] Signal any processing delay over 2 seconds with an audio cue
- [x] Never read sensitive information (account numbers, health data) aloud without privacy confirmation
- [x] Offer to route sensitive output to screen or notification

---

## Quick reference: the 10-second design review

Before shipping any voice interaction, check these ten questions:

1. **Is every system turn under 30 words?** If not, cut it.
2. **Does every system turn end with one clear question or action signal?** If not, restructure.
3. **Is the persona consistent with its defined spectrums, including in this error state?** If not, rewrite.
4. **Are all three error levels designed: non-specific, guided, escalate?** If not, add the missing ones.
5. **Is there a no-input handler at every listening prompt?** If not, add it.
6. **Does the flow support repeat, help, home, goodbye, and human at this step?** If not, add them.
7. **Is any sensitive information (financial, health, personal) read aloud?** If yes, route to screen.
8. **Is the dialogue written in spoken language (contractions, natural flow)?** If not, rewrite for the ear.
9. **Is there an explicit confirmation for any irreversible or financial action?** If not, add it.
10. **Has every line been read aloud or run through TTS?** If not, do it now.
