# Sitemap & Site-Architecture Audit — www.goldrivermarketing.co.uk

**Prepared for:** Gold River Marketing
**Date:** 7 August 2026
**Source:** `https://www.goldrivermarketing.co.uk/sitemap.xml` (92 URLs, supplied by the client)
**Scope:** URL architecture, duplication/cannibalisation risk, and sitemap hygiene. This complements the earlier audit (`goldrivermarketing-co-uk-seo-audit-2026-08-07.md`); page-level HTML checks remain outstanding pending site access.

---

## Snapshot

| Section | URLs | Notes |
|---|---|---|
| /insights | 42 | 11 old flat articles + 4 category hubs + 22 categorised articles + index + 2 guides |
| /locations | 20 | 14 area pages + 6 borough pages |
| /case-studies | 6 | 1 updated Aug 2026 (Wembley driving school) — good freshness |
| /blog | 6 | A second, parallel article section |
| Service pages | 7 | GBP ×2, map pack, web design, automation, AI receptionist, local marketing |
| Core pages | 8 | home, free-audit, packages, our-process, contact, about, playbook, insights index |
| Legal/utility | 4 | privacy, terms, cookies, **unsubscribe** |

---

## Priority 1 — Content duplication and cannibalisation (High)

### 1.1 The insights section contains two generations of the same articles

The sitemap lists an **old flat set** (`/insights/<slug>`, lastmod 6–13 May) *and* a **new categorised set** (`/insights/{fundamentals|ranking|reviews|playbooks}/<slug>`, lastmod 28–29 June) covering the same topics. At least nine direct topic pairs are both present:

| Old flat URL | Newer categorised URL |
|---|---|
| /insights/why-your-google-business-profile-is-not-ranking | /insights/ranking/why-gbp-isnt-ranking |
| /insights/google-business-profile-optimisation-checklist *and* /insights/google-business-profile-optimisation-guide | /insights/fundamentals/complete-gbp-optimisation-guide *(3-way)* |
| /insights/how-google-reviews-impact-local-rankings | /insights/reviews/how-reviews-impact-seo |
| /insights/service-area-vs-storefront-seo | /insights/fundamentals/service-area-vs-storefront |
| /insights/how-long-does-gbp-seo-take | /insights/ranking/how-long-does-local-seo-take |
| /insights/common-google-business-profile-mistakes | /insights/fundamentals/common-gbp-mistakes |
| /insights/how-to-get-more-google-reviews | /insights/reviews/get-more-google-reviews |
| /insights/google-maps-ranking-factors | /insights/ranking/maps-ranking-factors |
| /insights/best-categories-for-google-business-profile | /insights/fundamentals/best-gbp-categories |

**Why it matters:** Whichever is true, it's a problem. If both versions resolve with 200 status, Google sees near-duplicate articles competing for the same queries — it will pick one arbitrarily (often splitting links and impressions between them) and may devalue both. If the old URLs already redirect to the new ones, then the sitemap is feeding Google redirecting URLs, which wastes crawl budget and delays consolidation.

**Fix:**
1. 301-redirect every old flat URL to its categorised successor.
2. Remove the redirected URLs from the sitemap — a sitemap should contain only canonical, 200-status, indexable URLs.
3. Update internal links to point straight at the new URLs.

### 1.2 Three parallel article sections: /insights, /blog, /playbook

`/blog` (6 posts, July) overlaps `/insights` directly on topic:

- Map pack: **four** articles across sections — /blog/how-to-appear-in-google-map-pack-london, /blog/what-is-the-google-map-pack, /insights/how-to-rank-in-google-map-pack, /insights/ranking/local-pack-2026-guide
- Reviews: **three** — /blog/how-to-get-more-google-reviews-london, /insights/how-to-get-more-google-reviews, /insights/reviews/get-more-google-reviews

**Why it matters:** Topical authority concentrates when one section owns a topic. Four map-pack articles in three URL patterns split internal links, freshness signals and rankings four ways — for the exact queries this business most needs to win.

**Fix:** Pick one home for editorial content (recommend `/insights`, it has the structure). Merge each blog post into the best existing insights article (or move it in), 301 the old URLs, and retire `/blog` from the sitemap and navigation. Where two articles genuinely target different intents (e.g. "what is the map pack" = definitional vs "how to rank" = how-to), keep both but interlink them and differentiate titles.

### 1.3 Location pages exist in two tiers — with four same-name collisions

