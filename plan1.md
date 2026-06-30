You are a senior repository-customization and agent-workflow architect working inside the `investments-technical-commons` repository.

Your task is to inspect the current repository state and then create or update a minimal, production-usable agent-instructions system for this project.

The repository is a shared Java technical library.
Its purpose is to contain reusable, service-independent technical capabilities used by multiple services, for example:
- validators
- parsers
- converters
- formatters
- normalizers
- exception helpers
- date/time helpers
- BigDecimal/money helpers
- technical annotations
- test helpers intended for reuse
- other cross-service technical utilities

Important business context:
- Functions in this library are created ONLY by AI agents.
- Agents working on other services may discover a need for a reusable technical function while implementing service changes.
- When this happens, they should not make library decisions ad hoc.
- Instead, they should hand off the problem and full context to a specialized library agent for `investments-technical-commons`.
- The library agent must evaluate whether the function already exists, whether it belongs in this library, how it should be named, where it should live, and how documentation must be updated.

Your task is NOT to implement production Java code in this step.
Your task is to create the repository instruction structure and files that establish this workflow.

## Objectives

Create a coherent, minimal, non-overengineered customization system for Copilot-centered work in this repository.

The structure must:
1. Work well with GitHub Copilot and VS Code customizations.
2. Stay understandable for humans.
3. Avoid duplicated instructions across many files.
4. Establish one clear expert agent for library governance.
5. Allow agents from other services to delegate requests into this repository in a repeatable way.
6. Align with the existing repository state instead of blindly overwriting files.
7. Reuse and reference existing documentation such as `docs/capability-index.md` where appropriate.

## Required design principles

Follow these principles:

- Prefer a small number of focused files over many overlapping files.
- Put broad repository-wide rules in `.github/copilot-instructions.md`.
- Put reusable cross-agent project guidance in `AGENTS.md`.
- Put Java/path-specific rules in `.github/instructions/*.instructions.md`.
- Create one main specialized custom agent for this repository:
  - `technical-commons-librarian`
- Create one hidden read-only research subagent:
  - `commons-researcher`
- Create one prompt file that other agents can use to delegate a request into the library:
  - `propose-or-add-commons-utility`
- Do not introduce hooks, skills, plugins, or many extra agents unless the existing repository already clearly needs them.
- Avoid overdocumentation.
- Avoid copying the same long rules into multiple files.
- If two files would repeat the same content, keep one file canonical and make the other file shorter and reference it.

## Files to create or update

At minimum, create or update these files if they do not already exist:

- `AGENTS.md`
- `.github/copilot-instructions.md`
- `.github/instructions/java-public-api.instructions.md`
- `.github/instructions/docs-and-javadoc.instructions.md`
- `.github/instructions/capability-index.instructions.md`
- `.github/agents/technical-commons-librarian.agent.md`
- `.github/agents/commons-researcher.agent.md`
- `.github/prompts/propose-or-add-commons-utility.prompt.md`

You may also create one supporting document if needed:

- `docs/contributing/reusable-utility-policy.md`

Only create this extra file if it meaningfully reduces duplication and keeps the setup cleaner.

## Scope and content rules for each file

### `AGENTS.md`

This is the cross-agent canonical project guide.
It must be plain Markdown without YAML frontmatter.

Keep it short and practical.

It should contain:
- what this repository is
- what belongs here
- what must not be added here
- where to look first before adding a new utility
- how to verify whether a similar utility already exists
- the required completion gate for public reusable API changes
- a concise workflow for:
  - using the library
  - adding a new reusable function
- references to:
  - `docs/capability-index.md`
  - relevant `.github/...` files where useful

It should define the completion gate roughly as:
- code compiles
- tests pass
- public reusable API has Javadoc
- `docs/capability-index.md` is updated if needed
- no duplicate utility was introduced
- naming/package placement follows repository conventions

### `.github/copilot-instructions.md`

This must be short.
Do not duplicate the full content of `AGENTS.md`.

It should contain only broad Copilot-relevant rules, such as:
- this repo is a shared Java technical commons library
- keep changes service-independent
- do not add business-process-specific logic
- before adding a utility, check `docs/capability-index.md`
- public reusable API changes require tests and Javadoc
- if adding a reusable capability, use the `technical-commons-librarian` custom agent workflow

### `.github/instructions/java-public-api.instructions.md`

This must use YAML frontmatter with at least:
- `name`
- `description`
- `applyTo: "src/main/java/**/*.java"`

This file should define Java public API rules, including:
- what counts as public reusable API in this library
- naming conventions for validators/parsers/formatters/converters/normalizers/helpers
- package placement expectations
- Javadoc expectations for public reusable API
- test expectations
- prohibition of service-specific business logic
- requirement to update `docs/capability-index.md` when applicable

