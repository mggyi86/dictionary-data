# Smoke Test Results

Generated during implementation on 2026-05-29.

## Summary

| Language | Hit rate | Notes |
|----------|----------|-------|
| english | ~52–100% | Full word-list API works; use rate limiting (1s+ delay) |
| german, french, spanish, … (22 others) | 0% | Seed words returned not-found pages with correct `solId` cookie |

Full machine-readable results: [`smoke_test_summary.json`](smoke_test_summary.json)

## Findings

1. **English–Myanmar** is fully crawlable via `/api/oem/word/list` and `/definition?q=...` with `solId=en`.
2. **Myanmar words** resolve via the English dictionary context and the `/api/ome/word/suggest` API.
3. **Other 22 language dictionaries** expose landing pages and word counts, but server-side `/definition` lookups returned not-found for all tested seed words even with the correct `solId` cookie set after visiting `/dictionary/{slug}`.
4. Use **`cache/seeds/{slug}.txt`** to add language-specific seed words if additional discovery paths are found later.

## Commands

```bash
# Quick English test (5 items)
PYTHONPATH=. python3 -m scrapy crawl dictionary -a languages=english -a max_words=5 -s CLOSESPIDER_ITEMCOUNT=5

# Per-language smoke test (50 words each, ~20+ min for all languages)
PYTHONPATH=. python3 scripts/smoke_test.py
```
