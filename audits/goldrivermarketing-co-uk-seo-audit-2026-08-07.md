# SEO Audit — goldrivermarketing.co.uk

**Prepared for:** Gold River Marketing
**Date:** 7 August 2026
**Scope:** Technical and on-page SEO — title tags, meta descriptions, heading structure, schema markup, image alt text, internal linking, and mobile-friendliness signals in the HTML.

---

## Scope & Methodology

This audit covers two parts of the Gold River Marketing web estate:

1. **The live public site** (`www.goldrivermarketing.co.uk` and the `local.goldrivermarketing.co.uk` subdomain) — assessed via search-engine index data (indexed titles, URLs, and site architecture). *Note: a direct crawl of the live domain was not possible from the audit environment due to network restrictions, so HTML-level checks for the live site (e.g. exact meta descriptions, schema on live pages) should be re-verified with a crawler such as Screaming Frog once access is available. Findings for the live site below are based on what search engines have indexed.*
2. **The client preview-site estate** published from the `business-previews-2026` repository (the GoldRiver preview template plus 6 published preview pages). This HTML was audited in full, line by line.

---

## Executive Summary

The web estate has a solid foundation — pages are lightweight, responsive, served with valid `lang` and `viewport` declarations, and the main homepage title tag is well-formed. However, there are several issues that are actively limiting search performance and, in two cases, creating **risk to the main domain's rankings**:

- **The preview pages are indexable and have no `noindex` directive.** Six thin, near-duplicate template pages about *other businesses* (hypnotherapists, nutritionists) are publishable under the Gold River brand. If these are crawled, they dilute topical relevance and can be classified as thin/doorway content.
- **The site is indexed under two hostnames** (`goldrivermarketing.co.uk` and `www.goldrivermarketing.co.uk`), splitting link equity.
- **The `local.` subdomain duplicates the main site's location pages** (e.g. `/locations/muswell-hill` on www vs `/local-seo/haringey-hornsey/` on local.), competing against itself and carrying doorway-page risk.
- **No structured data (schema markup) exists anywhere in the audited HTML** — a significant gap for a local SEO agency that should be demonstrating LocalBusiness schema on its own estate.
- **5 of 6 preview pages have no meta description**, and none of the audited pages have canonical tags, Open Graph tags, favicons, or a single `<img>` tag with alt text (all images are CSS backgrounds, invisible to image search and screen readers).

A prioritised issue list with fixes follows.

---

## Priority 1 — Critical (fix within 1–2 weeks)

### 1.1 Preview pages are indexable thin content on the brand's domain

**Found:** None of the 7 audited HTML files (template + 6 previews) contain a `<meta name="robots">` tag, a canonical tag, or any indexation control. The pages are near-duplicate template output about unrelated businesses, several containing placeholder-style copy ("Ready to make this website yours?", "Website Preview").

**Why it matters:** If these pages are served on (or subdomained under) `goldrivermarketing.co.uk`, Google will crawl thin, near-duplicate, off-topic pages under the agency's brand. This dilutes the domain's topical focus (local marketing services) and risks a thin-content quality assessment that affects the whole host.

**Fix:** Add to the `<head>` of the preview template (and re-publish all existing previews):

```html
<meta name="robots" content="noindex, nofollow">
```

Also exclude the preview directory in `robots.txt` as a belt-and-braces measure:

```
User-agent: *
Disallow: /previews/
```

### 1.2 Two hostnames indexed — www vs non-www split

**Found:** Google's index contains both `https://goldrivermarketing.co.uk/terms-and-conditions/` (non-www) and `https://www.goldrivermarketing.co.uk/` (www).

**Why it matters:** Two indexed hostnames split link equity and crawl budget, and can cause the "wrong" version to rank. Every backlink pointing at the non-canonical host is diluted.

**Fix:** Pick one canonical host (recommend `www.goldrivermarketing.co.uk`, since the homepage is indexed there), then:

1. 301-redirect all non-www URLs to their www equivalents at the server level.
2. Add a self-referencing canonical tag to every page: `<link rel="canonical" href="https://www.goldrivermarketing.co.uk/...">`.
3. Update internal links, the XML sitemap, and Google Search Console's preferred property.

### 1.3 No structured data (schema markup) anywhere

