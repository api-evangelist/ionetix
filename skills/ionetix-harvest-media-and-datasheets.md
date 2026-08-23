---
name: ionetix-harvest-media-and-datasheets
description: >-
  Locate and resolve Ionetix media assets — the ION-12SC Cyclotron System data sheet PDFs, facility and
  product photography, and leadership portraits — through the ionetix.com media library API.
api: ionetix:ionetix-media-api
base_url: https://ionetix.com/wp-json
auth: none (anonymous read)
operations:
  - getWpV2Media
  - getWpV2MediaById
  - getWpV2Search
generated: '2026-08-23'
method: generated
source: openapi/ionetix-media-api-openapi.yml
---

# Harvest Ionetix media and data sheets

291 media items are exposed anonymously, including the technical PDFs that are the densest factual
material Ionetix publishes.

## 1. Find documents, not images

    GET /wp/v2/media?media_type=file&per_page=100
      &_fields=id,date,slug,link,title,media_type,mime_type,source_url
                                              # getWpV2Media

`media_type` is one of `image`, `file`, `video`, `audio`. Filter to `file` and then inspect
`mime_type` for `application/pdf` to isolate data sheets. The declared `mime_type` filter also works:

    GET /wp/v2/media?mime_type=application/pdf&per_page=100

## 2. Or search the library by title

    GET /wp/v2/media?search=cyclotron&per_page=50

The ION-12SC data sheet is published under `/wp-content/uploads/` and resolves directly from
`source_url` — no token, no referrer check.

## 3. Fetch one item

    GET /wp/v2/media/{id}                     # getWpV2MediaById

For images, `media_details.sizes` lists every generated derivative with its own `source_url`, width and
height. Pick the smallest size that meets your need rather than always taking `full`.

## 4. Tie an asset back to the page that uses it

Media rows carry `post`, the id of the post or page the file was uploaded to (`0` when unattached).
Resolve it against `/wp/v2/posts/{id}` or `/wp/v2/pages/{id}`.

## Rules

- **Pace yourself:** `Crawl-delay: 3`; there is no rate-limit header and no `Retry-After` to observe.
  Downloading 291 assets in a burst is exactly the behaviour the crawl delay exists to prevent.
- **Read only, and never delete.** `DELETE /wp/v2/media/{id}` is authentication-gated, and per
  `conventions/ionetix-conventions.yml` there is no reliable trash/restore path for attachments — treat
  it as unconditionally one-way and never call it.
- **Respect copyright.** These are Ionetix's assets. Link to `source_url`; do not rehost, and do not
  present the data sheets as your own.
- **PDFs are the good source.** When a marketing page and a data sheet disagree on a specification,
  cite the data sheet and say which document you used.
