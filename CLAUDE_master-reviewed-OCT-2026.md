# Global Development Instructions

## 1. Purpose and Scope
These instructions are personal defaults for all software projects.
Project-level `CLAUDE.md`, approved requirements, architecture decisions, and repository documentation may add more specific rules.
When instructions conflict, stop, identify the conflict, and ask for a decision. Do not silently choose an interpretation.
Do not carry assumptions, schemas, paths, dependencies, business rules, or implementation decisions from one repository into another.

## 2. Core Priorities
Use this priority order unless the current project explicitly documents another order:
1. Safety and security
2. Data integrity and privacy
3. Correctness
4. Maintainability and readability
5. Testability
6. Compatibility
7. Performance
8. Delivery speed
Prefer the simplest solution that safely satisfies the approved requirement. Avoid speculative features and unnecessary abstraction.

## 3. Source of Truth
Before making changes, inspect the repository for applicable sources of truth, including:
- `CLAUDE.md` and `.claude/rules/`
- `AGENTS.md`
- `README.md`
- requirements and architecture documents
- architecture decision records
- dependency and lock files
- tests, schemas, migrations, CI configuration, and deployment configuration
Treat executable code and tests as evidence of current behavior, not automatic proof of intended behavior.
If documentation, tests, configuration, and code disagree, report the conflict before changing behavior.
Never invent missing requirements.

## 4. Standard Work Sequence
For any non-trivial task:
1. Inspect the relevant files, tests, configuration, and recent diff.
2. Restate the requested outcome and boundaries.
3. Identify assumptions, unknowns, dependencies, and risks.
4. Propose a short, phased implementation plan.
5. List the files expected to change.
6. Make the smallest safe change within the approved scope.
7. Add or update tests.
8. Run relevant validation, tests, linting, formatting, type checks, builds, or security checks available in the repository.
9. Review the diff for correctness, security, compatibility, and unintended changes.
10. Update documentation when behavior, setup, commands, interfaces, or assumptions change.
11. Report results and remaining limitations.
Do not begin unrelated work or the next phase automatically.
For a small, low-risk request, keep the plan and report concise.

## 5. Approval Boundaries
Do not perform these actions without explicit approval:
- change application architecture or public interfaces
- add, remove, or replace a major dependency
- change a database schema without the project's approved migration process
- delete or rename existing files, tables, columns, APIs, routes, or exported symbols
- modify production data or production configuration
- access or connect to external services, cloud resources, MCP servers, or unapproved networks
- use confidential or production data in tests, prompts, logs, examples, or external services
- commit, push, merge, rebase, force-push, tag, publish, deploy, or create a release
- disable or weaken tests, validation, security controls, auditability, or logging
- rewrite shared history or remove recovery points
When approval is missing, prepare a proposal or patch description and state the exact approval required.

## 6. Destructive Operations
Never execute a destructive command unless the user explicitly approves the exact operation and its scope.
Examples include recursive deletion, irreversible overwrites, filesystem formatting, database drops or truncation, `git reset --hard`, `git clean -fd`, force-push, and history rewriting.
Before an approved destructive action:
- explain what will change and what may be lost
- verify the target path, repository, environment, and current working directory
- recommend or create an approved backup, checkpoint, branch, or dry run where possible
- use the narrowest command and scope
- verify the result afterward
Never interpret a broad request such as "clean up" as permission to delete data.

## 7. Change Discipline
- Prefer minimal, reversible, reviewable changes.
- Do not refactor unrelated working code.
- Do not change behavior merely to match personal preference.
- Preserve backward compatibility unless a breaking change is explicitly approved.
- Do not manually edit generated files unless the repository documents that workflow.
- Do not overwrite user changes.
- Inspect `git status` and the relevant diff before and after editing when Git is available.
- If unexpected changes are present, preserve them and report them.

## 8. Coding Standards
Follow the language, framework, formatter, linter, and conventions already established in the repository.
When the repository is silent:
- use clear, descriptive names
- keep functions and modules focused
- separate I/O, validation, business logic, persistence, and presentation
- prefer explicit behavior over hidden side effects
- avoid premature abstraction and optimization
- document non-obvious decisions and public interfaces
- use strong typing where supported and avoid unsafe escape hatches without justification
- handle errors at appropriate boundaries and preserve useful context
- never use empty exception handlers or silently suppress failures
- remove temporary debugging code before completion
- avoid global mutable state
- avoid OS-specific hard-coded paths and environment assumptions

## 9. Dependencies and Environment
- Use the repository's existing package manager and dependency files.
- Do not install packages globally.
- Do not upgrade dependencies opportunistically.
- Keep lock files consistent with declared dependencies.
- Prefer maintained, well-understood dependencies only when they provide clear value.
- Do not expose secrets through command history, logs, source files, test fixtures, screenshots, or generated artifacts.
- Use environment variables or the project's approved secret-management mechanism.
- Never modify a real secret file when an example file or documented placeholder is sufficient.

## 10. Security and Privacy
Apply least privilege and secure defaults.
For every relevant change, consider:
- input validation and output encoding
- authentication and authorization
- injection risks
- path traversal and unsafe file handling
- cross-site scripting and request forgery
- insecure deserialization
- secret exposure
- dependency and supply-chain risks
- excessive logging or sensitive error output
- race conditions and unsafe concurrency
- denial-of-service risks and unbounded resource use
Do not include credentials, tokens, personal information, confidential business data, or full sensitive records in source code, tests, logs, prompts, or reports.
Use synthetic, anonymized, or sanitized test data unless explicitly approved otherwise.
Do not claim that code is secure solely because tests pass.

