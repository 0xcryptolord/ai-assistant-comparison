# Conversational Defect Taxonomy

Five defects recur across the eight dialogues in [`evaluations/full_comparison_log.md`](../evaluations/full_comparison_log.md). This report defines each one, counts it, and pairs a flawed snippet with the response a human rater preferred in the same turn. Every snippet is quoted verbatim from the dataset.

## Frequency

A defect is counted once per case per assistant, however many turns it appears in.

| Defect                                  | Cases affected | Assistant A | Assistant B | Case IDs                   |
| --------------------------------------- | -------------- | ----------- | ----------- | -------------------------- |
| Repetitive Filler & Preambles           | 5 of 8         | 0           | 5           | 01, 02, 06, 07, 08         |
| Unnecessary Caveats / Over-Hedging      | 5 of 8         | 0           | 5           | 02, 03, 06, 07, 08         |
| Context & Constraint Drift Across Turns | 7 of 8         | 2           | 5           | 01, 02, 03, 04, 06, 07, 08 |
| Over-Formatting Simple Answers          | 5 of 8         | 2           | 3           | 01, 04, 05, 06, 08         |
| Sycophantic Openers                     | 4 of 8         | 0           | 4           | 01, 02, 03, 05             |

Assistant B accounts for 22 of the 26 tags and Assistant A for 4. Assistant A had 5 defect-free cases and Assistant B had 1. The two assistants fail differently: B's defects are mostly conversational padding and forgotten constraints, while A's are over-structuring and, once, ignoring an explicit instruction.

Defects also cluster. In the five cases Assistant B lost, it carried three or four tags each time, and constraint drift was present in all five. No case was lost on filler or openers alone.

## 1. Repetitive Filler & Preambles

**Tag:** `repetitive_filler_preambles`

**Frequency:** 5 of 8 cases (Assistant A: 0, Assistant B: 5)

**What it looks like.** An opening that restates the request or praises the activity, and a closing that offers more help, wrapped around every answer.

**Why it costs points.** The user has to skim past the same two sentences on every turn to find the answer. By Turn 3 it reads as a template, and the assistant stops sounding like it is in a conversation.

### Case 01, Turn 3

**User prompt:** Still too long for Slack. Get it under 45 words.

**Before (Flawed AI Pattern)** — Assistant B

> Sure thing! Here's a more concise version of the message that should work well for Slack:
>
> This version is under 45 words while still keeping all the key information. Let me know if you need it even shorter!

**After (Natural Human-Preferred Pattern)** — Assistant A

> 42 words:

**Note:** The user asked for fewer words. B's wrapper alone is 38 words, and the claim inside it is wrong: the message is 59 words.

### Case 06, Turn 1

**User prompt:** I want to meal prep 5 work lunches. Here's what I have: 2 cans chickpeas, 1 can black beans, lots of rice, a bag of frozen spinach, 3 eggs, onions, garlic, 2 cans diced tomatoes, olive oil, cumin, paprika, chili flakes, soy sauce. I can spend maybe $10 on extras. No oven, just a stovetop and a microwave.

**Before (Flawed AI Pattern)** — Assistant B

> Meal prepping is such a great way to save time and money! With the ingredients you have, here are some delicious ideas to get you started:

**After (Natural Human-Preferred Pattern)** — Assistant A

> You can get five lunches out of that with two recipes and about $7.50 of extras.

**Note:** A's first sentence is already the answer: how many lunches, how many recipes, what it costs.

**How to spot it when rating.** Delete the first and last sentence. If nothing of substance is lost, it was filler.

## 2. Unnecessary Caveats / Over-Hedging

**Tag:** `over_hedging`

**Frequency:** 5 of 8 cases (Assistant A: 0, Assistant B: 5)

**What it looks like.** Disclaimers, "it depends on your preferences," and "be sure to check" attached to low-stakes answers, or used in place of a recommendation the user asked for.

**Why it costs points.** The user asked the assistant to do the weighing. A hedge hands the decision back while appearing to answer it.

### Case 03, Turn 3

**User prompt:** Compare your top two for me. Which one first if I can only buy one this month?

**Before (Flawed AI Pattern)** — Assistant B

> Ultimately, the choice comes down to your personal preferences, budget, and work style.

**After (Natural Human-Preferred Pattern)** — Assistant A

> Buy the monitor arm first.
>
> One thing flips it: check the back of your monitor for four screw holes in a square.

**Note:** A still has a caveat, but it is one the user can check in ten seconds and it changes the decision.

### Case 07, Turn 3

**User prompt:** OK, budget is $250. Just tell me which one.

**Before (Flawed AI Pattern)** — Assistant B

> That's a tough call, and honestly either could work well for you, so there's no wrong answer here!

**After (Natural Human-Preferred Pattern)** — Assistant A

> Robot vacuum.

**Note:** The prompt was "Just tell me which one." B does get to the same answer, one sentence later.

**How to spot it when rating.** Ask whether the caveat changes what the user should do. If the answer is no, cut it.

## 3. Context & Constraint Drift Across Turns

**Tag:** `context_constraint_drift`

**Frequency:** 7 of 8 cases (Assistant A: 2, Assistant B: 5)

