---
name: mui-frontend
description: Use this skill for frontend implementation or review in UC Talent React/Next.js projects that use MUI. Trigger for UI, layout, component, responsive, theme, sx, form, dialog, table, card, and design consistency tasks. Do not use for backend or non-MUI work.
---

# MUI Frontend Taste Skill

Use when working in:

| Repo | Stack |
|------|-------|
| `uc-frontend-nextjs-v1` | Next.js + MUI |
| `uc-ats-react-app` | React + MUI |

---

## Resource loading rule

When this skill is used, first inspect the target file/module and then read the relevant resource files before implementing or reviewing.

Always read:

- [styling-guide.md](resources/styling-guide.md)
- [component-library.md](resources/component-library.md)
- [theme-customization.md](resources/theme-customization.md)

Use `SKILL.md` as the main routing rule. Use files in `resources/` as deeper implementation guidance.

---

## Core rules

| Do | Don't |
|----|-------|
| Follow existing project structure | Create new files/folders without checking neighbors |
| Reuse common components first | Build one-off duplicates |
| Use MUI as the primary UI system | Add shadcn / Chakra / raw HTML controls |
| Prefer `sx` for styling | Use inline `style` |
| Use theme tokens | Hardcode colors/sizes unless no token exists |
| Match nearby screens | Over-design or invent new patterns |

**Taste bar:** clean, simple, production-looking UI — not flashy or over-abstracted.

---

## MUI import rules

Prefer path-based imports when the repo/file already uses them:

```ts
import Box from '@mui/material/Box';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import Button from '@mui/material/Button';
```

Avoid barrel imports when the repo already uses path imports:

```ts
import { Box, Stack, Typography, Button } from '@mui/material';
```

Match whichever style the target file and module already use. Consistency within a file beats global preference.

---

## Component rules

- Functional React components only
- TypeScript types — no `any`
- Clear, descriptive prop names
- Keep components small and readable
- Extract reusable UI only when genuinely reused in 2+ places
- Do not abstract too early

---

## Layout

Reach for:

- `Stack`
- `Box`
- `Grid`
- `Container`
- `Card`
- `Paper`

Spacing rhythm:

```tsx
sx={{
  gap: 2,
  p: 2,
  mb: 2,
}}
```

Responsive `sx`:

```tsx
sx={{
  flexDirection: { xs: 'column', md: 'row' },
  gap: { xs: 2, md: 3 },
}}
```

---

## Forms

1. Use existing form patterns in the module first
2. Keep labels, helper text, and errors visible and aligned
3. Controlled state — predictable `value` / `onChange`
4. Reserve space for error text so layout does not jump on validation

---

## Dialogs & modals

- Separate title, content, and actions clearly
- Submit buttons must have loading + disabled states during async work
- Comfortable padding on mobile
- Avoid cramped dialogs

---

## Tables & lists

1. Use existing table/list patterns first
2. Handle loading, empty, error, and pagination states
3. Row actions should be obvious but not noisy
4. Avoid icon-only actions without labels/tooltips when clarity matters

---

## Review checklist

Before finishing frontend work, confirm:

- [ ] UI matches nearby screens
- [ ] Responsive layout works on mobile / tablet / desktop
- [ ] Loading, empty, disabled, and error states handled
- [ ] Imports are clean
- [ ] Path vs barrel imports match file convention
- [ ] No unnecessary new dependencies
- [ ] No backend / DDL / migration changes

---

FILE: .agents/skills/mui-frontend/resources/styling-guide.md

# MUI Styling Guide

## Purpose

Use this resource for MUI styling decisions, especially `sx`, theme tokens, responsive styling, and style organization.

---

## Main rule

Prefer `sx` and theme tokens over raw CSS, inline `style`, or hardcoded values.

Good:

```tsx
<Box
  sx={{
    p: 2,
    color: 'text.primary',
    bgcolor: 'background.paper',
    borderRadius: 2,
  }}
/>
```

Avoid:

```tsx
<Box style={{ padding: '16px', color: '#111' }} />
```

---

## Style organization

For small components, colocate styles near the component:

```tsx
const styles = {
  root: {
    p: 2,
    display: 'flex',
    flexDirection: 'column',
    gap: 2,
  },
};
```

For large components, use a separate `.styles.ts` file only when the file is already becoming hard to read.

Do not create a style file just because there are 3-5 `sx` blocks.

---

## Responsive styling

Use mobile-first responsive values:

```tsx
sx={{
  width: { xs: '100%', md: 420 },
  flexDirection: { xs: 'column', md: 'row' },
  gap: { xs: 2, md: 3 },
}}
```

Avoid desktop-only layout assumptions.

---

## Spacing

Prefer MUI spacing scale:

```tsx
sx={{
  p: 2,
  mt: 1,
  mb: 3,
  gap: 2,
}}
```

Avoid random pixel values:

```tsx
sx={{
  padding: '17px',
  marginBottom: '23px',
}}
```

---

## Colors

Use palette tokens first:

```tsx
sx={{
  color: 'text.secondary',
  bgcolor: 'background.paper',
  borderColor: 'divider',
}}
```

Only use hardcoded colors if the project already does it in nearby code or no theme token exists.

---

## Hover and interaction

Keep interactions subtle:

```tsx
sx={{
  transition: 'background-color 0.2s ease',
  '&:hover': {
    bgcolor: 'action.hover',
  },
}}
```

Avoid flashy hover effects unless existing design already uses them.

---

## Taste checklist

- Use fewer visual effects
- Prefer alignment, spacing, and hierarchy
- Keep typography readable
- Keep action buttons obvious
- Match nearby screens before inventing new styling

---

FILE: .agents/skills/mui-frontend/resources/component-library.md

# MUI Component Library Patterns

## Purpose

Use this resource when choosing MUI components or composing UI.

---

## Layout components

Use:

```tsx
<Box />
<Stack />
<Grid />
<Container />
<Paper />
<Card />
```

Guidance:

- `Box`: generic wrapper
- `Stack`: simple vertical/horizontal layout
- `Grid`: responsive multi-column layout
- `Container`: centered page content
- `Paper`: elevated section
- `Card`: object/entity preview

---

## Page section pattern

```tsx
<Stack spacing={3}>
  <Stack spacing={1}>
    <Typography variant="h5">Title</Typography>
    <Typography variant="body2" color="text.secondary">
      Description
    </Typography>
  </Stack>

  <Paper sx={{ p: 2 }}>
    Content
  </Paper>
</Stack>
```

---

## Card pattern

```tsx
<Card>
  <CardContent>
    <Stack spacing={1}>
      <Typography variant="h6">Card title</Typography>
      <Typography variant="body2" color="text.secondary">
        Card description
      </Typography>
    </Stack>
  </CardContent>
</Card>
```

Rules:

- Keep card hierarchy clear
- Do not overload card with too many actions
- Prefer one primary action and secondary actions as subtle buttons/icons

---

## Button pattern

Use button intent clearly:

```tsx
<Button variant="contained">Primary</Button>
<Button variant="outlined">Secondary</Button>
<Button variant="text">Tertiary</Button>
```

Rules:

- Use `contained` for main action
- Use `outlined` for secondary action
- Use `text` for low-emphasis action
- Async submit buttons must show loading or disabled state

---

## Dialog pattern

```tsx
<Dialog open={open} onClose={onClose} fullWidth maxWidth="sm">
  <DialogTitle>Title</DialogTitle>

  <DialogContent>
    <Stack spacing={2} sx={{ pt: 1 }}>
      Content
    </Stack>
  </DialogContent>

  <DialogActions>
    <Button onClick={onClose}>Cancel</Button>
    <Button variant="contained" disabled={isLoading}>
      Save
    </Button>
  </DialogActions>
</Dialog>
```

Rules:

- Title explains the action
- Content has enough spacing
- Actions are aligned and clear
- Do not put heavy business logic directly inside dialog markup

