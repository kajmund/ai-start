# SEC EDGAR sample downloader

Optional reference script for fetching a small 10-K corpus from SEC EDGAR. Useful for testing ingestion and retrieval without preparing your own documents first.

Edit the params at the top of `download.py`, then run from the repo root:

```bash
uv run data/examples/sec-edgar/download.py
```

Downloads land in `data/examples/sec-edgar/downloads/` (gitignored).
