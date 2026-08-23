---
name: ionetix-map-product-and-site-network
description: >-
  Read the Ionetix product surface — Cardiac PET / N-13 Ammonia, the N-13 Ammonia site network,
  Cyclotron Solutions, Actinium-225, Astatine-211, targeted alpha therapy and PSMA PET — plus the
  leadership roster, straight from the ionetix.com page tree.
api: ionetix:ionetix-pages-api
base_url: https://ionetix.com/wp-json
auth: none (anonymous read)
operations:
  - getWpV2Pages
  - getWpV2PagesById
  - getWpV2Search
generated: '2026-08-23'
method: generated
source: openapi/ionetix-pages-api-openapi.yml, openapi/ionetix-search-api-openapi.yml
---

# Map the Ionetix product surface and leadership

## 1. List the whole page tree

    GET /wp/v2/pages?per_page=100&_fields=id,slug,link,parent,menu_order,title,modified
                                              # getWpV2Pages

29 pages at time of profiling. The product pages worth reading:

| slug | subject |
|---|---|
| `cardiac-pet` | cardiac PET perfusion imaging |
| `n-13ammonia` | the N-13 Ammonia tracer |
| `n-13-ammonia-sites` | the installed ION-12SC site network |
| `cyclotron-solutions` | the ION-12SC compact superconducting cyclotron |
| `alpha-therapy` | targeted alpha therapy |
| `actinium-225` | Ac-225 production |
| `astatine-211` | At-211 production |
| `psma-pet` | PSMA PET / Ga-68 Gozetotide |
| `education` | clinical education material |
| `careers`, `apply`, `position-interest-form` | hiring funnel |
| `about` | company, with leadership profiles as children |

## 2. Walk the leadership roster by hierarchy, not by guessing

Pages are self-referential through `parent`. The `/about/` page's id is the parent of every executive
profile page.

    GET /wp/v2/pages?parent=<about-id>&per_page=100&_fields=id,slug,link,title
                                              # getWpV2Pages

## 3. Fetch a page body

    GET /wp/v2/pages/{id}                     # getWpV2PagesById

`content.rendered` is Avada-theme HTML with layout shortcodes. Strip markup and shortcodes before
extracting claims, and quote the page URL when you cite anything.

## 4. Or search, when you do not know the slug

    GET /wp/v2/search?search=actinium&per_page=20&type=post&subtype=any
                                              # getWpV2Search

Search returns a thin projection: `id`, `title`, `url`, `type`, `subtype`. Follow `url` or re-fetch by
`id` against the right collection for the full record.

## Rules

- **Pace yourself:** `Crawl-delay: 3` in robots.txt; no rate-limit headers exist to tell you otherwise.
- **Read only.** Never write.
- **Marketing copy is a claim, not a fact.** These pages are the company's own positioning. Attribute
  anything you extract to `https://ionetix.com/<slug>/` rather than stating it as independent fact.
- **There is no site/dose/inventory API.** The N-13 Ammonia site network is described in prose on a
  page; it is not queryable. Do not synthesize a site list into a structured answer that implies an API
  returned it.
- **Regulated-product caution.** Ionetix makes radiopharmaceuticals. Do not derive dosing, clinical or
  treatment guidance from these pages.
