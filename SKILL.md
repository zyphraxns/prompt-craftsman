---
name: prompt-craftsman
description: Create and refine high-quality prompts for any LLM through structured interviewing. Use this skill whenever the user wants to write a new prompt from scratch, improve an existing prompt that gives poor or inconsistent results, or asks for prompt engineering, design, or optimization help — even when they don't say the word "prompt" (e.g. asking for instructions to give an AI). Trigger on phrases like "write me a prompt", "generate a prompt", "optimize/improve/fix my prompt", "make this prompt better", "write instructions for an AI that...", in any language.
---

# Prompt Craftsman

Craft and refine LLM prompts through structured interviewing.

A prompt is only as good as the requirements behind it. Jumping straight to writing when the user gives a vague direction ("help me write a prompt for organizing school notes") produces generic, forgettable prompts. This skill instead runs a short structured interview — a few questions per round, each with a recommended answer — then writes a prompt that actually fits the task.

## Two modes

- **Generate** — the user wants a new prompt and no usable one exists yet.
- **Optimize** — the user already has a prompt and wants it better (vague, poor output, inconsistent format, missing structure, wrong tone...).

Detect the mode from what the user says. When in doubt, ask.

## Core rules

These rules exist to prevent the single most common failure: writing a generic prompt before understanding the task. Apply them in both modes.

1. **Never write the final prompt on the first turn.** When the request is vague, ask clarifying questions first. Only a request that already covers the essentials (task + output format at minimum) may skip questioning.
2. **Ask in rounds, not all at once.** Each round asks at most 3-4 questions, ordered by dependency — ask the questions whose answers you can know now, and leave questions that depend on those answers for later rounds. Wait for the user's answers before asking the next round.
3. **Every question carries your recommendation.** Most users don't know their options. Present the realistic options and mark the one you recommend, so the user gets a fast default while still being free to choose otherwise.
4. **Before writing, confirm length tier and prompt language.** Offer three tiers — Concise / Standard / Detailed (defined below) — with a recommendation based on the use case. Confirm the prompt's language at the same time: default to the conversation language, but note that some users want the prompt itself in English.
5. **Prefer the AskUserQuestions tool when available** (up to 4 questions per call, with the recommended option marked). If it is unavailable, ask as numbered text questions with a "➡️ Recommendation:" line after each.
6. **Stop when the checklist is covered.** Don't pad the interview. See the completeness checklist.

## Completeness checklist (when you may start writing)

- **Required:** Task (what the AI should do) and Output Format (what the answer should look like).
- **Recommended:** Role, Context, Input, Constraints, Quality Criteria.
- **Detailed tier adds:** Examples (and one anti-example).

Start writing once the required items are covered and you have a reasonable picture of the rest — or the user says "just write it". Don't chase every optional module.

## Length tiers

- **Concise** — a single tight paragraph. Role, task, key constraint, and output format inline. For quick, single-use prompts.
- **Standard** — sectioned with headers: Role, Context, Task, Constraints, Output Format. The default for most use.
- **Detailed** — every module, plus examples and quality criteria. For production prompts, agent workflows, or anything reused often.

## The modules (what each prompt section is for)

- **Role** — who the AI should be: expertise, persona, tone.
- **Context** — the situation and background the AI needs to respond well.
- **Task** — one clear, verifiable instruction: what to do with the input.
- **Input** — what the user will supply, and in what form.
- **Constraints** — do's and don'ts: style, audience, length, forbidden output, edge cases to handle.
- **Output Format** — the exact structure of the answer (headers, bullets, table, length limits...).
- **Quality Criteria** — checkable signs the output is good.
- **Examples** — 1-2 exemplars; in Detailed, one anti-example showing what to avoid.

Trim modules that don't matter. A prompt for "summarize my meeting notes" does not need Role; a prompt for "act as my editor" lives or dies on Role.

## Generate flow

1. **Interview.** Ask rounds of 3-4 questions from the pool below, dependency-first, each with a recommendation. A typical first round covers: what the task is, what the prompt is for, what input the user will provide, and what the output should look like.
2. **Confirm tier + language.** When the checklist is nearly covered, offer the three length tiers and confirm the prompt language. Recommend a tier from context — Standard for everyday use, Detailed for reusable or agent-bound prompts, Concise for quick one-off tasks.
3. **Write.** Assemble the prompt from the modules, trimmed to need. If an essential detail is still unknown (the user skipped a question), make a reasonable assumption and mark it as `PENDING: ...` inside the draft so the user can correct it.
4. **Deliver.** The final prompt in a fenced code block (easy to copy), followed by a short "Why it's built this way" section — 3-5 bullets on the key design choices.
5. **Iterate lightly.** Ask once whether the user wants to test-run the prompt and refine it based on the results. Don't push.

### Question pool (ask only what's needed — never all of it)

- What is the task, concretely? What counts as done?
- Who should the AI be (role / expertise), and what tone?
- What background or context matters?
- What input will you supply, and in what form?
- Any constraints — audience, style, length, must-nots, edge cases?
- What should the output look like (format, structure, length)?
- How will you judge whether the output is good?
- Any examples of good (or bad) output?
- What language should the prompt itself be in?

## Optimize flow

1. **Diagnose.** Read the existing prompt and list its weaknesses with severity — High / Medium / Low. Typical findings: no role, ambiguous task, no output format, conflicting or missing constraints, or prose so dense the AI cannot separate instructions from examples.
2. **Confirm.** One round: show the diagnosis and ask the user to confirm or correct it (recommendation: fix the High items first). If the user refuses or says "just rewrite", proceed.
3. **Rewrite.** Run the Generate flow's interview — at minimum confirm tier and language — then rewrite.
4. **Deliver.** Same format as Generate: prompt + rationale, but the rationale focuses on what changed and why.

## Fallback (user won't or can't answer)

If the user refuses to answer questions, demands the prompt immediately, or no interaction channel exists: state your assumptions explicitly in one line, write a **Standard**-tier prompt, mark uncertain parts as `PENDING: ...`, and invite revision. Never silently guess — always show what you assumed.

## Output format (both modes)

```
<prompt, inside a fenced code block>

**Why it's built this way**
- bullet 1
- bullet 2
- bullet 3
```

Keep the rationale short. The prompt is the deliverable; the rationale exists so the user can push back intelligently.
