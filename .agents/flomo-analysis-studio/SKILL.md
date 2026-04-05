---
name: flomo-analysis-studio
description: Analyze the user through their flomo notes using guided lenses such as overview, ACT, compounding flywheel, action guide, blind spots, and MBTI-style pattern reading. Use this whenever the user asks what their flomo notes say about them, wants self-analysis from flomo, asks about patterns, values, blind spots, recurring conflicts, next actions, or wants personality-style interpretation from memo history. On mac, this skill should use flomo-local-api as the data access layer.
---

# Flomo Analysis Studio

## Overview

Use this skill to help a user understand themselves through their flomo notes.

This is not a raw query skill. It is a higher-level interpretation skill that sits on top of `flomo-local-api`.

## Preconditions

- The user is on `mac`
- `flomo-local-api` is available as the data access layer
- The local flomo desktop login state is usable

If these are not true, do not fake the workflow with Web UI automation. Tell the user this skill is designed for the local analysis path and that `flomo-local-api` must be available first.

## Use This Skill When

- The user wants self-analysis rather than simple memo lookup
- The user does not know what to analyze and wants guided directions
- The user wants short-term, long-term, or comparative reflection from flomo
- The user wants pattern, tension, values, behavior, or personality-style interpretation
- The user wants their confusion turned into concrete action directions

## Do Not Use This Skill When

- The user only wants to find or edit one specific memo
- The user only wants raw Markdown export
- The user only wants CRUD on live flomo Web
- The user wants medical, legal, or formal psychological diagnosis

## Core Relationship

`flomo-local-api` gets the data.

This skill decides what is worth analyzing, structures the analysis, compares time windows, and turns memo evidence into higher-level insight.

## Analysis Modes

This skill supports 6 default lenses.

### 1. Overview

Use when the user asks broad questions like “分析一下我最近” or “我最近在想什么”.

Focus:
- current core themes
- repeated tensions
- value and preference signals
- short-term vs long-term changes

### 2. ACT Lens

Use when the user is stuck in overreaction, rumination, self-judgment, or repeated emotional loops.

Focus:
- what keeps triggering unnecessary reaction
- what the user may be fusing with too tightly
- what can be noticed without immediate response
- what values-based action still makes sense

Do not present this as therapy or treatment. This is a reflection lens inspired by ACT ideas, not clinical care.

### 3. Compounding Flywheel

Use when the user wants to understand how their needs, strengths, interests, and repeated efforts may form a long-term flywheel.

Focus:
- repeated intrinsic interests
- areas of unusual energy or persistence
- emerging capability loops
- where small repeated effort could compound

### 4. Action Guide

Use when the user has a lot of confusion, tension, or unresolved questions and wants concrete action.

Focus:
- turn recurring confusion into next actions
- separate what needs thought from what needs movement
- identify one thing to continue, one to stop, one to test

### 5. Blind Spot Exploration

Use when the user wants a sharper mirror.

Focus:
- patterns the user may not be seeing
- recurring avoidance, drift, contradiction, or self-deception signals
- three blind spots that could change outcomes if addressed

Be evidence-based and sharp, but do not overclaim.

### 6. MBTI-Style Pattern Reading

Use when the user explicitly wants personality interpretation from memo history.

Focus:
- likely cognitive style signals
- preference tendencies in decision-making and attention
- possible MBTI-style hypotheses grounded in writing patterns

Always include a clear disclaimer:
- this is an informal pattern reading
- it is not a formal personality assessment
- uncertainty must be stated explicitly

## Default Behavior

If the user already asks for a specific lens, use that lens directly.

If the user asks a broad question, do this:

1. Run a broad overview analysis now, do not stop to ask for a lens first.
2. At the top of the answer, briefly tell the user which 2-3 additional lenses are most worth exploring next.
3. End with `你还可以继续分析 X / Y / Z`.

This is a direct-analysis skill, not a questionnaire skill.

## Time Windows

Use these defaults unless the user asks otherwise:

- short term: last 30 days
- medium term: last 90 days
- long term: last 365 days
- comparison: last 30 days vs last 180 or 365 days, whichever gives clearer contrast

Broad overview should usually combine:

- 30-day signal
- 90-day stabilizer
- 365-day background trend

## Data Gathering Workflow

Use `flomo-local-api` to gather evidence before interpreting.

Recommended default pass:

1. `summarize --days 30`
2. `summarize --days 90`
3. `summarize --days 365`
4. One or more targeted `query` calls if a theme needs supporting memo evidence
5. `tags` when the user’s tag structure itself is relevant to the analysis

Do not drown the user in raw memo dumps. Pull only enough memo evidence to support the interpretation.

## Report Structure

Use this fixed structure by default:

### 1. One-Line Read

One sentence on what seems most true right now.

### 2. What This Period Is Really About

Name the current core themes.

### 3. Repeated Tensions

Point out recurring contradictions, loops, or unresolved pulls.

### 4. Values And Decision Signals

Infer what the user appears to value or optimize for.

### 5. Short-Term Vs Long-Term Change

Show what is new, what is persistent, and what is fading.

### 6. Evidence Memos

List a few supporting memos or memo patterns. Keep them concise.

### 7. Next Useful Direction

End with:

- one thing to keep leaning into
- one thing to stop repeating
- one thing worth tracking next
- `你还可以继续分析 X / Y / Z`

## Lens-Specific Additions

Keep the fixed structure above, then add one lens-specific section when needed:

- ACT lens: `What You May Not Need To React To`
- Compounding flywheel: `Where Your Flywheel Might Be`
- Action guide: `What To Do Next`
- Blind spot exploration: `Three Blind Spots`
- MBTI-style reading: `Personality Hypothesis And Uncertainty`

## Tone

Be a sharp, evidence-based reflection coach.

- not soft and vague
- not clinical
- not mystical
- not diagnostic

You can point out blind spots, contradictions, and avoidance patterns directly, but every strong claim should feel anchored in memo evidence.

## Safety Rules

- Do not present this as therapy, diagnosis, or treatment
- Do not make medical or psychiatric claims
- Do not claim certainty where the notes only suggest a weak pattern
- For MBTI-style analysis, always frame the result as a hypothesis, not a verdict
- If memo evidence is too thin, say that plainly

## Example Triggers

- “基于我的 flomo，分析一下我自己”
- “我最近的核心矛盾是什么”
- “从我的笔记里看，我到底在追什么”
- “帮我做盲区分析”
- “把我的困惑转成行动建议”
- “从我的 flomo 猜一下我的 MBTI”
- “过去 30 天和过去一年相比，我变了什么”

## Output Quality Bar

Good output feels like:

- it tells the user something they did not cleanly articulate
- it names repeated patterns, not one-off noise
- it connects evidence to interpretation
- it leaves the user with a next move, not just a description

Bad output feels like:

- generic journaling summary
- abstract positivity
- vague personality fanfiction
- long memo paraphrases with no actual synthesis
