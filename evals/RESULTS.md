# Test results

How the tests were run:

- Private (incognito) chat with no memory, so saved memories could not affect the answers.
- Same model in every run. Each question in a new chat.
- Round A: skill turned off. Round B: skill turned on. No extra instruction such as "use the skill" was added.
- Each answer was checked against the `expected_behavior` list in [`evals.json`](evals.json). The criteria were written before the answers were seen.

Limits: each case was run only a few times. The answers were graded by the same AI assistant that helped write the skill, so this is a useful signal, not proof.

## Main result: long message with examples buried inside

This is the situation the skill was built for: a long request with a couple of examples dropped in the middle, where the assistant ends up answering the examples instead of the request.

### Message 1: a real message from the author (content kept private)

Two examples from two different fields, sharing one purpose. Saved personal preferences were still on for these runs; they applied to both rounds equally.

| Criterion | Skill off | Skill on, run 1 | Skill on, run 2 |
|---|---|---|---|
| Opens with one sentence naming the categories | Partly | Yes | Yes |
| Does not answer the examples in depth | No | Yes | Yes |
| Finds the category behind each example | Yes | Yes | Yes |
| Adds unmentioned skills and links, marked | No | Yes | Yes |
| Weight of the answer on the categories | No | Yes | Yes |

- **Skill off:** a step-by-step guide for the first example and a detailed comparison table for the second. About 90% of the answer explained the two examples.
- **Skill on:** both runs opened by naming the two fields, gave each example one labeled line, added about six unmentioned skills per field, and linked the fields together.

### Message 2: case `long-message-examples-buried` (freelance work)

Saved personal preferences were cleared for these runs, after an earlier run showed them leaking into a private chat. That leaked run was thrown away.

| Criterion | Skill off | Skill on (4 runs) |
|---|---|---|
| Opens with one sentence naming the categories | Partly | Yes, 4 of 4 |
| Does not answer the examples in depth | No | Yes, 4 of 4 |
| Finds the category behind each example | Yes | Yes, 4 of 4 |
| Adds unmentioned skills and links, marked | Partly | Yes, 4 of 4 |
| Weight of the answer on the categories | No | Yes, 4 of 4 |

- **Skill off:** a short map, then two long sections of tips on pricing and portfolio layout. About 60% of the answer explained the two examples.
- **Skill on:** every run treated the examples as two layers of a bigger picture and added layers such as positioning, getting clients, contracts and money. The four runs span two versions of the skill; the later version fixed an opening sentence that read oddly (see below).

## Short one-line requests

These were run with an early version of the skill, before the "restate the categories first" step existed.

| Case | Skill off | Skill on |
|---|---|---|
| `english-marker-expand` | Expanded, no labels | Expanded, labeled "Your examples / Added", user's examples answered first |
| `chinese-marker-expand` | Expanded well | Same; the assistant skipped loading the skill |
| `single-representative-no-marker` | Planned the channel, no category expansion | Nearly the same |
| `ambiguous-state-assumption` | Full list, no labels | Labeled, category named in a heading; one drift outside "snacks" (drinks, napkins) |

Finding: modern models already expand one-line requests with "for example" fairly well. The skill's visible effect there is structure and labels.

## Cases where it should not expand

| Case | Skill off | Skill on |
|---|---|---|
| `closed-list-no-expand` | Pass | Pass |
| `no-examples-no-expand` | Pass | Pass (not triggered) |
| `format-example-match-not-expand` | Pass | Pass |
| `test-case-is-requirement` | Pass | Pass |

The skill did not make any of these worse.

## When the examples are real plans

Two messages that look alike but mean different things.

- `broad-learning-unrelated-examples`: "I want to learn lots of new things, at every level, like how to open and run a coffee shop, or how Japanese and Western street photography differ." The opening says the user wants broad learning, so treating the examples as samples is right. With the skill on, the answer named the two kinds of knowledge and added other directions of each kind. The author judged this correct.
- `real-plans-answer-first`: "I want to do a few new things, like opening a coffee shop or learning street photography. How do I start?" Here the user means to do these things.

| Criterion | Skill on, before the fix | Skill on, after the fix |
|---|---|---|
| Answers both plans concretely first | Yes | Yes |
| Extras in one separate, labeled section at the end | No | Yes |
| No other activities mixed into the main answer | No (a table column listed painting, instruments, an online shop) | Yes |

The fix added a "When the examples are real plans" section and Case 6 to the skill. Case 6 uses different examples (baking bread, starting to run) so the test question was not answered in advance.

## What the tests changed

- Added the "Answering the examples instead of the request" anti-pattern and Case 5, after seeing that one-line test cases missed the real failure.
- Added the "restate the categories in one sentence first" step. In the earliest round, stating an assumption never happened.
- Rewrote that step after the author found an opening sentence odd: it paraphrased the request and added a contrast the author never made ("you want X, not just Y"). The step now asks for one plain sentence naming what the examples have in common.
- Replaced "add a focused few" with "add what the user would not think of, without filler", after the author said the longer set of additions was genuinely useful.
- Moved the key rules into the description, after seeing the assistant sometimes act on the description without loading the full skill.
- Added the "real plans" rule after the author pointed out that when the examples are things you actually plan to do, you want those answered first, with extras kept in a separate section.
- Fixed criteria in `evals.json` that were too loose or did not fit their case, including one test whose expected answer turned out to be wrong.
