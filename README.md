# How to get Google Maps business contact data via API

You need a list of local businesses — say, every dentist in Berlin or pizzeria in Madrid — with a working public email or phone for each one. This guide shows the manual route, where it grinds, and the automated route.

## The manual way, and its friction

Google Maps is built for looking at one business at a time. Building a list by hand means:

- Running a search, then opening every result card one by one to copy the name, address, phone, and website.
- Then visiting each business's own website separately to hunt for the contact page — because the email address usually is not on Maps at all.
- Then deduplicating by hand when the same business shows up under several searches.

An afternoon of clicking gets you a few dozen rows, with gaps.

## The Actor way

The [Google Maps Business Contact Extractor](https://apify.com/apitory/google-maps-contacts) on Apify does both phases in one run. It searches Google Maps for your terms and returns one structured record per business — name, category, address, phone, website, rating, review count — and then visits each business's own website (homepage plus likely contact pages) to extract the **public** contact details it publishes: emails, phone numbers, and social profile links. Places are deduplicated by Google place ID across all your search terms before anything is charged.

Quickstart input (Console or Apify API, same JSON):

```json
{
  "searchTerms": ["dentist in Berlin"],
  "maxPlaces": 20
}
```

Contact extraction is on by default; turn `extractContacts` off for place data only. Anywhere Google Maps has listings works — set `proxyCountry` to the search's country for the most consistent results.

## Sample output

One real record from a Madrid run — the full record is in [`sample-output.json`](sample-output.json). Contact values are redacted in this public sample; real runs return them in full.

```json
{
  "name": "Baldoria Madrid",
  "category": "Restaurante italiano",
  "address": "C. de José Ortega y Gasset, 100, Salamanca, 28006 Madrid",
  "website": "https://baldoriamadrid.com/",
  "rating": 4.8,
  "reviewsCount": 28976,
  "contactFound": true
}
```

In a 12-place Madrid restaurant test, 11 places had websites, 8 of 11 (73%) produced a public contact and 7 of 11 (64%) a public email. Hit rates vary by niche and country — restaurants and local services publish contacts often; some trades and regions publish far fewer.

## What people use it for

- **Outreach lists for local services** — every dentist, plumber, or pizzeria in a city, with a working public email or phone, ready for a CRM or sequencer.
- **Agency lead generation** — local businesses in a niche, including the ones whose websites and listings you could improve.
- **Local market research** — who is in a niche in a city, how they are rated, and how they can be contacted.
- **List enrichment** — start from Maps searches instead of stale directories, so contacts are the ones businesses publish today.

## Pricing

Pay per event, in two tiers:

| Event | Price | When it is charged |
|---|---|---|
| Actor start | $0.00005 per GB of memory | Once, when the run starts |
| Place scraped | $0.004 / place | For every place record in the dataset |
| Lead scraped | $0.10 / lead | **Only** when the business's website yields at least one public email or phone |

No contact found → no lead charge. No website → no lead charge. You never pay the premium for an empty record, and duplicates are never charged.

## Run it

**[Google Maps Business Contact Extractor — Apify Store](https://apify.com/apitory/google-maps-contacts)**

Public data only: the Actor collects contact details that businesses publish on Google Maps and on their own websites. Two honest limits from the listing: website fetching is plain HTTP, so a minority of sites that block datacenter IPs or hide contacts behind JavaScript come back with empty contact arrays (and are not charged as leads); and phone numbers found in page text rather than `tel:` links can include false positives — treat text-only numbers as unverified. Support: apitoryhq@gmail.com.

---

## More from Apitory

Same family, different cut:

- [Google Maps Home Services Leads](https://github.com/Apitory/google-maps-home-services-leads) — run it on Apify: https://apify.com/apitory/google-maps-home-services-leads
- [Google Maps Dentist Leads](https://github.com/Apitory/google-maps-dentist-leads) — run it on Apify: https://apify.com/apitory/google-maps-dentist-leads

Full catalogue: https://apitory.com · All Actors: https://apify.com/apitory · All projects: https://github.com/Apitory
