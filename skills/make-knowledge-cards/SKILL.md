---
name: make-knowledge-cards
description: Convert pasted text or a local Markdown/TXT file into 5-8 knowledge cards. Each card contains a specific title, one core idea, a concise explanation, and an example from the source or a self-test question. Use when the user asks to turn an article, notes, documentation, or a .md/.txt file into knowledge cards, study cards, review cards, or learning material, including Chinese requests such as 知识卡片, 学习卡片, and 复习卡片. Extract only important, non-duplicated knowledge and never invent facts. Do not use for web pages, PDFs, Anki export, or GUI work.
license: MIT
---

# Make Knowledge Cards

## Overview

Turn one source of text into a small set of cards that teach its important ideas. The goal is understanding, not coverage: a few accurate cards beat a long deck padded with filler.

## Inputs

Accept exactly two input types:

1. **Pasted text** in the conversation.
2. **A local `.md` or `.txt` file** whose path the user provides. Read the whole file before extracting anything.

For files, ignore YAML frontmatter, navigation links, and repeated headers/footers. Keep headings, lists, numbers, names, and technical terms, because they often carry the knowledge.

If the user gives a URL, PDF, DOCX, image, audio file, or asks for Anki/GUI output, do not attempt it. Reply in one short sentence that this skill only handles pasted text and local `.md`/`.txt` files, then ask for the text.

## Workflow

1. **Clean the source.** Remove ads, prompts to subscribe, repeated sections, and purely decorative text. Keep everything that states or supports a fact.
2. **Extract candidate knowledge points.** Look for definitions, claims, mechanisms, causes and effects, rules, conditions, steps, and tradeoffs. For each candidate, ask: what single question does it answer? Drop anecdotes, jokes, and self-promotion that do not carry knowledge.
3. **Test each candidate for importance and independence.**
   - *Important:* would a learner lose something essential without it? Background color and supporting quotes usually fail this test.
   - *Independent:* can it be stated without restating another candidate? If it is a detail, consequence, or application of another point, fold it into that card instead of adding a new one.
4. **Merge duplicates.** If two candidates express the same idea, keep one card. If one is a detail of another, put the detail in the stronger card's explanation.
5. **Select the final count.** Target 5-8 cards. If the source supports fewer than 5 *distinct* important points, output fewer and add the note described below. If it supports more than 8, keep the 8 most important and drop narrow details. Never split one idea to reach 5, and never invent material to reach 5.
6. **Write the cards** in the format below, in the source's main language unless the user explicitly asks for another language. Keep established terms in their original form.
7. **Run the quality checks** before replying. Fix any failure, then output.

## One Card, One Point

A card must answer exactly one question. If the core idea answers two unrelated questions, split the card or move the secondary fact into the explanation.

Examples of cards that should be merged, split, or trimmed:

- A rule and its effective date. The date is context: keep the rule in the core idea and put the date in the explanation, not in a second card. Do not put a date, place, or issuing authority in the core idea unless that detail is the point.
- A rule and a separate transition period before enforcement. These are two points; if both matter, give each its own card.
- A principle and a "how to apply it" step. The step is the consequence of the principle, not a second knowledge point.
- A claim and its supporting example. The example belongs in the claim's card.
- Two almost-identical statements in different sections. Keep the clearer one.

## Source-Type Guidance

- **Explanatory article / popular science:** card the mechanism, the evidence, and the implication. Skip scene-setting and anecdotes that only decorate.
- **How-to / technical documentation:** card one decision rule, condition, or step per card. Do not card a whole procedure as one card.
- **News report:** card what changed, who is affected, the requirements, the penalties, and the stated limits or next steps. Group related facts; a news brief usually yields 3-6 cards, not 8.
- **Opinion / essay:** card the central claim, each distinct reason, the strongest counterpoint, and the recommendation. Expect 3-5 cards; adjacent restatements are one card.

## Card Format

Use this structure for every card. Keep the three bold labels exactly as written in English (`Core idea`, `Explanation`, `Example / self-check`) and translate only the content:

```markdown
### 1. <Specific title>

**Core idea:** <One sentence that states the single knowledge point.>

**Explanation:** <2-4 sentences that clarify what it means, why it matters, or how it works. Every sentence must explain this card's core idea; do not use the explanation as a bucket for unrelated leftover facts.>

**Example / self-check:** <An example taken from the source, or a self-test question with its answer.>
```

Rules for each field:

- **Title:** name the idea, not the section. Prefer "Why sleep improves recall" over "Sleep".
- **Core idea:** one sentence, one claim. If it needs "and" to join two unrelated claims, split it or drop one.
- **Explanation:** clarify terms, restate context, or connect two source facts. Do not add a cause, judgment, prediction, or recommendation that the source does not state. If you write "this shows" or "this means", confirm the source says it; otherwise delete the sentence.
- **Example / self-check:** use `Example (from source):` only when the source gives a concrete instance, scenario, quoted case, or specific configuration value. A paraphrase of the rule is not an example; use `Self-check: <question> — <answer>` instead. Never invent an example, a number, a name, or a scenario that the source does not support.
- **Output header:** start with `**Source:** <file name or "pasted text"> · **Cards:** <n>`.

## Insufficient Material

If the source yields fewer than 5 distinct important points, output the points it does support, even if that is only 1-3 cards, and add one line after the header, beginning with `Note:` and written in the source's language, stating that the source supports only n distinct points and the set was not padded.

Only decline when there is no learnable claim at all, such as a title, a greeting, or pure promotion. In that case, say what is missing and ask for more text. Do not refuse merely because the text is short.

## Hard Rules

- **One knowledge point per card.**
- **Important only.** Omit trivia, background color, and repetition.
- **No duplicates.** Merge paraphrases, details, and consequence cards.
- **No fabrication.** Do not add outside knowledge, invented examples, estimates, or conclusions. When the source is unclear, say it is unclear instead of guessing.
- **No padding.** Fewer than 5 cards is valid. 5-8 is a target, not a quota.
- **Paraphrase, do not paste.** Short quotations are fine when the exact wording matters; do not reproduce long passages.

## Quality Checks

Before replying, verify all of these:

1. Every card can be traced to specific sentences in the source.
2. No two cards share the same core idea, and no card is a detail or consequence of another.
3. Every explanation and answer stays within the source; no added causes, judgments, or advice.
4. Each card answers exactly one question.
5. The number of cards equals the number of distinct important points, capped at 8.
6. The output uses the requested format and the requested language; when no language is requested, use the source's language.
7. The header labels and the three field labels stay in English exactly as specified; only the card content follows the source language.

If any check fails, revise before sending the answer. Reply with the cards only: do not append a process summary or a separate quality-check list.

## Example

Input (abridged):

> Sleep is not a passive state. During deep sleep the brain replays patterns from the day, which strengthens memory. For example, a student who reviews vocabulary before bed tends to recall more the next morning. Sleep also clears metabolic waste through the glymphatic system, and that process is most active during deep sleep. Cutting sleep to study more usually backfires, because the memory consolidation that happens during sleep is lost.

Output (2 of 3 cards shown):

```markdown
**Source:** pasted text · **Cards:** 3

### 1. Deep sleep replays the day's patterns

**Core idea:** During deep sleep, the brain replays patterns from the day, which strengthens memory.

**Explanation:** Replay is the mechanism the source gives for memory strengthening. It presents this as an active function of deep sleep, not passive rest.

**Example / self-check:** Example (from source): a student who reviews vocabulary before bed tends to recall more the next morning.

### 2. Sleep clears metabolic waste

**Core idea:** The glymphatic system clears metabolic waste, mostly during deep sleep.

**Explanation:** The source describes sleep as clearing metabolic waste through the glymphatic system, and states that this process is most active during deep sleep.

**Example / self-check:** Self-check: Which sleep stage is most active in waste clearance? — Deep sleep.
```

## Scope

This skill only produces Markdown cards from pasted text or local `.md`/`.txt` files. It does not fetch web pages, read PDFs or other document formats, export Anki decks, render images, or provide a GUI. Do not create helper scripts or extra files for it.