**Found:** Zero `application/ld+json` blocks across all audited HTML. No LocalBusiness, Organization, Service, FAQPage, or Review schema.

**Why it matters:** For a business selling *local SEO and Google Business Profile optimisation*, missing LocalBusiness schema on its own site is both a rankings gap (eligibility for rich results, stronger entity signals for the Map Pack) and a credibility gap with prospective clients. The preview template even contains ready-made FAQ and review sections that are ideal schema candidates.

**Fix:** Add JSON-LD to the main site's pages. Homepage example:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "ProfessionalService",
  "name": "Gold River Marketing",
  "url": "https://www.goldrivermarketing.co.uk/",
  "telephone": "+447875044781",
  "email": "info@goldrivermarketing.co.uk",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "3 Steeds Road, Muswell Hill",
    "addressLocality": "London",
    "postalCode": "N10 1JB",
    "addressCountry": "GB"
  },
  "areaServed": "London",
  "priceRange": "££"
}
</script>
```

For client sites built from the preview template, add `LocalBusiness` (with the client's NAP data), `FAQPage` (the template already has an FAQ section), and aggregate rating markup only where reviews are displayed on-page and verifiable.

---

## Priority 2 — High (fix within 30 days)

### 2.1 Keyword-stuffed and malformed title on the local. subdomain homepage

**Found (indexed title):**
`Google Business Profile Optimisation | Link Building | Local SEO | Web Design | Gold River Marketing[0787 5044781]` — 113 characters, four stacked keywords, and a phone number concatenated into the title without a space.

**Why it matters:** Titles over ~60 characters are truncated in results; keyword-stacked titles are frequently rewritten by Google and read as spam signals; a phone number in a title tag wastes the single strongest on-page relevance element.

**Fix:** Rewrite to one primary keyword + brand, e.g.:
`Local SEO & Google Business Profile Experts | Gold River Marketing` (66 chars — trim to taste). Move the phone number to visible page content and LocalBusiness schema, where it belongs.

### 2.2 Subdomain vs main-domain duplication (doorway-page risk)

**Found:** Location-targeted pages exist on **both** hosts — e.g. `www.goldrivermarketing.co.uk/locations/muswell-hill` and `www.goldrivermarketing.co.uk/locations/stoke-newington` on the main site, while `local.goldrivermarketing.co.uk` carries a large parallel set (`/local-seo/camden-camden-town/`, `/local-seo/haringey-hornsey/`, `/local-seo/barnet-edgware/`, `/web-design/enfield-winchmore-hill/`, and many more) with near-identical "Expert Local SEO in [Area]" titles.

**Why it matters:** Subdomains do not automatically inherit the main domain's authority, so `local.` pages compete from a weaker position — while also cannibalising the main site's `/locations/` pages for the same queries. Large sets of templated area pages with interchangeable content match Google's definition of doorway pages and are vulnerable to being ignored or demoted wholesale.

**Fix:**
1. Consolidate: migrate the `local.` subdomain's pages into the main site under `/locations/` (or retire them), with 301 redirects mapping each old URL to its closest equivalent.
2. Keep only location pages you can make genuinely distinct — local testimonials, local case studies, area-specific copy — and prune the rest.
3. One page per service × area combination, on one host.

### 2.3 Missing meta descriptions (5 of 6 preview pages; verify live site)

**Found:** Only `hypnomind-london-clinical-hypnotherapy` has a meta description. The other five preview pages have none. (Live-site pages should be re-checked by crawl.)

**Why it matters:** Without a description, Google composes its own snippet from page copy — often poorly. A well-written description materially improves click-through rate even when rankings are unchanged.

**Fix:** Add a unique 140–155-character description per page, front-loading the service and location, ending with a call to action. Template example for the preview system (so every generated page gets one automatically):

```html
<meta name="description" content="{{CATEGORY}} in {{CITY}} — {{BUSINESS_NAME}}. Rated {{RATING}} from {{REVIEWS}} Google reviews. Call {{PHONE}} or book online today.">
```

### 2.4 No image alt text — all imagery is CSS backgrounds

**Found:** Across all 7 audited files there are **zero `<img>` elements**. Every image (hero, about, photo panels) is a CSS `background-image`, hotlinked from Unsplash at up to 2200px width.

**Why it matters:**
- CSS backgrounds cannot carry alt text: the pages are invisible to Google Image search and provide nothing to screen readers (an accessibility failure that increasingly correlates with quality assessment).
- Hotlinking Unsplash means render-blocking external requests, no control over availability, and a large un-optimised LCP (Largest Contentful Paint) image — a Core Web Vitals cost.

**Fix:**
1. Convert content-bearing images to real `<img>` elements with descriptive alt text, e.g. `<img src="/assets/images/hero.webp" alt="Hypnotherapy session in a London clinic" width="1800" height="1200" loading="eager" fetchpriority="high">`; keep purely decorative textures as CSS backgrounds or give them `alt=""`.
2. Self-host optimised WebP images (the repository already contains a categorised WebP library in `/assets/images/` — use it) and add `loading="lazy"` to below-the-fold images.

---

## Priority 3 — Medium (fix within 60 days)

### 3.1 Title tag quality is inconsistent across the estate

**Found (audited pages, with character counts):**

| Page | Title | Length | Verdict |
|---|---|---|---|
| www homepage (indexed) | Local Marketing Done For You \| Gold River Marketing \| London | 60 | ✔ Good |
| /locations/stoke-newington (indexed) | Rank Higher, Get More Calls in 3 Pack | — | ✖ Vague, jargon ("3 Pack"), no location |
| hypnomind preview | Hypnomind London - Clinical Hypnotherapy \| Website Preview by GoldRiver Marketing | 81 | ✖ Truncates |
| hypnotherapy-space | Hypnotherapy Space \| Website Preview | 36 | ✖ No service/location keyword |
| kirsti-holm | Kirsti Holm \| Hypnotherapy service in London | 44 | ✔ Good pattern |
| nutritionist-london | Nutritionist London \| Professional Dietary Advice in London | 59 | ⚠ "London" duplicated |
| stop-smoking-clinic | Stop Smoking Hypnotherapy Clinic | 32 | ✖ Brand only, no location |
| total-hypnotherapy | Total Hypnotherapy Crouch End \| Professional Hypnotherapy Services in London | 76 | ✖ Truncates |

**Fix:** Standardise on **`{Primary service} in {Location} | {Brand}`**, 50–60 characters, one location mention, no filler words. E.g. `Stop Smoking Hypnotherapy in North London | SSH Clinic`.

### 3.2 Heading structure: the keyword headline is an H2, not the H1

**Found:** In the preview template, the H1 is `"I built this website for {{BUSINESS_NAME}}."` (a message to the business owner, not a search-relevant statement), while the actual service headline (`{{HERO_HEADLINE}}`, e.g. "Transform your mind with professional hypnotherapy") is demoted to H2. Heading hierarchy is otherwise clean (no skipped levels, logical H2/H3 nesting) and each page has exactly one H1.

**Why it matters:** The H1 is the strongest on-page heading signal. Spending it on preview messaging wastes it; on any preview converted into a live client site, this line must not survive.

**Fix:** In the template, make the reveal-screen line a styled `<p>` or `<div>`, and promote the hero headline to the H1:
`<h1>{{HERO_HEADLINE}}</h1>` with `{{CATEGORY}} in {{CITY}}` retained as the eyebrow line above it. For the two previews already using service-led H1s (kirsti-holm, total-hypnotherapy), no change needed.

### 3.3 Mobile navigation disappears below 900px — no menu replaces it

**Found:** The template's CSS hides the nav links on mobile (`@media (max-width: 900px) { .nav-links { display: none; } }`) with **no hamburger menu or alternative navigation**. Only the "Call" button remains. Positive mobile signals are otherwise in place: `<meta name="viewport" content="width=device-width, initial-scale=1.0">` on every page, responsive grid collapse at 900px/560px breakpoints, fluid `clamp()` typography, and ≥52px tap targets on buttons.

**Why it matters:** Google indexes mobile-first. Section links that exist only on desktop weaken internal anchor discoverability on the version of the page Google actually crawls, and mobile users lose one-tap access to Services/Reviews/FAQ/Contact.

**Fix:** Add a simple disclosure-based mobile menu (a `<details>`/`<summary>` burger or a small JS toggle) exposing the same five links below 900px.

### 3.4 Internal linking is minimal and partially broken

**Found:**
- All preview-page navigation is same-page anchors only. Two pages contain **broken or empty anchors**: `stop-smoking-hypnotherapy-clinic` has two `href="#"` links and *no* anchor IDs at all (its nav goes nowhere), and `hypnotherapy-space` has one `href="#"` dead link.
- The only cross-estate link is a single footer link to `goldrivermarketing.co.uk` — which points at the **non-www** host in the template while the newest preview links to the **www** host (compounding issue 1.2).
- No breadcrumbs, no links between related pages, no link from the main site to services from within body copy (to be confirmed by live crawl).

**Fix:**
1. Repair the dead `href="#"` links (point them at real section IDs) and add the missing section IDs on the stop-smoking page.
2. Standardise every footer/template link to the canonical host `https://www.goldrivermarketing.co.uk/`.
3. On the main site: add descriptive-anchor internal links from homepage body copy to each service page, and from each location page to its parent service page.

