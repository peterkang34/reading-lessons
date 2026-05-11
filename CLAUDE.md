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
- **Agency lessons page** — translates the book to individual agency operating principles. Always include this; the translation distance from any business book to an agency context adds real value.

Optionally:
- **Holdco lessons page** — only include when the source book is *not* itself a holdco book. For books like ALDI/Trader Joe's (operating retailers) or other operating-company books, holdco-lessons does meaningful translation. For books like The Outsiders or The Compounders, which are already about capital allocation at the parent-company level, a holdco-lessons page mostly restates the book's thesis and adds little. Skip it.

The judgment call: does extracting "lessons for holdcos" from this book require a translation step, or is the book already at that altitude?

#### Writing the agency lessons

Three principles, learned the hard way:

**1. Default to agency-type-agnostic vocabulary.** The reader's portfolio skews implementation-heavy (UX, data, Amazon, platform implementation, B2B SaaS services) — not creative or ad shops. Don't assume awards, trade press, creative directors, brand systems, or campaigns are universal vocabulary. When picking examples:
- *Universal*: RFPs, AR days, scope creep, account leads, niche pivot, retainers, utilization, non-billable count, partner/founder economics
- *Creative-shop only*: awards, trade press, brand systems, creative director, campaign-as-outcome
- *Tech/platform-agency only*: partner-tier badges (Shopify Plus, AWS, Salesforce), platform certifications, ROAS, conversion lift, adoption metrics

When you must use a category-specific example, either pick one that spans agency types or explicitly contrast both ("creative shops chase X; tech and platform agencies chase Y").

**2. Apply the honesty test: does the analogy actually hold?** The source book's CEOs ran specific kinds of businesses with specific dynamics. Some patterns transfer cleanly to agencies (capital allocation discipline, decentralization, niche dominance, cash culture, frugality signals). Some don't.

The Outsider CEOs' aversion to publicity is the canonical example of an analogy that breaks: their companies' buyers came to them. Agencies are service businesses that live on top-of-funnel — writing, talks, podcasts. "Don't seek the spotlight" is bad advice for an agency owner. The honest reframe was to identify what *does* transfer (audience discipline: be known by buyers, not by peers).

When the analogy breaks down, name the gap explicitly in the lesson body, then reframe around what survives. Don't paper it over with vague platitudes — readers dismiss those as glib, and the writer feels like a hypocrite.

**3. Lesson titles use language an agency owner actually speaks.** Bad titles borrow public-company or corporate vocabulary that creates a translation gap before the reader gets to the body:
- "Optimize for value per share" → public-company language; agencies don't have shares
- "Decentralize at the team level" → corporate-speak; not visceral
- "Stay independent, stay rational" → vague; could mean anything

Good titles are visceral, agency-native, and predict the lesson:
- "Headcount is a vanity metric, not a scoreboard"
- "The founder is the ceiling"
- "Be known by buyers, not by peers"
- "Awards don't pay payroll" *(if you know the audience runs creative shops; otherwise broaden)*
- "Build for the decade-long client, not the next project"

The test: would an agency owner reading the title know roughly what the lesson is about?

### Step 4: Update the library page

Add a card to `/reading-lessons/index.html` with the book cover and description.

### Step 5: Accuracy audit (REQUIRED before commit)

**The standard is anti-hallucination, not anti-edit.** Every quote must be derivable from a real highlight in the research `.md` (no invented words, no fake attributions, no fictional speakers). Light condensing for readability — dropping a leading connector, a footnote marker, a tangential example — is fine. Fabricating content is not.

The same rule applies to editorial prose: every numeric figure, transaction, named action must be traceable to a real highlight or removed.

#### 5.1 Use the local `.md` as the source of truth (do NOT re-pull from Readwise)

Step 2 of this workflow already produced `{book-slug}.md` — a research file containing every highlight pulled fresh from Readwise. That file is the audit source. Re-pulling via the MCP is slow, expensive, and unnecessary; the `.md` was the canonical extract at the time the microsite was built.

If the `.md` is missing, stale, or you suspect it dropped highlights during the original pull, only then re-pull. Otherwise, work from the file you already have.

#### 5.2 Run the audit script against the HTML files