---

## Form pattern

```tsx
<Box component="form" onSubmit={handleSubmit}>
  <Stack spacing={2}>
    <TextField
      label="Email"
      value={email}
      onChange={handleEmailChange}
      error={Boolean(error)}
      helperText={error || ' '}
      fullWidth
    />

    <Button type="submit" variant="contained" disabled={isSubmitting}>
      Submit
    </Button>
  </Stack>
</Box>
```

Rules:

- Keep helper text stable to prevent layout jump
- Use existing validation pattern
- Keep disabled/loading state clear
- Use fullWidth on mobile-heavy forms

---

## Table/list pattern

Always handle:

- loading
- empty
- error
- pagination if data is paginated

Avoid:

- icon-only actions with no tooltip
- dense tables without spacing
- hidden row actions that are hard to discover

---

FILE: .agents/skills/mui-frontend/resources/theme-customization.md

# MUI Theme Customization

## Purpose

Use this resource when working with project theme tokens, palette, typography, spacing, radius, shadows, and breakpoints.

---

## Theme-first rule

Before adding hardcoded visual values, inspect the existing theme and nearby components.

Prefer:

```tsx
sx={{
  color: 'primary.main',
  bgcolor: 'background.paper',
  borderColor: 'divider',
  borderRadius: 2,
  boxShadow: 1,
}}
```

Avoid:

```tsx
sx={{
  color: '#1976d2',
  backgroundColor: '#ffffff',
  borderRadius: '13px',
}}
```

---

## Palette

Common safe tokens:

```tsx
'primary.main'
'primary.dark'
'secondary.main'
'text.primary'
'text.secondary'
'background.default'
'background.paper'
'divider'
'action.hover'
'error.main'
'success.main'
'warning.main'
```

Use semantic tokens where possible.

---

## Typography

Prefer MUI variants:

```tsx
<Typography variant="h5" />
<Typography variant="subtitle1" />
<Typography variant="body1" />
<Typography variant="body2" />
<Typography variant="caption" />
```

Rules:

- Do not use random font sizes unless needed
- Match nearby typography hierarchy
- Use `fontWeight` sparingly
- Avoid multiple competing headings in one card/section

---

## Shape and radius

Prefer theme radius scale:

```tsx
sx={{
  borderRadius: 1,
  borderRadius: 2,
}}
```

Avoid custom radius unless nearby UI uses it.

---

## Shadows

Use MUI shadow scale:

```tsx
sx={{
  boxShadow: 1,
}}
```

Avoid heavy shadows unless existing design requires them.

---

## Breakpoints

Use responsive object syntax:

```tsx
sx={{
  display: { xs: 'none', md: 'block' },
  px: { xs: 2, md: 3 },
  gridTemplateColumns: {
    xs: '1fr',
    md: 'repeat(2, 1fr)',
  },
}}
```

Rules:

- Mobile first
- Avoid fixed desktop widths on mobile
- Do not hide important actions on mobile unless alternative action exists

---

## Theme changes

Only edit theme files when the task explicitly requires global design changes.

Before editing theme:

1. Check existing theme structure
2. Check whether a local `sx` fix is enough
3. Avoid changing global tokens for one screen only
4. Consider impact across both frontend repos

---

FILE: AGENTS.md append section

## Frontend MUI workflow

For frontend work in `uc-frontend-nextjs-v1` or `uc-ats-react-app`, use the `$mui-frontend` skill before implementing or reviewing UI changes.

When `$mui-frontend` is used, read the skill resources under:

- `.cursor/skills/mui-frontend/resources/styling-guide.md`
- `.cursor/skills/mui-frontend/resources/component-library.md`
- `.cursor/skills/mui-frontend/resources/theme-customization.md`

Frontend work includes MUI components, layout, responsive UI, dialogs, modals, tables, forms, cards, theme/styling, and design consistency.

Do not use this skill for backend, FastAPI, NestJS, migrations, DDL, or infrastructure work.