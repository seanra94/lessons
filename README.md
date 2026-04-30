# Lessons Repository

## Purpose

This repository stores reusable cross-project lessons for ChatGPT/Codex-assisted software and game-mod work.

The primary reader is a future ChatGPT agent. The goal is to preserve hard-won information that would otherwise be expensive to rediscover across adjacent projects.

This repo is not a general work log. It is not for routine chat summaries, stale plans, or obvious facts.

## How this repo is used with GPT DocSync

GPT DocSync maps lesson targets to area-specific `LESSONS.md` files. A ChatGPT agent should update this repo by giving the user a raw `@@DOCSYNC v1` bundle to copy into GPT DocSync.

Normal user workflow:

1. ChatGPT provides a raw `@@DOCSYNC v1` bundle.
2. User copies only the raw bundle text.
3. User runs GPT DocSync.
4. User chooses the normal live path, inspects the preview, and confirms only if correct.

Current lesson targets:

- `lessons.starsector` -> `Starsector/LESSONS.md`
- `lessons.general-coding` -> `General Coding/LESSONS.md`
- `lessons.rimworld` -> `Rimworld/LESSONS.md`
- `lessons.palworld` -> `Palworld/LESSONS.md`

## What belongs in LESSONS.md

Add a lesson only when it is:

- reusable across adjacent projects;
- evidence-based or directly learned from debugging/research;
- non-obvious enough that future agents would likely make the same mistake;
- concise enough to be scanned quickly;
- written as durable guidance, not as a chronological note.

Good lesson categories include:

- engine or framework behavior;
- modding API pitfalls;
- data-file and generated-file traps;
- deployment and validation traps;
- runtime/UI behavior that differs from build-time behavior;
- compatibility problems between tools, versions, mods, or platforms;
- formulas or domain rules that were verified from source files, runtime behavior, or reliable references.

## What does not belong

Do not add:

- routine chat summaries;
- one-off project trivia;
- speculation without evidence;
- stale plans;
- obvious facts;
- broad advice that does not encode a specific reusable lesson;
- unresolved hypotheses unless they are clearly labelled and likely to prevent repeated failed investigation.

## Preferred update style

Prefer `append` for new lessons.

Example:

@@DOCSYNC v1
commit: docs: add Starsector validation lesson

@@FILE lessons.starsector append
heading: ## Validation and deployment
@@CONTENT
- A passing build is not sufficient evidence that a Starsector mod fix works. Validate generated files, deployed artifacts, startup/load behavior, and the specific in-game UI or gameplay flow affected by the change.
@@END

@@ENDDOCSYNC

When correcting an existing lesson, use the narrowest available update method. Do not duplicate an existing lesson with slightly different wording.

## Area guidance

### Starsector

Use `lessons.starsector` for reusable Starsector engine/modding behavior, weapon/stat formulas, CSV/data-file pitfalls, generated-file traps, deployment issues, validation traps, UI/runtime behavior, save/mod-interaction issues, and other lessons likely to help adjacent Starsector projects.

### General Coding

Use `lessons.general-coding` for reusable lessons about Git, build systems, Python, Windows scripting, local automation, testing, packaging, config design, and general software project maintenance.

### Rimworld

Use `lessons.rimworld` for reusable RimWorld modding behavior, XML/Def pitfalls, Harmony patching issues, load order behavior, save compatibility, UI/runtime validation, and deployment traps.

### Palworld

Use `lessons.palworld` for reusable Palworld modding, Unreal tooling, asset/data layout, pak/mod deployment, validation, and runtime behavior lessons.

## Agent responsibilities

When working with the user:

- consider whether a new lesson is warranted after a difficult bug, validation failure, or research result;
- choose the correct `lessons.<area>` target;
- keep entries short and durable;
- include enough context that a future agent can apply the lesson without rereading the original chat;
- avoid writing lessons merely because a conversation occurred.

If unsure whether a finding belongs here, prefer not to add it unless it would probably save future debugging time.
