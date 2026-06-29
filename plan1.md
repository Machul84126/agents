You are a senior Java documentation and repository analysis agent working in an SDD-based project that uses AI agents.

Your task is to create only one documentation file:

`docs/capability-index.md`

for the Java shared technical library:

`investments-technical-commons`

This library contains reusable technical utilities used by multiple services, for example validators, parsers, converters, formatters, normalizers, exception helpers, date/time utilities, BigDecimal/money utilities, Spring-independent helpers, and similar reusable technical functions.

The goal of `docs/capability-index.md` is to give other agents a fast, reliable map of all public/reusable capabilities available in this library, so they can check what already exists without scanning the entire codebase and wasting context.

Do not create other documentation files in this task.
Do not refactor production code.
Do not change method implementations.
Do not add new utilities.
Do not generate detailed per-category documentation yet.
Only create or update `docs/capability-index.md`.

---

# Main objective

Create a complete `docs/capability-index.md` file that lists all methods, classes, functions, utilities and capabilities from `investments-technical-commons` that can reasonably be used by external services.

The file must answer:

1. What reusable technical capabilities exist in this library?
2. When should an external service use them?
3. What is the main class/method to use?
4. Where is the implementation located?
5. What category does the capability belong to?
6. Are there any important usage notes or limitations?

---

# Important scope rules

Include items that are intended or reasonably suitable for external service usage, especially:

* public classes,
* public static utility methods,
* public validators,
* public parsers,
* public converters,
* public formatters,
* public normalizers,
* public exception helpers,
* public constants that services may use,
* public annotations,
* public configuration helpers,
* public DTO-like helper structures if they are part of the technical library API,
* public enums used by technical helpers,
* reusable test helpers if they are intended for service tests,
* Spring helpers if they are generic and service-independent.

Do not include:

* private methods,
* purely internal implementation details,
* generated code,
* build output,
* target directories,
* IDE files,
* one-off code not intended for reuse,
* test-only helpers unless they are clearly reusable by external service tests,
* business-process-specific logic that should not be reused by other services.

If you are unsure whether something is public/reusable, include it in a separate section called:

`## Needs review`

and explain why it needs manual review.

---

# Context orchestration requirements

The project is large. You must be very careful with context usage.

Use subagents for repository research. Do not try to load the whole codebase into one context.

You must orchestrate the work in phases:

## Phase 1 - Repository map

Use a research subagent to scan the project structure and identify:

* Maven modules if present,
* source roots,
* package structure,
* main Java packages,
* test packages,
* existing docs,
* existing README/AGENTS/CHANGELOG if present,
* public API candidates.

The subagent should return only a compact summary, not full file contents.

## Phase 2 - Public API discovery

Use one or more research subagents to discover all public/reusable Java API candidates.

The scan must include at least:

* all `public class`,
* all `public final class`,
* all `public abstract class`,
* all `public interface`,
* all `public enum`,
* all `public @interface`,
* all public methods,
* all public static methods,
* all public constructors,
* all public constants.

Use repository search commands where possible, for example:

* `find`
* `rg`
* `grep`
* Maven source tree inspection
* package-based scanning

Do not rely only on filenames.
Do not rely only on folder names.
Do not rely only on README.
You must inspect source structure and public declarations.

## Phase 3 - Categorization

Group discovered capabilities into practical categories.

Suggested categories:

* Validators
* Parsers
* Converters
* Formatters
* Normalizers
* Date / Time
* String utilities
* BigDecimal / Money
* Collections
* Exceptions
* Serialization / JSON
* Spring utilities
* Security utilities
* Test utilities
* Constants / Enums
* Annotations
* Other technical utilities
* Needs review

Use only categories that actually exist in the project.

Do not invent capabilities.
Do not invent classes.
Do not invent methods.
Everything in the final file must be traceable to actual source code.

## Phase 4 - Verification pass

After the first draft, perform a separate verification pass.

Use a different subagent or a separate scan to check whether anything was missed.

The verification must compare:

1. All public Java declarations found in source code.
2. All items listed in `docs/capability-index.md`.
3. All public classes that have no documented capability.
4. All public methods that may be reusable but were not listed.
5. All packages that are missing from the index.

Create a temporary internal checklist during the task.
Do not necessarily commit the checklist unless the repository convention requires it.
Use it to ensure completeness.

