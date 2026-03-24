---
name: shincn
description: Use when setting up a new project with ShinCN design system, or when updating an existing project's UI to match ui.ch.sh — Open Runde typography, Phosphor Icons, skeuomorphic buttons, and Base UI primitives via the ui.ch.sh registry.
---

# ShinCN

## Overview

ShinCN is a personal design system built on shadcn/ui, distributed at `https://ui.ch.sh`. This skill sets up new projects or audits and updates existing ones to match the latest ShinCN patterns.

**Always fetch the latest docs at runtime before doing anything:**

```
https://ui.ch.sh/docs
```

Use the shadcn skill for all component operations if available.

## Detect: New or Existing Project

Check for `components.json` in the project root.

- **Not found** → New project setup
- **Found** → Existing project audit

---

## New Project Setup

### Step 1: Ask for base color

Before running any commands, ask the user:

> "What base color would you like? (e.g. zinc, slate, stone, gray, neutral)"

Use their answer as `--base-color` in the init command.

### Step 2: Init with ui.ch.sh registry

```bash
npx shadcn@latest init --registry https://ui.ch.sh/r --base-color <chosen-color>
```

### Step 3: Always install button

```bash
npx shadcn@latest add button --registry https://ui.ch.sh/r
```

### Step 4: Install any other components needed for the project

Always use `--registry https://ui.ch.sh/r` for every component install. Never pull from the default shadcn registry.

```bash
npx shadcn@latest add <component> --registry https://ui.ch.sh/r
```

---

## Existing Project Audit & Update

### Step 1: Check current registry

Read `components.json`. If the registry is not `https://ui.ch.sh/r`, update it.

### Step 2: Update installed components

For each installed component, pull the latest from ui.ch.sh:

```bash
npx shadcn@latest add <component> --registry https://ui.ch.sh/r --overwrite
```

### Step 3: Audit code for violations

Scan the codebase for these patterns and fix them:

| Issue | Fix |
|---|---|
| Raw color classes (`bg-white`, `text-neutral-950`) | Replace with semantic tokens (`bg-background`, `text-foreground`) |
| `space-x-*` / `space-y-*` | Replace with `flex gap-*` |
| `w-4 h-4` on equal dimensions | Replace with `size-4` |
| Icons with `size={N}` inside Button | Remove size prop, add `data-icon="inline-start"` or `data-icon="inline-end"` |
| Raw `<button>` elements | Replace with `Button` component |
| Manual `dark:` color overrides | Remove — use semantic tokens only |
| `className` string concatenation | Replace with `cn()` from `@/lib/utils` |

---

## Design Language

Fetch `https://ui.ch.sh/docs` for the latest. Core principles:

**Typography**
- Font: Open Runde (Regular 400, Medium 500, Semibold 600, Bold 700)
- Fallback: Inter, system-ui, sans-serif
- Loaded via `@font-face` with WOFF2

**Icons**
- Library: Phosphor Icons (`@phosphor-icons/react`)
- Inside Button: use `data-icon="inline-start"` or `data-icon="inline-end"`, no size classes
- Standalone: explicit size prop is fine

**Buttons**
- Style: skeuomorphic (`btn-classic` class layer)
- Variants: `default`, `outline`, `secondary`, `ghost`, `destructive`, `link`
- Never override button colors with `className`

**Border Radius**
- Buttons/inputs: `--radius-md`
- Cards: `--radius-lg`
- Dialogs: `--radius-xl`

**Motion**
- Animate `transform` and `opacity` only
- Always include `prefers-reduced-motion` support

**Colors**
- Use OKLCH semantic tokens only
- Never use raw Tailwind color values (`neutral-200`, `white`, etc.)
- Dark mode is handled automatically through tokens — never add `dark:` overrides for colors

---

## Installing This Skill

```bash
npx skills add 0xchsh/agent-skills@shincn -g
```

Browse at: https://skills.sh/0xchsh/agent-skills/shincn
