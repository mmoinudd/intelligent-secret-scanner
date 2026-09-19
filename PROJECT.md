# Intelligent Secret Scanner

AI-powered AppSec tool that finds leaked secrets and explains how to fix them.

**Status:** Planned — build after Python + OWASP training  
**Stack:** Python, Gitleaks / detect-secrets, FastAPI or Streamlit, Ollama (local LLM), Docker

## Goal

Scan repos, files, and Docker images for secrets. Use a local LLM to reduce noise and suggest remediation. First portfolio project for mid-level AppSec / Security Software Engineer roles.

## Why this project

- Maps to real AppSec work (secrets in repos, tokens, credential leakage)
- Lets you demonstrate an AI security application
- Shows architecture: scanner → findings store → API → AI explainer → report

## MVP features

1. Scan a local directory or git repo
2. Detect keys, tokens, passwords
3. Store findings (SQLite is fine)
4. Send findings to a local LLM for risk explanation + fix steps
5. Simple web UI or API report with severity

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

Do not log raw secrets. Redact in UI and logs.

## Open-source tools only

- Gitleaks or detect-secrets
- Python
- FastAPI or Streamlit
- Ollama (Llama 3 / Mistral locally)
- Docker

## Build order

1. CLI that runs Gitleaks and prints JSON
2. Parse findings into a clean model
3. Add SQLite storage
4. Add Ollama prompt: explain risk + remediation
5. Add FastAPI endpoints
6. Add a basic UI
7. Dockerize
8. Optional later: deploy a demo on AWS

## Interview talking points

- Why local LLM instead of cloud
- How false positives get handled
- Trust boundaries between scanner and API
- What you would change at company scale (queue, vault, IAM)

## Do not commit

- Real secrets
- `.env` files
- AWS keys
- Production scan output