The final answer must include a short verification summary with:

* number of source files scanned,
* number of public classes/interfaces/enums/annotations found,
* number of public/reusable capabilities documented,
* list of packages covered,
* list of items placed in `Needs review`,
* explicit statement whether any files or packages could not be analyzed.

If anything could not be analyzed, say so clearly.
Do not claim full coverage unless the verification pass actually supports it.

---

# Required structure of docs/capability-index.md

Create the file with this structure:

```md
# investments-technical-commons - Capability Index

This file is a fast map of reusable technical capabilities available in `investments-technical-commons`.

It is intended for AI agents and developers implementing external services.

This file does not replace source code or Javadoc.  
Before using a capability, inspect the referenced class and method.

## How to use this index

1. Find the category that matches your technical need.
2. Check whether an existing capability already solves the problem.
3. Open the referenced class/method.
4. Use the existing API instead of duplicating logic in a service.
5. If a capability is missing, add it to `investments-technical-commons` only if it is reusable and service-independent.

## Capability summary

| Category | Capability count | Main packages |
|---|---:|---|
| Validators | X | `...` |
| Parsers | X | `...` |

## Validators

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|
| Validate Polish NIP | You need to validate a Polish tax identifier | `NipValidator.isValid(String)` | `src/main/java/.../NipValidator.java` | Accepts formatted and unformatted values |

## Parsers

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Converters

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Formatters

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Normalizers

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Date / Time

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## BigDecimal / Money

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## String utilities

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Exceptions

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Spring utilities

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Test utilities

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Constants / Enums

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Annotations

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Other technical utilities

| Capability | Use when | Main class/method | Location | Notes |
|---|---|---|---|---|

## Needs review

| Item | Location | Reason |
|---|---|---|

## Coverage verification

| Area | Result |
|---|---|
| Source files scanned | X |
| Public classes found | X |
| Public interfaces found | X |
| Public enums found | X |
| Public annotations found | X |
| Public/reusable capabilities documented | X |
| Packages covered | `...` |
| Packages with no reusable public API | `...` |
| Items requiring manual review | X |
```

Remove empty categories if there are no matching items, unless keeping them helps readability.

---

# Style requirements

Write concise descriptions.

The file is for fast agent navigation, not long-form documentation.

Each capability should be described in one short sentence.

Use practical names for capabilities, for example:

* `Validate NIP`
* `Parse BigDecimal from Polish decimal string`
* `Normalize whitespace`
* `Convert LocalDate to SQL Date`
* `Format amount for display`
* `Build validation exception`

Avoid vague capability names like:

* `Utility method`
* `Helper`
* `Common function`
* `Validation stuff`

Use exact class and method names.

For overloaded methods, list the most important overloads or mention that overloads exist.

Example:

```md
| Parse monetary amount | You need to parse user/import decimal values into `BigDecimal` | `MoneyParser.parse(String)`, overloads available | `src/main/java/.../MoneyParser.java` | Handles comma decimal separator |
```

---

# Completeness requirements

You must not stop after a superficial scan.

Before finishing, verify at minimum:

1. Every source package was inspected.
2. Every public top-level type was considered.
3. Every public static utility method was considered.
4. Every public enum was considered.
5. Every public annotation was considered.
6. Every test utility package was considered separately.
7. Existing docs/README files were checked for references to utilities.
8. No obvious public reusable utility was left undocumented.

If there is an existing `docs/capability-index.md`, do not overwrite it blindly.
Merge with it, correct it, and remove stale entries that no longer exist.

If a documented item cannot be traced to source code, remove it or place it in `Needs review`.

---

# Required final response

After editing `docs/capability-index.md`, respond with:

1. A short summary of what was created.
2. The path of the created file.
3. The verification summary.
4. Any uncertain items placed in `Needs review`.
5. Any limitations or files that could not be analyzed.

Do not include the full file content in the final response unless requested.

---

# Completion gate

You are done only when all of the following are true:

* `docs/capability-index.md` exists.
* It contains a capability summary.
* It contains categorized capability tables.
* Every listed item points to a real source location.
* Every public/reusable API candidate was considered.
* A verification pass was completed.
* Uncertain items are listed in `Needs review`.
* The final response includes a coverage summary.
* No unrelated files were changed unless strictly necessary.
