# Lessons Repository Plans

## Active goal

Use this repo as durable cross-project memory for hard-won lessons learned during ChatGPT/Codex-assisted development.

## Current status

Initial lesson areas are established:

- Starsector
- General Coding
- Rimworld
- Palworld

The next priority is not broad expansion. The next priority is disciplined use: add lessons only when they are likely to save future debugging or research effort.

## Near-term next steps

1. Use GPT DocSync bundles for lesson updates during real project work.
2. Prefer small append-only lesson entries.
3. Add Starsector lessons first when ongoing Starsector work reveals reusable engine/modding behavior, data pitfalls, formulas, deployment issues, or validation traps.
4. Add General Coding lessons when GPT DocSync, local Git automation, Windows scripting, or repo maintenance produces reusable lessons.
5. Add Rimworld and Palworld lessons only when active work in those areas produces hard-won reusable findings.
6. Periodically check for duplicate or overlapping lessons and consolidate if needed.

## Acceptance criteria for new lessons

A new lesson should satisfy all of these:

- It is reusable across adjacent projects.
- It is not obvious.
- It was learned through debugging, validation, source inspection, runtime behavior, or reliable research.
- It is short enough to scan.
- It contains enough context to be useful without the original chat.
- It is placed in the correct area file.

## Future improvement candidates

- Add an idempotent block-update mode in GPT DocSync to reduce duplicate lesson entries.
- Add a periodic review pass for duplicate lessons.
- Add new areas only when there is a real recurring project category.
- Add root-level DocSync targets for this repo if README/HANDOVER/PLANS updates become frequent.

## Out of scope

- Routine project logs.
- Chat transcripts.
- Temporary plans.
- Repo-specific details that belong in that repo?s `HANDOVER.md` or `PLANS.md`.
- Unverified speculation.
