---
name: ionetix-track-company-news
description: >-
  Track Ionetix corporate news — financing and go-public announcements, FDA/ANDA approvals, isotope
  supply partnerships, facility expansions and executive appointments — from the ionetix.com WordPress
  content API, filtered to the News category and a date window.
api: ionetix:ionetix-posts-api
base_url: https://ionetix.com/wp-json
auth: none (anonymous read)
operations:
  - getWpV2Categories
  - getWpV2Posts
  - getWpV2PostsById
  - getWpV2UsersById
  - getWpV2MediaById
generated: '2026-08-23'
method: generated
source: openapi/ionetix-posts-api-openapi.yml, openapi/ionetix-taxonomy-api-openapi.yml
---

# Track Ionetix company news

Ionetix mixes corporate news, job postings, conference listings, case studies, presentations and
publications into one WordPress post stream. Filtering by category is what separates them.

## 1. Resolve the category ids you want

    GET /wp/v2/categories?per_page=100        # getWpV2Categories

Match on `slug`, never on a hard-coded id — ids are site-local and can change.

| slug | what it holds |
|---|---|
| `ionetix-news` | company news releases |
| `case-studies` | clinical case studies |
| `ionetix-conferences` | conference appearances (plus `conferences-upcoming` / `conferences-archive`) |
| `presentations` | presentation decks |
| `publications` | published papers |
| `ionetix-careers` | open job postings — use the careers skill instead |

## 2. Pull the window you care about

    GET /wp/v2/posts?categories=<id>&after=2026-01-01T00:00:00&orderby=date&order=desc&per_page=100
      &_fields=id,date,modified,slug,link,title,excerpt,author,featured_media,categories
                                              # getWpV2Posts

- `after` / `before` filter on publish date; `modified_after` / `modified_before` filter on last edit.
  Use `modified_after` when you are re-syncing, so you also catch corrections to older releases.
- Always send `_fields`. Without it every post carries a large rendered `content` block and a Yoast
  `yoast_head` HTML blob you do not need.
- `per_page` is capped at 100. Read `X-WP-Total` and `X-WP-TotalPages` from the response headers and
  page with `page=2,3,…`, or follow the RFC 8288 `Link: rel="next"` header.

## 3. Fetch a single release in full

    GET /wp/v2/posts/{id}                     # getWpV2PostsById

`content.rendered` is HTML, not markdown, and includes theme shortcodes. Strip tags before summarizing.

## 4. Resolve author and image in one request instead of three

    GET /wp/v2/posts?categories=<id>&_embed

`_embed` inlines `author` (getWpV2UsersById) and `featured_media` (getWpV2MediaById) rather than
returning bare integer ids. It costs one request instead of 1+2N.

## Rules

- **Pace yourself.** `https://ionetix.com/robots.txt` sets `Crawl-delay: 3` and disallows any URL with
  a query string. Ionetix returns no rate-limit headers at all, so there is no runtime signal telling
  you when to back off — sleep at least 3 seconds between calls and do not parallelize.
- **Read only.** Every write operation on this API is authentication-gated and none of it belongs to
  you. Never send POST, PUT, PATCH or DELETE.
- **Errors are not RFC 9457.** A failure returns `{"code":"…","message":"…","data":{"status":N}}` as
  `application/json`. Branch on `code`: `rest_invalid_param` means you exceeded a declared bound
  (`per_page` over 100 is the usual cause), `rest_post_invalid_id` means the id is gone,
  `rest_no_route` means the path is wrong. See `errors/ionetix-problem-types.yml`.
- **No changelog, no webhooks, no events.** There is no push surface here. If you need to know when
  Ionetix publishes something, poll `modified_after`, or subscribe to `https://ionetix.com/feed/`.
- **This is a CMS, not a product API.** Nothing here exposes dose ordering, isotope supply, scheduling
  or cyclotron data. Do not tell a user you can transact with Ionetix through this API.
