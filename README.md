# ValiChord at Home

**Automated deposit quality checker for scientific research.**

ValiChord at Home runs 100+ checks on a research deposit (code + data + documentation) and produces a structured report of reproducibility issues — missing README, hardcoded paths, unpinned dependencies, absent data dictionaries, and more — before a study goes to formal peer validation.

It is a standalone researcher prep tool. It does not run the ValiChord commit-reveal protocol. For the full protocol, see [ValiChord](https://github.com/topeuph-ai/ValiChord).

---

## Live deployment

The web interface runs on Render. Submit a deposit via the API:

```bash
curl -X POST https://valichord-at-home.onrender.com/validate \
  -F "file=@your_deposit.zip" \
  -H "X-ValiChord-Key: <your-key>"
```

`GET /health` — liveness check  
`GET /docs` — Swagger UI  
`GET /openapi.yaml` — OpenAPI 3.0 spec  

---

## Run locally

```bash
git clone https://github.com/topeuph-ai/Valichord_at_home.git
cd Valichord_at_home
pip install -r backend/requirements.txt
python backend/app.py          # starts on port 5000
```

Or use the CLI directly:

```bash
python valichord_at_home/valichord.py path/to/deposit.zip
```

---

## Structure

```
valichord_at_home/
  detectors/
    failure_modes_simple.py   — 100+ pattern-based detectors
    failure_modes_ast.py      — AST-based detectors
    claude_semantic.py        — Claude-powered semantic analysis
  generators/
    report.py                 — CLEANING_REPORT.md generator
    drafts.py                 — README / requirements draft generators
    log.py                    — VALICHORD_LOG generator
  valichord.py                — CLI entry point

backend/
  app.py                      — Flask API (Render deployment)
  openapi.yaml                — OpenAPI 3.0 spec
  requirements.txt
```

---

## Environment variables

| Variable | Required | Description |
|---|---|---|
| `ANTHROPIC_API_KEY` | Yes | Claude API key for semantic analysis |
| `VALICHORD_API_KEYS` | No | Comma-separated API keys; empty = open mode |
