# Kaggle Auto-Auditor
### An Agentic System for Self-Correcting Data Science

## Description:
The Kaggle Auto-Auditor is a **multi-agent system** designed to solve the "Reproducibility Crisis" in public data science. Unlike passive linters that simply flag errors, the Auto-Auditor acts as an **active enhancer**. It autonomously downloads Kaggle notebooks, audits them for code quality and documentation clarity, and—crucially— enters a self-correcting loop to generate fixes until the notebook meets a high "Trust Score."

## The Problem: The "Black Box" Notebook
Kaggle is the world’s largest data science community, but it suffers from a high noise-to-signal ratio—users often encounter redundant, poorly documented, or non-reproducible notebooks that make finding reliable solutions time-consuming.

* **Reproducibility**: Many notebooks fail to run due to hardcoded paths or missing dependencies.
* **Documentation**: Complex logic is often presented as a "wall of code" with no narrative explanation.
* **Verification**: Manually vetting a notebook takes 30-60 minutes per file.

## The Solution: An Agentic Loop
The Auto-Auditor automates this vetting process using a Cyclic Multi-Agent Architecture.

* **Ingestion**: Fetches the raw .ipynb JSON from Kaggle.
* **Audit**: Three specialized agents (Code, Docs, Capabilities) critique the work.
* **Correction**: If the "Trust Score" is below 95/100, the Corrective Agent rewrites the cells.
* **Verification Loop**: The system re-audits the new code. This cycle continues until the score is maximized or the iteration limit is reached.

## Technical Architecture
The system is built on LangGraph to manage state and Google Gemini 2.5 Flash for high-speed, cost-effective reasoning. It employs a Human-in-the-loop (HITL) pattern where the "Human" is replaced by a "Critic Agent," creating a fully autonomous feedback loop.

<img width="1111" height="1481" alt="Flow_diagram" src="https://github.com/user-attachments/assets/1398b50a-5256-4d63-b714-17f7f9d88a41" />

### Key Components
* GraphState: A strictly typed TypedDict that serves as the shared memory for the swarm.
* Structure-Aware Ingestion: Unlike simple text scrapers, this system preserves the .ipynb cell structure (Code vs. Markdown), allowing it to reconstruct valid notebooks after editing.
* Resiliency Layer: Includes robust Regex parsing to handle malformed LLM outputs and automatic time.sleep backoffs to handle API Rate Limits (429 Errors).
