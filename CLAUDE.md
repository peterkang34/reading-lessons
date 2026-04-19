# Reading Lessons — Microsite Collection

A collection of book analysis microsites by Peter Kang. Each microsite pulls highlights from Readwise, organizes them by theme, and applies the lessons to running agencies and building holding companies.

**Live site**: `read.peterkang.com`
**Repo**: `peterkang34/reading-lessons`
**Hosting**: GitHub Pages with custom domain (CNAME → `peterkang34.github.io`, DNS-only on Cloudflare)

## Project Structure

```
reading-lessons/
├── CLAUDE.md                    ← This file
├── CNAME                        ← GitHub Pages custom domain
├── index.html                   ← Library landing page (read.peterkang.com)
├── discount-retail/             ← "The Discount Philosophy" microsite
│   ├── index.html               ← Overview: ALDI + TJ connection
│   ├── bare-essentials-aldi.html ← ALDI book highlights (sidebar layout)
│   ├── becoming-trader-joe.html  ← TJ book highlights (sidebar layout)
│   ├── agency-lessons.html       ← Applied lessons for agencies
│   ├── styles.css                ← Shared CSS for this microsite
│   ├── *.jpg                     ← Book cover images
│   ├── *.md                      ← Research/highlights files
│   └── CLAUDE.md                 ← Microsite-specific docs
└── the-compounders/             ← "The Compounding Playbook" microsite
    ├── index.html               ← Overview page
    ├── highlights.html          ← Full highlights analysis (sidebar layout)
    ├── holdco-lessons.html      ← Applied lessons for holdcos
    ├── agency-lessons.html      ← Applied lessons for agencies
    ├── styles.css               ← Shared CSS (imports discount-retail base)
    ├── *.jpg                    ← Book cover image
    └── *.md                     ← Research/highlights file
```

## Workflow: Creating a New Microsite

### Step 1: Pull highlights from Readwise

Use the Readwise MCP tool `mcp__claude_ai_Readwise__readwise_search_highlights` to pull all highlights for the book.

Key details:
- The API returns max 50 results per query and uses vector similarity
- You MUST run multiple searches (8-15) with varied terms to maximize coverage. Expect 85-95% recovery of total highlights.
- Use `full_text_queries` with `document_title` filter to restrict to the target book
- Vary search terms significantly between queries: topics, company names, people, concepts, financial terms, specific vocabulary from the book
- Track unique highlight IDs across all searches to avoid duplicates
- Export all unique highlights to a JSON file for processing

Example search pattern:
```
Search 1: "acquisition compounding serial acquirer platform company"
Search 2: "decentralization culture leadership management autonomy"
Search 3: "capital allocation ROIC returns margin pricing"
Search 4: "[specific company names from the book]"
Search 5: "employee incentive compensation ownership founder"
...keep going until new searches return mostly duplicates
```

### Step 2: Create the research markdown file

ALWAYS create a `{book-slug}.md` file that organizes all highlights by theme. This is the source of truth for the microsite content.

Structure:
```markdown
# Book Title
**By Author Name**
*X highlights organized by theme*

---

## 1. Theme Name

Description of theme.

> "Exact quote from highlight"

> "Another quote"

---

## Key Takeaways

1. **Takeaway title.** Brief description.
```

### Step 3: Build the microsite pages

Each microsite should have at minimum:
- **Overview/index page** with intro, core principles, and navigation to sub-pages
- **Highlights page** with sidebar navigation and all quotes organized by theme
- **Applied lessons page(s)** connecting the book's ideas to holdcos and/or agencies

### Step 4: Update the library page

Add a card to `/reading-lessons/index.html` with the book cover and description.

### Step 5: Deploy

```bash
cd /Users/peterkang/Desktop/reading-lessons
git add -A
git commit -m "Add [microsite name]"
git push
```

GitHub Pages deploys automatically from the main branch.

## Voice Filter

ALL editorial prose on every page must follow the voice filter. The full spec is at `/Users/peterkang/Desktop/Readwise/voice-filter-extracted/voice-filter/SKILL.md` and installed at `~/.claude/skills/voice-filter/SKILL.md`.

Core rules:
- Short paragraphs (1-3 sentences)
- First person when appropriate
- Concrete over abstract. Name the thing.
- No em dashes. Use periods, commas, or colons.
- No paired adjectives ("comprehensive and robust")
- No filler transitions ("It's worth noting," "Interestingly")
- No sycophantic openings
- No closing summaries that restate what was said
- No dramatic fragment sentences staged for effect
- Write like you're explaining to a sharp colleague over coffee

**What NOT to filter**: Book quotes inside `.quote-text`, `.dual-quote-text`, `.big-quote-text`, `.lesson-quote-text` stay exact.

## Design System

### Typography (all microsites)
- **Serif**: Instrument Serif (headings, quotes, book titles)
- **Sans**: DM Sans (body text, descriptions, UI)
- **Mono**: JetBrains Mono (labels, eyebrow text, nav, numbers)
- Google Fonts `<link>` in each HTML `<head>`, not CSS `@import`

### Shared Colors
- `--ink: #1A1A18` (text)
- `--cream: #F5F0E8` (body background)
- `--paper: #FFFDF7` (card background)
- `--rule: #D4CFC2` (borders, dividers)
- Hero backgrounds: `#111110`
- Nav text: `rgba(242,237,228,*)` at various opacities

### Per-Microsite Accent Colors
Each microsite gets its own `--accent` color defined in its page-specific `<style>` tags:

| Microsite | Accent | Hex |
|-----------|--------|-----|
| Discount Philosophy - ALDI | Red | `#C83C2C` |
| Discount Philosophy - TJ | Orange | `#D4623B` |
| Discount Philosophy - Agency | Green | `#2D6A4F` |
| Discount Philosophy - Overview | Blue | `#2A5A7A` |
| The Compounding Playbook | Steel Blue | `#2B4C7E` |