```bash
cd /Users/peterkang/Desktop/reading-lessons/<microsite>
python3 <<'PY'
import re, html
from pathlib import Path

# Use the local research markdown as the source of truth
md = Path('<book-slug>.md').read_text()

def normalize(s):
    s = html.unescape(s)
    s = s.replace('“','"').replace('”','"').replace('‘',"'").replace('’',"'")
    s = s.replace('–','-').replace('—','-').replace('…','...')
    s = re.sub(r'\s+', ' ', s).strip().lower()
    return s

haystack = normalize(md)
files = ['index.html','highlights.html','holdco-lessons.html','agency-lessons.html']
patterns = [r'class="quote-text">(.*?)</div>',
            r'class="lesson-quote-text">(.*?)</div>',
            r'class="big-quote-text">(.*?)</div>',
            r'class="dual-quote-text">(.*?)</div>']
total = 0; mismatches = []
for f in files:
    txt = Path(f).read_text()
    for pat in patterns:
        for m in re.finditer(pat, txt, re.DOTALL):
            q_norm = normalize(m.group(1)).strip('"').strip("'").strip()
            total += 1
            # check multiple anchor positions — survives light trims
            found = False
            for start in [0, 30, 60, max(0, len(q_norm)-80)]:
                seg = q_norm[start:start+50]
                if len(seg) >= 30 and seg in haystack:
                    found = True; break
            if not found:
                mismatches.append((f, m.group(1)[:160]))
print(f"Quotes: {total} | Mismatches: {len(mismatches)}")
for f, q in mismatches:
    print(f"  [{f}] {q}")
PY
```

The script uses **anchor matching at multiple positions in the quote** — so a quote that drops a leading connector or a closing clause still matches if the middle survives. That's intentional. Light condensing passes the audit. Fabricated content does not.

#### 5.3 Resolve every mismatch

A mismatch means *no anchor of the quote was found in the research `.md`*. Two real causes:

1. **Coverage gap in the original pull** — the quote is real, but the Step 1 Readwise pull missed it (typical 85-95% recovery). Verify by searching Readwise directly with `full_text_queries` for a distinctive phrase. If found, the quote is legitimate — **append the rediscovered highlight to the `.md`**. Not optional. If you skip this, the next audit will surface the same mismatches and someone will redo the verification work. (We learned this the hard way: the Compounders microsite ran two audits months apart, both flagging the same 13 real-but-missing highlights, because we never appended.)

2. **Fabricated content** — quote does not exist in Readwise. Remove the quote-card or replace it with a verifiable highlight. Never reconstruct from memory.

There is *no* category for "light trim." Trims are fine and don't trigger mismatches because the script's anchor matching tolerates them.

#### 5.4 Audit editorial prose for fabricated specifics

Quote matching only catches falsified quotes. Fabricated facts in editorial prose (lesson bodies, intros, CEO cards, principle descriptions) need a separate pass. Common failure modes from prior audits:

- **Made-up numerical specifics**: "beat the market by 20x" when the highlights only say "twenty-plus year tenures." "Bought ABC for $3.5B" when only the Disney sale price is in the highlights.
- **Reframed actions**: "Bought GEICO" when the highlight says "first stock he recommended."
- **Aggregated claims that aren't in the highlights**: "All eight reinvested 80%+ of cash flow at 20% ROIC" — verify each numeric claim has a supporting highlight.

For each numeric figure, named transaction, and named action in the editorial prose, grep the `.md` for the underlying claim. If it's not there, soften ("delivered exceptional returns") or remove.

#### 5.5 Re-run the audit after fixes

The audit script must report `Mismatches: 0` and the editorial-prose grep must find no unsupported specifics. Only then proceed to Step 6.

### Step 6: Deploy

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
- The standard is **anti-hallucination, not anti-edit**. Every quote must be derivable from a real highlight in the `.md`. No invented words, no fake attributions, no fictional speakers.
- **Light condensing for readability is fine.** Drop a leading connector, a footnote marker ("10", "11"), a tangential example, an attribution if it gets in the way of flow. The goal is browsability — a wall of full passages buries the insight.
- **What's not fine**: inventing words the source doesn't contain, putting a quote in the wrong person's mouth, combining two unrelated passages into one fake quote, fabricating a number or transaction that isn't in the source.
- **Readwise is the only source.** If it's not in your `.md` (or findable in Readwise via a coverage-gap re-search per Step 5.3), it does not go on the page. Specifically banned:
  - Pulling quotes from a PDF or ebook in the project folder, even if one exists. (The Compounders folder has one — don't use it. The PDF bypasses the Readwise → `.md` provenance chain that the audit relies on.)
  - Web-searching for additional book quotes.
  - Recalling quotes from training data or model knowledge of the book. You may know the book; that doesn't make your recollection a real highlight.
  - Synthesizing a "representative" quote from two unrelated highlights.
  - Asking the model to write a quote in the style of [author].
- If you want to make a thematic point but Readwise doesn't have a highlight that supports it, **cut the point** — don't manufacture a quote. The microsite is honest about what the highlights actually say. That honesty is the product.
- Use HTML entities: `&ldquo;` `&rdquo;` for quotes, `&rsquo;` for apostrophes, `&mdash;` inside quotes.
- Don't reference Readwise in public-facing content.
- Step 5 of the workflow (accuracy audit) catches fabrications. Run it before every commit.

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
- [ ] **Accuracy audit script run and reports 0 mismatches** (Step 5.2)
- [ ] **All editorial prose specifics (numbers, transaction names, named actions) verified against raw highlights** (Step 5.4)
- [ ] Book quotes are exact, never trimmed or paraphrased
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
