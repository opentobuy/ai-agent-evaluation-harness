# AI Agent Evaluation & Recovery Harness

**Automated AI Quality Assurance with n8n, Ollama, Qwen, and JavaScript**

A local AI evaluation workflow that combines rule-based validation, LLM-powered quality assessment, automated content repair, and structured QA reporting.

This project demonstrates how AI-generated content can be systematically tested, evaluated, and improved through a controlled recovery process.

![AI Agent Evaluation & Recovery Workflow](workflow-overview.png)

## 1. Project Overview

AI-generated content can contain structural errors, missing information, misleading claims, and unsupported promises.

This project explores how to detect these issues and automatically attempt corrections while maintaining clear evaluation criteria.

The workflow uses a hybrid approach:

- **JavaScript** for deterministic validation.
- **Qwen via Ollama** for semantic and factual-claim evaluation.
- **n8n** for workflow orchestration and conditional routing.
- **Automated retry logic** for content repair and re-evaluation.
- **Structured JSON reports** for test results and failure analysis.

The entire workflow runs locally without requiring paid cloud LLM APIs.

## 2. Workflow Architecture

The workflow consists of four main stages.

### Stage 1 — Test Cases & Initial Evaluation

Three predefined advertising-copy test cases are loaded and evaluated against five quality rules.

JavaScript validates structural requirements, while the LLM evaluates audience relevance and unsupported claims.

### Stage 2 — Automated Repair & Re-Evaluation

When a test case fails, the workflow:

1. Collects the identified QA issues.
2. Sends the original content and findings to the repair model.
3. Generates revised advertising copy.
4. Re-evaluates the revised content against all five rules.
5. Determines whether another repair attempt is required.

### Stage 3 — Multi-Round Retry & Recovery

If the repaired content still fails, the workflow can perform additional repair attempts.

A maximum of **three repair attempts** is supported.

Cases that remain noncompliant after the final attempt are retained as failures rather than automatically marked as successful.

### Stage 4 — Final QA Reporting

The final reporting logic summarizes:

- Total test cases
- First-pass successes
- Successful repairs
- Final passes and failures
- Total repair attempts
- Remaining QA issues

## 3. Evaluation Rules

| Rule | Validation Criteria | Method |
|---|---|---|
| Rule 1 | Exactly three advertising-copy variations | JavaScript |
| Rule 2 | Every variation includes Headline, Body, and CTA | JavaScript |
| Rule 3 | Each variation contains fewer than 80 English words | JavaScript |
| Rule 4 | Content is relevant to the target audience and campaign objective | LLM |
| Rule 5 | No unsupported features, statistics, rankings, guarantees, or absolute promises | LLM |

The test scenario uses a fictional project-management product called **TaskFlow**, targeting small businesses with 5–30 employees.

Its confirmed features are task tracking, team collaboration, and deadline reminders.

## 4. Test Scenarios

| Test Case | Scenario | Expected Behavior |
|---|---|---|
| TC-001 | Valid advertising copy | Pass initial evaluation |
| TC-002 | Unsupported claims and exaggerated promises | Detect violations and attempt repair |
| TC-004 | Missing variation and CTA | Detect structural issues and attempt repair |

### Example Recovery Test

A controlled test run was used to exercise the multi-round recovery path.

| Test Case | Final Result | Repair Attempts |
|---|---|---|
| TC-001 | PASS | 0 |
| TC-002 | PASS | 1 |
| TC-004 | FAIL | 3 |

**Observed summary:**

- Total test cases: 3
- Final passed: 2
- Final failed: 1
- Total repair attempts: 4

In this test, TC-004 remained noncompliant after three repair attempts because the generated content still contained an unsupported absolute promise.

This demonstrates that the workflow can retain unresolved QA findings rather than treating every repair attempt as successful.

*Note: The multi-round failure path was exercised using a controlled test condition. LLM outputs and results may vary between runs.*

## 5. Technology Stack

| Technology | Purpose |
|---|---|
| n8n | Workflow orchestration |
| Ollama | Local LLM runtime |
| Qwen 3.5 9B | AI evaluation and content repair |
| JavaScript | Rule validation, comparison, and reporting |
| Docker | Local deployment |

## 6. How to Run

### Prerequisites

- Docker Desktop or another supported n8n installation
- n8n with LangChain nodes
- Ollama installed and running
- The configured Qwen model available locally

### Setup

1. Import `marketing-agent-workflow-public.json` into n8n.
2. Configure the Ollama credentials for the model nodes.
3. Verify that the configured model is available in Ollama.
4. Execute the workflow using the Manual Trigger.
5. Review the initial QA results and repair branches.
6. Inspect the final QA report.

The sample test cases are embedded in the workflow.

No external API keys are required for the local LLM configuration.

## 7. Known Limitations

This is a proof-of-concept evaluation harness, not a production-ready platform.

Current limitations include:

- A small predefined test suite.
- LLM evaluation variability.
- Limited robustness against malformed model outputs.
- No persistent test-result database.
- The final reporting node may execute multiple times when different terminal branches are reached.

The workflow is intended to demonstrate evaluation and recovery techniques rather than guarantee error-free AI outputs.

## 8. Future Improvements

- Expand test coverage with additional edge cases.
- Add persistent QA execution history.
- Consolidate reporting into a single final execution.
- Introduce additional evaluation metrics.
- Add regression testing and automated report delivery.
- Extend the framework to evaluate tool-using AI agents.

## 9. Project Objective

This project was built as a hands-on exercise in **AI evaluation, automated QA, failure analysis, and recovery workflow design**.

The focus is on building practical testing mechanisms that make AI-assisted workflows more observable, measurable, and reliable.

---

**Project Type:** Personal Portfolio / Proof of Concept

**Author:** Liang Zhang
