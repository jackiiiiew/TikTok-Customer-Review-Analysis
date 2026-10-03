# TikTok Customer Review Analysis

Collect and analyze public TikTok app reviews from Google Play and Trustpilot for a rolling 30-day window. The workflow writes normalized review records, per-source collection status, and an interactive HTML report.

## Report

Once GitHub Pages is enabled for the `main` branch with the repository root as its source, view the interactive report at [TikTok Customer Review Analysis](https://jackiiiiew.github.io/TikTok-Customer-Review-Analysis/report.html).

## Requirements

- Python 3.9 or newer
- Network access to the public review pages

Install dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

## Run

From this directory:

```bash
python scrape_reviews.py --days 30
python analyze_reviews.py --days 30
```

The collector saves `data/reviews.csv` and `data/collection_status.json`. The analysis script writes `report.html`. To change the window, pass the same `--days` value to both commands. Trustpilot pagination is capped at 100 pages by default; adjust with `--trustpilot-max-pages` if needed.

## Record fields

Each CSV row includes source, review identifier, author, star rating, UTC review date, title, review text, helpful count, language, app version, and source URL. Fields that a source does not expose are left empty.

## Collection and interpretation notes

- Google Play reviews are requested in English, for the US storefront, newest first. Pagination stops once results pass the cutoff.
- Trustpilot is read from its public review pages. If it returns an access-denied/rate-limit response (403/429), or its page markup does not expose supported review records, the collector stops and records that outcome; it does not evade the restriction.
- “Past month” means the last 30 days from the time the collector runs. Source availability, pagination limits, and site access can prevent a complete census; check `data/collection_status.json` before interpreting coverage.
- Sentiment in the report is a VADER text score, not a star-rating substitute. Theme counts use transparent keyword matching and are exploratory, not a trained topic model.
- Follow each source's terms, robots policy, and applicable privacy rules. The CSV includes public author names only when exposed by the source.