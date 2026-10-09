# Project Instructions
This is a new, independent project. Do not reuse assumptions, schemas, column names, database structures, or implementation decisions from other repositories unless they are explicitly documented in this repository.

## 1. Architecture Rule
Prefer:

Simple
Readable
Maintainable

over

Clever
Complex
Over-engineered

solutions.

## 2. Source of Truth

Read the following files before making changes:

- `README.md`: setup and usage
- `docs/requirements.md`: approved requirements
- `docs/architecture.md`: approved architecture
- `docs/data_dictionary.md`: input data and database definitions
- `docs/decisions/`: architecture decision records
- `docs/project_status.md`: current implementation status and known issues

When documentation and code conflict, stop and report the conflict. Do not silently choose one.

## 3. Working Agreement

Before editing code:

1. Inspect the relevant code, tests, configuration, and documentation.
2. Restate the requested scope.
3. List assumptions and unresolved questions.
4. Present a short implementation plan.
5. List files expected to change.
6. Identify migration, security, data-loss, and compatibility risks.

Do not implement unrelated improvements. Prefer the smallest safe change that satisfies the approved scope.

## 4. Approval Boundaries

Do not perform the following without explicit approval:

- Add or replace major dependencies
- Change the application architecture
- Change the database schema without a migration
- Delete or rename existing files, tables, columns, APIs, or routes
- Modify production data
- Run destructive database or filesystem commands
- Connect to external services, MCP servers, or cloud resources
- Commit, push, merge, rebase, tag, or publish releases
- Disable tests, security checks, validation, or logging
- Use real confidential production data in tests

If approval is missing, prepare the proposed change and explain what approval is required.

## 5. Before implementation:

1. Analyze requirements
2. Review exisiting code
3. Identify risks
4. Create implementation plan
5. Implement
6. Create tests
7. Review security
8. Review maintainability
9. Report results
10. 

Never skip these steps.
Work on one phase at a time. Do not start the next phase automatically.

## 6. Never execute commands that can delete data.

Never run:
    rm -rf
    del /f /s
    git reset --hard
    git clean -fd

    without approval.

Use only dependencies declared in the project dependency file. Do not install packages globally.

## 7. Repository Structure

```text
#Project_Name/
├─ CLAUDE.md
├─ README.md
├─ pyproject.toml
├─ .env.example
├─ .gitignore
├─ app/
│  ├─ models/
│  ├─ routes/
│  ├─ services/
│  ├─ templates/
│  └─ static/
├─ migrations/
├─ tests/
│  └─ fixtures/
├─ scripts/
├─ docs/
│  ├─ requirements.md
│  ├─ architecture.md
│  ├─ data_dictionary.md
│  ├─ project_status.md
│  └─ decisions/
├─ logs/
├─ uploads/
└─ instance/
```

Business logic belongs in `app/services/`, not in route handlers or templates.

## 8. Coding Standards

- Use type hints for all public functions and service boundaries.
- Prefer dataclass or Pydantic models.
- DO not use Any unless justified.
- Use descriptive names; do not invent unclear abbreviations.
- Keep functions focused and testable.
- Separate I/O, validation, transformation, persistence, analysis, and presentation.
- Do not use broad `except Exception` unless re-raising with context at an application boundary.
- Never silently discard invalid rows, duplicate keys, missing values, or failed conversions.
- Use `pathlib.Path` for filesystem paths.
- Avoid OS-specific hard-coded paths.
- Remove Windows-invalid and invisible characters from generated filenames.
- Use logging.
- Never use print() except temporary debugging.
- Remove debug prints before completion.
- Do not place secrets, credentials, tokens, or confidential values in source code.

## 9. Data and Excel Rules

- Never modify the original uploaded Excel file.
- Validate file extension, size, workbook readability, sheet names, and required columns.
- Display available sheets and columns when validation fails.
- Normalize column names without losing the original names.
- Preserve raw source values for traceability.
- Treat `Serial Number` and `SN` as configurable business keys.
- Record missing keys, duplicate keys, unmatched keys, and join rates.
- Do not silently deduplicate. Document the rule and retain an audit record.
- Use explicit merge validation such as `one_to_one` or `many_to_one` where appropriate.
- Store import batch ID, source filename, file hash, import timestamp, row count, and validation result.
- Do not log full confidential records or associate names.

## 10. Database Rules

- Every schema change requires an Alembic migration.
- Never edit an applied migration.
- Use transactions for imports and multi-step updates.
- Roll back the transaction when any required step fails.
- Use indexes for frequently queried identifiers and timestamps.
- Enforce constraints in the database where practical.
- Prevent duplicate file imports using a file hash or approved idempotency key.
- Do not delete data permanently unless the requirement explicitly calls for it and approval is provided.

## 11. Manufacturing Analytics Rule

### Statistical Analysis Rules

