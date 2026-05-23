# Condé Nast

Condé Nast is a global media company owned by Advance Publications,
producing editorial content across more than two dozen iconic brands —
Vogue, GQ, The New Yorker, Wired, Vanity Fair, Bon Appétit,
Architectural Digest, Pitchfork, Allure, Glamour, Self, Teen Vogue,
Condé Nast Traveler, Epicurious, Tatler, House & Garden, The World of
Interiors, Ars Technica, Vogue Business, La Cucina Italiana and
Johansens.

There is no public Condé Nast API. The publicly observable surface is a
federated set of brand-level RSS feeds, schema.org NewsArticle JSON-LD
on every article page, monthly XML sitemaps, syndicated video on
YouTube, and an authenticated-only internal developer portal at
`technology.condenast.com`. The CMS powering every site is the
internally-developed **Copilot** platform — visible publicly only
through `github.com/CondeNast/copilot-util` and the auth-gated portal.

This profile catalogs those surfaces and gives them a normalized shape.

## Content surfaces

| Surface | Path / location | Notes |
| --- | --- | --- |
| Brand RSS feed | `/feed/rss` on every brand domain | RSS 2.0 + Dublin Core + Media RSS + Atom; CloudFront-cached; soft rate limit of 100 req/window via `x-ratelimit-userlimit` |
| Article JSON-LD | `<script type="application/ld+json">` on every article page | schema.org `NewsArticle` + `BreadcrumbList` |
| Sitemap index | `/sitemap.xml` on every brand domain | Per-month sub-sitemaps (`/sitemap-YYYY-MM.xml`) going back several years |
| Video | Brand-level YouTube channels (`@WIRED`, `@Vogue`, `@GQ`, `@VanityFair`, `@bonappetit`, etc.) | No public catalog API; use YouTube Data API against the channel handle |
| Newsletters / podcasts | Per-brand opt-in pages and standard podcast RSS | No API |
| Internal developer portal | `technology.condenast.com` | Returns HTTP 307 to `/auth/signin` — not public |

## Brands covered

US flagships (`/feed/rss` confirmed live, May 2026):

- Vogue · vogue.com
- GQ · gq.com
- The New Yorker · newyorker.com
- Wired · wired.com
- Vanity Fair · vanityfair.com
- Bon Appétit · bonappetit.com
- Architectural Digest · architecturaldigest.com
- Pitchfork · pitchfork.com
- Allure · allure.com
- Glamour · glamour.com
- Self · self.com
- Teen Vogue · teenvogue.com
- Condé Nast Traveler · cntraveler.com
- Epicurious · epicurious.com

International editions of Vogue (`/feed/rss` confirmed live): UK, France,
Italy, Mexico & Latin America, Spain, India, Taiwan. International
editions of GQ: UK, Italy, Mexico.

Additional brands listed in the corporate brand book but not separately
profiled here: Vogue Business, Tatler, House & Garden, The World of
Interiors, Ars Technica, La Cucina Italiana, Johansens.

## Repository layout

```
conde-nast/
├── apis.yml                                        # APIs.json — five logical surfaces
├── README.md                                       # this file
├── json-ld/
│   └── conde-nast-context.jsonld                   # JSON-LD context bridging RSS + schema.org
├── json-schema/
│   └── conde-nast-article-schema.json              # normalized article schema
├── vocabulary/
│   └── conde-nast-vocabulary.yml                   # editorial / brand vocabulary
└── examples/
    ├── wired-article-example.json                  # NewsArticle from Wired
    └── epicurious-recipe-example.json              # Recipe roundup from Epicurious
```

## What is NOT in this repo (and why)

- **No `openapi/`** — Condé Nast publishes no OpenAPI spec; the content
  surface is RSS and embedded JSON-LD, neither of which is a REST API.
- **No `rules/`** — without OpenAPI there is nothing for Spectral to
  lint.
- **No `capabilities/`** — Naftiko capabilities target programmable APIs;
  there is no programmable API here to model.
- **No `plans/`, `rate-limits/`, `finops/`** — Condé Nast does not
  publish per-API pricing, rate limits or billing surfaces. Content
  licensing is bilateral and brokered through
  `condenast.com/contact`.
- **No status page** — Condé Nast does not run a public status page.

## Notable findings

- The `/feed/rss` pattern is universal across US flagships and across
  international Vogue and GQ editions — making the brand portfolio
  trivially crawlable as a single homogeneous federation of feeds.
- Every brand site embeds two JSON-LD blocks per article
  (`NewsArticle` + `BreadcrumbList`) — the practical contract for any
  programmatic consumer.
- The `CondeNast` GitHub org has 142 public repositories but is
  dominated by tooling (Puppet, Ember, React utilities), hackathon
  experiments and the public face of Copilot
  (`CondeNast/copilot-util`) — no SDKs, no public API client libraries.
- `technology.condenast.com` exists but is locked behind a sign-in —
  Condé Nast operates a real internal developer platform, just not a
  public one.

## Scale (from public sources)

- "More than 72 million consumers in print"
- "394 million in digital"
- "454 million across social media platforms"

## Ownership

Condé Nast is wholly owned by **Advance Publications**, the privately
held S.I. Newhouse family holding company.

## Maintainer

Kin Lane — kin@apievangelist.com
