# Agent Observability — Contract Review Agent

Companion notebook for [What Your Agent Is Doing When You're Not Watching](https://matlog.dev/posts/ai-agent-observability) on matlog.dev.

A `dspy.ReAct` agent that reviews commercial contracts using five tools, traced end-to-end with MLflow. Built to make the observability concepts in the post concrete and runnable.

## What's in here

**`contract_review_agent.ipynb`** — the full walkthrough:
1. Loading the CUAD dataset and building a contract store
2. Defining seven tools (`list_contracts`, `search_clauses`, `assess_clause`, `web_search`, `fetch_url`, `find_contracts_with_clause`, `list_clause_types`)
3. Building the agent with `dspy.ReAct`
4. Three tasks of increasing complexity, each forcing the agent to iterate across different tools
5. Inspecting the resulting MLflow traces programmatically

## Setup

### 1. Dependencies

```bash
uv sync
```

Or install manually:

```bash
pip install dspy mlflow datasets python-dotenv requests
```

### 2. MLflow server

```bash
# Port 5000 is taken by AirPlay on macOS
uvx mlflow server --port 5001
```

### 3. Environment

```bash
cp .env.example .env
# Fill in your credentials
```

| Variable | Description |
|---|---|
| `PROVIDER_ENDPOINT` | Provider endpoint, e.g. `openai/gpt-4o` or `azure/gpt-4o` |
| `PROVIDER_API_KEY` | Your API key |
| `MODEL` | Model name |
| `MLFLOW_TRACKING_URI` | `http://localhost:5001` |
| `BRAVE_API_KEY` | For `web_search` — free tier at [brave.com/search/api](https://brave.com/search/api/) |

### 4. Run

Open `contract_review_agent.ipynb` and run cells top to bottom. The notebook downloads the CUAD dataset on first run (~200 MB, cached by HuggingFace).

## Data

Contracts come from [CUAD](https://huggingface.co/datasets/theatticusproject/cuad) — the Contract Understanding Atticus Dataset, 510 commercial agreements labeled for 41 clause types. Publicly available, no client data.

The notebook uses the pre-processed variant [`dvgodoy/CUAD_v1_Contract_Understanding_clause_classification`](https://huggingface.co/datasets/dvgodoy/CUAD_v1_Contract_Understanding_clause_classification) which has cleaner per-clause labels.
