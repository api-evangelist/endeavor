---
name: endeavor-read-corporate-announcements
description: >-
  Read WME Group (formerly Endeavor Group Holdings) corporate announcements and press releases as
  JSON from the company's own WordPress content API, with correct pagination and error handling.
api: WME Group Content API (WordPress REST wp/v2)
base_url: https://wmegrp.com/wp-json
auth: none (anonymous read)
operations:
  - getPosts
  - getPostsById
  - getCategories
  - getMediaById
generated: '2026-09-06'
method: generated
source: >-
  Grounded in openapi/endeavor-content-api-openapi.yml, which was transcribed from
  https://wmegrp.com/wp-json/. Every operationId and parameter below appears in that contract; every
  behaviour was observed on a live anonymous request on 2026-09-06.
---

# Read WME Group corporate announcements

WME Group is the renamed Endeavor Group Holdings. It publishes no developer API, but its corporate
site runs WordPress and the content collections answer anonymously. This is a **read-only** flow.

## Before you start

- Base URL: `https://wmegrp.com/wp-json`
- No credentials. Do not send an `Authorization` header; you do not have one and the write surface
  is not open to the public.
- The archive is small — `X-WP-Total: 6` on 2026-09-06 — and `Cache-Control: public, max-age=604800`.
  Cache for a week. Do not poll.

## Steps

1. **List announcements** — `getPosts`

   `GET /wp/v2/posts?per_page=20&orderby=date&order=desc`

   Useful parameters from the contract: `page`, `per_page` (max 100), `offset`, `search`, `after`,
   `before`, `categories`, `orderby`, `order`, `_fields`, `_embed`, `context`.

   Read `X-WP-Total` and `X-WP-TotalPages` from the response headers to size the archive before
   paging, and follow `Link: <...>; rel="next"` rather than incrementing `page` blindly.

   Keep the payload small with `_fields=id,date,slug,link,title,excerpt`.

2. **Fetch one announcement in full** — `getPostsById`

   `GET /wp/v2/posts/{id}`

   `title.rendered`, `content.rendered` and `excerpt.rendered` contain HTML, not plain text. Strip or
   render it; do not treat it as a string literal.

3. **Resolve the category labels** — `getCategories`

   `GET /wp/v2/categories` — the `categories` array on a post holds numeric term IDs, not names.

4. **Resolve a featured image** — `getMediaById`

   `GET /wp/v2/media/{id}` using the post's `featured_media` id, or skip both calls by requesting
   `?_embed` on step 1 and reading `_embedded['wp:featuredmedia']`.

## Errors

The envelope is `{"code": "...", "message": "...", "data": {"status": N}}` — branch on `code`, never
on `message`. It is not RFC 9457 problem+json.

- `rest_no_route` (404) — wrong path or method. Re-read `https://wmegrp.com/wp-json/`. Do not retry.
- `rest_post_invalid_id` / 404 on step 2 — the id does not exist. Do not retry.
- `rest_invalid_param` (400) — a parameter failed validation; `data.params` names it. Fix and retry.
- `rest_forbidden` (401) — you asked for a gated collection (`/wp/v2/users`, `/wp/v2/settings`) or
  passed `context=edit`. Drop back to `context=view`. Retrying without credentials will not help.

Full catalog: `errors/endeavor-problem-types.yml`.

## Rules

- **Read-only.** Every write on this API is authentication-gated, has **no idempotency mechanism**,
  and media deletion is irreversible. Do not attempt a write.
- No rate-limit headers are returned. There is no backoff signal, so be conservative: cache, and
  respect the week-long `Cache-Control`.
