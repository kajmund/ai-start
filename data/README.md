# Data

Local data for development — corpora, fixtures, eval sets, and sample download scripts.

- `corpus/` — your source files (documents, CSVs, JSON fixtures)
- `examples/` — optional reference scripts for fetching sample data
- Large payloads are **gitignored** — never commit production data

## Usage by project type

| Project type | What goes in `data/` |
| ------------ | -------------------- |
| RAG chat | Source documents for ingestion (`corpus/`) |
| Classification | Labeled examples for eval (`corpus/labels.csv`) |
| Extraction | Sample PDFs/HTML with expected output (`corpus/expected/`) |
| Batch processing | Input files queued for offline jobs |
| API-only | Fixtures for local integration tests |

## Add your data

Drop files into `data/corpus/` or write a fetch script under `data/examples/`:

```bash
uv run data/examples/<your-script>.py
```

Ingestion scripts that write to Supabase belong in `backend/scripts/` — they import from `app.*` and run against your configured database.

## Optional: SEC EDGAR sample

Reference downloader for SEC 10-K filings — useful for testing RAG ingestion:

```bash
uv run data/examples/sec-edgar/download.py
```

Edit params at the top of the script (especially `USER_AGENT`). Downloads land in `data/examples/sec-edgar/downloads/`.

This is an example, not a requirement. Replace with scripts for your own data sources.
