---
name: implement-strapi-cms
description: Implement Strapi 5 CMS in ucl-web-cms/. Content types, controllers, plugins. Serves ucl-web and uc-frontend-nextjs-v1. Follow strapi-cms rule.
---

# Implement Strapi CMS — ucl-web-cms

Implements **Strapi 5** content management for Unchain Labs and UCTalent.

## Input

- **task_summary**, **task_description**, **acceptance_criteria**
- **consumer_repos** — `ucl-web/` and/or `uc-frontend-nextjs-v1/` when schema affects frontends

## Process

1. **Identify content type** — `src/api/<name>/content-types/<name>/schema.json`
2. **Check consumers** — Which frontend reads this type and how
   - `ucl-web/src/services/cmsService.ts` — UCL marketing content
   - `uc-frontend-nextjs-v1/src/services/strapi*.ts` — UCTalent FAQ, articles, roadmap
3. **Implement schema/controller** — Match Strapi 5 patterns; CKEditor for rich text where existing types use it
4. **CMS contract gate** — Field names, API slugs, populate params must match consumers
5. **Self-review**
   ```bash
   cd ucl-web-cms && pnpm build
   ```

## Content type map

| Type | Consumer | API slug |
|------|----------|----------|
| `uc-talent-article` | `uc-frontend-nextjs-v1` | `uc-talent-articles` |
| `uc-talent-faq` | `uc-frontend-nextjs-v1` | `uc-talent-faqs` |
| `uc-talent-roadmap` | `uc-frontend-nextjs-v1` | `uc-talent-roadmaps` |
| `uc-talent-tos` | `uc-frontend-nextjs-v1` | `uc-talent-tos` |
| `uc-talent-privacy` | `uc-frontend-nextjs-v1` | `uc-talent-privacies` |
| `insight` | `ucl-web` | `insights` |
| `hero-content`, `team-member`, `service`, … | `ucl-web` | plural API slug |

## Conventions

- Strapi 5, PostgreSQL, GCS upload plugin
- Rich text: `plugin::ckeditor5.CKEditor` or `blocks` type (see existing schemas)
- `draftAndPublish: true` on most collection types
- Do not change schema without verifying consumer field mapping
- Do not run DB migrations/DDL unless user explicitly approves

## Output

```json
{
  "task_summary": "...",
  "acceptance_criteria_met": ["AC1"],
  "content_types_changed": [],
  "consumer_impact": ["ucl-web", "uc-frontend-nextjs-v1"],
  "cms_contract_check": "passed | failed | not_applicable",
  "self_review": "build passed | not_run"
}
```
