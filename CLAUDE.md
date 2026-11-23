@AGENTS.md

# VSCodesLabs portfolio: notes for Claude

The website of **VSCodesLabs**, a two-partner software studio run by **Shantanu Kyada** and **Vedant Chauhan**.
It sells their services (e-commerce, SaaS, custom business software, AI chatbots, websites, maintenance), shows
their case studies (Apparel 60, Appointment SaaS, Diamond Polymer, MV Bot) and builds trust in the two founders
(profile pages, career timelines, a Hire page per partner). The site is complete and working; what's left is mostly
real content and launch setup, tracked in `CONTENT_CHECKLIST.md`.

Read `README.md` for the full guide (structure, editing content, animations table, deployment). This file is the
short list of rules and traps.

## How the owners want you to work

- **Ask before any major change**: new sections or pages, redesigns, new animations or effects, new libraries,
  removing features. Small fixes and content edits they've given you are fine. When an idea comes from a reference
  site, only use it if it suits this brand, and ask first.
- **Never invent facts.** No made-up statistics, clients, testimonials, dates, years of experience, career
  entries, technologies or results. Where real data is missing, write neutral copy and leave a `// TODO:` in the
  data file. Placeholder career entries must carry `sample: true` (shown only in development).
- Brand name is **VSCodesLabs** everywhere (monogram "V/S"). Spell it "Diamond Polymer", "e-commerce", "B.Tech".
- Effects must stay **elegant and professional**: thin lines, short distances, soft easing. No custom cursor.
- **Photos:** remove all metadata (EXIF/GPS) before adding any photo; iPhone HEIC files carry GPS coordinates.
  Never print dates on photos. Don't re-render the marketing screenshots unless asked.
- Keep **`README.md`, `CONTENT_CHECKLIST.md` and `docs/website-build-prompt.md`** in sync with every change
  (the prompt is a from-scratch rebuild spec of the whole site).
- Personal details (emails, phone numbers) stay private: never send them to outside services.
- Commit each finished piece of work with a clear message. Never push, deploy or change DNS/accounts without asking.

## Commands

```bash
npm install
npm run dev          # http://localhost:3000
npm run lint
npm run typecheck    # next typegen (route types like PageProps) + tsc; plain tsc fails on a fresh checkout
npm run build && npm start
npx --yes prettier@3.6.2 --print-width 120 --write <files>   # formatting used throughout
```

Node 20.9 or newer. Environment variables: see `.env.example` (copy to `.env.local`).

## Where things live

- **Content is data, not markup:** `src/data/` holds `site.ts` (name, contact, WhatsApp, budgets),
  `team.ts` (partners, portraits, `career`), `projects.ts`, `services.ts`, `company.ts` (stats, process, FAQ,
  tech stack), `hire.ts`, `testimonials.ts`. Types are in `src/types/content.ts`. Empty optional fields hide
  their UI automatically.
- Pages: `src/app/` (home, services, projects/[slug], about, team/[slug], hire, hire/[slug], contact).
- Components: `src/components/{layout,sections,motion,career,hire,projects,contact,ui}`.
- Design tokens (colours, light/dark, motion) and all custom CSS: `src/app/globals.css`.
- Images: `public/images/team/` (portraits, avatar crops, timeline photos), `public/images/projects/`.

## Stack and traps

- **Next.js 16.4.6 App Router, React 19.3, TypeScript, Tailwind CSS 4.3 (CSS-first), GSAP 3.15 + ScrollTrigger,
  Lenis 1.3, lucide-react.** This Next version differs from older training data: read
  `node_modules/next/dist/docs/` before using an API you're unsure of.
- `cacheComponents` is on and pages are prerendered. `new Date()` and other time/request values must sit in a
  `"use cache"` function (see `now()` in `src/lib/career.ts`, the footer's `CopyrightYear`). Dynamic route params
  are read inside `<Suspense>` with `generateStaticParams`.
- `next/image`: `priority` is deprecated; use `loading="eager"` + `fetchPriority="high"` for above-the-fold images.
- **Theme:** tokens are defined three times in `globals.css` (`:root`, the dark media query guarded by
  `:root:not([data-theme="light"])`, and `:root[data-theme="dark"]`). Add new colours to all three. An inline
  `bootScript` (`src/lib/motion.ts`) sets `data-theme` and `data-motion="on"` before paint.
- **Tailwind v4 layers:** utilities are layered, so unlayered CSS in `globals.css` overrides them. `.magnetic`
  and `.link-fx` deliberately set the whole `transition` list for that reason.
- **Reveal on scroll:** wrap content in `<Reveal>` (`variant`, `delay`, `as`, `margin`). Children animate with
  `data-draw` (lines/bars), `data-pop` (markers) and `data-settle` (photos). Reveal drops its attributes 1s after
  entering, so keep child delay + duration under that. Content at the very end of a page needs `margin="0px"`.
  Never hide above-the-fold content behind a reveal.
- **GSAP:** import from `src/lib/gsap.ts`; wrap effects in `gsap.matchMedia()` with `MOTION_OK`; pin with CSS
  `position: sticky` (never ScrollTrigger `pin`); in event handlers use `context.add(() => …)`, not `contextSafe`
  during render (the react-hooks lint rule fails).
- **Lenis smooth scroll** owns scrolling: anchors rely on `scroll-padding-top` on `<html>` (don't add
  `scroll-mt-*`), `data-lenis-prevent` keeps native scrolling, and `window.scrollTo` is overridden (in browser
  tests, navigate to `#anchor` URLs or use the mouse wheel instead).
- **View transitions:** pages fade (`.page`), project covers and portraits morph (`.morph`), the theme switch is a
  circular reveal, and the click-ripple layer has its own `view-transition-name`.
- **Pointer effects** (`components/motion/pointer-effects.tsx` + `globals.css`): click ripple, magnetic buttons
  (`magnetic` is in `buttonClasses`), `.link-fx` underline sweep for plain text links, and an arrow nudge in
  `@layer base`. All are mouse/pen only and off for touch, keyboard and reduced motion.
- **Career data** (`career` in `team.ts`): `point: true` for single dates, `projects` (case-study slugs, shown as
  chips), `photos` (polaroids beside the entry, never in the chart). `visibleCareer` hides `sample` entries in
  production.
- Partner `photo` is a 4:5 portrait with the face in the upper third (also shown as 5:4 and 4:3 crops, positioned
  at 50% 20%); `avatar` is a square head-and-shoulders crop for small round avatars.

## Before you say something is done

1. `npm run lint`, `npm run typecheck` and `npm run build` pass.
2. Check the changed pages in a real browser (Playwright and Chromium work): desktop and phone widths (1280, 390,
   360), light and dark mode, no horizontal overflow, nothing left invisible after scrolling (`[data-reveal]`
   without `data-revealed`), and with reduced motion everything still shows.
3. Run `npm run dev` once and confirm the browser console has no errors or hydration warnings.
4. Update the docs listed above. Say exactly what you checked, and report anything that failed with its output.
