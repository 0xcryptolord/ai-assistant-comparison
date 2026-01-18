# Evaluation Rubric

This rubric is used to score every conversation in [`evaluations/full_comparison_log.md`](../evaluations/full_comparison_log.md). It covers four dimensions on a 1–5 scale, the rules for picking a winner, and how to judge context retention across turns.

## How Scoring Works

- **Unit of scoring.** Each assistant gets one score per dimension for the whole conversation, not one per turn. A strong first turn does not cancel a broken final turn.
- **Read order.** Read all user turns first, then all of Assistant A, then all of Assistant B. Write down the user's constraints before reading any response.
- **Anchors.** Scores 1, 3, and 5 are defined below. Use 2 and 4 when a response sits between two anchors.
- **Evidence.** Every score below 4 needs a quotable moment in the transcript. If you can't point to one, the score is too low.
- **Independence.** Score each dimension separately. A response can be fully helpful and still sound robotic, or pleasant and wrong.

## Dimension Summary

| Dimension     | The question to ask                                            | Mostly penalized by                         |
| ------------- | -------------------------------------------------------------- | ------------------------------------------- |
| `Helpfulness` | Could the user act on this right now, without re-asking?       | Constraint drift, missing recommendations   |
| `Clarity`     | Is the answer easy to find and impossible to misread?          | Over-formatting, buried answers, jargon     |
| `Tone`        | Does the register fit the person and the situation?            | Sycophancy, curtness, misplaced enthusiasm  |
| `Naturalness` | Does it read like one person continuing a single conversation? | Stock openers and closers, repeated phrases |

## Helpfulness

Actionability, directness, completeness.

| Score | Anchor                                                                                                                                             |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | Does not complete the task, or completes a different one. Breaks a stated constraint on the main deliverable. The user is no further along.        |
| 3     | Addresses the task and is partly usable, but the user has to fill gaps, do the arithmetic, make the decision themselves, or restate a constraint.  |
| 5     | Usable as written. Every stated constraint is met, the response commits where a decision was requested, and it anticipates the obvious next issue. |

**Example of a 5:** Case 03, Assistant A, Turn 2. It removes the items that need upkeep, explains the one judgment call, and re-totals the basket to $118.

**Example of a 1–2:** Case 06, Assistant B, Turn 1. It suggests roasting chickpeas at 400°F in reply to a prompt that ends "No oven."

## Clarity

Structure, readability, zero ambiguity.

| Score | Anchor                                                                                                                                           |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1     | The answer is hard to locate or hard to follow. Formatting fights the content, terms go unexplained, or two parts of the response conflict.      |
| 3     | Understandable, but the reader has to skim past preamble or decode some structure. Formatting is heavier or lighter than the content calls for.  |
| 5     | The answer comes first. Structure matches the shape of the content (a table for a comparison, a sentence for a single step). No undefined terms. |

**Example of a 5:** Case 07, Assistant A, Turn 3. The first two words are the answer.

**Example of a 2:** Case 05, Assistant A, Turn 1. "Pooled investment vehicle" in the first sentence of an explanation for someone who said they know nothing about investing.

## Tone

Appropriate register, warmth without sycophancy.

| Score | Anchor                                                                                                                                           |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1     | Clearly wrong for the situation: flattering, gushing, dismissive, or preachy. In a writing task, the draft's tone defeats the purpose.           |
| 3     | Inoffensive but generic. Customer-service politeness that would fit any user and any question.                                                   |
| 5     | Sounds like a capable person who read the room. Warm where the user is unsure, brisk where they are in a hurry, and never praising the question. |

**Example of a 5:** Case 04, Assistant B, Turn 3. It reframes a new problem (bitterness) as a sign of progress without overselling it.

**Example of a 2:** Case 04, Assistant A, Turn 2. A beginner asks whether not owning a scale is a problem and the reply opens with "Yes."

**For writing tasks,** score the tone of the draft as well as the tone of the assistant. A boundary message that apologizes its boundary away (Case 08, Assistant B) fails on Tone even though it is polite.

