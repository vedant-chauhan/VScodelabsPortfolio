# What we still need before launch

The site is fully built and working with placeholder content. Everything below is something only you can
provide or decide. Each item names the file it goes into.

**Priority:** 🔴 needed before launch · 🟡 strongly recommended · ⚪ nice to have

---

## 1. Brand & domain

- [x] **Studio / company name:** *"VSCodesLabs"*, monogram "V/S". → `site.ts` (`name`, `monogram`)
- [ ] 🔴 **Domain name** (e.g. `yourstudio.com` / `.in`). → `NEXT_PUBLIC_SITE_URL` env variable
- [ ] 🟡 **Logo** as SVG (plus a square version for the favicon). → `components/layout/logo.tsx`, `app/icon.svg`
- [ ] ⚪ **Brand colour**, if you have one. Default is blue `#2f54eb`. → `app/globals.css`
- [ ] ⚪ **Tagline / headline** you're happy with. → `site.ts`, `components/sections/hero.tsx`

## 2. Contact details

- [x] **Email:** `vscodeslabs@gmail.com`, also where contact-form enquiries are delivered. → `site.ts` (`email`)
  - [ ] ⚪ Once you have a domain, an address on it (e.g. `hello@yourdomain.com`) looks more established
- [ ] 🔴 **WhatsApp number** (is either phone number on WhatsApp?). Turns on the floating "Chat on WhatsApp" button (it stays hidden until set). → `site.ts` (`whatsapp`, and optionally edit `whatsappMessage`)
- [x] **Phone numbers:** +91 91048 39400 and +91 91735 08734, shown on the contact and Hire pages and in the
  footer. → `site.ts` (`phones`)
- [ ] 🟡 **City / location** to display. → `site.ts`
- [ ] 🟡 **Company profiles:** LinkedIn, GitHub, Instagram, Upwork/Fiverr/Clutch if you use them. → `site.ts`
- [ ] 🟡 **Budget ranges and currency** for the contact form (currently in ₹). → `site.ts` (`budgets`)
- [ ] ⚪ **Reply time** promise (currently "within one working day"). → `site.ts`

## 3. The partners (Shantanu & Vedant)

Each partner has a card on the home and About pages and a full profile page at `/team/<name>`.
These are what make clients trust you, so they matter as much as the projects.
For **each** partner → `src/data/team.ts`, photos in `public/images/team/`:

- [x] **Portrait**, 4:5 portrait orientation, at least 1000×1250, face in the upper third
  - [x] Shantanu: the beach photo (also cropped to a round avatar)
  - [x] Vedant: the scooter photo (also cropped to a round avatar)
- [ ] 🔴 **Title / role**, e.g. "Co-founder · Product", "Co-founder · Delivery & Data"
- [ ] 🔴 **Headline**: one line on what you're known for. The current placeholders call you both "full-stack
  engineers", which doesn't match a product-management (Shantanu) and data analyst and co-founder (Vedant) path,
  so please replace them
- [ ] 🔴 **Short bio**, 1–2 sentences, for the cards
- [ ] 🔴 **Your story**, 2–3 short paragraphs in first person, for the profile page
- [ ] 🔴 **Technologies** you use most (the "Technologies" group is empty right now)
- [x] **Career history**: both partners' real roles, graduation and projects are in, plotted on each profile and
  in the About page's "Our journey" chart. → `career` in `src/data/team.ts`
  - [ ] ⚪ **University and branch** for each B.Tech (e.g. "B.Tech, Computer Engineering · XYZ University"),
    so the graduation entry names where you studied
  - [ ] ⚪ Anything else worth adding: internships, certifications (with a verification link), or freelance work
  - [x] Shantanu's timeline photos (beside his current role)
  - [x] Vedant's timeline photos: the terrace and Holi photos beside his Co-founder entry (the crowd behind him in
    the Holi photo was removed)
  - [ ] ⚪ Optional short captions for any of the photos (e.g. "Weekend ride")
- [ ] 🟡 **Years of experience** (shown as "N+ years experience"). Your career history starts in 2024, so "2+"
  would be accurate today
- [ ] 🟡 **LinkedIn and GitHub URLs**, plus a direct email if you want one
- [ ] 🟡 **City**, and the **languages** you speak with clients (e.g. English, Hindi, Gujarati)
- [x] **Which projects each of you worked on**: both of you on all four, matching your career history.
  → `team` in `src/data/projects.ts`
- [ ] ⚪ **A personal quote**: one line on how you work with clients
- [ ] ⚪ **The studio story** for the About page: how you met, when you started, why clients pick you

