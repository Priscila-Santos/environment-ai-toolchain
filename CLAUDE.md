# CLAUDE.md

Environment and AI development toolchain for the **FlyRank AI Frontend Engineering Internship**. This repository documents how to work in this project and the conventions to follow when building responsive, production-ready web interfaces.

## Project context

- **Goal**: Ship a responsive, mobile-optimized website or ecommerce-style project with clean Tailwind execution.
- **Stack**: React with Next.js, Vite, or Astro; Tailwind CSS; TypeScript preferred.
- **Deployment**: Vercel, Netlify, Cloudflare Pages, or GitHub Pages — the final artifact must be publicly reviewable.
- **AI toolchain**: Cursor (or VS Code + Copilot) with Claude as a pair-programmer. Prefer small, reviewable diffs over large rewrites.

## Commands

Run these from the project root once the app scaffold exists:

```bash
npm install          # Install dependencies
npm run dev          # Start local dev server
npm run build        # Production build
npm run lint         # Lint source files
npm run typecheck    # TypeScript check (if configured)
npm run test         # Run tests (if configured)
```

If a command is missing from `package.json`, check the README or ask before inventing one.

## Directory structure

Organize frontend code by feature, not by file type:

```
src/
  app/              # Routes, layouts, page entry points (Next.js App Router)
  components/
    ui/             # Reusable, stateless primitives (Button, Input, Card)
    features/       # Feature-specific components colocated with their domain
  hooks/            # Custom React hooks
  lib/              # Utilities, API clients, shared helpers
  styles/           # Global styles, Tailwind config extensions
  types/            # Shared TypeScript types and interfaces
public/             # Static assets (images, fonts, favicons)
```

Keep components close to where they are used. Extract to `components/ui/` only when a pattern is reused in two or more places.

## Frontend conventions

### React and TypeScript

- Use **functional components** and hooks. No class components.
- Prefer **named exports** for components; default exports only for Next.js page/layout files.
- Type all props with explicit interfaces (`type` or `interface`). Avoid `any`.
- Colocate component-specific types in the same file or an adjacent `.types.ts` file.
- Extract reusable logic into custom hooks (`useCart`, `useMediaQuery`) rather than bloating components.
- Keep components **small and focused** — one responsibility per file.

### Styling (Tailwind CSS)

- Use **Tailwind utility classes** as the default styling approach.
- Follow a **mobile-first** breakpoint strategy (`sm:`, `md:`, `lg:`).
- Use semantic HTML elements (`nav`, `main`, `section`, `article`, `button`) before reaching for generic `div`s.
- Avoid inline `style` attributes unless animating values Tailwind cannot express.
- Extract repeated utility combinations into `@apply` in CSS modules or component variants only when the pattern appears three or more times.
- Do not mix CSS frameworks. Stick to Tailwind for consistency.

### State and data

- Prefer **local state** (`useState`, `useReducer`) for UI-only concerns.
- Use **React Context** or a lightweight store only when state is shared across distant components.
- Fetch data in server components (Next.js) or dedicated hooks — never scatter `fetch` calls inside presentational components.
- Handle loading, error, and empty states explicitly in every data-dependent view.

### Accessibility (a11y)

- Every interactive element must be keyboard-accessible and have a visible focus state.
- Images require meaningful `alt` text; decorative images use `alt=""`.
- Form inputs must have associated `<label>` elements or `aria-label` attributes.
- Use `aria-live` regions for dynamic content updates (toasts, cart counts).
- Maintain a logical heading hierarchy (`h1` → `h2` → `h3`) — one `h1` per page.
- Test tab order and screen-reader behavior for modals, dropdowns, and mobile menus.

### Performance

- Lazy-load images (`loading="lazy"`) and heavy components (`React.lazy` / dynamic imports).
- Optimize images (WebP/AVIF, appropriate dimensions) before placing in `public/`.
- Avoid unnecessary re-renders: memoize expensive computations with `useMemo`, stable callbacks with `useCallback` only when profiling shows a benefit.
- Prefer static generation or server-side rendering for content pages; reserve client-side rendering for interactive islands.

### Responsive design

- Design and implement **mobile-first**, then enhance for tablet and desktop.
- Test at 320px, 768px, and 1280px minimum widths.
- Touch targets must be at least 44×44px on mobile.
- Avoid horizontal scroll on any viewport.

## Code quality rules

- **Minimize scope**: Make the smallest correct change. Do not refactor unrelated code in the same diff.
- **Match existing patterns**: Read surrounding files before writing. Mirror naming, imports, and file structure.
- **No over-engineering**: Skip abstractions for one-off cases. No premature optimization.
- **Self-documenting code**: Add comments only for non-obvious business logic or tricky browser quirks.
- **No secrets in source**: API keys and tokens belong in environment variables (`.env.local`), never committed.
- **Useful tests only**: Write tests for user-facing behavior and edge cases, not implementation details.

## AI-assisted workflow

When using Claude or Cursor in this project:

1. **Read before writing** — inspect existing components, styles, and config before generating code.
2. **Small diffs** — prefer incremental changes over full-file rewrites.
3. **Explain trade-offs** — when multiple approaches exist (e.g., SSR vs CSR), state the choice and why.
4. **Verify builds** — run `npm run build` and `npm run lint` after substantive changes.
5. **Preserve accessibility** — do not remove ARIA attributes, labels, or focus management to simplify code.
6. **Update this file** — when a repeated mistake is corrected, add a rule here so it does not recur.

## Git conventions

- Write commit messages in the imperative mood: `Add cart summary component`, `Fix mobile nav overflow`.
- One logical change per commit.
- Do not commit `.env`, credentials, or `node_modules`.
- Do not force-push to `main`.

## Capstone checklist

Before submitting the final project for review:

- [ ] Site is deployed to a public URL
- [ ] Responsive on mobile, tablet, and desktop
- [ ] All pages pass a basic accessibility audit (keyboard nav, alt text, contrast)
- [ ] No console errors in production build
- [ ] Lighthouse performance and accessibility scores are reasonable (>80)
- [ ] README documents how to run, build, and deploy the project