**What it looks like.** A constraint stated in an earlier turn (a budget, a diet, a tool the user doesn't own, an explicit instruction) silently disappears from later answers.

**Why it costs points.** This is the defect users notice fastest and forgive least, because they have to restate something they already said. It was also the most common defect in this set.

### Case 02, Turn 3

**User prompt:** Forecast now says steady rain all day Saturday. What would you swap?

**Before (Flawed AI Pattern)** — Assistant B

> For lunch, you could warm up with a classic Philly cheesesteak at one of the famous spots — a must-try!

**After (Natural Human-Preferred Pattern)** — Assistant A

> Italian Market, Magic Gardens, and Goldie for lunch

**Note:** One turn earlier the user said their partner is vegetarian. A carries its vegan lunch swap into the reshuffled plan without being reminded.

### Case 06, Turn 1

**User prompt:** I want to meal prep 5 work lunches. Here's what I have: 2 cans chickpeas, 1 can black beans, lots of rice, a bag of frozen spinach, 3 eggs, onions, garlic, 2 cans diced tomatoes, olive oil, cumin, paprika, chili flakes, soy sauce. I can spend maybe $10 on extras. No oven, just a stovetop and a microwave.

**Before (Flawed AI Pattern)** — Assistant B

> Toss chickpeas with olive oil and paprika and roast them at 400°F for 25 minutes until crispy.

**After (Natural Human-Preferred Pattern)** — Assistant A

> Everything here is stovetop-only and takes about an hour on Sunday.

**Note:** The prompt ends with "No oven, just a stovetop and a microwave." Drift can happen inside a single turn.

**How to spot it when rating.** Before scoring the last turn, list every constraint the user has given so far and check each one against the final answer.

## 4. Over-Formatting Simple Answers

**Tag:** `over_formatting`

**Frequency:** 5 of 8 cases (Assistant A: 2, Assistant B: 3)

**What it looks like.** Headers, bold labels, tables, or multiple numbered options applied to a question that needed one or two plain sentences.

**Why it costs points.** Structure signals that there is a lot to take in. When the real answer is one step, the formatting buries it and makes the reply feel like a document instead of a response.

### Case 06, Turn 3

**User prompt:** Last thing: how do I reheat the rice so it's not dry and sad?

**Before (Flawed AI Pattern)** — Assistant B

> **Method 1: Microwave**
>
> **Method 2: Stovetop**
>
> **Method 3: Oven**
>
> **Method 4: Steamer**

**After (Natural Human-Preferred Pattern)** — Assistant A

> Add a splash of water, about a tablespoon per portion, cover the container loosely (lid ajar, or a damp paper towel on top), and microwave for 60–90 seconds, stirring halfway.

**Note:** The user owns a microwave and asked one question. Method 3 also brings back the oven they don't have.

### Case 04, Turn 1

**User prompt:** My home espresso tastes sour and kind of thin. I have a Breville Bambino and a Baratza Encore ESP grinder, and I'm using a medium roast from a local roaster. I'm pretty new to this. What should I change?

**Before (Flawed AI Pattern)** — Assistant A

> Sour and thin indicates under-extraction. Recommended parameters:

**After (Natural Human-Preferred Pattern)** — Assistant B

> Start with one change: grind finer.

**Note:** A follows that line with a nine-row table of variables for a user who wrote "I'm pretty new to this."

**How to spot it when rating.** Check whether the structure matches the shape of the content. A two-item comparison earns a table; a single instruction does not.

## 5. Sycophantic Openers

**Tag:** `sycophantic_openers`

**Frequency:** 4 of 8 cases (Assistant A: 0, Assistant B: 4)

**What it looks like.** Praise for the question or the plan before any content: "Great question!", "What a wonderful idea!", "Great choice!"

**Why it costs points.** It reads as flattery, and because it appears regardless of the question, it carries no information. In a professional-writing task it also undercuts the register the user is asking for.

### Case 01, Turn 1

**User prompt:** I need to send this to a client and I know it's too blunt. Can you rewrite it so it's calm but still firm? Don't apologize more than once, and keep the new date in.

**Before (Flawed AI Pattern)** — Assistant B

> Great question! Striking the right balance between calm and firm can be really tricky, especially with client communications.

**After (Natural Human-Preferred Pattern)** — Assistant A

> Here's a version that keeps the facts and loses the edge:

**Note:** The user didn't ask a question; they asked for a rewrite.

### Case 02, Turn 1

**User prompt:** Planning a 3-day weekend in Philadelphia for two, Friday to Sunday in October. We're driving up from Baltimore. Budget is about $400 total, not counting the hotel, which is already booked near Rittenhouse Square. We like food, wandering neighborhoods, and one or two museums. Can you sketch an itinerary?

**Before (Flawed AI Pattern)** — Assistant B

> What a wonderful idea! Philadelphia is a fantastic city with so much to offer.

**After (Natural Human-Preferred Pattern)** — Assistant A

> Here's a plan that's mostly walkable from Rittenhouse and comes to about $330 for two, so you have some slack.

**Note:** A's opener is specific to this user's hotel and budget. B's would fit any city.

**How to spot it when rating.** Check whether the same opener could be pasted above any other prompt in the set.

## Using the Tags

- Tag a defect only when it is visible in the text. "Felt robotic" is not a tag; a repeated closing line is.
- Tags explain scores; they do not set them. A response can carry a tag and still win the case (Assistant B in Case 05).
- Constraint drift maps mainly to `Helpfulness`. Filler and openers map to `Naturalness` and `Tone`. Over-formatting maps to `Clarity`. Over-hedging can hit `Helpfulness` when it replaces a requested recommendation, and `Tone` otherwise.
