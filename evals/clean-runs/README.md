# Clean test runs for v1.1.0

Raw, unedited answers from the test rounds described in [`../RESULTS.md`](../RESULTS.md). The answers, keys and run instructions are in Chinese, because the test messages were in Chinese.

## How the runs were made

- A new claude.ai chat with memory turned off for that chat, and all of the author's own skills disabled, so the skill under test could not load by itself.
- Before each round, a check (step 0 in `instructions.md`): the runner and a fresh sub-agent listed everything they could see about the user. Neither saw any name, preferences, memory or past chats; the runner saw only the account email that the system adds automatically. The key lines of each check, with the email removed, are in [`step-0-checks.md`](step-0-checks.md). Round 1's instructions had no exception for this email; the runner stopped and asked, and the author told it to continue.
- Every answer comes from a new sub-agent with no shared context. Skill groups got the full `SKILL.md` text pasted in as an already-loaded skill; the no-skill group got the bare message. Every sub-agent was told not to use tools or other skills.
- File names are random three-digit numbers. `key.md` in each folder says which group and run each file belongs to.

## What the group names in each key mean

The runner only saw neutral names, so it did not know which version was newer. Full texts are in `skill-versions/`.

- Round 1
  - 无 skill 组 = no skill
  - 版本甲 = v1.0.0 (`skill-versions/v1.0.0.md`)
  - 版本乙 = draft A, patches 1–2 (`skill-versions/draft-A-patches-1-2.md`)
- Round 2
  - 版本丙 = draft B, patches 1–4 (`skill-versions/draft-B-patches-1-4.md`)
  - 版本丁 = draft A again, as a control
- Round 3
  - one version only: draft C, patches 1–5 (`skill-versions/draft-C-patches-1-5.md`)
  - 题 1–7 = the seven test messages listed in `round-3/instructions.md`
- Round 4
  - one version only: v1.1.0 (`skill-versions/v1.1.0.md`, same as `round-4/skill-snapshot.md` and the released `SKILL.md`), 7 runs over the five messages in `round-4/instructions.md`. Scoring rules: `round-4/rubric.md`, fixed before any answer was generated. Item-by-item scores: `round-4/scores.md`.
  - What changed from draft C: see `skill-versions/diff-draft-C-to-v1.1.0.diff` (the description, Case 1, Case 4, and the wording on how much weight examples get).
- Round 5
  - 甲 = v1.1.0, 乙 = v1.1.1 round-5 candidate (cases moved to `cases.md`, rules unchanged); 7 runs each, `cases.md` not given. Files: `round-5/` (`instructions.md`, frozen `rubric.md`, `file-integrity-check.md`, `answers/`, `key-runner.md`, `scores.md`). Graded blind by Claude; answers were copied into files by the runner, so they cannot be checked against the original output. Not released.
- Round 6
  - 甲 = v1.1.0, 乙 = v1.1.1 round-6 candidate, the released v1.1.1 (two rules from Case 1 added to the main text); 7 runs each, `cases.md` not given; each sub-agent wrote its own answer to a file. Files: `round-6/` (`instructions.md`, frozen `rubric.md`, `answers/`, `key-runner.md`, `key-version.md`, `scores-codex-blind.md`, `scores-claude.md`, `comparison.md`). Graded blind by OpenAI Codex; Claude's scores are not blind.

## Limits

- For rounds 1–4, the person who counted the results (the AI assistant that helped write the skill) saw the keys, so the counting was not blind. Round 5 was graded blind by the same assistant; round 6 was graded blind by OpenAI Codex. Everything here is published so anyone can recount it.
- Model: Claude Opus 5.5 was selected for rounds 1, 3, 4, 5 and 6 (round 4's runner did not report the model back); round 2 was not checked and may have run on Opus 4.8.
- Pasting the skill in tests how well a model follows it once loaded, not whether it triggers by itself.
- Seven runs per group at most.
