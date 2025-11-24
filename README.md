# [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/HyperspaceDan/Kaggle_Auto_Auditor/blob/main/Kaggle_Auto_Auditor_(CapstoneProject).ipynb)

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


## Setup & Usage
Option 1: Run in Google Colab (Recommended)
1. Click the "Open in Colab" badge above.
2. Upload kaggle.json: You will be prompted to upload your Kaggle API key file.
3. Set Secrets: Add your GEMINI_API_KEY to the Colab "Secrets" manager (Key symbol on the left sidebar).
4. Run All: The notebook will install dependencies and start the UI.
   
Option 2: Run Locally / Kaggle Notebook
Clone this repository:


```Bash

git clone https://github.com/HyperspaceDan/Kaggle_Auto_Auditor.git
cd Kaggle-Auto-Auditor
```
Install dependencies:

```Bash

pip install -r requirements.txt
```
Set Environment Variables:

```Python

export GOOGLE_API_KEY="your_key_here"
# Ensure ~/.kaggle/kaggle.json exists
```

## Methodology & Limitations
* Clean Slate Policy: To ensure absolute reproducibility, the Auto-Auditor clears all pre-computed outputs (graphs, tables) and strips binary attachments (images dragged into markdown). This forces the user to re-run the corrected_notebook.ipynb to verify that the code actually generates the results claimed.
* Rate Limits: The system is optimized for the Gemini Free Tier. If it hits a Rate Limit (429), it will automatically sleep for 60 seconds and retry.

## Project Status
Track: Freestyle
Status: Complete
License: BSD 3-Clause License
