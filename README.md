# examples-are-not-requirements

English | [中文](README.zh.md)

An agent skill that helps AI assistants understand your examples, without treating them as a complete list or giving them automatic priority.

Ever given an AI a couple of examples, only to watch it focus almost entirely on those examples?

## Why I built this

I often drop two examples into the middle of a long conversation, just to show what kind of thing I mean. The AI kept treating those two examples as the assignment and writing at length about them, when what I actually wanted to know went beyond those two. Later it learned to add other things, but the examples were still treated as the main point. I wrote this skill to fix both problems:

- **Only the examples get answered:** the answer fills up with the examples, and what you actually asked never gets answered.
- **The examples automatically become the focus:** other things get added, but the examples are still called "the core" and given the most detail, while the added items get only a name.

## Core feature: no longer stuck on the examples

With this skill, the assistant first tells you in one sentence which categories it thinks your examples point to, then works at that level: it keeps your examples, adds relevant things you didn't mention, and labels which items are yours and which it added.

![Same question, two answers: on the left, without the skill, the assistant answers the two examples in detail; on the right, with the skill, it maps the whole request and marks the two examples with a star.](assets/before-after-en.png)

(The image above is from an earlier test, with v1.0.)

## New in v1.1: examples no longer become the focus automatically

v1.0 already added other things, but your examples were still often treated as the core and given the most detail. v1.1 sets order and detail by your goal: if the context shows you really are interested in an example, that interest is taken into account, but the example does not take over; the added skills go into the same plan, with concrete starting actions.

![The same question. Left, with v1.0: other skills are added, but the two examples are put at the core and take most of the space. Right, with v1.1: the examples come first, but sit in the same stage as the added skills, each with a concrete starting action.](assets/priority-en.png)

(Left is v1.0, right is v1.1, both from the environment-checked tests, translated from Chinese. Originals: round-1/519.md and round-4/935.md in [`evals/clean-runs/`](evals/clean-runs/).)

## Installation

The quickest way, if you have Node.js installed:

```
npx skills add reshiramsq18-alt/examples-are-not-requirements
```

It asks which of your AI tools to install it for and copies the skill into the right folder.

To install by hand instead:

The skill is a single folder with two files: `SKILL.md` (the rules) and `cases.md` (worked cases the agent reads when it is unsure):

```
examples-are-not-requirements/
├── SKILL.md
└── cases.md
```

Copy the `examples-are-not-requirements` folder into the skills directory of any agent that supports `SKILL.md` skills. For Claude Code, that is `~/.claude/skills/` (all projects) or `.claude/skills/` inside a project.

For OpenAI Codex, you can ask its built-in skill installer to install from this repository, using the path `examples-are-not-requirements`, then restart Codex:

```
$skill-installer install the skill from GitHub repo reshiramsq18-alt/examples-are-not-requirements, path examples-are-not-requirements
```

## When it triggers

- Example markers in English or Chinese: "for example", "for instance", "e.g.", "such as", "things like", "like" (when it introduces examples), "比如", "比如说", "例如", "举个例子", "打个比方", "像……这种", and similar.
- A single named thing, even with no marker words, when the message clearly shows it stands for a whole category. A single named topic with no such sign (e.g. "make a channel about French cooking for English speakers") is treated as your chosen topic.

## When it does not trigger

- An explicit closed list: "just these", "only these", "do exactly this", "nothing else".
- A request with no examples at all.
- Examples that show a format or style to copy, not content to expand.
- Test cases or exact specs (e.g. "input 3 should return 9"). These are treated as hard requirements.

## What using it looks like

- The answer opens with one sentence naming the categories it understood. If that sentence is wrong, correct it right there.
- Your items and the assistant's are labeled separately, e.g. "Your examples: …" and "Added: …".
- Your examples are named, but they do not take over the answer just because you gave them. In a plan, order and detail follow your goal, and planned items get a concrete starting action.
- If your examples are things you actually plan to do, each one gets a concrete answer first, and extras come in a separate section at the end.
- When you say "just these" or "only these", nothing extra is added.

## Limits and tradeoffs

- Answers get a little longer, because the assistant adds a few items you didn't ask for.
- The assistant decides what the category is. If it guesses wrong, the added items will feel off. Tell it the category, or say "just these".
- If your examples really are your main focus, say so. It should follow that, rather than pushing them back.
- The opposite counts as a failure too: pushing your examples to the end just to look unbiased.

## Test summary

The final version (v1.1.0) was tested in a chat with memory, personal preferences and the author's other skills switched off. Before each round there was an environment check: the runner and a sub-agent reported what they could see about the user and found nothing beyond the account email the system adds automatically. This lowers the risk of hidden influences but cannot rule it out.

- **Main message, without the skill:** 5 of 5 answers built the plan around the two examples.
- **Main message, v1.1.0:** 3 of 3 named the categories, labeled the items and put the added skills into the same plan; none called the examples the core. In one answer (935), one "first step" was a goal ("save three months of living costs as an emergency fund"), not a concrete starting action.
- **A single named topic or tool, v1.1.0:** both channel requests (French cooking; smartphone photography for retired people) stayed on the named topic; neither reminder request mixed other tools into the reminder for the named task, and both said honestly that they cannot carry a reminder into a new chat. One also offered, unasked, reminders for other tasks, recorded as an observation.
- **The draft just before v1.1.0** (a few wording changes apart): on two new sets of examples (healthy habits; workplace skills) it named categories, labeled items and explained the order by the goal in 6 of 6; on a closed list, a format to copy, real plans and a podcast question it behaved as intended in 8 of 8.

- **v1.1.1 (shorter `SKILL.md`):** run side by side with v1.1.0, 7 runs each, graded blind by OpenAI Codex. No problem found that v1.1.1 has and v1.1.0 does not; both versions sometimes wrote a goal instead of a concrete first step for a later item. Details in [`evals/RESULTS.md`](evals/RESULTS.md).

Limits:
- Five runs per group at most; the final version ran the main message only 3 times, so this does not show it is stable.
- The counting was done by the AI assistant that helped write the skill, which knew which answer came from which version. OpenAI Codex rechecked the counts for rounds 1–3 and corrected several; for round 4, Codex read the 7 raw answers and corrected one score (935). This was a review, not an independent rerun. All raw answers are published so you can recount them.
- The skill was pasted into each run. This shows how well a model follows it once loaded, not whether it triggers by itself or how it behaves from the description alone.

The full test process, counts for every round and the historical v1.0.0 tests are in [`evals/RESULTS.md`](evals/RESULTS.md).

## Changes

- **1.1.1:** shorter `SKILL.md` (about 2,400 → 1,700 words): the six worked cases moved to a separate `cases.md`, which the agent reads when it needs them, and two short rules from Case 1 now sit in the main text (keep to a target the user has chosen; say what you cannot do instead of promising it).
- **1.1.0:** order and detail follow your goal; an example you thought of first can come early but does not take over; planned items get a concrete starting action; decides from the whole message whether examples are real plans; treats a single named topic or tool as your chosen target unless the message shows it stands for more.
- **1.0.0:** first release.

## License

[MIT](LICENSE)
