---
name: aviation-embedded-docs
description: Create, review, and maintain DO-178C-style Markdown document sets for aviation embedded software, including coding standards, systematic code review against CS-* rules (template 13), system requirements, HLR, LLR, architecture/design, code review records, unit tests, integration tests, verification summaries, change impact analysis, and traceability matrices. Use when working on Linux embedded aviation software today or RTOS/bare-metal migration later.
---

# Aviation Embedded Docs

## Overview

Use this skill to create or review a Markdown-based aviation embedded software evidence set. Default to Chinese prose with aviation terms and IDs in English, DO-178C-style lifecycle discipline, DAL B/C rigor, Linux embedded assumptions, and RTOS/bare-metal migration hooks.

This skill helps prepare engineering documentation and internal review evidence. Do not claim the output is a complete certification package unless the user provides project-specific certification plans, authority guidance, and approval context.

## Workflow

1. Identify the artifact type: full document set, single document, traceability update, review, or gap analysis.
2. Read `references/document-set.md` before generating or reviewing documents.
3. For new documents, copy the closest template from `assets/templates/` and fill project-specific content.
4. Preserve the ID scheme and trace links across requirements, design, code, tests, issues, and changes.
5. Call out missing evidence explicitly instead of inventing unavailable verification results.


## Document Defaults

- Language: Chinese first; keep terms such as DO-178C, DAL, HLR, LLR, MC/DC, traceability, baseline in English.
- Rigor: DAL B/C style by default.
- Platform: Linux embedded by default; include RTOS/bare-metal impact fields where timing, scheduling, concurrency, memory, ISR, or hardware coupling matters.
- Format: Markdown tables and concise sections suitable for version control review.
- Traceability: every requirement should link forward to verification and backward to source intent.

## Template Selection

- Use `00_project_plan.md` for lifecycle, roles, baselines, review independence, and evidence strategy.
- Use `01_coding_standard.md` for C/C++/Linux embedded coding rules and future RTOS constraints.
- Use `02_system_requirements.md`, `03_high_level_requirements.md`, and `04_low_level_requirements.md` for requirement decomposition.
- Use `05_architecture_design.md` and `06_detailed_design.md` for structure, interfaces, state, timing, resource, and failure behavior.
- Use `07_code_review.md` for peer review summary, approval, and issue closure.
- Use `13_code_review_against_standard.md` when reviewing **source code** against `01_coding_standard.md` rule IDs (`CS-*`), including tool logs and deviation references; feed findings into `07_code_review.md`.
- Use `08_unit_test.md`, `09_integration_test.md`, and `10_verification_summary.md` for verification planning and evidence.
- Use `11_traceability_matrix.md` whenever requirements, code, or tests change.
- Use `12_change_impact_analysis.md` for change requests, defect fixes, or platform migration.

## Review Rules

- Prefer findings over summaries when reviewing existing documents.
- Flag any requirement that is ambiguous, unverifiable, compound, implementation-only at the wrong level, or missing trace links.
- Flag any test case that lacks expected results, pass/fail criteria, target environment, or requirement coverage.
- Flag any matrix row where the chain from source requirement to test evidence is incomplete.
- Keep generated content conservative: use `TBD` only when project facts are genuinely unknown.