---

## Priority 4 — Low (housekeeping, fix within 90 days)

### 4.1 No Open Graph or Twitter Card tags
**Found:** Zero `og:*` or `twitter:*` meta tags in any audited page.
**Why it matters:** Links shared on WhatsApp, Facebook, LinkedIn or X render with no image and an arbitrary snippet — a lost conversion touchpoint (not a direct ranking factor).
**Fix:** Add per page: `og:title`, `og:description`, `og:image` (1200×630), `og:url`, `og:type`, plus `twitter:card` = `summary_large_image`.

### 4.2 No favicon
**Found:** No `rel="icon"` link in any audited page.
**Fix:** Add a favicon and touch icon. Google shows favicons in mobile results; a missing one hurts SERP polish and brand recall.

### 4.3 Contact forms are non-functional
**Found:** Preview forms have no `action`/`method`, and the submit button is `type="button"` with no handler — submissions go nowhere. Not a ranking factor, but it's the primary conversion path on pages whose stated purpose is "built to convert".
**Fix:** Wire forms to a backend or forms service, use `type="submit"`, add `name` attributes and a success state.

### 4.4 Performance hygiene on hero images
**Found:** The hero/reveal background loads a 2200px-wide external Unsplash JPEG with no preload and full-viewport overlay gradients — the LCP element on every preview page depends on a third-party host.
**Fix:** Self-host the LCP image as WebP (~150–250KB), add `<link rel="preload" as="image" ...>`, and keep the gradient overlays.

