# DeFlock Utah

An independent civic-transparency project tracking automated license plate reader (ALPR)
and Flock Safety camera deployments across all 255 incorporated cities and towns in Utah,
with links to each department's policies and financial records.

Not affiliated with Flock Safety, DeFlock.me, or the State of Utah.

## Repo layout

```
index.html                  ← the site (no need to edit)
data/municipalities.json    ← base list of all 255 cities/towns (don't edit)
data/records.json           ← ★ THE FILE YOU EDIT ★
README.md                   ← this file
```

## How to add or update a city

1. On GitHub, open `data/records.json` and click the ✏️ pencil icon.
2. Find the city (entries are keyed by exact city name) or add a new one.
3. Commit directly to `main`. GitHub Pages redeploys automatically
   (usually live within a minute or two — hard-refresh the page).

If you make a typo that breaks the JSON, the site will show an error banner
telling you so. Fix it by pasting the file into [jsonlint.com](https://jsonlint.com)
to locate the bad line, or open the file's **History** on GitHub and revert
the last commit.

### Entry template

Copy this into the `"records"` object. The key (`"CityName"`) must **exactly**
match the name in `data/municipalities.json` — e.g. `"Clinton"`, `"West Valley City"`,
`"St. George"`.

```json
"CityName": {
  "status": "flock",
  "agency": "CityName Police Department",
  "vendor": "Flock Safety",
  "cameras": 5,
  "since": "2025",
  "financial": "$25,000/yr contract",
  "note": "Anything worth flagging, e.g. retention period.",
  "submitted": true,
  "submittedNote": "Documents provided by City of X.",
  "documents": [
    { "type": "policy",    "label": "ALPR policy (PDF)",    "url": "https://..." },
    { "type": "financial", "label": "Flock contract (PDF)", "url": "https://..." }
  ],
  "sources": [
    { "label": "City council minutes, Jan 2026", "url": "https://..." }
  ]
}
```

### Field guide

| Field | Values / notes |
|---|---|
| `status` | `"flock"` = confirmed Flock Safety · `"alpr_other"` = confirmed ALPR, other/unknown vendor · `"none"` = city confirmed it has **no** ALPR |
| `cameras` | A number. Delete the line if unknown. |
| `since` | Free text — `"2025"`, `"Sept. 2025"`, `"by July 2025"` all fine. |
| `financial` | Free text summary, e.g. dollar amounts from Transparent Utah. |
| `submitted` | `true` shows the green "Submitted by city / resident" badge. |
| `documents[].type` | `"policy"`, `"financial"`, `"contract"`, `"minutes"`, or `"other"` — controls the little tag on the button. |
| everything except `status` | Optional. Delete lines you don't need. **Watch the commas** — no trailing comma after the last item in a list or object. |

### Hosting submitted PDFs

When a city or resident sends you an actual PDF (rather than a link), commit it
into a `docs/` folder in this repo, e.g. `docs/clinton-alpr-policy.pdf`, then
reference it with a relative URL:

```json
{ "type": "policy", "label": "Clinton ALPR policy (PDF)", "url": "docs/clinton-alpr-policy.pdf" }
```

That way the document is preserved even if the city later removes it from
their own website, and its addition is timestamped in the git history.

### Other settings in records.json

| Key | Purpose |
|---|---|
| `formUrl` | Your Google Form link. Feeds the header button and every per-city "Submit a record" link. |
| `lastUpdated` | Shown in the site footer — bump it when you commit new data. |

## Data sources & verification

- [Transparent Utah](https://transparent.utah.gov/) — official state vendor-payment portal; search a city, filter payee "Flock Safety"
- [Atlas of Surveillance (EFF)](https://www.atlasofsurveillance.org/search?location=Utah)
- [DeFlock.me](https://deflock.me/) — crowdsourced map of individual camera locations
- [Eyes on Flock](https://eyesonflock.com/) — aggregated Flock transparency-portal data
- Utah retention baseline: Utah Code §41-6a-2005 (nine-month cap on ALPR data retention)

"Unknown" status means no public record was found — **not** that a city has no cameras.
