# Group 3 · Ferguson RFQ Challenge

**Team:** Sanchit Vijay, Pramod B Belagali, Akepati Thulasi, Raghav Rana

## What we built

An RFQ agent that turns request emails into filled, sourced Excel workbooks. Base flow: email → product IDs → lookup (SerpApi Google search → ferguson.com product pages at 12/min → backup catalog) → spec sheet PDFs → `RFQ.xlsx` + `reply.eml`. Agentic arm: **RFQ Quality Agent** (option 2) standardizes units, flags missing/suspicious cells, runs an AI contradiction check, and adds a Summary sheet showing fill rates and what was not found.

## Live link

Not deployed yet. The app runs locally with FastAPI, SQLite, Redis and arq workers, plus a React web interface.

## Architecture

**Flow:** Gmail extension or web paste → FastAPI backend → SQLite job storage + Redis queue → N worker processes

**ID extraction** (deterministic, no LLM): masks PO/phone/dates/URLs, extracts 6–7 digit Ferguson IDs and competitor codes (dashed/alphanumeric)

**Lookup pipeline:** cache → SerpApi (Google `site:ferguson.com`) → Chrome CDP :9333 (your own browser, 12 pages/min) → backup catalog (2026-09-24) → spec sheet PDFs (fill blanks only) → not found

**Quality Agent:** code checks (units ft/in/mil/lb, ranges, APWA color codes, URLs, Yes/No) + OpenAI contradiction check (email/page text as untrusted) → flags cells, never overwrites

**Output:** RFQ workbook with Sources sheet (every value sourced, with links), Supplier sheet, Notes, Summary, and `reply.eml`

**Around it:** human-in-the-loop editor, Excel upload/download (3 formats), versioned edits (409 on conflict), idempotent Send to POC flow

## How to run it

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements-dev.txt
cp .env.example .env                # fill in keys (see below)
(cd frontend && npm install && cp .env.example .env.local)
scripts/chrome.sh        # Chrome with remote debugging on port 9333
scripts/dev.sh           # API :8000, worker, web :5173
```

Or test the CLI:
```bash
.venv/bin/python rfq/build.py emails/email-1-ids-in-body.eml --out out
```

Keys needed in `.env`: `API_KEY`, `SERPAPI_API_KEY`, `OPENAI_API_KEY`, `OPENAI_MODEL`, `BOT_EMAIL`, `BOT_PASSWORD`, `FERGUSON_RATE_PER_MINUTE`, `SERPAPI_TIMEOUT_S`, `SCRAPE_AFTER_SERPAPI`

## Results

- **121 pytest tests passed**: email 1 offline matches reference RFQ workbook cell-by-cell
- **Live verification**: all 4 sample emails processed through Chrome (9333), SerpApi, and fallback catalog
- **AI check**: gpt-4o-mini ran real contradiction detection (caught Application vs. Lettering mismatch in email 1)
- **Human-in-the-loop**: edit autosave, versioned conflicts (409), double-click send idempotency (local SMTP), concurrency (Redis/SQLite locking)
- **Robustness**: rate limiting verified (≤12 pages/min), Chrome failure → catalog fallback, Redis outage → in-memory cache, spec PDFs handle no JSON-LD
- **Verified (not mocked):** SerpApi Developer plan, Gmail extension in real Gmail, unfamiliar ferguson.com pages (no JSON-LD, pseudo-tags, redirects)

## Known limits and next steps

**Not built:** competitor part number finder (option 1, returns "not searched"), user accounts (one shared API key, editor name entered in header), Azure Function deployment, official Brand Connect colours

**Limits:** SerpApi fresh searches take 20–60 s; Google ignores `site:` for some unlisted products (e.g. 225123, 4570880); Gmail App Password needs valid config; time-left ETA is rough early on; no captcha solving or fingerprint spoofing

**Next:** Azure Function wrapper for offline-only mode, user accounts + audit log, competitor codes via web search, live Brand Connect colors, Gmail sending edge cases, mobile view for the web app
