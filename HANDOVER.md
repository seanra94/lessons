# Lessons Repository Handover

## Purpose

This repo stores durable, reusable lessons for ChatGPT/Codex-assisted software and game-mod work.

The primary consumer is a future ChatGPT agent deciding whether to add, consult, or update area-specific `LESSONS.md` files.

## Repository facts

- GitHub repo: `seanra94/lessons`
- Local path: `D:\Sean Mods\Lessons`
- Branch: `main`
- Write path: local Git / GPT DocSync, not the ChatGPT GitHub connector.

## Area files

Current lesson areas:

- `Starsector/LESSONS.md`
- `General Coding/LESSONS.md`
- `Rimworld/LESSONS.md`
- `Palworld/LESSONS.md`

Current GPT DocSync targets:

- `lessons.starsector`
- `lessons.general-coding`
- `lessons.rimworld`
- `lessons.palworld`

## Durable rules

- This repo is not a work log.
- Add only reusable cross-project lessons.
- Lessons should be concise, evidence-based, and hard-won.
- Avoid routine summaries, stale plans, obvious facts, and speculation.
- Prefer append for new lessons.
- Avoid duplicate entries; consolidate or correct when necessary.

## GPT DocSync usage

Future ChatGPT agents should provide raw `@@DOCSYNC v1` bundles for updates.

Normal lesson update flow:

1. Agent emits raw `@@DOCSYNC v1` bundle.
2. User copies it into GPT DocSync.
3. User runs the normal preview/confirm flow.
4. Only the mapped `LESSONS.md` file should change.

Example target usage:

- Starsector reusable lesson -> `lessons.starsector`
- General coding/tooling lesson -> `lessons.general-coding`
- RimWorld modding lesson -> `lessons.rimworld`
- Palworld modding lesson -> `lessons.palworld`

## Validation expectations

A lesson update is acceptable when:

- the target area is correct;
- the entry is reusable beyond the current repo;
- the entry is concise and durable;
- the entry does not merely summarize the chat;
- GPT DocSync preview shows only intended lesson file changes.

## Known operational pitfalls

- Repeated append bundles can duplicate text. Check whether the lesson already exists before appending near-duplicate material.
- Area keys are stable identifiers. Do not rename directories or targets casually because project custom instructions and DocSync bundles may refer to them.
- The lessons repo root docs may be updated directly by Codex/local Git unless dedicated DocSync root targets are later added.
