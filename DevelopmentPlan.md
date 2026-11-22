# NCTB AI Study Copilot development plan

## Product vision

An AI study partner for students in grades 6–12 in Bangladesh. It helps students understand concepts at their grade level and prepare for school exams, admissions, and competitions. It can also help students brainstorm science fair ideas grounded in concepts from the Bangladesh National Curriculum and Textbook Board (NCTB) curriculum.

The tutor uses page-level citations, breaks complex ideas into smaller steps, and guides homework questions with hints and worked analogies. After an explanation, it asks the student to respond in their own words, compares the response with a rubric, and offers a follow-up question or hint where understanding is still incomplete. Students can also practice with mock exams and see which concepts to revisit.

## Public proof of concept

This repository is a learning and portfolio project for AI Forward Deployed Engineering (FDE). Start with a small selection from Class 9–10 Physics, targeting the Motion chapter. Verify the textbook edition and page numbering before adding fixtures. Use synthetic learner records and share only curriculum material cleared for redistribution.

Later production work can expand curriculum coverage, exam formats, competition and science fair support, and persistent progress tracking. A separate production application can support authorized parent access, on-demand reports, and scheduled delivery. The PoC demonstrates reports with synthetic data in the app.

## Phase 1 — Grounded tutor in React and TypeScript

- [ ] **1.1 — App and chapter fixture.** Create a Next.js App Router app in `apps/web` and prepare a small Markdown or JSON fixture with edition, grade, subject, chapter, stable excerpt IDs, and page references.
- [ ] **1.2 — Streaming explanation.** Use the Vercel AI SDK to stream an explanation from the chapter fixture. Validate citation references against the provided excerpts and show an explicit insufficient-evidence response when the loaded material cannot support an answer.

**Learn and apply** React state and forms for the chat UI, Next.js server routes for the streaming boundary, and Zod for runtime validation. [React Learn](https://react.dev/learn), [Next.js Learn](https://nextjs.org/learn), [Vercel AI SDK](https://ai-sdk.dev/docs/introduction), [Zod](https://zod.dev/)

**Done when** a learner can ask one supported question and inspect the cited page reference, and a question outside the chapter scope returns the insufficient-evidence response.

## Phase 2 — Interactive tutoring, tools, and tracing in TypeScript

- [ ] **2.1 — Teach-back loop.** Use typed outputs to render a formula card and a short free-text check. Assess the learner's explanation against a small rubric, show targeted feedback or a follow-up hint, and end the loop when the rubric is met or the attempt limit is reached.
- [ ] **2.2 — MCP tool.** Add a TypeScript MCP server in `packages/mcp-nctb-tools` and connect one read-only tool that retrieves practice prompts for the active chapter from a small question bank.
- [ ] **2.3 — Observability.** Add Langfuse tracing for model calls and tool invocations with curriculum scope, latency, and token usage.

**Learn and apply** structured outputs, streaming tool UI, MCP tool schemas and client integration, and trace inspection. [Vercel AI SDK](https://ai-sdk.dev/docs/introduction), [MCP server tutorial](https://modelcontextprotocol.io/docs/develop/build-server), [Langfuse docs](https://langfuse.com/docs)

**Done when** a learner can revise an incomplete answer after feedback and a trace shows how a practice prompt was fetched.

## Phase 3 — Python service and hybrid retrieval

- [ ] **3.1 — Typed retrieval API.** Create `services/ai-engine` with Python 3.12+, uv, FastAPI, and Pydantic. Connect one retrieval endpoint to the existing Next.js tutor.
- [ ] **3.2 — Ingestion and search.** Ingest the same chapter while preserving pages and formulas. Filter by curriculum scope, combine pgvector similarity with PostgreSQL full-text ranking using reciprocal rank fusion, and inspect Bangla and English terms and formulas in the retrieved excerpts.
- [ ] **3.3 — Retrieval evaluation.** Start with a small versioned set of supported questions, expected page references, and unsupported questions. Compare keyword-only and hybrid retrieval on the same inputs. Record citation correctness, unsupported-question handling, latency, and token usage or cost when available.

**Learn and apply** Python typing and async APIs, dependency management, schema validation, SQL text search and embeddings, and repeatable evaluation. [Python tutorial](https://docs.python.org/3/tutorial/), [uv projects](https://docs.astral.sh/uv/guides/projects/), [FastAPI tutorial](https://fastapi.tiangolo.com/tutorial/), [Pydantic](https://pydantic.dev/docs/validation/latest/get-started/), [PostgreSQL text search](https://www.postgresql.org/docs/current/textsearch.html), [pgvector](https://github.com/pgvector/pgvector), [Langfuse evaluation](https://langfuse.com/docs/evaluation/overview)

**Done when** both retrieval modes can be evaluated against the same cases and the results explain where hybrid search helps or falls short.

## Phase 4 — Mock exam, progress report, and demo

- [ ] **4.1 — Stateful mock exam.** Build one LangGraph flow that generates a short mixed-format chapter exam, checkpoints while waiting for submission, grades against a rubric, and suggests a revision topic. Retrying a submission should produce a single recorded result.
- [ ] **4.2 — Progress preview.** Show a synthetic learner's attempts, rubric results, and suggested next steps in a parent-facing report that can be generated on demand in the demo.
- [ ] **4.3 — Reproducible showcase.** Package local dependencies with Docker Compose, document setup and model credential requirements, and capture a walkthrough with sample citations, evaluation results, and one trace.

**Learn and apply** workflow state, interrupts and resumption, idempotent persistence, and local service orchestration. [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview), [Docker Compose](https://docs.docker.com/compose/)

**Done when** a fresh checkout can run the demo from the documented setup and complete one tutoring session and one mock exam through to the progress report.

## Progress and decisions

Work through the phases in order. If you fork the project, each phase's completion check gives you a reproducible stopping point. Mark tasks complete after verification and record decisions and measured results here.

| Date | Phase / task | Decision or evidence |
| --- | --- | --- |
| 2026-11-20 | Planning | Start with one chapter and a TypeScript tutor. Introduce Python when retrieval moves to a dedicated service. |