### 4.5 robots.txt and XML sitemap (verify on live site)
The repository serves pages with no robots.txt or sitemap of its own. Confirm on the live site that an XML sitemap exists, contains only canonical (www) URLs, excludes preview/thin pages, and is registered in Google Search Console.

---

## What's Already Good

- Valid `<html lang="en">` and UTF-8 charset on every page.
- Viewport meta tag present everywhere; genuinely responsive layout with sensible breakpoints and fluid type.
- Exactly one H1 per page with no skipped heading levels.
- External links correctly use `rel="noopener"`.
- Lightweight pages (6–25KB HTML, inline CSS, no framework bloat) — excellent baseline for Core Web Vitals.
- The indexed www homepage title (`Local Marketing Done For You | Gold River Marketing | London`) is well-constructed.
- `tel:` links present for one-tap calling on mobile.

---

## Recommended Action Order (summary)

| # | Action | Priority | Effort |
|---|--------|----------|--------|
| 1 | `noindex` all preview pages + robots.txt exclusion | Critical | Low |
| 2 | Consolidate www vs non-www with 301s + canonicals | Critical | Low–Med |
| 3 | Add LocalBusiness/FAQ JSON-LD schema | Critical | Medium |
| 4 | Rewrite local. subdomain homepage title; remove phone from titles | High | Low |
| 5 | Consolidate local. subdomain into main-site /locations/ | High | High |
| 6 | Add unique meta descriptions to every page (template + live) | High | Low |
| 7 | Convert content images to `<img>` with alt text; self-host WebP | High | Medium |
| 8 | Standardise title formula across estate | Medium | Low |
| 9 | Promote hero headline to H1 in template | Medium | Low |
| 10 | Add mobile menu; fix broken `#` anchors; standardise footer host | Medium | Low |
| 11 | OG tags, favicon, working forms, LCP preload, sitemap check | Low | Low–Med |

