# Ionetix

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

Ionetix (IONETIX Corporation) is a Lansing, Michigan radiopharmaceutical and cyclotron company. It
built the first commercial compact superconducting cyclotron (ION-12SC) and runs a network of them at
hospital PET programs to produce N-13 Ammonia for cardiac PET perfusion imaging, sold pay-by-the-dose,
alongside a Lansing alpha-isotope facility producing Actinium-225 and Astatine-211 for targeted alpha
therapy.

- Website: https://ionetix.com/
- News & Events: https://ionetix.com/news-events/
- Careers: https://ionetix.com/careers/

## What this profile found

**Ionetix publishes no developer platform.** There is no API documentation, no developer portal, no
SDK, no OpenAPI, no MCP server, no A2A agent card, no `.well-known` surface, no webhooks, no status
page and no changelog. All of that was probed and recorded, not assumed — see
`well-known/ionetix-well-known.yml`, `mcp/ionetix-mcp.yml` and `lifecycle/ionetix-lifecycle.yml`.

What ionetix.com *does* serve is a live, anonymously readable **WordPress REST API** at
`https://ionetix.com/wp-json/`. Its `wp/v2` namespace returns the company newsroom, careers postings,
conference and case-study archives, product pages, leadership profiles, the media library (including
the ION-12SC Cyclotron System data sheet PDFs), taxonomies and a site-wide search endpoint as JSON.

The nine OpenAPI documents in `openapi/` were **derived mechanically** by API Evangelist from that
site's own route-discovery index and from the JSON Schema each collection returns to an HTTP `OPTIONS`
request — 44 operations, with every path, parameter and schema taken verbatim from those descriptors.
Ionetix did not author them, and they describe a CMS content API, not a product API: nothing here
exposes dose ordering, isotope supply, scheduling, cyclotron telemetry or clinical data.
