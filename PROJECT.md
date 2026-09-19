# Intelligent Secret Scanner

AI-powered AppSec tool that finds leaked secrets and explains how to fix them.

**Status:** Planned
**Stack:** Python, Gitleaks / detect-secrets, FastAPI or Streamlit, Ollama (local LLM), Docker, SQLite

## Goal

Scan repositories, files, and Docker images for leaked secrets. Classify findings and generate remediation guidance with a local LLM. Raw secrets are redacted in logs, storage displays, and the UI.

## Scope

- Secrets in source and config (API keys, tokens, passwords, cloud credentials)
- Local directory and git repo scans
- Optional image/filesystem scan later
- Human-readable risk notes and fix steps per finding

## MVP features

1. Scan a local directory or git repo
2. Detect keys, tokens, and passwords
3. Store findings (SQLite)
4. Send redacted findings to a local LLM for risk explanation and fix steps
5. Web UI or API report with severity

## Architecture

```
User / CI
   ↓
API (FastAPI)
   ↓
Scanner engine (Gitleaks / detect-secrets)
   ↓
Findings store (SQLite)
   ↓
AI explainer (Ollama)
   ↓
Dashboard / JSON report
```

The scanner never forwards raw secret values to logs, the UI, or the LLM. Only redacted snippets and metadata (type, path, line, severity) leave the scan engine.

## Tools

- Gitleaks or detect-secrets
- Python
- FastAPI or Streamlit
- Ollama (Llama 3 / Mistral locally)
- Docker

## Design notes

- Local LLM avoids sending credential material to a third-party API.
- False positives are handled by rule allowlists plus optional human review before a finding is marked confirmed.
- Trust boundary sits between the scan engine (sees raw file content) and the API/UI/LLM (sees redacted findings only).
- At larger scale this would add a job queue, a secrets vault for scanner credentials, and IAM around scan targets.

## Build order

1. CLI that runs Gitleaks and prints JSON
2. Parse findings into a clean model
3. Add SQLite storage
4. Add Ollama prompt: explain risk and remediation
5. Add FastAPI endpoints
6. Add a basic UI
7. Dockerize
8. Optional: hosted demo

## Do not commit

- Real secrets
- `.env` files
- Cloud keys
- Production scan output