Keep the rules concrete and imperative.
Include at least a few short examples of good names.

### `.github/instructions/docs-and-javadoc.instructions.md`

Use YAML frontmatter.
Apply to:
- `src/main/java/**/*.java`
- `docs/**/*.md`

This file should define:
- how Javadoc should be written for public reusable API
- how docs should stay aligned with code
- what belongs in `docs/capability-index.md`
- what should stay only in Javadoc and not be duplicated in docs
- how to describe null handling, exceptions, side effects, and usage boundaries

Do not create a huge style guide.
Keep it operational.

### `.github/instructions/capability-index.instructions.md`

Use YAML frontmatter.
Apply to:
- `docs/capability-index.md`

This file should define:
- that `capability-index.md` is a navigation map, not full API documentation
- entries must point to real code locations
- only externally reusable/public capabilities should be listed
- internal/private details must not be listed
- categories must stay practical
- stale entries must be removed
- public reusable capabilities should be easy for agents to scan quickly

### `.github/agents/commons-researcher.agent.md`

This agent should be hidden from normal direct use if supported by the target format.
It should be a read-only research specialist.

Its purpose:
- search for duplicate or similar utilities
- inspect package structure and naming patterns
- inspect `docs/capability-index.md`
- identify the best existing category/package/class neighbors
- return a compact decision-support summary

Its tools should be read/search oriented only.
Do not give it broad edit/write permissions.

Its output should be a structured summary with sections like:
- Existing matches
- Near matches
- Recommended category/package
- Naming recommendation
- Risks / ambiguities

### `.github/agents/technical-commons-librarian.agent.md`

This is the main expert agent for the repository.

It should:
- be user-invocable
- be framed as the library governance and implementation expert
- use the `commons-researcher` as a subagent if supported
- first perform a duplicate/reuse/governance check
- only then decide whether to:
  - reuse an existing utility
  - extend an existing utility
  - add a new one
  - reject the change as not appropriate for this library

Its instructions must explicitly tell it to evaluate:
1. does an equivalent or near-equivalent capability already exist?
2. is the requested function technical and reusable across services?
3. is it free from bounded-context or service-specific business logic?
4. what is the best package and class location?
5. what is the best public API shape and naming?
6. what docs and Javadocs must be updated?

It should also define the required outputs when it performs a change:
- code changes
- tests
- Javadocs
- capability-index updates when required
- concise final summary of decisions taken

### `.github/prompts/propose-or-add-commons-utility.prompt.md`

This prompt file must invoke the `technical-commons-librarian` agent.

It should help a calling agent provide complete structured input, including:
- originating service and feature
- problem being solved
- why reuse in commons is needed
- candidate function/class names if any
- sample inputs and outputs
- whether a similar utility was already searched for
- constraints
- done criteria

The prompt should instruct the library agent to:
- first assess duplication and fit
- then decide whether to add, extend, reuse, or reject
- then implement or document the decision as appropriate

## Research and repository inspection requirements

Before writing files:
1. Inspect the repository structure.
2. Inspect whether any of these files already exist.
3. Inspect existing docs, especially `docs/capability-index.md`.
4. Inspect current package naming and Java utility structure if present.
5. If a file already exists, merge and improve; do not overwrite blindly.
6. Avoid creating files that duplicate already-existing high-quality files.

## Style rules

- Write everything in clear English unless an existing repository convention strongly indicates another language.
- Prefer short sections and bullet lists over dense prose.
- Keep instructions specific and operational.
- Avoid generic statements like “write clean code”.
- Prefer rules like “Do not add service-specific DTOs to this library.”
- If referencing another file, use relative Markdown links when useful.
- Do not include raw external URLs in content unless the repository convention already does so.
- Do not invent repository facts. Base the content on the real repository state.

## Anti-overengineering constraints

Do NOT:
- create many extra files “just in case”
- create a huge documentation framework
- create multiple overlapping custom agents without a strong reason
- create hooks or skills unless clearly justified by existing repository needs
- duplicate the same full ruleset in AGENTS.md and copilot-instructions.md
- create templates that are too abstract to be usable

## Final response requirements

After making changes, respond with:
1. the list of files created or updated
2. what role each file serves
3. any files you intentionally did not create and why
4. any assumptions you had to make based on missing repository conventions
5. any follow-up recommendations, clearly marked as optional

## Completion gate

You are done only when:
- the target file structure exists
- the content is coherent and non-duplicative
- the specialized library-agent workflow is clearly established
- the setup is minimal rather than overengineered
- the files fit the actual repository state
- the prompt file is usable by agents from other services
- the repository now has a clear, repeatable workflow for deciding and implementing reusable utilities in `investments-technical-commons`
