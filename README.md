# examples-are-not-requirements

An agent skill that stops AI assistants from treating your examples as a complete list.

Have you ever given an agent an example, only for it to take that example as the actual requirement and run with it? This skill fixes that.

When you say "I want to learn things like photography and cooking", you are describing a *kind* of thing, not handing over a closed checklist. Without this skill, assistants often do exactly the two things you named and stop. With it, the assistant works out the category behind your examples, keeps your examples, and adds a few relevant items you didn't mention, clearly labeled so you can tell which are yours and which it added.

## Before and after

**You:** "My action list should have things I can do now, like writing a plan or making a budget."

**Without the skill:**

- Write a plan
- Make a budget

**With the skill:**

- Your examples: write a plan, make a budget
- Added: set a deadline, collect one reference example

## When it triggers

- Example markers in English or Chinese: "for example", "for instance", "e.g.", "such as", "things like", "like" (when it introduces examples), "比如", "比如说", "例如", "举个例子", "打个比方", "像……这种", and similar.
- A single named thing that clearly stands for a whole category, even with no marker words (e.g. "make a channel about French cooking for English speakers").

## When it does not trigger

- An explicit closed list: "just these", "only these", "do exactly this", "nothing else".
- A request with no examples at all.
- Examples that show a format or style to copy, not content to expand.
- Test cases or exact specs (e.g. "input 3 should return 9"). These are treated as hard requirements.

## What it guards against

The skill pushes against both failure modes:

- **Too little:** handing your examples back as the whole answer, or stopping after doing exactly the items you named.
- **Too much:** burying your request under a flood of additions, drifting outside the category, or replacing your examples with the assistant's own.

When the assistant can't tell whether you gave examples or a requirement, it either asks you one question or states its assumption before going ahead.

## Installation

The skill is a single folder containing `SKILL.md`:

```
examples-are-not-requirements/
└── SKILL.md
```

Copy the `examples-are-not-requirements` folder into the skills directory of any agent that supports `SKILL.md` skills. For Claude Code, that is `~/.claude/skills/` (all projects) or `.claude/skills/` inside a project.

## License

[MIT](LICENSE)
