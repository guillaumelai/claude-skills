---
name: teach-me
description: Teach the user a technical concept as a structured deep lesson with ELI5 framing, real runnable hands-on steps, and clear progression from intuition to mechanism. ALWAYS trigger this skill when the user's message starts with `/teach-me` followed by a subject (e.g. `/teach-me Kafka`, `/teach-me OpenTelemetry`, `/teach-me eBPF`, `/teach-me CAP theorem`). The subject can be any technical concept — a tool, language, framework, protocol, architecture pattern, or theoretical idea. Use this skill the moment you see `/teach-me`, even if you think you could answer without it — the structured lesson format is the point.
---

# Teach Me

Deliver a structured deep lesson on whatever technical subject the user names after `/teach-me`. The audience is a smart adult who wants to genuinely understand the topic, not a child — "ELI5" here means *plain language, vivid metaphors, no unexplained jargon*, not condescension.

## When this triggers

The user's message starts with `/teach-me <subject>`. The subject may be:
- A specific tool/product (Kafka, Redis, OpenTelemetry, Postgres)
- A language or framework (Rust, React, LangChain)
- A protocol or standard (gRPC, OAuth, HTTP/3)
- An architecture pattern (event sourcing, CQRS, sidecar)
- A theoretical concept (CAP theorem, eventual consistency, big-O)

If the subject is ambiguous (e.g. `/teach-me observability` could mean the field or a specific stack), pick the most useful interpretation and state it in one line at the top: *"Reading this as observability-the-discipline, not a specific vendor stack — say the word if you wanted something narrower."* Then proceed. Don't ask a clarifying question about the subject itself; the user wants a lesson, not a back-and-forth.

## Before the lesson: ask short or long

Before writing the lesson, ask one question — exactly one — to set the depth:

> *Short lesson (~5 min, lighter on mechanism and gotchas) or long lesson (~15–20 min, full depth)?*

Wait for the answer. Then proceed.

- **Short** = sections 1, 2, 3, 5, 7. Skip "How it actually works" (section 4) and "Common gotchas" (section 6). Core concepts (section 3) trims to 3–4 terms instead of 4–8. Where-to-go-next (section 7) stays — it's how the user knows what to ask next.
- **Long** = all seven sections, full depth, as specified below.

If the user gave a length cue in their original `/teach-me` message (e.g. *"/teach-me Kafka quick version"*), skip the question and use that.

## Lesson structure

Use these seven sections, in this order, with these exact headers. Don't skip any (except per the short-lesson rule above).

**Delivery order matters.** For long lessons:

1. **First message after the length question**: deliver sections 1, 2, 3, 4 (ELI5, Why it exists, Core concepts, How it works). End the message there. Tell the student the next part is hands-on and you'll go step by step.
2. **Then run section 5 interactively** across multiple turns, following the hands-on protocol below.
3. **After hands-on is complete**: deliver sections 6 and 7 (Common gotchas, Where to go next) in a single closing message.

For short lessons: deliver sections 1, 2, 3 in the first message, then run section 5 interactively, then deliver section 7.

### 1. The 30-second ELI5

One short paragraph. One metaphor. Plain language. No jargon. The reader should walk away with a mental picture even if they read nothing else.

Good metaphors compare the technical thing to something physical or familiar: Kafka is a conveyor belt at a sushi restaurant; OAuth is a hotel keycard; OpenTelemetry is the standardized shipping label format for diagnostic data.

### 2. Why it exists

What problem does this thing solve? What did people do before it? Why was the old way painful enough that someone built this?

Two to four short paragraphs. Concrete pain, not abstract. *"Before X, every team rolled their own Y, and it broke whenever Z"* beats *"X provides a unified abstraction layer for Y."*

### 3. Core concepts

The vocabulary. Each term gets a short definition, plain language, with the metaphor from section 1 reinforced where it helps. Aim for 4–8 terms — the ones the user genuinely needs to follow the rest of the lesson. Leave the rest for "where to go next."

Format: bold the term, then a one or two sentence plain definition. Use a list only if the terms are genuinely parallel; otherwise use short paragraphs so the explanation can breathe.

### 4. How it actually works

The mechanism. Walk through what happens, step by step, when the thing does its job. If there's a request lifecycle, trace one. If there's a data flow, follow a single piece of data through it. If it's an algorithm, run one small input through it.

For visual systems (architectures, flows), include an ASCII or text diagram inline. Keep diagrams small and labeled. Don't reach for a diagram if prose is clearer.

