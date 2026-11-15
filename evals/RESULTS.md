# Test results

## v1.1.0: clean tests

The v1.0.0 tests below were run with the author's saved personal preferences still on, and those preferences included a rule about examples. So for v1.1.0 everything that matters was retested in a clean setup. The raw answers, keys and run instructions are in [`clean-runs/`](clean-runs/).

How:

- A new chat with memory off for that chat, the author's personal preferences and own skills switched off. Before each round, the runner and a fresh sub-agent listed everything they could see about the user: nothing except the account email the system adds automatically.
- Every answer comes from a new sub-agent. Skill groups got the full `SKILL.md` pasted in as an already-loaded skill; the no-skill group got the bare message. The runner only saw neutral version names.
- The main test message: "我想学习各种能够提高生活质量的实用技能，比如摄影和烹饪。请帮我制定一个全面的学习计划。" ("I want to learn practical skills that improve my quality of life, such as photography and cooking. Please make me a full learning plan.")
- Results are counts of things anyone can check in the raw answers.

Limits: five runs per group at most; the counting was done by the AI assistant that helped write the skill, and it saw the keys, so it was not blind; round 2's model was not checked.

### Round 1: no skill vs v1.0.0 vs draft A (patches 1–2), 5 runs each

| Check | No skill | v1.0.0 | Draft A |
|---|---|---|---|
| Opens by naming what the examples have in common | 0 of 5 | 5 of 5 | 5 of 5 |
| Labels the user's examples and the added items | 0 of 5 | 5 of 5 | 5 of 5 |
| Calls the examples "core", "main line" or "the focus" | 5 of 5 | 3 of 5 | 1 of 5 |
| Gives each example its own full chapter | 2 of 5 | 0 of 5 | 0 of 5 |

Finding: in a clean setup, the model without the skill still builds the whole plan around the two examples. So the problem is the model's default, not the author's preferences, and the skill does fix the biggest part of it.

Left over: in both skill versions, the examples still got the concrete steps ("learn 5 home dishes", "rule of thirds"), while most added skills got one line.

### Round 2: draft B (patches 1–4) vs draft A, 5 runs each

Draft B adds one step: order and depth follow the user's goal, not who named an item, and every item in a plan gets at least one concrete first step.

| Check | Draft B | Draft A |
|---|---|---|
| Every item, added ones included, has a concrete first step | 5 of 5 | most added items had none |
| Calls the examples "core" or "the main line" | 0 of 5 | 2 of 5 |
| Pushes photography, which the user named, to the last stage | 2 of 5 | 1 of 5 |
| Tells the user "this is not because it was your example" | several times | 0 |

Two new problems: draft B over-corrected by pushing an example the user wanted to the back, and it said its own rule out loud.

### Round 3: v1.1.0 (patches 1–5), 19 runs over 7 messages

Patch 5: the user naming something shows they want it, and that interest is part of the goal, so an example is not pushed back just to avoid favoring it; reasons for the order are given in terms of the goal, never "whether it was your example".

- **Main message, 5 runs:** names the categories and labels items 5 of 5; calls the examples "core" 0 of 5; every item has a first step 5 of 5; photography is in the first stage 3 times and the second stage 2 times, never last.
- **New examples, 3 runs each** (to check the skill was not tuned to one message): healthy habits ("like going to bed early and drinking more water") and workplace skills ("like making slides and writing emails"). Same behavior in 6 of 6; order explained by the goal, e.g. structured thinking first because slides and emails both rest on it.
- **Cases that must not change, 2 runs each:** a closed list ("only these three"), a format to copy, real plans ("next month I'll start photography and cooking, 5 hours a week, how do I start?"), and a podcast message with examples. 8 of 8 passed.
- **Still seen once in 19 runs:** one answer said "not ordered by who proposed it". Left as is, to avoid adding rules for a single case.

## v1.0.0 tests

These ran with personal preferences on (see the correction below), so treat them as weaker evidence than the clean tests above.

How the tests were run:

- Private (incognito) chat with no memory, so saved memories could not affect the answers.
- Same model in every run. Each question in a new chat.
- Round A: skill turned off. Round B: skill turned on. No extra instruction such as "use the skill" was added.
- Each answer was checked against the `expected_behavior` list in [`evals.json`](evals.json). The criteria were written before the answers were seen.

Limits: each case was run only a few times. The answers were graded by the same AI assistant that helped write the skill, so this is a useful signal, not proof.

### Main result: long message with examples buried inside

This is the situation the skill was built for: a long request with a couple of examples dropped in the middle, where the assistant ends up answering the examples instead of the request.

#### Message 1: a real message from the author (content kept private)

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

#### Message 2: case `long-message-examples-buried` (freelance work)

Correction: an earlier version of this file said saved personal preferences were cleared for these runs. They were not. They stayed on for every run, skill off and skill on alike, and they included a rule about examples. So "skill off" here means "personal preferences only", which may already push the answer toward the skill's behavior. An earlier run that showed memory leaking into a private chat was thrown away.