### CSS Architecture
- Each microsite has a `styles.css` with shared styles for that microsite
- The discount-retail `styles.css` serves as the base/reference implementation
- Other microsites can import it (`@import`) or create their own following the same patterns
- Page-specific overrides (accent colors, hero gradients, unique components) go in `<style>` tags in each HTML file, loaded AFTER the shared CSS

### Layout Patterns
- **Overview/index pages**: Single column, max-width 900-1100px
- **Highlights pages**: Sidebar (260px) + main content grid. Sidebar is sticky with theme nav. On mobile (<900px), collapses to slide-out panel.
- **Applied lessons pages**: Single column, max-width 900px

### Shared Components
- **Site nav**: Fixed top bar, transparent over hero, gains dark backdrop on scroll (`.scrolled` class at 100px). Shows "Library / [Microsite Name]" on left, page links on right. On mobile (<600px), left side hides except Library link.
- **Hero**: Full-height dark section with eyebrow, title, subtitle, book cover(s), "Prepared by Peter Kang" link
- **Quote cards**: `.quote-card` with left-border accent. One `.featured` (dark bg) per section.
- **Principle/takeaway grids**: 2-column grid, 1px gap, collapses to 1-col on mobile
- **Newsletter signup**: Kit form 9327259, above the footer on every page
- **Footer nav**: Pill-shaped links to sibling pages
- **Book covers**: Slight rotation (±1.5deg), straighten on hover, centered in cards

### Hero Radial Gradients
Each page has a `hero::before` with radial gradients in its accent color(s). Opacity should be 0.15-0.25 for visibility. The hero gradient gives each page visual identity.

## Newsletter

Kit (ConvertKit) form ID: **9327259**
- Form action: `https://app.kit.com/forms/9327259/subscriptions`
- Script: `https://f.convertkit.com/ckjs/ck.5.js`
- Fields: email (required), first name (optional)
- Label: "Consumed/Created"
- Description: "A weekly newsletter by Peter Kang on building a holding company, acquiring and growing agency businesses, and other thoughts on business and life."
- Note: "No spam. Unsubscribe anytime."

Add to every page, above the footer.

## Content Guidelines

### Book Quotes
- Always exact text from Readwise. Never paraphrase.
- HTML entities: `&ldquo;` `&rdquo;` for quotes, `&rsquo;` for apostrophes
- Don't reference Readwise in public-facing content

### Applied Lessons Pages
- Don't directly quote from Peter's blog posts (snapshots in time)
- Holdco lessons focus on: capital allocation, M&A discipline, portfolio management, HQ design, incentive structures
- Agency lessons focus on: individual operating company principles (niches, pricing, talent, cash, clients)
- Holdco and agency lessons pages should NOT duplicate each other
- Balance quotes across source material (multiple companies, both books if applicable)
- Each lesson needs: a book quote, and the applied insight. Quote and body should connect clearly.

### Naming Conventions
- Folder names: lowercase, hyphens (e.g., `the-compounders`, `discount-retail`)
- HTML files: lowercase, hyphens (e.g., `agency-lessons.html`, `holdco-lessons.html`)
- Image files: lowercase, hyphens, no spaces (rename on import if needed)
- CSS files: `styles.css` in each microsite folder

## QA Checklist

Run this before committing and pushing any changes.

### Content
- [ ] All editorial prose passes the voice filter
- [ ] Book quotes are exact (not paraphrased)
- [ ] No Readwise references in public-facing content
- [ ] Quote-to-lesson connections are clear
- [ ] Roughly balanced source representation across applied lessons

### Cross-Page Consistency
- [ ] All pages link their microsite's `styles.css`
- [ ] All pages have the site nav with correct `active` class
- [ ] All pages have footer nav with sibling page links
- [ ] All pages have newsletter signup (Kit form 9327259)
- [ ] All pages have "PREPARED BY PETER KANG" linking to peterkang.com
- [ ] "Reading Lessons" eyebrow text on all heroes
- [ ] Hero backgrounds are `#111110`
- [ ] Typography is Instrument Serif + DM Sans + JetBrains Mono
- [ ] Book covers centered in cards, consistent shadow/border-radius
- [ ] Library link in site nav points to `../`

### Mobile (test at 375px and 768px)
- [ ] No horizontal overflow
- [ ] Site nav links fit on 375px without wrapping
- [ ] Library link visible on mobile nav
- [ ] Hero titles readable, not oversized
- [ ] Book covers scale down on small screens
- [ ] Sidebar pages collapse to slide-out panel on mobile (<900px)
- [ ] All grids collapse to single column
- [ ] Touch targets at least 44px
- [ ] Body text minimum 14px
- [ ] Footer nav wraps gracefully

### SEO & AI Search
- [ ] Unique, descriptive `<title>` per page
- [ ] `<meta name="description">` (150-160 chars) per page
- [ ] `<meta property="og:title">` and `<meta property="og:description">`
- [ ] Correct heading hierarchy (one h1, h2 for sections)
- [ ] All images have descriptive `alt` text
- [ ] All internal links use relative paths
- [ ] No broken links
- [ ] Semantic HTML (`<section>`, `<article>`, `<blockquote>`, `<nav>`, `<footer>`)

### Performance
- [ ] Images under 500KB each
- [ ] Google Fonts loads with `display=swap`
- [ ] Animations use transform/opacity only
- [ ] No unused CSS in page-specific `<style>` blocks

### Deployment
- [ ] All changes committed with descriptive message
- [ ] Pushed to main branch (GitHub Pages deploys automatically)
- [ ] Verified on live site after deploy (allow 1-2 min for propagation)