14 area pages (`/locations/<area>`) and 6 borough pages (`/locations/borough/<borough>`), where **Camden, Barnet, Enfield and Islington exist in both tiers** (e.g. `/locations/camden` *and* `/locations/borough/camden`). The earlier audit also found a third parallel set on the `local.` subdomain (e.g. `local.goldrivermarketing.co.uk/local-seo/camden-camden-town/`).

**Why it matters:** Two pages on the same host named "Camden" will compete for "local SEO Camden"-type queries unless their roles are sharply distinguished; a third on a subdomain makes it worse.

**Fix:** Make the borough pages true hubs (borough overview + links to area pages within that borough + borough-level proof/case studies) and keep detailed service content on area pages only. Where an area *is* the borough (Camden, Barnet, Enfield, Islington), keep one page and 301 the other. Fold the `local.` subdomain into this structure as previously recommended.

### 1.4 Overlapping service pages for the core service

- `/google-business-profile-optimisation-london` (0.9) **and** `/google-business-profile-optimisation-service` (0.9) — two pages for the same service.
- `/google-map-pack-london` (0.9) targets closely adjacent intent (map pack ranking *is* GBP optimisation's outcome).
- `/local-marketing-london` (0.95, changefreq weekly) overlaps the homepage's positioning ("Local Marketing Done For You … London").

**Fix:** One canonical page per service. Merge the two GBP pages (301 the weaker), differentiate `/google-map-pack-london` toward the "map pack" query family explicitly, and decide whether "local marketing London" belongs to the homepage or the landing page — then de-optimise the other for that phrase.

---

## Priority 2 — Sitemap hygiene (Medium)

1. **`/unsubscribe` is in the sitemap** (priority 0.1). Utility pages shouldn't be submitted for indexing — remove it from the sitemap and add `noindex` to the page.
2. **`lastmod` looks bulk-stamped, not real.** 48 of 92 URLs share `2026-05-06` and 26 share `2026-06-28`. Google ignores `lastmod` when it proves unreliable, which forfeits a genuinely useful crawl signal — the freshly updated case study (`wembley-driving-school`, 2026-08-05) shows the field *can* be accurate. Fix: emit true last-modified dates per page.
3. **`changefreq` on only 6 of 92 entries.** Google ignores `changefreq` and `priority` anyway; either drop both fields site-wide for a cleaner sitemap or don't worry about the inconsistency — but don't rely on them.
4. **Legacy indexed URLs outside the sitemap.** Google's index still holds `goldrivermarketing.co.uk/terms-and-conditions/` (non-www, old slug) while the sitemap lists `/terms` — evidence that old-site URLs were never redirected. Crawl the old site's URL set (Search Console → Pages report will show them) and 301 each to its current equivalent. This ties into the www/non-www consolidation from the first audit.

---

## What's good

- Clean, descriptive, keyword-relevant slugs throughout; no parameters, no session IDs.
- Logical new content architecture (`/insights/{category}/{article}`) with hub pages per category — the right pattern, it just needs the old generations retired.
- Case-study section with borough-relevant examples and at least one genuinely fresh update (Aug 2026).
- Sensible priority tiering (money pages 0.9–0.95, articles 0.6–0.7, legal 0.3).
- Location coverage matches the service area claimed on the site (North London boroughs).

---

## Recommended action order

| # | Action | Effort |
|---|---|---|
| 1 | 301 old flat /insights URLs → categorised equivalents; purge from sitemap | Low |
| 2 | Consolidate /blog into /insights (merge or move, 301, retire section) | Medium |
| 3 | Merge the two GBP service pages; differentiate map-pack and local-marketing pages | Medium |
| 4 | Resolve the 4 same-name borough/area collisions; define hub-vs-spoke roles | Medium |
| 5 | Remove /unsubscribe from sitemap + noindex it | Low |
| 6 | Fix lastmod to real dates; optionally drop changefreq/priority | Low |
| 7 | Redirect legacy old-site URLs (e.g. /terms-and-conditions/) as part of www consolidation | Low–Med |

---

## To complete the page-level audit

The sitemap tells us the architecture; the HTML tells us the on-page story. To finish the audit I need the HTML of these six representative pages (view-source → copy, or enable domain access for the audit environment):

1. `/` (homepage)
2. `/local-marketing-london` (highest-priority landing page)
3. `/google-business-profile-optimisation-london` (core service page)
4. `/locations/muswell-hill` (top area page)
5. `/locations/borough/haringey` (borough hub)
6. `/insights/ranking/local-pack-2026-guide` (representative article)

With those I can verify titles, meta descriptions, heading structure, schema markup, image alt text, internal linking and mobile signals on the live site itself.
