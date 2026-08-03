# Data

Local data artifacts for development live here.

- `corpus/` holds your source documents for ingestion (create this when you add files).
- `examples/` holds optional reference download scripts for sample corpora.
- Downloaded payloads are gitignored because corpora can get large.

## Add your corpus

Drop source files into `data/corpus/` or write a download script under `data/examples/`. Your ingestion pipeline reads from here during development.

Supported formats depend on your ingestion implementation — Markdown, HTML, PDF, and plain text are common starting points.

## Optional: SEC EDGAR sample

The template includes an example downloader for SEC 10-K filings in `data/examples/sec-edgar/`. Edit the params at the top of the script, then run:

```bash
uv run data/examples/sec-edgar/download.py
```

This is a reference implementation, not a requirement. Replace it with scripts that fetch your own document sources.