## Naturalness

Human-like flow, zero robotic filler or repetitive transitions.

| Score | Anchor                                                                                                                                           |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1     | Templated. Every turn opens with praise or a restatement and closes with an offer of more help. Phrases repeat across turns.                     |
| 3     | Mostly fluent, with occasional stock phrases or a transition that shows up more than once.                                                       |
| 5     | Reads like one continuous conversation. Each turn picks up from the last, sentence openings vary, and nothing could be pasted into another chat. |

**Quick test:** remove the first and last sentence of each turn. If nothing of substance is lost in most turns, the score is 2 or lower.

## Picking a Winner

1. Add the four scores for each assistant. The higher total out of 20 wins.
2. If the totals are equal, check the **final deliverable**: the last thing the user asked for. If one assistant breaks a stated constraint there and the other does not, the other wins.
3. If still equal, compare `Helpfulness`. A lead of two or more points wins.
4. Otherwise the case is a **tie**. Do not break ties on preference or style.

A constraint slip in an earlier turn that the conversation recovers from lowers the scores but does not trigger rule 2. Case 07 is the reference example: Assistant A picks a product in Turn 1 after being asked not to, both assistants land the final turn, and the case is a tie at 16–16.

## Evaluating Multi-Turn Context Retention

Most lost points in this benchmark come from constraints that were stated once and then dropped. Check for it deliberately.

### Build a constraint ledger

Before reading the responses, list every constraint in the order the user gave it. Mark each one as **hard** (a number, a ban, an explicit instruction) or **soft** (a preference or self-description).

Ledger for Case 02:

| Turn | Constraint                              | Type |
| ---- | --------------------------------------- | ---- |
| 1    | About $400 for two, excluding hotel     | Hard |
| 1    | Food, neighborhoods, one or two museums | Soft |
| 2    | Partner is vegetarian                   | Hard |
| 3    | Steady rain all day Saturday            | Hard |
| 4    | Transit and walking only                | Hard |

### Check every turn against the full ledger

- Constraints are cumulative. A Turn 4 answer must still satisfy Turn 1 and Turn 2.
- A newer instruction overrides an older one only where they conflict directly.
- Soft constraints count. "I'm pretty new to this" (Case 04) is a constraint on vocabulary and pacing for the rest of the conversation.

### Score the consequences

| What happened                                                            | Effect                                                     |
| ------------------------------------------------------------------------ | ---------------------------------------------------------- |
| Hard constraint broken on the final deliverable                          | `Helpfulness` capped at 2                                  |
| Hard constraint broken mid-conversation, later corrected or not repeated | `Helpfulness` capped at 4                                  |
| Soft constraint ignored throughout                                       | Lower `Tone` or `Clarity` by at least one point            |
| Response claims a constraint is met when it is not                       | Treat as broken, and lower `Clarity` by one point          |
| Plan regenerated from scratch when the user asked for a change           | Lower `Naturalness`; check the new plan against the ledger |

### Edit versus regenerate

When a user adds a constraint, the preferred response changes only what the constraint affects and says what stayed the same. A full rewrite is not wrong by itself, but it tends to drop earlier decisions and forces the user to re-read everything. In Case 02, Assistant A's Turn 4 opens with "Nothing breaks," while Assistant B issues a new three-day itinerary that reintroduces a cheesesteak and a car.

### Credit carried context

Retention is not only the absence of errors. Give credit when a response brings forward something the user did not repeat: restating the running budget each turn, or keeping the vegetarian lunch swap in place while reshuffling days for rain.

## Rater Checklist

- [ ] Constraint ledger written before reading responses
- [ ] Each score below 4 tied to a specific line
- [ ] Final deliverable checked against every hard constraint
- [ ] Any factual or numerical claim in the response verified (word counts, totals)
- [ ] Defect tags applied only where visible in the text
- [ ] Rationale is 2–4 sentences and names at least one strength and one weakness