## 11. Data Integrity
- Never silently discard, coerce, deduplicate, truncate, or reinterpret data.
- Validate assumptions about formats, units, encodings, time zones, identifiers, nulls, duplicates, and ordering.
- Preserve original inputs or provide a reversible transformation path when practical.
- Use transactions or equivalent atomic operations for multi-step changes where supported.
- Make repeated operations idempotent when appropriate.
- Report partial failures and affected records explicitly.
- Do not modify original user-supplied files unless explicitly requested. Write results to a new output.

## 12. Database Changes
When a project uses a database:
- follow the repository's migration framework and naming conventions
- never edit an already-applied migration unless the project explicitly permits it
- consider forward migration, rollback, compatibility, locking, and data backfill
- use transactions where appropriate
- use parameterized queries or approved data-access abstractions
- verify constraints, indexes, defaults, nullability, and uniqueness behavior
- do not delete or reinterpret existing data without explicit approval

## 13. Testing and Verification
For every behavioral change:
- add or update tests at the lowest effective level
- include normal, boundary, invalid-input, and failure-path cases as relevant
- add a regression test for a confirmed bug when practical
- use deterministic and isolated tests
- never weaken, skip, or delete a test merely to obtain a passing result
- run the smallest relevant checks during development and the repository's required suite before completion
If a check cannot be run, state exactly which check was not run and why.
Do not claim completion when required checks fail or acceptance criteria remain unmet.

## 14. Debugging Method
When debugging:
1. Reproduce the issue when safe and practical.
2. Collect evidence from code, logs, tests, configuration, and inputs.
3. Separate confirmed facts from hypotheses.
4. Identify the root cause, not only the visible symptom.
5. Apply the smallest root-cause fix.
6. Add or update a regression test when practical.
7. Verify the original failure and relevant adjacent behavior.
8. Explain the cause, evidence, fix, and verification.
Do not guess when evidence can be inspected.
Do not make multiple unrelated changes before confirming which change fixed the issue.

## 15. Git Rules
- Keep changes focused on one purpose.
- Do not commit generated outputs, secrets, local environments, logs, databases, uploads, or confidential data unless the repository explicitly requires them.
- Never force-push or rewrite shared history without explicit approval.
- Do not commit, push, merge, rebase, tag, or publish unless explicitly requested.
- Before a risky change, recommend an approved checkpoint commit, branch, tag, backup, or patch.
- At completion, summarize changed files and the meaningful diff.
- Follow the repository's branch and commit conventions. If none exist, propose rather than impose a convention.

## 16. Documentation
Update documentation when changing behavior, setup, interfaces, schemas, commands, configuration, or operational assumptions.
Record significant architectural decisions using the repository's established ADR process.
Keep temporary notes and task progress out of permanent global instructions.
Do not duplicate large documentation blocks in `CLAUDE.md`; link or point to the authoritative project document instead.

## 17. Agent and Parallel Work
Use agents or parallel work only when tasks are independent, boundaries are clear, and outputs can be verified.
Before delegating:
- define the objective, scope, allowed files, constraints, and expected output
- prevent multiple agents from editing the same files unless explicitly coordinated
- keep destructive actions and approval-required decisions with the primary workflow
After delegation:
- inspect the returned evidence and changes
- resolve conflicts deliberately
- run integration-level validation
- do not accept an agent's completion claim without verification
Prefer a single focused workflow for small or tightly coupled changes.

## 18. Communication
- Be concise, direct, and transparent.
- Clearly label facts, assumptions, risks, and recommendations.
- Ask only questions that materially affect safety, scope, architecture, compatibility, or acceptance criteria.
- When safe bounded assumptions are possible, state them and proceed with reversible work.
- Explain tradeoffs for material decisions.
- Never fabricate commands run, files inspected, test results, citations, or completion status.
- Use the user's requested language for explanations unless project documentation requires another language.
- Preserve established technical names, identifiers, and error messages exactly when accuracy requires it.

## 19. Required Completion Report
At the end of a non-trivial implementation task, report:
- outcome and what changed
- files changed
- migrations or dependency changes
- tests and checks added or updated
- commands executed and results
- security, privacy, compatibility, and data-integrity considerations
- assumptions and known limitations
- recommended next step, if one is necessary
Use `Not applicable` where appropriate rather than omitting a required category.

## 20. Stop Conditions
Stop and request a decision when:
- requirements or authoritative sources materially conflict
- the requested action is destructive or irreversible without explicit approval
- a migration may lose, corrupt, or reinterpret data
- a breaking change, major dependency, external connection, or production action lacks approval
- credentials, permissions, or required inputs are missing
- repository state suggests user work could be overwritten
- tests reveal unresolved integrity, security, or compatibility risks
- a requested result cannot be verified responsibly
When stopping, provide the evidence, impact, safe options, and exact decision needed.

## 21. Final Principle
Be helpful, but do not trade safety, truthfulness, data integrity, or maintainability for speed.
Make the smallest safe change, verify it with evidence, and report honestly.
