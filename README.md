# NYC Event Vendor Index (open data)

3,538 event vendors in New York City, Long Island, Westchester and nearby New Jersey: photographers, DJs, caterers, florists, planners, videographers, live bands, photo booths, hair and makeup artists, and cake and dessert makers. Each is rated on how well an AI assistant can find, price and book them, with the facts behind the rating and the page each fact came from. The data also includes the **NYC Event Price Index**: what these vendors charge, by category and by area.

This is the open data behind the [Agent Bookability Index](https://index.therunup.ai/). The site is rebuilt every night; this repository gets a monthly release.

**Release:** 2026-11-15 (data checked between 2026-10-22 and 2026-11-13)

## What event vendors charge in New York

The price figures are also published as pages, rebuilt every night from the same data: the [NYC Event Price Index](https://index.therunup.ai/prices/) with every category and area, and pages such as [what a caterer costs per guest](https://index.therunup.ai/cost/caterer-per-guest/), [what a wedding photographer costs in NYC](https://index.therunup.ai/cost/wedding-photographer/nyc/) and [what a wedding DJ costs in NYC](https://index.therunup.ai/cost/wedding-dj/nyc/). Findings about how vendors publish prices: [facts](https://index.therunup.ai/facts/).

## Query it live from an AI agent (MCP)

The same data, refreshed every night, is available to AI agents through a read-only MCP server, no key needed:

```
https://index.therunup.ai/mcp
```

Tools: `shortlist_vendors` (the most bookable matching vendors for each service an event needs, by area, event type, budget, guests and hours), `search_vendors` (also by needs such as kosher catering or an MC), `get_vendor`, `compare_vendors`, `get_price_summary` (what a service costs in an area, in one sentence with its source), `estimate_event_budget` (what all of an event's services cost together), `list_categories`, and `search` / `fetch` for ChatGPT. Without MCP, the same tools are plain GET requests described at https://index.therunup.ai/openapi.json . Every answer carries the page to cite and the date the vendor site was read. How to connect: https://index.therunup.ai/agents/ . Listed in the official MCP Registry as `ai.therunup.index/agent-bookability-index`.

## Files

| File | What it holds |
|---|---|
| `data/vendors.csv` | One row per vendor: name, index page, website, category, areas, lowest published price, score, date checked |
| `data/vendors.json` | The same vendors plus every extracted fact, each with its source page and the date it was checked |
| `data/price-index.json` | The NYC Event Price Index |
| `data/price-index-by-category.csv` | Price quartiles per category |
| `data/price-index-by-area.csv` | Price quartiles per category and area |
| `data/price-index-per-unit.csv` | Quartiles for prices stated per hour or per guest |

### vendors.csv columns

| Column | Meaning |
|---|---|
| `name` | Vendor name as shown on its website |
| `url` | The vendor's page on the index |
| `website` | The vendor's own website |
| `category` | `photographers`, `djs`, `caterers`, `florists`, `planners`, `videographers`, `bands`, `photo-booths`, `hair-and-makeup` or `cakes-and-desserts` |
| `areas` | Areas served, separated by `;` (for example `brooklyn;manhattan`) |
| `price_from_usd` | Lowest price the vendor publishes, in US dollars; empty when none is published |
| `score` | Agent Bookability score, 0 to 100 (see below) |
| `checked` | Date the vendor's website was last read |

### Facts in vendors.json

Each vendor has a `facts` object. Every fact has a `value`, the `source` page it was read from and the date it was `checked`. Common facts:
- `services`, `event_types`, `areas_served`, `does_private_events`, `contact_methods`;
- pricing: `starting_price`, `typical_price_range`, `packages`, `price_per_hour`, `price_per_guest`, `deposit`, `minimum_hours`, `minimum_guests`;
- booking: `quote_request_form`, `instant_quote`, `online_booking`, `booking_lead_time`, `cancellation_policy`;
- category details such as `cuisines`, `kosher`, `second_shooter`, `hours_included`, `equipment_included`, `planning_tiers`.

## How the score works

The score is out of 100 and uses only what each vendor publishes on its own website:
- **Reachable (5):** the site loads, AI assistants are allowed in robots.txt, and text shows without JavaScript.
- **Readable (10):** services, areas and event types are stated; the site has structured data.
- **Priced (30):** a price is published (18); packages or a price range (6); minimums are stated (6).
- **Availability (25):** open dates are shown online.
- **Bookable (30):** book or pay a deposit online (30), or an instant quote (18), or an online inquiry form (6).

Weights were set on 2026-11-08; any change is dated on the [method page](https://index.therunup.ai/method/). The score measures how easily an AI assistant can find, price and book a vendor. It does not rate the quality of their work.

## How the price index works

Prices come only from vendors that publish them. Each figure is the lower quartile, median and upper quartile of published prices, with the number of vendors behind it. Figures from fewer than 8 vendors are not shown. Live version: [index.therunup.ai/prices/](https://index.therunup.ai/prices/).

## Limits

- We read sites as RunupIndexBot (https://index.therunup.ai/bot) and respect each site's robots.txt. We read up to 15 pages per website, so facts in PDFs, images, pages that need JavaScript or pages we did not reach can be missing.
- A missing fact means we did not find it, not that the vendor does not offer it.
- Vendors whose website we could not read are left out until we can, and so are businesses whose site shows nothing about events.
- A vendor that serves several areas counts in each of them, so neighbouring areas can show the same figures. Some figures come from few vendors (8 at least); the vendor count is given with each one.
- Prices change. Confirm details with the vendor before booking.
- No email addresses or phone numbers are included. Where a vendor's own site names its staff in a price or policy (for example which photographer covers a package), that wording is kept as published.

## License and credit

The data is licensed under [CC BY 4.0](LICENSE). Please credit: **Agent Bookability Index by The Runup** (https://index.therunup.ai/).

The license covers our compilation, the scores and the values we extracted. It does not cover vendors' own words, names or trademarks.

**Sources.** The list of vendors was derived in part from [Overture Maps Foundation](https://overturemaps.org/) Places data, licensed under [CDLA-Permissive-2.0](https://cdla.dev/permissive-2-0/), and [Foursquare Open Source Places](https://opensource.foursquare.com/os-places/), licensed under [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0). See [NOTICE](NOTICE). Facts come from each vendor's own website.

**No warranty.** The data is provided as is, without warranty of any kind. Vendors' names and trademarks belong to their owners; no vendor listed is affiliated with or endorses The Runup or this data.

## Disclosure

The Runup, which runs this index, also builds software for event vendors. The same rules and scoring apply to every vendor, and no vendor can pay for a higher score or a better place in a list.

## Corrections

Any vendor, including a sole proprietor listed under their own name, can correct or remove their listing at [index.therunup.ai/claim/](https://index.therunup.ai/claim/) or by emailing info@therunup.ai. Corrections appear on the site the next night and in the next release here.

## Cite

```
The Runup. Agent Bookability Index: NYC event vendors and NYC Event Price Index, release 2026-11.
https://index.therunup.ai/ (data: https://github.com/therunup/nyc-event-vendor-index). CC BY 4.0.
```
