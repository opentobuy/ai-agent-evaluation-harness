# AI Agent Evaluation & Recovery Harness

An AI-powered marketing copy evaluation and recovery workflow built with **n8n, Ollama, Qwen, and JavaScript**.

## Overview

This project demonstrates a practical approach to evaluating AI-generated content using a combination of deterministic rule checks and LLM-based quality assessment.

When content fails evaluation, the workflow automatically attempts to repair it, re-evaluates the revised output, and records the remaining issues.

The project runs locally using Docker, n8n, and Ollama.

## Key Features

- **Hybrid Evaluation:** JavaScript-based validation combined with LLM evaluation.
- **Five Quality Rules:** Checks structure, completeness, word count, audience relevance, and factual accuracy.
- **Automated Recovery:** Generates revised content based on identified QA issues.
- **Multi-Round Retry:** Supports up to three repair attempts.
- **Structured QA Reporting:** Produces PASS/FAIL results, issue details, and test summaries.
- **Local LLM:** Uses Qwen through Ollama without requiring a paid cloud LLM API.

## Evaluation Rules

| Rule | Description | Method |
|---|---|---|
| Rule 1 | Exactly three copy variations | JavaScript |
| Rule 2 | Headline, Body, and CTA are present | JavaScript |
| Rule 3 | Fewer than 80 words per variation | JavaScript |
| Rule 4 | Relevant to the target audience | LLM |
| Rule 5 | No unsupported product claims | LLM |

## Test Scenarios

The workflow includes three sample test cases:

- **TC-001:** Valid advertising copy.
- **TC-002:** Unsupported product claims and exaggerated promises.
- **TC-004:** Missing variation and CTA.

The tests demonstrate initial evaluation, automatic repair, and re-evaluation. A separate stress-test run also demonstrated a case that remained noncompliant after three repair attempts.

## Technology Stack

- n8n — Workflow orchestration
- Ollama — Local LLM runtime
- Qwen 3.5 9B — Evaluation and repair model
- JavaScript — Deterministic validation and QA reporting
- Docker — Local deployment

## How to Run

1. Install and run n8n.
2. Install Ollama and download the configured Qwen model.
3. Import `marketing-agent-workflow-public.json` into n8n.
4. Configure Oll
