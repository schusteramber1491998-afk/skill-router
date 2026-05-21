---
name: skill-router
description: "Fast skill selection for Codex. Use at the start of non-trivial tasks to decide whether any installed skill should be loaded, which one(s), or whether no skill is needed. Routes UI/design, Figma, Unity, skill installation/creation, and Codex maintenance work to the smallest useful skill set without searching for or installing new skills unless explicitly asked."
---

# Skill Router

Fast skill selection for Codex. Use this as an instant preflight, not a deep planning framework.

The goal is to prevent both failure modes:

- forgetting a useful installed skill
- loading extra skills that add context noise without helping

Core boundary: this skill routes to installed skills only. It should not browse for new skills, install packages, or mutate the skill list unless the user explicitly asks for that.

## Routing Workflow

1. Classify the task by outcome:
   - Code/UI change
   - Design asset or visual system
   - Figma file operation
   - Unity project operation
   - Skill/plugin management
   - Codex maintenance
   - Plain answer or one-off command
2. Select the smallest installed skill set:
   - Prefer 0 skills when general reasoning and normal tools are enough.
   - Prefer 1 skill for a focused domain.
   - Use 2-3 skills only when the task crosses boundaries, such as web UI plus browser verification or Figma plus design generation.
3. Announce the chosen skill(s) in one short line before acting.
4. Load only the selected skill bodies. Do not load neighboring skills just because they sound related.
5. If a selected skill proves irrelevant, say so and continue without it.

Target time: under 2 seconds for normal requests. Only spend longer when the user explicitly asks for a skill strategy, the task crosses several domains, or the installed skill list has changed.

This skill only chooses from installed skills. Do not browse for new skills or install anything unless the user explicitly asks to find, install, or create a skill.

## Priority Rules

- Explicit user skill request wins: if the user names a skill, use it when available.
- Mandatory tool prerequisite wins: for example, use `figma-use` before `use_figma`.
- Specific beats broad: prefer `banner-design` for banners over generic `design`; prefer `skill-creator` for skill edits over this router.
- If the task is simple and no skill clearly helps, skip skills.
- If no installed skill fits but one would clearly help, say that briefly and ask before searching or installing.

## When No Skill Is Needed

Skip skills for:

- Pure backend/API/database work with no special local workflow.
- Simple shell answers, file reads, git inspection, or straightforward edits.
- General explanations, brainstorming, or tradeoff discussions unless the user asks to create or update a skill.
- UI tasks where the user only asks for a tiny copy/text/color tweak and existing code context is enough.
- Research or fact lookup where web browsing or source verification is the real requirement.

## Installed Skill Map

- `ui-ux-pro-max`: Web/mobile UI design quality, page layout, dashboards, admin panels, landing pages, color/type/style decisions, UX review.
- `ui-styling`: Concrete frontend styling with Tailwind, shadcn/ui, accessible components, responsive UI implementation.
- `design-system`: Design tokens, component specs, CSS variables, systematic UI foundations.
- `brand`: Brand voice, visual identity, messaging, brand guideline consistency.
- `design`: Broad creative design tasks, logos, icons, CIP/mockups, social images, presentations.
- `banner-design`: Banners, ads, website hero graphics, social creative assets.
- `slides`: HTML slide decks or strategic presentations.
- `figma-use`: Mandatory before any Figma Plugin API / `use_figma` operation.
- `figma-generate-design`: When building or updating full Figma screens from a page, code, or description. Use with `figma-use`.
- `unity-skills`: Unity Editor automation or Unity project work. Then pick relevant Unity sub-skill by task.
- `skill-installer`: Installing skills from curated lists or GitHub repos.
- `skill-creator`: Creating or updating skills.
- `keep-codex-fast`: Codex Desktop/CLI cleanup, slowness, logs, sessions, worktree maintenance.

## Common Routes

For a new webpage, landing page, admin screen, dashboard, or mobile UI:

- Use `ui-ux-pro-max`.
- Add `ui-styling` when implementing concrete components/styles.
- Add `design-system` only when the project needs durable tokens or a reusable component system.

For improving an existing web UI:

- Use `ui-ux-pro-max` for UX/design diagnosis.
- Use browser verification after implementation when a local app can run.

For Figma:

- If the user asks to write, create, inspect programmatically, or update a Figma file, use `figma-use`.
- If the task is a whole screen/page/view in Figma, use both `figma-use` and `figma-generate-design`.
- Do not use Figma skills for normal webpage coding unless Figma is the requested deliverable.

For Unity:

- Use `unity-skills` for Unity Editor/project work.
- For existing projects, scout first with `unity-project-scout` when architecture or broad changes are involved.
- For scripts, UI, GameObjects, prefabs, scenes, or tests, choose the matching Unity sub-skill.

For skill management:

- Installing a skill: `skill-installer`.
- Creating/updating a skill: `skill-creator`.
- Deciding whether skills are needed: this skill.
- Searching for a not-yet-installed skill: ask/confirm first, then use `skill-installer` or normal web/GitHub search as appropriate.

For Codex app maintenance:

- Use `keep-codex-fast` for slowness, cleanup reports, log/session/worktree bloat, or safe maintenance.

## Output Style

Before work, use one compact line such as:

`Using ui-ux-pro-max + ui-styling because this changes web UI layout and styling.`

If no skill is needed:

`No extra skill needed; this is a straightforward code/read/command task.`

Do not produce a long skill-routing report unless the user explicitly asks for one.

## Good Defaults

- If the user explicitly names a skill, use it.
- If a system/developer instruction says a skill is mandatory for a tool, follow that instruction.
- If two skills overlap, choose the more specific one.
- If unsure whether a skill helps, skip it and proceed with normal codebase/context inspection.
