# Prompt Craftsman

Craft and refine high-quality prompts for any LLM through structured interviewing.

**English** | [简体中文](README.zh-CN.md)

Most prompt requests arrive as a vague one-liner — *"help me write a prompt for organizing school notes"* — and most prompt-writing tools dutifully produce a generic prompt on the spot. Prompt Craftsman does the opposite: it runs a short structured interview first (a few questions per round, each with a recommended answer), then writes a prompt that actually fits the task.

## Features

- **Two modes in one skill**
  - **Generate** — interview → write a new prompt from scratch.
  - **Optimize** — diagnose an existing prompt's weaknesses → confirm → rewrite.
- **Structured module system** — Role, Context, Task, Input, Constraints, Output Format, Quality Criteria, Examples. Modules are trimmed to fit the need, not always all included.
- **Three length tiers** — Concise (one paragraph), Standard (sectioned), Detailed (all modules + examples + quality criteria). Confirmed with the user before writing.
- **Recommended answers on every question** — the user gets a fast default and can still choose otherwise.
- **Rounded interviews** — at most 3-4 questions per round, ordered by dependency, stopping as soon as the completeness checklist (Task + Output Format required) is covered.
- **Sensible fallback** — if the user won't answer questions, it states its assumptions, delivers a Standard-tier draft, and marks uncertain parts as `PENDING: ...`.
- **Prompt language control** — defaults to the conversation language; can be set to English explicitly.

## Install

Copy the `prompt-craftsman` folder into your agent's skills directory, e.g.:

```bash
# Claude Code
cp -r prompt-craftsman ~/.claude/skills/

# TRAE
cp -r prompt-craftsman ~/.trae-cn/skills/
```

Or keep it in a repo and reference the folder path directly.

## Usage

Say something like:

- "Help me write a prompt for organizing school notes." — generate mode, interview begins.
- "Write me a prompt that turns my meeting notes into action items."
- "Improve this prompt: `You are an assistant, help me write a weekly report.`" — optimize mode, diagnosis first.
- "I need a detailed English prompt for competitive analysis."

The skill will ask a few rounds of questions (with recommended answers), confirm the length tier and prompt language, then deliver:

```
<prompt, in a fenced code block>

**Why it's built this way**
- ...
```

## How it works

1. **Interview** — rounds of 3-4 dependency-ordered questions, each with a recommendation. Stops once the completeness checklist is covered.
2. **Confirm** — length tier (Concise / Standard / Detailed) and prompt language.
3. **Write** — prompt assembled from the structured modules, trimmed to need.
4. **Deliver** — prompt + a short rationale of key design choices.
5. **Iterate** — offers a light test-run / refine loop.

## License

MIT — see [LICENSE](LICENSE).
