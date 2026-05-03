# General Coding Lessons

## GPT DocSync
- GPT DocSync version 3 groups code repositories and lessons by project area while preserving stable target IDs. Code targets remain `<repo-key>.handover` and `<repo-key>.plans`; lesson targets use `lessons.<area-key>`. This avoids rewriting existing bundle target IDs when reorganizing config areas.

## Maintainability
- Use comments sparingly, but do use them for complex, unintuitive, or confusing code whose behavior only makes sense when the reasoning is known. Good comment targets include compatibility migrations, API workarounds, performance tradeoffs, input-routing hacks, reflection boundaries, and code that intentionally avoids the obvious-looking implementation. Avoid comments that merely restate straightforward code.
