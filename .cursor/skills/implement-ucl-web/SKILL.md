---
name: implement-ucl-web
description: Implement frontend in ucl-web/ (Unchain Labs site). Next.js 16 App Router, Tailwind 4, shadcn/radix. Consumes Strapi via cmsService. Do NOT use mui-frontend.
---

# Implement UCL Web — Next.js + Tailwind + shadcn

Implements the **Unchain Labs** marketing site at `ucl-web/`. Uses **Tailwind + shadcn**, not MUI.

## Input

- **task_summary**, **task_description**, **acceptance_criteria**
- **affected_files**, **design_context** (optional)

## Process

1. **Read existing UI** — Same route/module: Tailwind classes, shadcn components in `src/components/ui/`, layout in `src/app/`
2. **CMS integration** — If content-driven, check `src/services/cmsService.ts` and Strapi content types in `ucl-web-cms/`
3. **Implement** — Surgical changes; TypeScript; a11y; `next-intl` for i18n when touching localized routes
4. **Map to AC** — Each acceptance criterion traceable to code/UI
5. **Self-review**
   ```bash
   cd ucl-web && pnpm lint && pnpm build
   ```

## Conventions

- **Stack:** Next.js 16 App Router, React 19, Tailwind CSS 4, shadcn/radix, `@tanstack/react-query`, `next-intl`
- **Styling:** Tailwind utility classes + `cn()` from `@/lib/utils`; use existing shadcn components in `src/components/ui/`
- **CMS:** `strapiGet` from `@/lib/strapi`; parsers in `cmsService.ts`; blocks content via `@strapi/blocks-react-renderer`
- **Do NOT** add MUI, Chakra, or unrelated UI libraries
- **Do NOT** use `mui-frontend` skill for this repo

## Strapi content types (UCL)

Consumed from `ucl-web-cms/`: `insights`, `hero-content`, `hero-statistic`, `team-member`, `service`, `innovation`, `innovation-status`, `track-record`, `menu-item`, `tag`

## Output

```json
{
  "task_summary": "...",
  "acceptance_criteria_met": ["AC1"],
  "files_modified": [],
  "self_review": "lint/build passed | not_run",
  "cms_contract_notes": "..."
}
```
