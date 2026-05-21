---
name: skill-router
description: "Fast skill selection for Codex. Use at the start of non-trivial tasks to decide whether any installed skill should be loaded, which one(s), or whether no skill is needed. Routes planning, knowledge retrieval, web research, browser work, document handling, code analysis, bug fixing, review, testing, implementation, scheduling, and automation work to the smallest useful installed skill set."
---

# Skill Router

Fast skill selection for Codex. Use this as an instant preflight, not a deep planning framework.

The goal is to prevent both failure modes:

- forgetting a useful installed skill
- loading extra skills that add context noise without helping

Core boundary: this skill routes to installed skills only. It should not browse for new skills, install packages, or mutate the skill list unless the user explicitly asks for that.

## Routing Workflow

1. Classify the task by outcome:
   - Planning or long-running project coordination
   - Knowledge base retrieval or local material lookup
   - Web research, source checking, or citation writing
   - Browser operation or page interaction
   - Document, PDF, spreadsheet, or presentation work
   - Web/product design or interactive implementation
   - Repository analysis, bug fixing, code review, testing, or coding
   - Scheduling, retry, automation, or milestone governance
   - Skill/plugin management
   - Plain answer or one-off command
2. Select the smallest installed skill set:
   - Prefer 0 skills when general reasoning and normal tools are enough.
   - Prefer 1 skill for a focused domain.
   - Use 2-3 skills only when the task crosses boundaries, such as PDF analysis plus planning, web research plus citation writing, or implementation plus browser verification.
3. Announce the chosen skill(s) in one short line before acting.
4. Load only the selected skill bodies. Do not load neighboring skills just because they sound related.
5. If a selected skill proves irrelevant, say so and continue without it.

Target time: under 2 seconds for normal requests. Only spend longer when the user explicitly asks for a skill strategy, the task crosses several domains, or the installed skill list has changed.

This skill only chooses from installed skills. Do not browse for new skills or install anything unless the user explicitly asks to find, install, or create a skill.

## Priority Rules

- Explicit user skill request wins: if the user names a skill, use it when available.
- Mandatory tool prerequisite wins: if another skill is required before a specific tool call, load that prerequisite first.
- Specific beats broad: prefer a focused document, browser, test, review, or bug-fix skill over a broad general-purpose skill.
- If the task is simple and no skill clearly helps, skip skills.
- If no installed skill fits but one would clearly help, say that briefly and ask before searching or installing.

## When No Skill Is Needed

Skip skills for:

- Simple shell answers, file reads, git inspection, or straightforward edits.
- Pure backend/API/database work with no special local workflow.
- General explanations, brainstorming, or tradeoff discussions unless the user asks to create or update a skill.
- Tiny copy/text/color tweaks where existing code context is enough.
- Fact lookup where direct browsing or source verification is the real requirement and no installed research skill is needed.

## Installed Skill Categories

Treat these as categories, not fixed requirements. Choose the installed skill names that exist in the current environment. The examples below mirror a generic Hermes-style agent setup.

| Category | Use When | Example Skill Names |
| --- | --- | --- |
| Planning management | Complex task decomposition, long-running projects, plans tied to files | `planning-with-files`, `planner-agent` |
| Knowledge retrieval | Local knowledge base Q&A, semantic search, similar document lookup | `kb-retriever`, `semantic-searcher` |
| Knowledge maintenance | Curating notes, grouping sources, tagging, conflict checks, summaries | `kb-curator`, `conflict-detector`, `auto-summarizer` |
| Documentation lookup | Official docs, technical references, library/framework API lookup | `find-docs` |
| Web research | Web search, fact checks, market/competitor research, source-backed summaries | `web-researcher`, `research-agent`, `source-citation-writer` |
| Browser operation | Open pages, click flows, forms, page replay, screenshot-oriented checks | `browser-operator` |
| PDF handling | PDF reading, generation, extraction, ordinary structure analysis | `pdf` |
| Deep OCR | Scanned files, image-only PDFs, OCR-heavy or page-by-page parsing | `pdf-ocr-deep` |
| Word documents | Word reading, generation, editing, revision suggestions | `docx`, `docx-editor` |
| Presentations | Slide reading, generation, pitch/report decks | `pptx`, `pptx-builder` |
| Spreadsheets | Excel/CSV cleaning, formulas, pivots, statistical analysis | `xlsx`, `xlsx-analyst` |
| Web design implementation | Web pages, landing pages, dashboards, interactive prototypes | `web-design-engineer` |
| Repository analysis | Repo structure, dependencies, entry points, module boundaries | `repo-analyzer` |
| Bug fixing | Concrete bugs, errors, failure diagnosis, minimal fixes | `bug-fixer` |
| Code review | Review findings, security/regression risk, test gaps | `code-reviewer`, `reviewer-agent` |
| Test running | Identify and run tests, summarize failures | `test-runner` |
| Coding/execution agents | Implement patches, execute steps, record deliverables | `coder-agent`, `executor-agent` |
| Scheduling | Reminders, repeated tasks, long-term goal scheduling | `task-scheduler` |
| Automation governance | Retry, fallback, automation audit, milestone review | `failure-retry`, `automation-auditor`, `milestone-reviewer` |
| Skill management | Install, create, update, or validate skills | `skill-installer`, `skill-creator` |

## Common Routes

For planning-heavy work:

- Use a planning management skill when the task needs decomposition, tracking, sequencing, or file-aware planning.
- Add the relevant domain skill only when the plan immediately depends on a specific artifact type, such as PDF, spreadsheet, repository, or browser work.

For knowledge and research:

- Use a knowledge retrieval skill for local/private materials.
- Use a web research skill for public web research, fact checks, and source-backed summaries.
- Add a citation-writing skill only when the answer must include formal source attribution.

For browser tasks:

- Use a browser operation skill when the task requires navigation, clicking, forms, screenshots, or page inspection.
- Do not use browser skills for ordinary web facts when direct browsing/search is enough.

For documents:

- Use PDF, OCR, Word, presentation, or spreadsheet skills based on the file type and required transformation.
- Prefer OCR-specific skills only when the source is scanned, image-heavy, or extraction quality is likely to matter.

For web/product UI implementation:

- Use a web design implementation skill when the deliverable is an actual page, interface, dashboard, or interactive prototype.
- Add browser verification when a local app can run and visual/layout correctness matters.

For code work:

- Use repository analysis for broad orientation or architecture questions.
- Use bug-fixing for a concrete failure or error.
- Use code review for review-style requests.
- Use test-running when the main job is to discover, run, or explain tests.
- Use coding/execution agents only when implementation or a multi-step executable workflow is required.

For automation:

- Use scheduling skills for reminders, recurring tasks, or long-horizon follow-up.
- Use automation governance skills for retry policies, failure handling, audits, or milestone reviews.

For skill management:

- Installing a skill: use a skill installation skill.
- Creating/updating a skill: use a skill creation skill.
- Deciding whether skills are needed: use this skill.
- Searching for a not-yet-installed skill: ask/confirm first, then use a skill installation workflow or normal web/GitHub search as appropriate.

## Output Style

Before work, use one compact line such as:

`Using a PDF handling skill + a planning skill because this task needs document extraction and a follow-up plan.`

If no skill is needed:

`No extra skill needed; this is a straightforward code/read/command task.`

Do not produce a long skill-routing report unless the user explicitly asks for one.

## Good Defaults

- If the user explicitly names a skill, use it.
- If a system/developer instruction says a skill is mandatory for a tool, follow that instruction.
- If two skills overlap, choose the more specific one.
- If unsure whether a skill helps, skip it and proceed with normal codebase/context inspection.
