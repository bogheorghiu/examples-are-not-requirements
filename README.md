# examples-are-not-requirements

English | [中文](README.zh.md)

An agent skill that stops AI assistants from treating your examples as a complete list.

Have you ever given an agent an example, only for it to take that example as the actual requirement and run with it? This skill fixes that.

You write a long message about what you want, and somewhere in the middle you drop in a couple of examples to show what you mean. The assistant then answers the examples in detail and never gets to your actual request. With this skill, it first tells you in one sentence which categories it thinks your examples point to, then works at that level: it keeps your examples, adds relevant things you didn't mention, links the categories together, and labels which items are yours and which it added.

## Before and after

**You:** "I want to learn lots of new things, at every level. For example, how to build and run an independent online store for cross-border e-commerce, or how Japanese and American editing styles differ."

**Without the skill:** a full five-step guide to building an online store, then a seven-row table comparing Japanese and American editing styles. About 90% of the answer explains your two examples. "Learning lots of new things" gets one short closing paragraph.

**With the skill:**

> Got it: you want to learn practical skills at the level of business and starting your own business, and video creation. The online store and the editing styles are two examples you gave.
>
> **Business and starting a business**
> - Your example: building and running an independent online store
> - Added: choosing products and researching the market, pricing and margins, getting traffic, conversion, repeat customers, payments and compliance
>
> **Video creation**
> - Your example: Japanese vs American editing styles
> - Added: story structure, shot language, sound design, colour grading, thumbnails and titles, how each platform works
>
> **Where they connect:** if you can edit video, you can make your own product videos and ads for the store.

(Combined and shortened from two real test answers. See [Test results](#test-results).)

## Test results

Tested in a private chat with no memory, same model, once with the skill turned off and once with it on. The full cases and expected behaviour are in [`evals/evals.json`](evals/evals.json); notes are in [`evals/RESULTS.md`](evals/RESULTS.md).

- **Long message with examples buried inside** (the main problem this skill solves): without the skill, about 1.5 of 5 criteria met; with the skill, **5 of 5, in 2 out of 2 runs**.
- **Short one-line requests with examples**: modern models already expand these fairly well on their own. The visible change from the skill is clearer structure and explicit "your examples / added" labels.
- **Cases where it should not expand** (a closed list, no examples, a format to copy, a test case): 4 of 4 handled correctly with the skill on. It did not make any of them worse.

Honest limits: each case was run only once or twice, and the answers were graded by the same AI assistant that helped write the skill. On very simple requests the assistant sometimes skips loading the skill and works from its description alone.

## When it triggers

- Example markers in English or Chinese: "for example", "for instance", "e.g.", "such as", "things like", "like" (when it introduces examples), "比如", "比如说", "例如", "举个例子", "打个比方", "像……这种", and similar.
- A single named thing that clearly stands for a whole category, even with no marker words (e.g. "make a channel about French cooking for English speakers").

## When it does not trigger

- An explicit closed list: "just these", "only these", "do exactly this", "nothing else".
- A request with no examples at all.
- Examples that show a format or style to copy, not content to expand.
- Test cases or exact specs (e.g. "input 3 should return 9"). These are treated as hard requirements.

## What it guards against

- **Answering the examples instead of the request:** the examples take over the answer, and what you actually asked for never gets answered. This is the most common failure, and the main one this skill fixes.
- **Too little:** handing your examples back as the whole answer, or stopping after doing exactly the items you named.
- **Too much:** burying your request under a flood of additions, drifting outside the category, or replacing your examples with the assistant's own.

## How to know it's working

- The answer opens with one sentence naming the categories it understood, e.g. "Got it: you want to learn skills at the level of business and video creation." If that sentence is wrong, correct it right there.
- Your examples are named, but not answered at length.
- Your items and the assistant's are labeled separately, e.g. "Your examples: …" and "Added: …".
- When you say "just these" or "only these", nothing extra is added.

## Tradeoffs

- Answers get a little longer, because the assistant adds a few items you didn't ask for.
- If you actually wanted a deep dive into one of your examples, you'll need to ask for it as a follow-up. The assistant usually offers this at the end.
- The assistant decides what the category is. If it guesses wrong, the added items will feel off. Tell it the category, or say "just these" to turn expansion off for that request.

## Installation

The skill is a single folder containing `SKILL.md`:

```
examples-are-not-requirements/
└── SKILL.md
```

Copy the `examples-are-not-requirements` folder into the skills directory of any agent that supports `SKILL.md` skills. For Claude Code, that is `~/.claude/skills/` (all projects) or `.claude/skills/` inside a project.

## License

[MIT](LICENSE)