- Always report sample size, missing count, filters, and excluded records.
- Distinguish correlation, prediction, association, and causation.
- Do not automatically remove outliers without reporting the rule and affected count.
- Exclude constant or insufficient-variation columns from correlation calculations.
- Make analysis reproducible by saving parameters and input batch IDs.
- Treat exploratory findings as hypotheses, not confirmed root causes.
- Export the underlying analysis table alongside each graph when practical.

### Machine Learning Rules

- Prevent target leakage and post-outcome information leakage.
- Separate training, validation, and final test data.
- Use chronological splits for time-dependent manufacturing data unless documented otherwise.
- Choose thresholds using validation data only.
- Do not use final test data for model selection or threshold tuning.
- For imbalanced targets, report precision, recall, F1, average precision, balanced accuracy, and the confusion matrix.
- Report false positives, false negatives, alert rate, and false alerts per detected event.
- Save feature lists, excluded columns, model parameters, data period, random seed, and model version.
- Label exploratory models clearly. Do not present them as automatic production-control decisions.
- Do not infer root cause from feature importance alone.

## 12.

## 13. Security and Privacy

- Apply least privilege.
- Validate all uploaded files and user inputs.
- Protect against path traversal, SQL injection, XSS, CSRF, and unsafe file handling.
- Use ORM or parameterized SQL.
- Configure upload size and allowed file types.
- Store uploaded files outside the public static directory.
- Do not expose stack traces, filesystem paths, secrets, or raw SQL in the browser.
- Do not send organizational data to external services without explicit approval.
- Exclude real production files, databases, logs, models, secrets, and uploads from Git.
- Mask or omit personal identifiers unless required for an approved business purpose.

## 14. Testing Rules

For every change:

- Add or update tests before declaring completion.
- Include normal, boundary, missing-data, duplicate-key, invalid-input, and failure-path tests where relevant.
- Use synthetic or sanitized fixtures only.
- Do not weaken or remove tests to make a change pass.
- Run the smallest relevant test set during development, then run the required full suite before completion.

Required commands:

```powershell
python -m pytest -q
python -m ruff check .
python -m ruff format --check .
```

If commands differ, update this section and `README.md` together.

## 15. Git Rules

- Inspect `git status` and relevant diffs before editing.
- Keep commits focused on one purpose.
- Do not include generated outputs, uploaded Excel files, databases, logs, secrets, or virtual environments.
- Do not rewrite shared history.
- Do not commit or push unless explicitly requested.
- Never force push
- Never delete branches.
- Never modify git history without approval.
- Before a risky change, recommend creating a checkpoint commit or tag.
- Report changed files and summarize the final diff.

Suggested branch names:

```text
feature/<short-name>
fix/<short-name>
chore/<short-name>
docs/<short-name>
```

## 16. Error Handling and Logging

- Fail early on invalid input.
- Provide actionable user-facing errors without exposing confidential details.
- Log the technical error with timestamp, operation, batch ID, and correlation ID.
- Never use an empty `except` block.
- Do not continue processing after a validation or integrity failure unless the approved requirement explicitly permits partial success.

When debugging:
1. Identify root cause
2. Explain why it happened
3. Explain evidence
4. Fix cause, not symptoms
5. Verify fix

Do not guess.

## 17. Documentation Rules

Update documentation when behavior, setup, schema, commands, or assumptions change.

Record significant decisions in `docs/decisions/` using:

```text
# ADR-NNN: <Decision title>

## Status
Proposed | Accepted | Superseded

## Context

## Decision

## Consequences

## Alternatives considered
```

Keep temporary progress and unfinished tasks in `docs/project_status.md`, not in this file.

## 18. Required Completion Report

At the end of each implementation task, report:

1. What changed
2. Files changed
3. Database migrations created
4. Tests added or updated
5. Commands executed and results
6. Security and data-integrity considerations
7. Known limitations
8. Recommended next step

Do not claim completion if tests fail, required evidence is missing, or an acceptance criterion is unmet.

## 19. Stop Conditions

Stop and ask for a decision when:

- Requirements conflict or are materially ambiguous
- A destructive action is required
- A schema migration could lose or reinterpret data
- A requested dependency or external connection is not approved
- Actual sheet or column names do not match the documented data dictionary
- Test data indicates low join quality or integrity problems
- A machine-learning request risks target leakage or misleading evaluation
- Security or privacy requirements cannot be satisfied

## 20. Initial Project Workflow

For a new project, follow this order:

1. Inspect repository state
2. Create or review `docs/requirements.md`
3. Create or review `docs/architecture.md`
4. Create or review `docs/data_dictionary.md`
5. Identify risks and unresolved decisions
6. Propose phased implementation plan
7. Wait for approval before implementation
8. Implement one approved phase
9. Test and document
10. Stop and report results

Do not generate the full application in one step.

## 21. Refactor only inside approved scope.

Do not rewrite working code 
unless there is a clear benefit 
and approval exists.