---

## Appendix — Live-site index data sources

Live-site architecture and title findings were drawn from search-engine index data, including:

- [Homepage (www)](https://www.goldrivermarketing.co.uk/) — "Local Marketing Done For You | Gold River Marketing | London"
- [Terms and Conditions (non-www)](https://goldrivermarketing.co.uk/terms-and-conditions/) — evidence of dual-host indexing
- [local. subdomain homepage](https://local.goldrivermarketing.co.uk/) — keyword-stuffed title with embedded phone number
- [local. web-design hub](https://local.goldrivermarketing.co.uk/web-design/), [local-seo hub](https://local.goldrivermarketing.co.uk/local-seo/), [GBP hub](https://local.goldrivermarketing.co.uk/google-business-profile-optimisation/)
- Area pages: [Camden Town](https://local.goldrivermarketing.co.uk/local-seo/camden-camden-town/), [Hornsey](https://local.goldrivermarketing.co.uk/local-seo/haringey-hornsey/), [Edgware](https://local.goldrivermarketing.co.uk/local-seo/barnet-edgware/), [Enfield](https://local.goldrivermarketing.co.uk/local-seo/enfield-enfield/), [Hackney](https://local.goldrivermarketing.co.uk/local-seo/hackney/), [Walthamstow](https://local.goldrivermarketing.co.uk/local-seo/waltham-forest-walthamstow/), [Winchmore Hill](https://local.goldrivermarketing.co.uk/web-design/enfield-winchmore-hill/), [Dollis Hill](https://local.goldrivermarketing.co.uk/google-business-profile-optimisation/brent-dollis-hill/)
- Main-site location pages: [Muswell Hill](https://www.goldrivermarketing.co.uk/locations/muswell-hill), [Stoke Newington](https://www.goldrivermarketing.co.uk/locations/stoke-newington)

Repository HTML audited in full: `goldriver-template.html` and the published previews `hypnomind-london-clinical-hypnotherapy`, `hypnotherapy-space`, `kirsti-holm`, `nutritionist-london`, `stop-smoking-hypnotherapy-clinic`, `total-hypnotherapy-crouch-end`.

*Recommended follow-up: once crawler access to the live domain is available, run a full technical crawl (Screaming Frog or Sitebulb) plus Core Web Vitals field data (PageSpeed Insights / CrUX) to verify live-page meta descriptions, schema, canonical implementation, and page speed.*

---

## Addendum — Fixes applied (7 August 2026)

The following items were fixed directly in this repository on the date of the audit:

| Audit ref | Fix | Where |
|---|---|---|
| 1.1 | `<meta name="robots" content="noindex, nofollow">` added | Template + all 6 preview pages |
| 1.3 | Templated LocalBusiness JSON-LD schema added (activates with real NAP data on client conversion) | Template |
| 2.3 | Templated meta description added; unique descriptions written for the 5 preview pages missing one | Template + 5 preview pages |
| 3.2 | Hero headline promoted to H1; reveal-screen line demoted to a styled `<div>` | Template |
| 3.3 | Mobile hamburger menu added (shown below 900px, closes on tap) | Template |
| 3.4 | Dead `href="#"` CTA links repaired (now open a pre-filled email to GoldRiver); footer link standardised to the www host | stop-smoking, hypnotherapy-space, template |
| 4.1 | Open Graph + Twitter Card tags added | Template |
| 4.2 | Favicon added (inline SVG, gold "G") | Template + all 6 preview pages |
| 4.4 | `<link rel="preload">` added for the LCP hero image | Template + hypnomind preview |

**Still outstanding (requires access to the live site / hosting):** www vs non-www 301 consolidation and canonical tags (1.2), schema on the live www pages (1.3), the `local.` subdomain title rewrite and consolidation (2.1, 2.2), live-site meta descriptions (2.3), converting CSS background images to `<img>` with alt text using the repository's WebP library (2.4), title standardisation across the live estate (3.1), wiring the contact forms to a form service (4.3), and the sitemap/robots.txt check (4.5). Note: already-published previews will only pick up these fixes if re-served from this repository; newly published previews inherit them automatically from the template.
