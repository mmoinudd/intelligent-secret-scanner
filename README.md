# Intelligent Secret Scanner

Scan repositories, files, and container images for leaked secrets. Findings are redacted, stored, and explained by a local LLM with remediation steps.

**Status:** planned. See [PROJECT.md](PROJECT.md) for architecture, MVP scope, and build order.

## What it does

- Runs Gitleaks or detect-secrets against a local path or git repo
- Stores findings without raw secret values
- Uses a local Ollama model to explain risk and suggest fixes
- Serves results over a small FastAPI API and optional UI

## Stack

Python, Gitleaks / detect-secrets, FastAPI or Streamlit, Ollama, SQLite, Docker.

## Security constraints

Do not commit real secrets, `.env` files, cloud keys, or production scan output. The scanner redacts secret values before they reach logs, the UI, or the LLM.
