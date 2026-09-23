# Coupon-Page-Freshness-Detector

FreshCheck is an open-source web page freshness analyzer designed for coupon, affiliate, product, blog, comparison, and promotional pages.

It helps identify signals that may indicate a page needs review, including:

Outdated year references

Potentially expired promotions

Seasonal promotional language

Missing or old update signals

Broken internal links

Redirects

Merchant/content mismatches

Structured-data freshness signals

Stale promotional wording

Important: The included index.html is a static GitHub Pages demo. It does not crawl arbitrary websites from the browser. A production version should use a server-side crawler/API because browser CORS restrictions prevent reliable cross-origin crawling.

Demo

The current static demo accepts a URL and displays a deterministic example report so the interface can be published immediately on GitHub Pages.

The demo intentionally does not pretend that it has fetched the submitted website.

Features

Current frontend

Responsive landing page

URL analyzer form

Freshness score UI

Page status

Freshness signals

Promotion review section

Link-health section

Recommended actions

Mobile-friendly layout

No framework required

No build step required

Planned backend

A full crawler can add:

HTTP status checking

Redirect-chain detection

HTML extraction

Visible text extraction

Date detection

datePublished / dateModified extraction

HTTP Last-Modified detection

Coupon and promotion phrase detection

Expiry-date parsing

Seasonal content detection

Internal-link crawling

Broken-link detection

Canonical detection

Robots/indexability checks

OpenGraph metadata checks

JSON-LD/schema analysis

Merchant/entity detection

CSV bulk analysis

Historical freshness tracking

Suggested architecture

User
  |
  v
Static Frontend
  |
  v
API / Crawler
  |
  +--> Fetch page
  |
  +--> Parse HTML
  |
  +--> Extract visible text
  |
  +--> Detect dates
  |
  +--> Detect offers
  |
  +--> Check links
  |
  +--> Read metadata/schema
  |
  v
Freshness Scoring Engine
  |
  v
JSON Report
  |
  v
Results UI

Recommended production stack

Frontend

HTML/CSS/JavaScript for the simplest version

Or Next.js + TypeScript for a larger application

Tailwind CSS is optional

Backend

Recommended:

Node.js

Express or Fastify

Playwright

Cheerio

chrono-node

A JSON-LD parser

Database

For historical monitoring:

PostgreSQL

Supabase

SQLite for a local/self-hosted version

Hosting

Possible deployment options:

GitHub Pages — static frontend only

Vercel — frontend/API

Cloudflare Workers — lightweight API workloads

Render — Node.js crawler service

Railway — Node.js + database

Freshness scoring

FreshCheck should not claim that its score is a Google ranking score.

The score is an internal heuristic representing how many freshness signals deserve attention.

A starting scoring model:

Signal

Weight

Recent update evidence

20

Current promotional information

20

Expired offers

15

Outdated year references

10

Seasonal content

10

Broken links

10

Merchant consistency

5

Metadata freshness

5

Page accessibility

5

Total

100

The scoring system should expose the individual signals so users can understand why a page received its score.

Example report

ZARA RABATTCODE

Freshness Score
78 / 100

PAGE STATUS
✓ HTTP 200
✓ HTTPS
✓ Canonical detected
✓ Merchant detected

FRESHNESS SIGNALS
✓ Current-year references
⚠ Older-year references
⚠ Potentially stale promotion
✓ Update date found

PROMOTIONS
5 claims found
2 require review

LINK HEALTH
142 internal links
137 working
3 redirects
2 broken

RECOMMENDED ACTIONS
1. Review older promotional references
2. Check potentially expired offers
3. Review seasonal wording
4. Fix broken internal links
5. Verify visible offers against structured data

Coupon-page use case

FreshCheck is particularly useful for coupon websites where pages can contain:

Discount codes

Percentage discounts

Free-shipping offers

Student discounts

First-order offers

Seasonal promotions

Limited-time deals

Expiry dates

The tool should not claim that a coupon works merely because a code appears on a page.

Instead, it should report:

Potentially stale promotional claim — verify before publishing or retaining.

Actual coupon validation requires a separate checkout/test workflow.

Bulk analysis

A future version can accept a CSV:

url
https://example.com/zara-rabattcode
https://example.com/nike-rabattcode
https://example.com/adidas-rabattcode

and return:

url,score,status,issues
https://example.com/zara-rabattcode,92,fresh,0
https://example.com/nike-rabattcode,84,review,2
https://example.com/adidas-rabattcode,61,stale,5

This is particularly useful for large programmatic SEO websites.

Important crawling considerations

A production crawler should:

Respect robots.txt where appropriate

Use reasonable request rates

Set timeouts

Limit crawl depth

Limit maximum URLs

Identify itself with a clear User-Agent

Avoid crawling private/authenticated pages

Avoid bypassing access controls

Handle redirects safely

Prevent SSRF against internal/private IP ranges

Apply URL validation

Enforce response-size limits

Sanitize extracted HTML before rendering it

Avoid storing submitted URLs unnecessarily

If the service allows arbitrary URL fetching, SSRF protection is mandatory.

At minimum, the backend should reject localhost, loopback, link-local, private-network, and other internal addresses before making requests.

Local development

The current frontend requires no dependencies.

Simply open:

index.html

in a browser.

For local hosting:

python -m http.server 8080

Then open:

http://localhost:8080

GitHub Pages deployment

Create a new GitHub repository.

Add index.html.

Add README.md.

Commit and push.

Open the repository's Settings → Pages.

Select the branch containing index.html.

Select the root folder.

Save.

GitHub will publish the static demo.

The frontend can be hosted entirely as a static GitHub Pages project.

Suggested repository structure

freshcheck/
├── index.html
├── README.md
├── LICENSE
├── assets/
│   ├── favicon.svg
│   └── og-image.png
├── docs/
│   └── architecture.md
└── backend/
    ├── package.json
    ├── src/
    │   ├── crawler.js
    │   ├── dates.js
    │   ├── promotions.js
    │   ├── links.js
    │   ├── schema.js
    │   └── scoring.js
    └── README.md

Roadmap

v0.1

Static interface

URL input

Results UI

Responsive design

GitHub Pages compatibility

v0.2

Server-side URL fetching

HTTP status detection

HTML parsing

Date extraction

Metadata extraction

v0.3

Promotion detection

Expiry detection

Seasonal-content detection

Broken-link detection

v0.4

Real freshness scoring

JSON report

CSV export

Bulk URL analysis

v0.5

Historical snapshots

Scheduled monitoring

Email alerts

API

Team/agency projects

Contributing

Pull requests are welcome.

Before submitting a feature, consider whether it:

Produces an objectively useful signal.

Can explain its result to the user.

Avoids claiming certainty where only a heuristic is available.

Respects website owners and reasonable crawl limits.

Keeps the tool useful without requiring an AI model.

License

Choose a license before publishing. MIT is a straightforward option for a permissive open-source utility:

MIT License

If you use MIT, add a standard MIT LICENSE file with your name or organization and the current year.

Disclaimer

FreshCheck provides automated signals and heuristics. A freshness score is not a search-engine ranking score and does not guarantee that a page is outdated, current, indexed, or performing well in search.

Users should review flagged content before making changes.