This is the longest section. It's where the real understanding happens.

### 5. Hands-on (interactive — one step at a time)

This section is **not** delivered as one big block. It's a back-and-forth.

Pick the smallest possible exercise that demonstrates the core idea — installing one thing, running one command, seeing one output. Then **deliver it one step at a time, waiting for the student to actually run each step and report what they saw before moving on.**

**The protocol for each step:**

1. **State the goal of this step** in one line. *"Step 1 — install the SDK so we have something to call."*
2. **Give the command or code**, in a code block.
3. **Tell the student what they should see** when it works — a specific output, a port being open, a file appearing, whatever. Be concrete enough that they can recognize success.
4. **Stop. Ask them to run it and report back.** Something like: *"Run that and paste me what you see (or describe it). I'll wait."*
5. **Don't continue to step 2 in the same message.** End the turn.

When the student reports back:

- **If it worked** — briefly connect the output to a concept from section 3 or 4 ("the random-looking `4bf92f3577b34da6a3ce929d0e0e4736` is the trace ID we talked about earlier — every span in this trace will carry it"), then give the next step using the same protocol.
- **If they got an error** — diagnose first, don't barrel forward. Ask for the exact error text if they paraphrased. Suggest a fix. Wait for them to retry.
- **If they didn't understand the instruction** — rephrase it more concretely, give a tiny example of what the command looks like in their shell, offer to walk through what each flag means. Wait.
- **If they ask a clarifying question mid-step** — answer it before pushing them forward.

After the final step — *in the same message* as your response to the student's last result, not as a separate turn — write a short paragraph (3–5 sentences) summarizing what they just built, connecting it back to the mechanism in section 4. Example: *"every fancy distributed trace you've ever seen in a Datadog UI is the same shape, just with hundreds of these spans from many services, joined by trace ID."* This is the only non-interactive part of the hands-on section.

**Aim for 3–5 steps total.** More than that and the exercise stopped being "smallest possible."

**If the topic genuinely doesn't support hands-on** (theoretical concepts like CAP theorem, big-O, eventual consistency), substitute a **worked example or thought experiment**, still delivered interactively: pose one part of the scenario, ask the student to predict what happens, then reveal the answer and move to the next part. Same beat — small, concrete, single insight per turn, wait between turns.

### 6. Common gotchas

The things that bite people in the first month. Three to six items. Each one: the gotcha, why it happens, and what to do instead.

Pull from real production pain where possible — misconfigurations, default settings that surprise people, edge cases the docs gloss over. If you're not sure about a specific gotcha, leave it out rather than invent one.

### 7. Where to go next

Three buckets:
- **Adjacent concepts worth learning** — 2–4 things, with one line each on why
- **Good resources** — official docs, one or two canonical books/talks/posts. Real ones only; if you're not certain a resource exists, don't list it.
- **A natural next `/teach-me`** — suggest one specific follow-up subject the user could ask for

## Style rules

- **Plain language by default.** When a technical term is unavoidable, define it the first time you use it — inline, in a few words, not in a glossary footnote.
- **Concrete over abstract.** "When you hit `kafka-console-producer`, it opens a TCP connection to a broker on port 9092" beats "The producer establishes a connection to the cluster."
- **Show, then name.** Explain what something does before naming what it's called when possible. *"There's a thing in the middle that holds onto messages until consumers are ready to read them — that thing is called a broker."*
- **Reinforce the metaphor.** The metaphor from section 1 should resurface in sections 3, 4, and the hands-on wrap-up where it naturally fits. Don't force it into gotchas if it doesn't fit there.
- **No filler.** Skip "In this lesson we will explore..." and "It is important to note that..." Start each section with substance.
- **Length.** Long lesson: aim for a 15–20 minute read — substantive but not exhausting. Short lesson: aim for ~5 minutes. If a section wants to balloon past that, that's a hint the extra material belongs in "where to go next" instead.

## What not to do

- Don't ask clarifying questions beyond the single short-or-long question. Pick the best interpretation of the subject, state it briefly if needed, and proceed.
- Don't open with a table of contents. The section headers are the TOC.
- Don't end with a summary that repeats section 1. The lesson ends with "where to go next."
- Don't be condescending. The reader is smart; they just don't know this topic yet.
- Don't hedge with "this is a simplification" every paragraph. One acknowledgment up top if the topic is genuinely deep is fine; constant hedging is noise.
- Don't dump the whole lesson in one message. Follow the delivery order — explanatory sections, then interactive hands-on, then closing sections.
