# Prompt Engineering Workbook with [Ollama](https://ollama.com/)

A hands-on prompt-engineering tutorial that runs entirely against a **local model
served by Ollama** — no API key required. Work through the notebooks in order.

This project uses [uv](https://docs.astral.sh/uv/) for a single, reproducible
setup across macOS, Windows, and Linux. The committed `pyproject.toml` and
`uv.lock` pin an exact, verified environment, and uv provisions its own Python —
so everyone runs the same versions regardless of what's already on their machine.
Please use this path rather than installing dependencies another way.

## Contents

| Notebook | Topic |
|----------|-------|
| `00_Intro.ipynb` | Setup, model selection, and the `get_completion` helper |
| `01_Basic_Prompt_Structure.ipynb` | Messages, roles, system prompts |
| `02_Being_Clear_and_Direct.ipynb` | Clear, direct instructions |
| `03_Assigning_Roles_Role_Prompting.ipynb` | Role prompting |
| `04_Separating_Data_and_Instructions.ipynb` | Templating / separating data |
| `05_Formatting_Output_and_Speaking_for_the_Model.ipynb` | Output formatting & prefill |
| `06_Precognition_Thinking_Step_by_Step.ipynb` | Step-by-step reasoning |
| `07_Using_Examples_Few-Shot_Prompting.ipynb` | Few-shot prompting |
| `hints.py` | Exercise hints (imported by the notebooks) |

## Setup

Three steps, the same on every OS.

### 1. Install Ollama and pull the model

Install Ollama for your OS from [ollama.com/download](https://ollama.com/download).

Make sure the Ollama server is running:

- **macOS/Windows app** (`.dmg`/`.exe`): it starts automatically once installed.
- **Homebrew** (`brew install ollama`): run `ollama serve`, or `brew services start ollama` to keep it running in the background.
- **Linux**: `ollama serve`, or enable the systemd service.

Then pull the workbook's model:

```bash
ollama pull granite4.1:3b
```

`granite4.1:3b` is the model this workbook is built and tested against — a small
but capable model (~2.1GB disk space; 3–4GB of free RAM/VRAM to run comfortably).
Use it as-is so your results match the lessons.

### 2. Install uv

Follow steps documented at [https://docs.astral.sh/uv/getting-started/installation/](https://docs.astral.sh/uv/getting-started/installation/#installing-uv)

### 3. Launch the notebooks

From the project folder:

```bash
uv run jupyter lab
```

That one command reads `pyproject.toml` and `uv.lock`, builds the exact `.venv`
(downloading a compatible Python if needed), and opens JupyterLab in your
browser. No manual virtual-environment or `pip install` steps.

## How it works

`00_Intro.ipynb` sets `MODEL_NAME` and saves it with `%store`, so every other
notebook picks it up via `%store -r MODEL_NAME` — **run `00_Intro.ipynb` first.**
Each chapter defines a `get_completion` helper that wraps `ollama.chat(...)`,
mapping the usual prompt controls to Ollama's `options` dictionary
(`temperature`, `num_predict`, `stop`). The helper runs at `temperature=0` for
the most consistent output.

## A note on the exercises

The auto-graders use simple string/regex checks. Even at `temperature=0` a local
model can vary slightly from run to run, so an exercise may occasionally need a
retry to pass. Some exercises are also *designed* to show the model failing
first — that's the teaching moment, not a bug. The prompt-engineering techniques
themselves are model-agnostic; the workbook pins one model only so everyone sees
the same behavior.