When you run `npm run dev`, dashed "To do" notes show where content is missing. They never appear on the live site.

## 4. Projects

For **every** project → `projects.ts`, screenshots in `public/images/projects/`:

- [ ] 🔴 **Client permission** to show the name, logo and screenshots
- [ ] 🔴 **Tech stack actually used** (the "Built with" section stays hidden until filled)
- [ ] 🔴 **Check the features listed.** They're written from your one-line descriptions, so fix anything inaccurate
- [ ] 🟡 **3–5 screenshots**, 1600×1000 (desktop) plus 1–2 mobile; blur any real customer data
- [ ] 🟡 **Year** and **your exact role** (design? backend? hosting? maintenance?)
- [ ] 🟡 **Results, with numbers if possible**: orders processed, businesses onboarded, hours saved, uptime, page speed
- [ ] 🟡 **A client quote** (name, title, company, 1–2 sentences, with permission). Add it to `src/data/testimonials.ts`.
  The testimonials section appears automatically on the home page, the About page and that project's case study.

Project-specific questions:

| Project | What's missing |
| --- | --- |
| **Apparel 60** | Stack, launch year, payment gateway used, anything notable you built (size guides, COD, coupons, admin features) |
| **Appointment SaaS** | 🔴 **Product name** and whether it's live; public URL or demo link; who uses it (salons, clinics…?); how many businesses are on it; pricing/subscription model |
| **Diamond Polymer** | 🔴 Permission to name the client; is it web, desktop or mobile; modules beyond inventory → production → shipments (billing? GST invoices? reports?) |
| **MV Bot** | 🔴 **What it does and for whom**; where it runs (website, WhatsApp, Telegram…); what AI/model it uses; what "MV" stands for; demo link if possible |

- [ ] 🟡 **Other projects.** You mentioned many other live projects with various tools. List them
  (name · type · one line · link if public) and they'll appear under "Beyond the case studies".
  → `otherWork` in `projects.ts`

## 5. Services & business policies

- [ ] 🔴 **Tech stack you actually use.** The current list is a starter guess. → `company.ts` (`techStack`)
- [ ] 🟡 **Numbers in "Why clients work with us"** (currently 4+ products, 6 services, 2 partners, <24h reply).
  Swap in stronger *true* numbers when you have them: years in business, projects delivered, clients served. → `company.ts` (`stats`)
- [ ] 🟡 **Confirm the six services** and their deliverables (remove anything you don't want to sell). → `services.ts`
- [ ] 🟡 **Hire page terms**: the three ways to work together (fixed-scope project, monthly retainer, maintenance)
  and the "what happens next" steps (reply, discovery call, proposal, kick-off). Remove or reword anything you
  don't offer. → `src/data/hire.ts`
- [ ] 🟡 **FAQ answers.** These are policy statements (code ownership, timelines, maintenance), so confirm each is true for you. → `company.ts` (`faqs`)
- [ ] ⚪ **Pricing**: starting prices or packages (e.g. "Websites from ₹X", "Maintenance from ₹Y/month") if you want to show them
- [ ] ⚪ **Maintenance plan tiers**, if you offer fixed monthly plans

## 6. Accounts & setup (one-time)

- [ ] 🔴 **GitHub repository** for the code
- [ ] 🔴 **Hosting account**: Vercel (free) recommended
- [ ] 🔴 **Resend account** + verified domain + API key, so the contact form can email you (see README)
- [ ] 🔴 **Domain DNS access** at your registrar (GoDaddy, Hostinger, Namecheap, etc.)
- [ ] 🟡 **Google Search Console**: verify the domain and submit the sitemap
- [ ] 🟡 **Analytics**: pick one (Vercel Analytics, Google Analytics 4, or Plausible)
- [ ] ⚪ **Google Business Profile**, if you want to show up in local searches

## 7. Legal

- [ ] 🟡 **Privacy policy page.** The contact form collects names, emails and phone numbers, so you need one,
  especially under India's DPDP Act and for any EU/UK clients. Can be added as `/privacy`.
- [ ] ⚪ **Terms of service / engagement terms**, if you want them public
- [ ] ⚪ **Registered business details** (GSTIN, legal entity name) for the footer, if applicable

---

## Optional additions we can build next

Tell us which of these you want:

- **Book a call** button with Calendly / Cal.com embedded on the contact page
- **Blog / insights** section for SEO
- **Pricing** page or packages
- **Screenshot gallery** on each case study
- **Hindi / Gujarati** language versions
