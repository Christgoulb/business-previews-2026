# Implementation Checklist — goldrivermarketing.co.uk SEO fixes

Companion to the two audit reports in this folder. Work through in order; tick as you go.

## Route A (preferred): connect Lovable to GitHub
- [ ] Lovable → project → Settings → GitHub → Connect (creates a two-way synced repo)
- [ ] Share the repo name in the Claude session — the changes below can then be implemented directly as commits

## Route B: paste these prompts into Lovable's chat, one at a time

### 1. Retire the old insights articles (301 → new URLs)
```
Set up permanent 301 redirects from these old article URLs to their new equivalents, remove the old URLs from sitemap.xml, and update any internal links that point to the old URLs:
/insights/why-your-google-business-profile-is-not-ranking → /insights/ranking/why-gbp-isnt-ranking
/insights/common-google-business-profile-mistakes → /insights/fundamentals/common-gbp-mistakes
/insights/how-google-reviews-impact-local-rankings → /insights/reviews/how-reviews-impact-seo
/insights/service-area-vs-storefront-seo → /insights/fundamentals/service-area-vs-storefront
/insights/how-long-does-gbp-seo-take → /insights/ranking/how-long-does-local-seo-take
/insights/how-to-get-more-google-reviews → /insights/reviews/get-more-google-reviews
/insights/google-maps-ranking-factors → /insights/ranking/maps-ranking-factors
/insights/best-categories-for-google-business-profile → /insights/fundamentals/best-gbp-categories
/insights/how-to-rank-in-google-map-pack → /insights/ranking/local-pack-2026-guide
/insights/google-business-profile-optimisation-checklist → /insights/fundamentals/complete-gbp-optimisation-guide
/insights/google-business-profile-optimisation-guide → /insights/fundamentals/complete-gbp-optimisation-guide
```

### 2. Fold /blog into /insights
```
Merge the /blog section into /insights: move each /blog post into the matching /insights category, 301-redirect the old /blog URLs, remove them from sitemap.xml, and remove /blog from the navigation.
```

### 3. One GBP service page
```
Merge /google-business-profile-optimisation-service into /google-business-profile-optimisation-london (301 redirect the -service URL). Make sure only one page targets "Google Business Profile optimisation London".
```

### 4. Resolve borough/area collisions
```
For Camden, Barnet, Enfield and Islington I have both /locations/{name} and /locations/borough/{name}. Keep one page for each of these four and 301-redirect the other. Make the remaining borough pages act as hubs linking to their area pages.
```

### 5. Sitemap hygiene
```
Remove /unsubscribe from sitemap.xml and add a noindex robots meta tag to that page. Make sitemap lastmod dates reflect each page's real last-modified date instead of one bulk date.
```

### 6. Schema, descriptions, canonicals
```
Add LocalBusiness JSON-LD structured data to every page: name Gold River Marketing, address 3 Steeds Road, Muswell Hill, London N10 1JB, phone +447875044781, email info@goldrivermarketing.co.uk, areaServed North London. Add a unique meta description (140-155 characters) and a self-referencing canonical tag to every page.
```

## Outside Lovable (do these separately)
- [ ] **www vs non-www:** confirm the apex domain 301s to www in your domain/DNS settings; verify old non-www URLs (e.g. /terms-and-conditions/) redirect to current www pages
- [ ] **local.goldrivermarketing.co.uk:** fix or retire on its own platform — rewrite the keyword-stuffed homepage title (remove the phone number), and either differentiate its area pages or 301 the subdomain into the main site
- [ ] **Google Search Console:** after redirects go live, resubmit the cleaned sitemap and check the Pages report for legacy URLs
- [ ] **Verify:** spot-check redirects return real 301s (httpstatus.io), run the homepage through Google's Rich Results Test after schema is added

## Already done (this repo, 7 Aug 2026)
- Preview template + all 6 previews: noindex, meta descriptions, favicon, OG tags, LocalBusiness schema scaffold, H1 fix, mobile menu, dead links repaired
