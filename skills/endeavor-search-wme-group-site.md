---
name: endeavor-search-wme-group-site
description: >-
  Search everything WME Group (formerly Endeavor Group Holdings) publishes on wmegrp.com and get back
  typed JSON results, using the site's own WordPress search endpoint instead of scraping HTML.
api: WME Group Content API (WordPress REST wp/v2)
base_url: https://wmegrp.com/wp-json
auth: none (anonymous read)
operations:
  - getSearch
  - getTypes
  - getTaxonomies
  - getPagesById
generated: '2026-09-06'
method: generated
source: >-
  Grounded in openapi/endeavor-content-api-openapi.yml, transcribed from
  https://wmegrp.com/wp-json/. Behaviour observed on live anonymous requests on 2026-09-06.
---

# Search the WME Group corporate site

Use the site's own search endpoint rather than fetching and parsing `wmegrp.com` HTML. It returns
typed, linkable results and needs no credentials.

## Steps

1. **See what content types exist** — `getTypes` and `getTaxonomies`

   `GET /wp/v2/types` and `GET /wp/v2/taxonomies`

   Both answer HTTP 200 anonymously. Beyond core `post` and `page`, this site registers custom types
   including `businesses` and `businesses_cities` — the WME Group businesses directory. Read the
   response rather than assuming a fixed list; the set is site-specific and can change.

2. **Search** — `getSearch`

   `GET /wp/v2/search?search=<terms>&per_page=20`

   Contract parameters: `search`, `type`, `subtype`, `page`, `per_page`, `include`, `exclude`,
   `context`. Narrow with `subtype` once step 1 has told you which subtypes are real.

   Each result carries `id`, `title`, `url`, `type` and `subtype` — enough to link a user straight to
   the page without a second call.

3. **Fetch the full record**

   Use the result's `subtype` to choose the collection: `page` results resolve through
   `getPagesById` (`GET /wp/v2/pages/{id}`), `post` results through `getPostsById`.

## Notes and limits

- The corpus is genuinely small: four pages plus six press releases on 2026-09-06. An empty result
  set usually means the site does not cover the topic, not that the query was malformed.
- `X-WP-Total` and `X-WP-TotalPages` headers are returned; `Link: rel="next"` paginates.
- Sibling WME Group brands run separate sites with their own WordPress APIs
  (`https://www.wmeagency.com/wp-json/`) and are **not** searched by this endpoint. `img.com` is a
  TKO Group property, not WME Group — do not fold its content into a WME Group answer.

## Errors

Same WordPress envelope as the announcements skill — branch on `code`. `rest_forbidden` (401) means
a gated collection or `context=edit`; fall back to `context=view`. See
`errors/endeavor-problem-types.yml`.