| Criterion | Skill off | Skill on (4 runs) |
|---|---|---|
| Opens with one sentence naming the categories | Partly | Yes, 4 of 4 |
| Does not answer the examples in depth | No | Yes, 4 of 4 |
| Finds the category behind each example | Yes | Yes, 4 of 4 |
| Adds unmentioned skills and links, marked | Partly | Yes, 4 of 4 |
| Weight of the answer on the categories | No | Yes, 4 of 4 |

- **Skill off:** a short map, then two long sections of tips on pricing and portfolio layout. About 60% of the answer explained the two examples.
- **Skill on:** every run treated the examples as two layers of a bigger picture and added layers such as positioning, getting clients, contracts and money. The four runs span two versions of the skill; the later version fixed an opening sentence that read oddly (see below).

### Short one-line requests

These were run with an early version of the skill, before the "restate the categories first" step existed.

| Case | Skill off | Skill on |
|---|---|---|
| `english-marker-expand` | Expanded, no labels | Expanded, labeled "Your examples / Added", user's examples answered first |
| `chinese-marker-expand` | Expanded well | Same; the assistant skipped loading the skill |
| `single-representative-no-marker` | Planned the channel, no category expansion | Nearly the same |
| `ambiguous-state-assumption` | Full list, no labels | Labeled, category named in a heading; one drift outside "snacks" (drinks, napkins) |

Retest of `ambiguous-state-assumption` on the current version: crisps kept and labeled, five labeled groups of other snacks, no drinks, and napkins and plates only in a separate "Easy to forget" section at the end. The earlier drift is now handled by the real-plans rule.

Finding: modern models already expand one-line requests with "for example" fairly well. The skill's visible effect there is structure and labels.

### Cases where it should not expand

| Case | Skill off | Skill on |
|---|---|---|
| `closed-list-no-expand` | Pass | Pass |
| `no-examples-no-expand` | Pass | Pass (not triggered) |
| `format-example-match-not-expand` | Pass | Pass |
| `test-case-is-requirement` | Pass | Pass |

The skill did not make any of these worse.

### When the examples are real plans

Two messages that look alike but mean different things.

- `broad-learning-unrelated-examples`: "I want to learn lots of new things, at every level, like how to open and run a coffee shop, or how Japanese and Western street photography differ." The opening says the user wants broad learning, so treating the examples as samples is right. With the skill on, the answer named the two kinds of knowledge and added other directions of each kind. The author judged this correct.
- `real-plans-answer-first`: "I want to do a few new things, like opening a coffee shop or learning street photography. How do I start?" Here the user means to do these things.

| Criterion | Skill on, before the fix | Skill on, after the fix |
|---|---|---|
| Answers both plans concretely first | Yes | Yes |
| Extras in one separate, labeled section at the end | No | Yes |
| No other activities mixed into the main answer | No (a table column listed painting, instruments, an online shop) | Yes |

The fix added a "When the examples are real plans" section and Case 6 to the skill. Case 6 uses different examples (baking bread, starting to run) so the test question was not answered in advance.

### On another model: OpenAI Codex

The skill was installed in Codex straight from this GitHub repository with Codex's built-in skill installer. No errors or warnings, including about the description length.

| Case | Skill off | Skill on |
|---|---|---|
| `long-message-examples-buried` (freelance) | About 3.5 of 5: a good six-layer map with no labels, then a separate section answering both examples | 5 of 5: a ten-layer map along the whole freelance process, ★ marking the user's two examples, plus a list of easily missed skills |
| `real-plans-answer-first` | Not run | Pass: a two-week plan for each, extras in a separate labeled section at the end |
| `closed-list-no-expand` | Not run | Pass: asked only about the three items and said it would buy nothing else |

Codex without the skill already does better than Claude without it, so the gap is smaller. One note: in the real-plans answer Codex referred to the previous question, so it may have carried context over from an earlier session.

### What the tests changed

- Added the "Answering the examples instead of the request" anti-pattern and Case 5, after seeing that one-line test cases missed the real failure.
- Added the "restate the categories in one sentence first" step. In the earliest round, stating an assumption never happened.
- Rewrote that step after the author found an opening sentence odd: it paraphrased the request and added a contrast the author never made ("you want X, not just Y"). The step now asks for one plain sentence naming what the examples have in common.
- Replaced "add a focused few" with "add what the user would not think of, without filler", after the author said the longer set of additions was genuinely useful.
- Moved the key rules into the description, after seeing the assistant sometimes act on the description without loading the full skill.
- Added the "real plans" rule after the author pointed out that when the examples are things you actually plan to do, you want those answered first, with extras kept in a separate section.
- Fixed criteria in `evals.json` that were too loose or did not fit their case, including one test whose expected answer turned out to be wrong.
