---
name: implement-frontend
description: Implement frontend in uc-frontend-nextjs-v1/ or uc-ats-react-app/ under parent workspace. MUI + sx. frontend-coding-standards auto via globs; MUST load mui-frontend skill for MUI UI work. Self-review via pnpm lint/typecheck/build.
---

# Implement Frontend — MUI + sx

Implements frontend following **existing MUI patterns** in the repo. **Self-review** after — no `testing` agent.

**Repos:** `uc-frontend-nextjs-v1/`, `uc-ats-react-app/` only. For `ucl-web/` use **`implement-ucl-web`** — do not use this skill.

## Cursor rules (auto via globs)

When editing `{uc-frontend-nextjs-v1,uc-ats-react-app}/**/*.{ts,tsx,js,jsx}`, Cursor injects:

- `.cursor/rules/frontend-coding-standards.mdc`

No need to read this rule file manually — globs handle activation.

## Required skill — `mui-frontend`

**MUST** read and follow [`.cursor/skills/mui-frontend/SKILL.md`](../mui-frontend/SKILL.md) **before** writing or editing MUI UI code in this skill's repos.

Load `mui-frontend` when the task touches any of:

- MUI components (`Box`, `Stack`, `Button`, `TextField`, `Dialog`, `Table`, `Card`, …)
- layout, spacing, responsive UI
- forms, dialogs, tables, lists, cards, surfaces
- theme, `sx`, `styled()`, breakpoints
- design consistency with nearby screens

Also read `mui-frontend` resources before implement/review:

- `mui-frontend/resources/styling-guide.md`
- `mui-frontend/resources/component-library.md`
- `mui-frontend/resources/theme-customization.md`

**Do not** implement MUI UI from memory or generic React patterns alone — `mui-frontend` is the styling/component source of truth for these repos.

## Input

- **task_summary**, **task_description**, **acceptance_criteria** (pasted in prompt — no Jira fetch)
- **affected_files**, **design_context** (optional)

## Process

0. **API map** — Read `docs/contracts/frontend-api-map.md` and relevant `docs/domains/<module>/API.md` before new API calls.
1. **Explore (Codegraph)** — `codegraph_explore` for hooks, API clients, components (e.g. `"useApplications ApplicationForm"`). `codegraph_callers` before changing shared hooks. `codegraph_node` with `file` for indexed `.tsx` instead of blind `Read`. Policy: `uctalent-workflow.mdc` §4.2.
2. **Read existing UI** — Same module: MUI components, `ThemeProvider`, `sx` vs `styled`, layout patterns
3. **`mui-frontend` (required for UI)** — Read skill + resources; pick correct MUI components; use `sx` consistently with neighbors
4. **nextjs-best-practices** — Only in **`uc-frontend-nextjs-v1`** (not `uc-ats-react-app`)
5. **Implement** — Surgical changes; TypeScript; a11y (`aria-*`, labels on inputs)
6. **Map to AC** — Each acceptance criterion traceable to code/UI
7. **Self-review**
   ```bash
   pnpm lint && pnpm typecheck && pnpm build
   ```
8. **Karpathy** — No new UI libraries; no shadcn/Tailwind-only components unless file already uses them

## MUI conventions

```tsx
import { Box, Button, TextField, Typography } from '@mui/material';

export function Example() {
  return (
    <Box sx={{ display: 'flex', flexDirection: 'column', gap: 2, p: 2 }}>
      <Typography variant="h6">Title</Typography>
      <TextField label="Email" size="small" fullWidth />
      <Button variant="contained" sx={{ alignSelf: 'flex-start' }}>
        Submit
      </Button>
    </Box>
  );
}
```

- Prefer **`sx`** for one-off layout/spacing (match nearby files)
- Use **`styled()`** when the repo already uses it for shared pieces
- Reuse **theme** tokens: `theme.palette`, `theme.spacing`, breakpoints
- Do **not** add shadcn, Chakra, or raw HTML buttons if MUI is standard in that app

## Output

```json
{
  "task_summary": "...",
  "acceptance_criteria_met": ["AC1", "AC2"],
  "files_created": [],
  "files_modified": [],
  "components_added": [],
  "self_review": "lint/typecheck/build passed",
  "notes": ""
}
```

## Quality

- [ ] `mui-frontend` skill + resources read when task includes MUI UI work
- MUI components for interactive UI
- `sx` / theme aligned with surrounding code
- No `any` in TypeScript
- Coordinate API types with backend when fullstack

## AI assistant rules

1. Do not create `.md`/`.mdc` unless user asks.
2. Ask before `pnpm lint`, `pnpm typecheck`, `pnpm build`.
3. `read_lints` on changed files before complete.

## Not in scope

- shadcn, Playwright E2E, `testing` agent
