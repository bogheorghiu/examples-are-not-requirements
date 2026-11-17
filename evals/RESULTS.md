# Test results

## v1.1.0: environment-checked tests

The v1.0.0 tests further down were run with the author's saved personal preferences still on, and those preferences included a rule about examples. So for v1.1.0 the main behavior was retested in a setup checked for these influences. The raw answers, keys, run instructions and step-0 environment checks are in [`clean-runs/`](clean-runs/).

How:

- A new chat with memory off for that chat, and the author's personal preferences and own skills switched off. Before each round, the runner and a fresh sub-agent listed everything they could see about the user. Neither saw any name, preferences, memory or past chats; the runner saw only the account email that the system adds automatically (see `clean-runs/README.md` for how this was handled in round 1).
- Every answer comes from a new sub-agent. Skill groups got the full `SKILL.md` pasted in as an already-loaded skill; the no-skill group got the bare message. The runner only saw neutral version names.
- The main test message: "我想学习各种能够提高生活质量的实用技能，比如摄影和烹饪。请帮我制定一个全面的学习计划。" ("I want to learn practical skills that improve my quality of life, such as photography and cooking. Please make me a full learning plan.")
- Results are counts of things anyone can check in the raw answers. Each check below says what was counted.

Limits:

- Five runs per group at most, mostly on one message.
- The counting was done by the AI assistant that helped write the skill, and it saw the keys, so it was not blind. OpenAI Codex rechecked the counts for rounds 1–3 and corrected several; for round 4 it read the 7 raw answers and corrected one score (935). This was a review, not an independent rerun. The numbers below are the corrected ones.
- Pasting the skill in tests how well a model follows it once loaded. It does not test whether the skill triggers by itself, whether the agent actually opens the file, or how it behaves from the description alone.
- Round 2's model was not checked, and round 4's runner did not report it back.
- The environment checks rely on the runner and a sub-agent reporting what they can see. They found no user information, but cannot rule out every hidden influence, so these are "environment-checked" tests, not proof of a perfectly clean setup.

What "builds the plan around the examples" means below: the answer calls the two examples "core" or "the main line", says the plan is "mainly" or "based on" them, or makes them the only main focus of the first stage. The exact words are listed for each count. This is an observation, not a failure in itself: whether it is reasonable depends on the user's goal, preferences and constraints, not on where the examples sit in the plan.

### Round 1: no skill vs v1.0.0 vs draft A (patches 1–2), 5 runs each

| Check | No skill | v1.0.0 | Draft A |
|---|---|---|---|
| Opens by naming what the examples have in common | 0 of 5 | 5 of 5 | 5 of 5 |
| Labels the user's examples and the added items | 0 of 5 | 5 of 5 | 5 of 5 |
| Builds the plan around the examples | 5 of 5 | 3 of 5 | 1 of 5 |
| Gives each example its own full chapter | 2 of 5 | 0 of 5 | 0 of 5 |

- No skill: "摄影和烹饪是重点" (883), "核心" layer (461), first stage "主攻：烹饪 + 摄影" (786), "主线组合" (624), "核心" and "主线" (053). Own chapters: 883, 053.
- v1.0.0: "以你的两个例子为主" (261), "以你的两个例子为主线" (809), "放在核心位置" (519).
- Draft A: "摄影和烹饪会作为核心保留" (756).

Finding: in these five runs on this message, the model without the skill built the whole plan around the two examples, even with no preferences in play. Both skill versions reduced this.

Left over: in both skill versions, the examples still got the most detailed steps ("learn 5 home dishes", "rule of thirds"), while added skills usually got shorter ones.

### Round 2: draft B (patches 1–4) vs draft A, 5 runs each

Draft B adds one step: order and depth follow the user's goal, not who named an item, and every item in a plan gets at least one concrete first step.

| Check | Draft B | Draft A |
|---|---|---|
| Format: explicitly labels a "first step" (第一步) for each item in the staged plan | 5 of 5 | 0 of 5 |
| Behavior: every item in the staged plan has a concrete starting action | not scored separately | not scored separately |
| Builds the plan around the examples | 0 of 5 | 2 of 5 ("作为核心保留" 249, "以你的两个例子为主" 196) |
| Photography, which the user named, starts only in the final stage | 1 of 5 (245) | 0 of 5 |
| Photography starts in the second half of the plan | 2 of 5 (245, 976) | 1 of 5 (858) |
| Tells the user the order is "not because it was your example" or similar | 4 of 5 (706, 492, 245, 976) | 0 of 5 |

The "first step" row only shows a change in format. Draft A answers also gave concrete actions for many items (for example 196.md), so it is not evidence that the plans became more actionable.

Two things to fix: draft B started photography later more often (the answers gave reasons such as "it is more about enjoyment than daily needs", so this shows a later start, not proof of deliberate over-correction), and it stated its own rule to the user.

### Round 3: draft C (patches 1–5), 19 runs over 7 messages

Patch 5: naming something is a sign of interest, which is part of the goal, so an example is not pushed back just to avoid favoring it; reasons for the order are given in terms of the goal, never "whether it was your example".

