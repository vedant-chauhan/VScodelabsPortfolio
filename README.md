# Studio portfolio website

Portfolio and lead-generation site for the studio run by **Shantanu Kyada** and **Vedant Chauhan**.
It presents your services, case studies (Apparel 60, Appointment SaaS, Diamond Polymer, MV Bot), the team,
and a contact form that emails enquiries to you.

> **Before launch:** work through [`CONTENT_CHECKLIST.md`](./CONTENT_CHECKLIST.md). It lists every
> placeholder and every decision still needed from you.

## Tech stack

| Layer | Choice |
| --- | --- |
| Framework | [Next.js 16](https://nextjs.org) (App Router, Cache Components, TypeScript) |
| Styling | Tailwind CSS v4, design tokens in `src/app/globals.css` (automatic light & dark mode) |
| Icons | lucide-react |
| Fonts | Geist Sans and Geist Mono via `next/font` |
| Contact form | Server Action and [Resend](https://resend.com) email API (no extra SDK) |
| Hosting | Vercel recommended; any Node.js 20+ host works |

## Getting started

Requires **Node.js 20.9 or newer**.

```bash
npm install
```

```bash
cp .env.example .env.local
```

```bash
npm run dev
```

Open http://localhost:3000. In development the contact form works without email keys: submissions are
printed to the terminal.

| Command | What it does |
| --- | --- |
| `npm run dev` | Development server with hot reload |
| `npm run build` | Production build (every page is prerendered as static HTML) |
| `npm start` | Serve the production build |
| `npm run lint` | ESLint |
| `npm run typecheck` | Generates Next's route types (`next typegen`), then type-checks with `tsc` |

## Project structure

```
portfolio/
├── CONTENT_CHECKLIST.md        ← everything we still need from you
├── CLAUDE.md                   ← rules and technical notes for Claude (loaded automatically by Claude Code)
├── docs/
│   ├── claude-kickoff-prompt.md  ← paste into Claude Code to continue development from this code
│   └── website-build-prompt.md   ← full spec to rebuild the site from scratch
├── .env.example                ← environment variables to copy into .env.local
├── public/
│   └── images/
│       ├── projects/           ← project screenshots (1600×1000)
│       └── team/               ← partner portraits, avatar crops and timeline photos
└── src/
    ├── data/                   ← ALL CONTENT LIVES HERE
    │   ├── site.ts             ← name, contact details, socials, nav, budget ranges
    │   ├── projects.ts         ← case studies + "other work" list
    │   ├── services.ts         ← services and deliverables
    │   ├── team.ts             ← partner profiles
    │   ├── testimonials.ts     ← client quotes (empty until you add some)
    │   └── company.ts          ← highlights, process, tech stack, values, FAQ
    ├── types/content.ts        ← shape of the content above
    ├── app/                    ← pages (one folder = one URL)
    │   ├── layout.tsx          ← header, footer, fonts, default SEO
    │   ├── page.tsx            ← /            home
    │   ├── services/           ← /services
    │   ├── projects/           ← /projects and /projects/[slug] case studies
    │   ├── team/[slug]/        ← partner profile pages (/team/shantanu-kyada, /team/vedant-chauhan)
    │   ├── hire/               ← Hire pages: /hire (studio) and /hire/[slug] (one partner)
    │   ├── about/              ← /about
    │   ├── contact/            ← /contact (+ actions.ts: form handler)
    │   ├── not-found.tsx       ← 404 page
    │   ├── sitemap.ts          ← /sitemap.xml
    │   ├── robots.ts           ← /robots.txt
    │   ├── opengraph-image.tsx ← link-preview image for WhatsApp/LinkedIn/X
    │   ├── icon.svg            ← favicon
    │   └── globals.css         ← colours and theme tokens
    ├── components/
    │   ├── layout/             ← header, mobile nav, footer, logo, WhatsApp button
    │   ├── sections/           ← hero, services, featured work, why us, founders, testimonials, process, stack, FAQ, CTA
    │   ├── motion/             ← reveal observer, count-up, marquee
    │   ├── projects/           ← project card and cover illustration
    │   ├── contact/            ← contact form (client component) and the Hire button
    │   ├── hire/               ← the full Hire page layout
    │   └── ui/                 ← buttons, containers, headings, icons
    └── lib/utils.ts
```

## Editing content

You should rarely need to touch components. Edit the files in `src/data/`:

| To change… | Edit |
| --- | --- |
| Studio name, email, phone, WhatsApp, socials | `src/data/site.ts` |
| WhatsApp chat button and its pre-filled message | `whatsapp` / `whatsappMessage` in `src/data/site.ts` (button is hidden while the number is empty) |
| Add, remove or reorder a project | `src/data/projects.ts` (`featured: true` shows it on the home page) |
| Smaller projects without a case study | `otherWork` in `src/data/projects.ts` |
| Services and deliverables | `src/data/services.ts` |
| Partner profiles (bio, story, expertise, experience, education, certifications, links) | `src/data/team.ts` |
| Hire page: ways to work together and "what happens next" | `src/data/hire.ts` |
| Which partner worked on which project | `team` on each project in `src/data/projects.ts` |
| Client testimonials | `src/data/testimonials.ts` (section hidden until you add one) |
| Animated numbers in "Why clients work with us" | `stats` in `src/data/company.ts` |
| Scrolling words under the hero | `marquee` in `src/data/company.ts` |
| Tech stack, process, FAQ, values | `src/data/company.ts` |
| Colours (light & dark) | CSS variables at the top of `src/app/globals.css` |
| Home page headline | `src/components/sections/hero.tsx` |

**Adding a project:** copy an existing entry in `projects.ts`, give it a unique `slug`, and it gets a card
and a page at `/projects/<slug>` automatically. It is also added to the sitemap.

**Adding screenshots:** put the image in `public/images/projects/` and set `cover.image` in the project, e.g.
`image: "/images/projects/apparel-60.jpg"`. Until then a drawn illustration in the project's colours is shown.

**Adding photos:** put 4:5 portraits (at least 1000×1250, face in the upper third) in `public/images/team/` and set
`photo` in `team.ts`. The same image is shown as a 5:4 card and, on phones, a 4:3 crop, both kept on the face. For the
small round avatars, add a square head-and-shoulders crop (at least 256×256) as `avatar`; without one, the avatar
uses `photo`. Shantanu's portrait and avatar are cropped from his beach photo, Vedant's from his scooter photo.
A styled initials card is shown until then.

**Missing-content notes:** while running `npm run dev`, dashed "To do" boxes point to content that's still
missing (testimonials, work history, education, technologies). They are never rendered in a production build.

Empty optional fields (phone numbers, WhatsApp, social links, project stack, year, client) are hidden automatically,
so you can fill them in gradually.

## Animations

Most motion is plain CSS. Scroll-scrubbed effects use **GSAP** (with ScrollTrigger; free for commercial
use) and wheel scrolling is smoothed by **Lenis**. Both load only on the client and switch off for
reduced-motion visitors.

| Effect | Where | How |
| --- | --- | --- |
| Staggered entrance | Hero, page headers, profile intro | `motion-safe:animate-rise` with `delay()` from `src/lib/motion.ts` |
| Floating project covers and drifting glow | Hero | `animate-float`, `animate-drift` |
| Hand-drawn underline | Hero headline | SVG stroke animation (`draw` keyframe) |
| Scrolling ticker | Under the hero | `components/motion/marquee.tsx` (pauses on hover) |
| Scroll reveal | Headings, cards, lists, CTA | Wrap content in `<Reveal>` (`variant="fade"` / `"scale"`, `delay={ms}` to stagger, `as="li"` etc.) |
| Lines that draw in | Process steps, values | A child with `data-draw` inside a `<Reveal>` |
| Count-up numbers | "Why clients work with us" | `components/motion/count-up.tsx` |
| Hover micro-interactions | Cards, icons, buttons, portraits | Tailwind transitions |
| Click ripple | Every click with a mouse or pen (not touch, not reduced motion) | `components/motion/pointer-effects.tsx` + `.click-ripple` in `globals.css`. Full accent on links and buttons, softer elsewhere |
| Magnetic buttons | Every button, the process-deck arrows, the WhatsApp button | The `magnetic` class (in `buttonClasses`): leans up to 6px toward the cursor and presses in on click |
| Underline sweep | Nav, footer, contact and inline text links | The `link-fx` class: draws in from the left, slips out to the right; the current page stays underlined |
| Arrow nudge | Any arrow or chevron icon inside a link or button | CSS in `globals.css` (`@layer base`), so components with their own arrow motion keep it |
| Words rising out of a mask | Section headings, page titles | `components/ui/split-words.tsx` + `.word` styles |
| Shimmering accent text | Hero and Hire headlines | `.text-shimmer` |
| Cursor spotlight | Cards with the `spotlight` class | `components/motion/spotlight-tracker.tsx` |
| Fanned project covers | "Why us" tiles | `.fan-card` |
| Page transitions and morphs | Every navigation; project covers and portraits morph card → page | React `<ViewTransition>` |
| Light/dark theme reveal | Header toggle | `components/layout/theme-toggle.tsx` (View Transitions API) |
| Plotted career chart and timeline | Profiles, About | `components/career/*` |
| Smooth, inertial scrolling | Every page (mouse wheel/trackpad; touch stays native) | `components/motion/smooth-scroll.tsx` (Lenis, synced with GSAP) |
| Pinned services wheel | Home, desktop | `components/sections/services-wheel.tsx`: service names roll through a sticky frame; the active one turns accent with its details and a project beside it. Click a name to jump to it |
| Interactive process deck | Home, Services | `components/sections/process-stack.tsx`: the steps form a deck of cards. Click or tap the front card (or swipe it on phones) and it drops away, slips behind the deck and settles at the back while the others step forward, cycling endlessly. Arrows, dots and the step list jump around; a live region announces each step |
| Hero mouse parallax | Home hero, desktop | `components/motion/hero-parallax.tsx`: elements with `data-depth` follow the cursor |
| Two-tone headings | Section titles | `SectionHeading` `title` (bold lead) + `titleMuted` (muted continuation) |

Keyframes live in the `@theme` block in `src/app/globals.css`. `<Reveal>` (`src/components/motion/reveal.tsx`)
shares one IntersectionObserver across the page and only updates itself after it has hydrated, so server and
client HTML always match.

Accessibility and safety: visitors with *reduce motion* switched on get no animation. Content is never hidden
without JavaScript, because an inline script only turns reveals on once JavaScript is running, with a 3-second
fallback. Count-up numbers render their final value on the server.

### GSAP and Lenis notes

- `src/lib/gsap.ts` registers the plugins once; import `gsap`, `ScrollTrigger` and `useGSAP` from there.
- Wrap effects in `gsap.matchMedia()` with `MOTION_OK`, so they never run for reduced-motion visitors and clean
  up automatically.
- Pin with CSS `position: sticky` and let GSAP only *scrub* animations (no ScrollTrigger `pin`), so React's DOM
  is never wrapped or moved.
- The services wheel shows only on large screens when JavaScript and motion are on (`.services-wheel` in
  `globals.css`). Phones, reduced-motion visitors and no-JavaScript visitors get the card grid.
- Anchor jumps use `scroll-padding-top` on `<html>`, which both the browser and Lenis respect. Don't add
  `scroll-mt-*` to sections.
- Add `data-lenis-prevent` to any element that should keep native wheel scrolling (for example a scrollable panel).

## Light and dark mode

The site follows the visitor's device setting until they use the sun/moon toggle in the header (or in the mobile
menu). Their choice is saved in `localStorage` and applied before the page paints, so there's no flash. Switching
animates the sun into a moon and spreads the new theme out in a circle from the button. Visitors who prefer
reduced motion get an instant switch.

Colours for both themes are CSS variables at the top of `src/app/globals.css`. Light values are in `:root`;
dark values are repeated in the `prefers-color-scheme: dark` block and in `:root[data-theme="dark"]`, so keep
those two in sync.

## Career timelines

Each partner's `career` list in `src/data/team.ts` is plotted in three places:

- **Profile page, "At a glance":** a chart on a years axis. Roles are bars, single dates are markers (a
  graduation is a dot, milestones are diamonds, certifications are rings), and a dashed line marks today. Bars
  grow in and markers pop in as you scroll.
- **Profile page, "The story":** a zig-zag timeline, newest first, with cards sliding in from alternating sides.
  Each role lists the projects worked on as links to their case studies.
- **About page, "Our journey":** both partners on one shared axis.

An entry looks like this:

```ts
{
  kind: "work",                     // education | work | freelance | founder | milestone | certification
  title: "Product Manager",
  organization: "GNWebSoft Pvt. Ltd.",
  start: "2026",                    // "YYYY" or "YYYY-MM"
  // end: "2027",                   // leave out while it's ongoing ("Present")
  projects: ["apparel-60", "diamond-polymer", "appointment-saas"], // case-study slugs
}
{ kind: "education", title: "Graduated with a B.Tech", start: "2025", point: true } // a single date
```

**Photos:** add `photos: [{ src, alt, caption? }]` to an entry and they appear beside it in the timeline
as tilted polaroids (under the card on phones), never in the chart. Use 4:5 crops in `public/images/team/`, and
strip location data from phone photos first: iPhone HEIC files usually carry GPS coordinates. Shantanu's two
photos are attached to his current role.

Milestones and certifications are always single dates; set `point: true` for anything else that is one (like a
graduation year with no start year). Entries marked `sample: true` are placeholders: visible (with a "Sample"
badge) in development, never in production.

## Hire pages

`/hire` is the main conversion page. Every "Hire us" / "Start a project" button links to it, and each partner has
their own version at `/hire/<slug>`: the founder cards' "Hire Shantanu" / "Hire Vedant" buttons and the profile
pages' "Hire me" button link there. Each page has:

1. A hero with the founders (or one partner) and direct contact details.
2. Step 1, a "Who would you like to hire?" picker (Both of us / each partner) that switches between the pages
   without losing your scroll position.
3. Step 2, the project brief form beside a "What happens next" timeline.
4. Ways to work together, recent work, testimonials (once added) and the FAQ.

The chosen partner is sent with the enquiry: the email subject reads "Hire request for <name>", and if that
partner has a direct `email` in `team.ts` they're added as a recipient. To add a Hire button anywhere:
`<HireButton />` (studio) or `<HireButton partner="shantanu-kyada" />`.

## Contact form (email delivery)

The form in `src/app/contact/actions.ts` validates input, blocks simple spam bots with a hidden honeypot field,
and sends the enquiry to you through Resend.

1. Create a free account at https://resend.com and verify your domain.
2. Create an API key.
3. Set these environment variables (locally in `.env.local`, in production in your host's settings):
   - `RESEND_API_KEY`: the key
   - `CONTACT_TO_EMAIL` (optional): where enquiries go, comma-separated for several. Defaults to the site email,
     `vscodeslabs@gmail.com`
   - `CONTACT_FROM_EMAIL`: sender on your verified domain, e.g. `Website <website@yourdomain.com>`

If `RESEND_API_KEY` is missing in production, the form shows an error with your email address instead of
silently dropping the message.

## Deploying

**Vercel (recommended, free tier is enough):**

1. Push this folder to a GitHub repository.
2. Import the repository at https://vercel.com/new.
3. Add the environment variables from `.env.example`, with `NEXT_PUBLIC_SITE_URL` set to your real domain.
4. Add your domain under *Project → Settings → Domains* and update DNS at your registrar.

**Any other Node host (VPS, Render, Railway, etc.):** run `npm run build` and then `npm start`. The app
listens on port 3000 by default (`PORT` to change).

After going live, submit `https://yourdomain.com/sitemap.xml` in Google Search Console.

## Notes for developers

- This is Next.js 16 with `cacheComponents` enabled. Pages are prerendered; anything time-based or
  request-based must be cached (`"use cache"`) or wrapped in `<Suspense>`. See the footer's `CopyrightYear`.
- `AGENTS.md` is generated by Next.js for AI coding tools; leave it in place. `CLAUDE.md` imports it and adds
  this project's rules and traps for Claude.
- **Handing the project to Claude:** open the folder in Claude Code and paste the prompt from
  `docs/claude-kickoff-prompt.md`. Keep `CLAUDE.md` up to date when conventions change.
- Bundled framework docs for this exact version live in `node_modules/next/dist/docs/`.
