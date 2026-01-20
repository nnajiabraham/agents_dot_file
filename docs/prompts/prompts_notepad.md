# Prompts Notepad
Notepad for my reusable prompts.

----------------------------------------------------------
**Prompt Title**: Implement RFC
**Prompt Template**:
```
Continue implementing RFC-XXX-XXX.md from Task X:XXXXX. The frontend dev server is running on port 5173 and you should use the Playwright browser mcp tools available (mcp_playwright_browser_navigate, mcp_playwright_browser_snapshot, mcp_playwright_browser_take_screenshot, etc...) to verify UI changes instead of curl since React renders client-side. After completing each task, update the RFC document's checklist and Implementation Notes section with details about what was done, then proceed to the next task sequentially until all tasks are complete. Read the implementation notes summary section and task completed to figure out where we are with implementing this RFC and continue. Also take note of all the and always use the @Makefile for running cmds. Also you have access to context7 mcp tool always use this to verify you're using the latest or up to date instruction for external libraries. Iam not sure about the checked items and checked test cases so verify their implementation before to figure out if we should check off those items or we need to do stuff for those task.
```
----------------------------------------------------------


----------------------------------------------------------
**Prompt Title**: Design & Plan Project with Learning Goals
**Purpose**: Use this when you have a project idea/spec and want to design the architecture + create a phased implementation plan optimized for learning a new language/framework.
**Prompt Template**:
```
I am an experienced software engineer with production experience in [YOUR_LANGUAGES e.g. JS/TS, Python, Java] learning [NEW_LANGUAGE e.g. Rust] by building a real project.

I have a project specification: @[PATH_TO_SPEC_DOC]

Help me design and plan this project with the goal of learning [NEW_LANGUAGE] while building it. We'll work together to create:

1. **Architecture document** (`docs/[project]-architecture.md`):
   - System overview with Mermaid diagrams (use conservative syntax: no parentheses in labels, no `\n` escapes)
   - Storage model decisions with tradeoffs explained (e.g., JSONL vs JSON, why)
   - Concurrency/safety considerations appropriate for the project scope
   - "Right-sized" code structure (avoid over-engineering; this is a single-user/simple tool, not enterprise scale)
   - References section with relevant docs/articles

2. **Phased implementation & learning plan** (`docs/[project]-design-and-phases.md`):
   - "Rust concepts by phase" overview table
   - Each phase broken down with:
     - **Learning goals**: specific language concepts to focus on (with `[ ]` checkboxes)
     - **Implementation tasks**: granular file/module-level tasks (with `[ ]` checkboxes)
     - **Test cases**: explicit unit + integration tests serving as harness for both me and the agent (with `[ ]` checkboxes)
     - **Review checklist**: what I should look at in the diff to learn (with `[ ]` checkboxes)
   - TDD approach: tests validate implementation correctness
   - Suggested crates/libraries with docs links
   - "Definition of Done" summary checklist

Guidelines:
- Keep MVP scope tight; avoid abstractions that aren't directly needed
- Phases should be small enough to review in one sitting
- Integration tests must use temp dirs and never touch real user config
- Include a Makefile with common dev commands (build, test, lint, format, clean, install)
- Use canonical log lines / wide events pattern for observability where appropriate
- Always use context7 MCP to verify latest API docs for libraries before recommending patterns

Start by reading the spec, then ask clarifying questions before proposing architecture. Include Mermaid diagrams for visual alignment. After we agree on architecture, create the phased plan document.
```
----------------------------------------------------------


----------------------------------------------------------
**Prompt Title**: Implement Phased Learning Project
**Purpose**: Use this to kick off (or continue) implementation of a project that already has architecture + phased learning plan docs. Optimized for learning a new language while building.
**Prompt Template**:
```
I am learning [NEW_LANGUAGE e.g. Rust] by building [PROJECT_NAME].

Project documents:
- Spec: @[PATH_TO_SPEC e.g. docs/project-mvp-spec.md]
- Architecture: @[PATH_TO_ARCH e.g. docs/project-architecture.md]
- Implementation plan: @[PATH_TO_PLAN e.g. docs/project-design-and-phases.md]

Current status: [Starting Phase 0 | Continuing from Phase X task Y | etc.]
Phases to implement: [n e.g. 1, 3, or "all"]

Pre-implementation checks:
1. Verify `.gitignore` includes build/test artifacts (e.g., `target/`, `*.lock`, coverage files)
2. Use context7 MCP to check toolchain installation requirements for [NEW_LANGUAGE]
3. Report all required toolchains and how to install them before proceeding

Implementation workflow:
1. Read the plan document to understand where we are (check completed items)
2. For the current phase:
   - Implement tasks in order, checking off `[ ]` items as completed
   - Write tests FIRST (TDD) - they serve as the verification harness
   - Run `make test` and `make lint` after each significant change
   - Log what was done in the plan doc's checklist
3. After completing a phase:
   - Verify all tests pass (`make test`, `make lint`, `make format-check`)
   - Update the plan doc (check off completed items)
   - Update `docs/learning_guide.md` (create if not exists) with:
     - Concepts encountered in this phase
     - Language-specific syntax and terminologies
     - How concepts are implemented in the project (with code references)
     - Short notes for learning review
   - Commit all relevant changes (ensure build artifacts are excluded)
   - If more phases remain (n > 1), proceed to next phase

Guidelines:
- Always use context7 MCP to verify latest crate APIs/docs before writing code
- Use the Makefile for all build/test/lint commands
- Never use `unwrap()` in production code paths (tests may use it)
- Integration tests must use temp dirs (never touch real user config)
- If a test fails, fix the implementation, don't skip the test
- Keep commits/changes small and reviewable
- If you're unsure about a design decision, ask before implementing

After each phase, tell me:
- What was implemented
- What [NEW_LANGUAGE] concepts I should pay attention to in the diff
- Any gotchas or patterns worth noting

Let's begin.
```
----------------------------------------------------------


----------------------------------------------------------
**Prompt Title**: Continue Phased Learning Project
**Purpose**: Short prompt to continue an in-progress phased learning project in a new chat context.
**Prompt Template**:
```
Continue implementing [PROJECT_NAME] from Phase [X], Task: [TASK_DESCRIPTION].

Docs:
- Spec: @[PATH_TO_SPEC]
- Architecture: @[PATH_TO_ARCH]
- Plan: @[PATH_TO_PLAN]

Read the plan doc to see completed items and pick up where we left off. Follow the implementation workflow:
- TDD: write/verify tests first
- Use `make test` and `make lint` after changes
- Check off completed items in the plan doc
- Use context7 MCP for latest crate docs
- After each task, summarize what I should review to learn

Begin.
```
----------------------------------------------------------
