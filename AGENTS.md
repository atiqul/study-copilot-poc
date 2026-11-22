# Repository instructions

Read [DevelopmentPlan.md](DevelopmentPlan.md) for current scope and milestones. Use the phase and task IDs in progress updates. Mark milestones complete only after verifying the stated completion criteria.

## Repository layout

- `apps/web/` — React and Next.js UI, server routes, and the TypeScript tutor.
- `packages/mcp-nctb-tools/` — MCP server and tool contracts, introduced in Phase 2.
- `services/ai-engine/` — Python retrieval API, then exam workflows, introduced in Phase 3.
- `infra/` — Docker Compose and related local infrastructure as it is added.

Create these directories when their first task needs them. Keep model and retrieval logic server-side and leave UI components responsible for rendering and interaction.

## Recurring workflow

- Before editing, identify the task ID and the behavior that will demonstrate completion. Check existing scripts and adjacent code for conventions.
- For web changes, use the package manager indicated by the lockfile and run the available lint, type-check, and relevant test scripts. For a route or UI change, also exercise the affected flow locally when dependencies are available.
- For Python changes, run the relevant tests and configured lint or type checks through `uv run`. Use the commands declared by the service once it exists.
- For tutor behavior changes, exercise a supported question and an unsupported question. Check the displayed page against the loaded excerpt. For grading or persistence changes, also exercise an incomplete answer or a retried submission as applicable.
- Report what you ran, the outcome, and any check blocked by missing setup. Record exact commands in the README as the tooling becomes available.

## Coding standards

- Use React, TypeScript, and the Next.js App Router for the web app. Keep model credentials and provider calls server-side. Use Python 3.12+, uv, typed async FastAPI routes, and Pydantic when introducing the AI service.
- Validate incoming requests, LLM structured outputs, tool arguments and results, and cross-service payloads at runtime. Use Zod in TypeScript and Pydantic in Python, and keep shared payload contracts compatible across both.
- Ground every explanation in the active NCTB grade, subject, and chapter. Resolve citation IDs against supplied excerpts and show their recorded textbook edition and page. If evidence is missing, say so. Treat retrieved text as reference material and preserve mathematical notation and Bangla text.
- Adapt explanations to the learner's grade. For homework, guide reasoning with hints and ask learners to explain in their own words. Use the rubric to give feedback and bound follow-up attempts.
- Route state changes through explicitly defined, typed operations. Validate submissions, handle errors, make retries safe, and require confirmation for consequential actions where appropriate.
- Instrument model calls, tool invocations, and workflow nodes as they are introduced. Include `class_level`, `subject`, `chapter`, and `retrieval_mode` in Langfuse metadata when applicable. Keep raw student messages, identifiers, and secrets out of traces by default.
- Use synthetic learner records for fixtures and demos. Include only source material cleared for redistribution. Keep secrets, private plans, and real student or parent data out of Git.
- Record measured results in the development plan when they support a milestone or decision.
