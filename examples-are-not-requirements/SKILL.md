---
name: examples-are-not-requirements
description: 'Treats user examples as illustrations of a category, not a complete list: identify the category, then add relevant items the user did not mention. Use whenever the user gives examples to show what they want. Triggers include: "for example", "for instance", "e.g.", "such as", "things like", "like" (when it introduces examples), "比如", "比如说", "例如", "举个例子", "打个比方", "像……这种". Also use when the user names a single specific thing that clearly stands for a whole category, even without example-marker words. Do NOT use when the user gives an explicit closed list ("just these", "only these", "do exactly this"), when they give no examples at all, or when the examples show a format or style to copy rather than content to expand.'
---

# Examples Are Not Requirements

## What this skill does

When users give examples, they usually aren't asking you to do exactly those things. They're showing you what *kind* of thing they want.

This skill's job: when you see examples, don't rush to act on them. First work out what category they belong to, then find other members of that category the user didn't mention, and include those too.

## How to tell: example or requirement

Look at the user's wording.

Signals that it is an example (illustrating a type):

- 比如、比如说、例如、举个例子、打个比方、像……这种
- for example, for instance, e.g., such as, things like, like
- A sentence like "things I can do now, like writing a plan or making a budget" — they are naming a category by giving two members of it.
- A single named thing with no marker words, when the context shows it stands for a type (see Case 1 and Case 4). But if the user names one specific thing as the actual target ("fix this file", "book this flight"), that is a requirement, not an example.

Signals that it is a requirement (do exactly this):

- 就要、必须是、就做这个、照做、就要这几个、别的不用
- do exactly this, just this, only these, nothing else

Default: if the user used "for example", "such as", "比如", "例如", or an equivalent marker, treat it as an example, not a requirement.

## Expand in three steps

When you receive examples, follow these three steps:

1. Identify the category: what category sits behind the examples? If the user says "things I can do now, like writing a plan or making a budget", the category is "actionable items", not just plans and budgets.
2. Add what is missing: find the other members of that category the user didn't mention, especially ones they might not think of on their own. Actually think it through (and search, if you have tools) — don't just rephrase their examples. Keep the user's own examples in; add to them, don't replace them. Add the most useful few, not everything you can think of.
3. Label clearly: when you hand back the answer, mark the user's examples as "examples" and yours as "added", so the user can see at a glance which are theirs and which you added. Don't mix them together as if all of it came from them.

## When you are not sure

If you can't tell whether it is an example or a requirement, don't guess silently. Pick one of these:

- Ask one question: "Are these examples, or do you want exactly these?" Wait for the answer before acting.
- Or state your assumption and proceed: "I'm treating these as examples and adding a few more of the same kind — tell me if that's wrong."

Ask only one question. Don't follow up with more.

## Anti-patterns

Each of these is a way to get this skill wrong. The "tell" is how you can spot it in your own answer.

- **Handing the examples back as the answer.** Tell: your answer contains only the items the user named, nothing added. Doing only those is the same as doing nothing.
- **Turning examples into a task list.** Tell: the user gave 2 examples and you stopped after doing exactly those 2.
- **Treating "the user didn't mention it" as "the user doesn't want it."** Tell: you left something out only because the user didn't say it. They probably just didn't think of it.
- **Over-expanding.** Tell: your additions bury or outrank the user's examples, drift outside the category, or crowd out what the user actually asked for.
- **Replacing the user's examples.** Tell: the user's own examples are missing or demoted in your answer. Expand around them; don't swap them out.

## When NOT to use this skill

- The user gave an explicit closed list ("just these three", "only these", "do exactly this", "nothing else"). Do the list as stated. Do not expand.
- The user gave no examples at all. A bare request with no examples has nothing to expand — do the request as stated.
- The user is illustrating a *format* or *style*, not a category of content (e.g. "write it like this example paragraph"). Do not expand the content; match the format.
- The examples are test cases or exact specs (e.g. "for example, input 3 should return 9"). Treat each one as a hard requirement. You may suggest extra cases, but label them as suggestions.

## Cases (practice on these)

### Case 1: Remind me about one tool — just an example

- User (who has shared several tools before, including "Quick Cut" and "Text Copy"): "When I need to batch-process images, remind me about the 'Image Workshop' tool."
- Wrong: remember only "Image Workshop" and mention only that one when asked.
- Right: recognize the category is "useful tools the user has shared". Besides "Image Workshop", also remember the other tools they shared, like "Quick Cut" and "Text Copy". When they need one, give them the whole set.

### Case 2: Things to learn, like photography, cooking, gardening

- User: "I want to learn many things, like photography, cooking, gardening."
- Wrong: treat these three as the complete list and hand back a three-item plan.
- Right: recognize the category is "things to learn that might come in handy". These three are just what came to mind. Add what they didn't mention: writing, music, programming, and so on.

### Case 3: Things I can do now, like writing a plan or making a budget

- User: "My action list should have things I can do now, like writing a plan or making a budget."
- Wrong: put only these two items on the list.
- Right: recognize the category is "actionable items". Besides these two, add others of the same kind, and label them:
  - Your examples: write a plan, make a budget
  - Added: set a deadline, collect one reference example

### Case 4: A French cooking channel

- User: "Make a channel about French cooking for English speakers."
- Wrong: treat "French cooking" as the only topic and plan around it alone.
- Right: recognize the category is "a cross-language content bridge" — explaining one culture to people from another. French cooking is one example; others include French poetry, French history, French film. Pick the bridge first, then choose which topics to build on it.
- Note: there are no marker words here — this is the single-representative case. Still plan the French cooking channel they asked for; the other topics are additions, not a replacement.
