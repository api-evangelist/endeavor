# Endeavor (endeavor)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Endeavor was a global sports and entertainment company representing talent and owning and operating events, with subsidiaries including WME, IMG, and UFC. Following the 2024 take-private transaction by Silver Lake and the separation of TKO Group Holdings (UFC and WWE), the remaining talent, media, marketing, and licensing businesses were rebranded as WME Group. This repository tracks Endeavor as a corporate entity. It publishes no developer program, no API documentation and no OpenAPI; the only machine-readable surfaces on its own hosts are the wmegrp.com WordPress content API, which is anonymously readable, and a live but authentication-gated WordPress MCP Adapter endpoint.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/endeavor/refs/heads/main/apis.yml)

## Scope

- **Type:** Contract
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Sports, Entertainment, Talent, Media, Licensing, Marketing

## Timestamps

- **Created:** 2026-03-21
- **Modified:** 2026-04-28

## APIs

Endeavor / WME Group runs **no developer program**. There is no developer portal, no API
documentation, no SDK, no published OpenAPI, and no `api.` / `developer.` / `docs.` host in DNS on
either `wmegrp.com` or `wme.com`. Every `/.well-known/` path returned HTTP 404 on every WME Group
host probed.

Two machine-readable surfaces do exist on the company's own host, both belonging to its corporate
site's CMS rather than to a product:

### WME Group Content API (WordPress REST wp/v2)

`https://wmegrp.com/wp-json/wp/v2` — the WordPress REST API behind the corporate site. Anonymously
readable for press releases (6), corporate pages (11), the media library (177), taxonomies, site
search and content-type discovery; administrative collections (`users`, `settings`, `themes`,
`plugins`, `menus`, `widgets`, `templates`) return HTTP 401. Page-number pagination with
`X-WP-Total` / `X-WP-TotalPages` / RFC 8288 `Link` headers; errors use WordPress's
`{code,message,data.status}` envelope, not RFC 9457.

- [OpenAPI (derived)](openapi/endeavor-content-api-openapi.yml) — 120 paths, 256 operations,
  transcribed mechanically from `https://wmegrp.com/wp-json/`. **Not published by the company**
  (`x-provider-published: false`).
- [Error catalog](errors/endeavor-problem-types.yml) · [Rate limits](rate-limits/endeavor-rate-limits.yml)

### WME Group MCP Server (WordPress MCP Adapter)

`https://wmegrp.com/wp-json/mcp/mcp-adapter-default-server` — a live Model Context Protocol endpoint
registered by the site's WordPress MCP Adapter. The namespace index answers HTTP 200; an anonymous
`tools/list` returns HTTP 401 `rest_forbidden`, as does the backing `wp-abilities/v1` registry, so
the tool set is **unknown, not empty**.

- [MCP server profile](mcp/endeavor-mcp.yml) · [Tool crosswalk](mcp/endeavor-tool-crosswalk.yml)

## Artifacts

- [Authentication](authentication/endeavor-authentication.yml) ·
  [Conventions](conventions/endeavor-conventions.yml) ·
  [Conformance](conformance/endeavor-conformance.yml) ·
  [Lifecycle](lifecycle/endeavor-lifecycle.yml)
- [Plans and pricing](plans/endeavor-plans-pricing.yml) (none published) ·
  [Well-known probe](well-known/endeavor-well-known.yml) (all 404) ·
  [Domain security](security/endeavor-domain-security.yml)
- [Agent skills](skills/_index.yml) · [llms.txt](llms/endeavor-llms.txt)

## Common Properties

- [Website](https://wmegrp.com/)
- [Privacy Policy](https://wmegrp.com/privacy-policy/)
- [Terms of Use](https://wmegrp.com/terms-of-use/)
- [Careers](https://wmeimg.wd1.myworkdayjobs.com/WMEGRP)
- [LinkedIn](https://www.linkedin.com/company/endeavor-co)
- [Successor: WME Group](https://wmegrp.com/)
- [Spinoff: TKO Group Holdings](https://www.tkogrp.com/)

## Businesses

WME Group comprises [WME](https://www.wmeagency.com/) (talent agency),
[160over90](https://www.160over90.com/) (marketing), [IMG Licensing](https://imglicensing.com/) and
[Pantheon Media Group](https://www.pantheonmedia.com/) (non-scripted content). None of them publishes
an API. `img.com` — IMG's sports rights, media and events business — serves a real `llms.txt`, but
IMG sits under **TKO Group Holdings**, not WME Group, so nothing from that host is credited here.

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
