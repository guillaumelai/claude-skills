# claude-skills
A collection of Claude Skills

## Skills

### `grill-me`
**Trigger:** mention "grill me", or ask Claude to stress-test a plan or design.

Interviews you relentlessly about every aspect of a plan or design until reaching a shared understanding. Works through each branch of the decision tree one question at a time, resolving dependencies between decisions. For each question, Claude provides its recommended answer. If a question can be answered by exploring the codebase, Claude explores it instead.

---

### `teach-me`
**Trigger:** `/teach-me <subject>` (e.g. `/teach-me Kafka`, `/teach-me CAP theorem`)

Delivers a structured deep lesson on any technical concept — tools, languages, frameworks, protocols, architecture patterns, or theoretical ideas. The subject can be anything technical.

Lessons are split into short (~5 min) and long (~15–20 min) formats and cover:
1. **30-second ELI5** — a plain-language metaphor to anchor understanding
2. **Why it exists** — the problem it solves and the pain before it
3. **Core concepts** — the vocabulary you need to follow along
4. **How it actually works** — step-by-step mechanism with diagrams where helpful *(long only)*
5. **Hands-on** — an interactive exercise delivered one step at a time, waiting for your output before continuing
6. **Common gotchas** — real production pain points *(long only)*
7. **Where to go next** — adjacent concepts, good resources, and a suggested follow-up lesson