- **Main message, 5 runs:** names the categories 5 of 5; labels items 5 of 5; builds the plan around the examples 0 of 5; explicitly labels a first step for every item in the staged plan 5 of 5 (format; actionability not scored separately); photography starts in the first stage 3 times and the second stage 2 times, never in the last.
- **New examples, 3 runs each** (to check the skill was not tuned to one message): healthy habits ("like going to bed early and drinking more water") and workplace skills ("like making slides and writing emails"). Names the categories 6 of 6; labels items 6 of 6; explains the order by the goal 6 of 6, e.g. structured thinking first because slides and emails both rest on it.
- **Cases that must not change, 2 runs each:** a closed list ("only these three"): nothing added, 2 of 2. A format to copy: three lines in the given format, 2 of 2 (one answer also added a short opening line and an offer to adjust, 315). Real plans ("next month I'll start photography and cooking, 5 hours a week, how do I start?"): both answered concretely first, extras in a separate last section, 2 of 2. A podcast message with examples: categories named and items labeled, 2 of 2.
- **Stated its own rule once in 19 runs:** "不按是谁提出的来排" (290). Left as is, to avoid adding rules for a single case.

### Round 4: v1.1.0, targeted regression check, 7 runs

After an outside review, v1.1.0 changed the description, Case 1, Case 4 and the wording on how much weight examples get. Round 4 checks those changes. The scoring rules were fixed before any answer was generated ([`clean-runs/round-4/rubric.md`](clean-runs/round-4/rubric.md)); item-by-item scores with quotes are in [`clean-runs/round-4/scores.md`](clean-runs/round-4/scores.md).

- **Main message, 3 runs:** names the categories, labels items, adds unmentioned skills and explains the order by the goal, 3 of 3. Every item in the staged plan has a concrete starting action in 2 of 3: in 935, one "first step" ("save three months of living costs as an emergency fund") is a goal, not an action. This was first scored as a pass and corrected after review (see `scores.md`). No answer stated its own rule. Both examples started in the first stage each time, with goal-based reasons ("used every day", "interest keeps you going").
- **A named channel topic, 1 run each:** the original French cooking case and a new one ("a channel teaching smartphone photography to retired people"). Both stayed on the named topic and audience; other directions appeared only as one line marked optional.
- **A named reminder, 1 run each:** the original Case 1 and a new one ("remind me to use 压缩宝 when I compress videos", with two unrelated tools mentioned). Both reminded only about the named tool for the named task, and both said honestly that they cannot carry a reminder into a new chat.
- **Noticed while scoring:** one answer (910) also promised, unasked, to remind the user about the other two tools for other tasks. The frozen rules did not cover this, so it is recorded as an observation, not a failure.

Conclusion: across these 7 targeted regression checks, one item did not meet the frozen rules (935, a goal written as a first step); no other target problem was found. This does not show the version is stable, and does not test whether it triggers by itself after a normal install.

## v1.1.1: shorter SKILL.md, rounds 5–6

v1.1.1 moves the six worked cases from `SKILL.md` into `cases.md` in the same folder, and adds two short rules distilled from Case 1 to the main text. A program checked that the cases moved word for word ([`clean-runs/round-5/file-integrity-check.md`](clean-runs/round-5/file-integrity-check.md)). Both rounds pasted `SKILL.md` without `cases.md`, ran v1.1.0 at the same time as a baseline, interleaved the runs, and used the same 5 messages and 7 runs per version as round 4.

### Round 5: cases moved out, rules unchanged

Graded blind by Claude ([`clean-runs/round-5/`](clean-runs/round-5/)). No failures in either group under the frozen rules. Two differences between the groups, recorded as observations: on both reminder messages the candidate also listed the user's other tools as preferences and suggested saving them, once guessing what a tool does from its name; on the main message it put both examples in the first stage 3 of 3 times (v1.1.0: 1 of 3). A check then found that Case 1 carried constraints the main text did not state (mention other tools only when they help the same task; seeing a wider category is not a reason to widen the task; say what you cannot do instead of promising it). This candidate was not released. Later correction: Claude did not check each "advanced" item's first step in this round, which is inconsistent with round 4; under the stricter reading, both groups may have 1d failures.

### Round 6: the released v1.1.1

The two distilled rules were added under "When the user has chosen a target". Sub-agents wrote their own answers to files, so no answer was copied by hand. Graded blind by OpenAI Codex; Claude also graded but knew which version was which ([`clean-runs/round-6/comparison.md`](clean-runs/round-6/comparison.md)).

- v1.1.0: 1 failure (581, a goal written as a first step), 1 unclear (442, said it would remember in later chats once memory is on).
- v1.1.1: 1 failure (899, an optional item whose first step was only "pick one of these", and an advanced cooking item with no concrete action).
- No failure type new to v1.1.1 and no rise in failure count, so it met the release rule fixed in advance. On the reminder messages, neither group added other tools, guessed tool functions or claimed to have saved anything.
- Both versions sometimes give later "advanced" or optional items a goal or a direction instead of a concrete first step. This is shared, not caused by the split.

Conclusion: in these targeted regression checks, no problem was found that v1.1.1 has and v1.1.0 does not. This does not show the two versions behave the same, and automatic triggering and reading `cases.md` after a normal install are still untested.

## v1.0.0 tests (historical)

These ran with personal preferences on (see the correction below), and their raw answers were not kept, so they cannot be checked independently. Treat them as a historical record, weaker than the environment-checked tests above.

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
