# 🔍 Evaluating CrewAI Agents using Portkey

A hands-on project demonstrating how to **monitor, trace, and evaluate CrewAI multi-agent systems** using [Portkey AI](https://portkey.ai) — an LLM observability and gateway platform. Built with [CrewAI](https://github.com/crewAIInc/crewAI).

## 🧠 Overview

This project integrates CrewAI with Portkey to route all LLM calls through the **Portkey Gateway**, enabling full observability of agent behavior including:
- Real-time execution traces
- Token usage and cost tracking
- Agent decision-making logs
- Task execution flow and timing
- Tool usage details

Two multi-agent crews are demonstrated:
1. **Single Agent Crew** — A Software Developer agent that writes HTML on demand
2. **Multi-Agent Sequential Crew** — A Product Manager + Software Developer that collaboratively build a Ping Pong game

## 🏗️ Architecture

```
CrewAI Agents
      │
      ▼
Portkey Gateway (PORTKEY_GATEWAY_URL)
      │
      ├── Virtual Key → OpenAI GPT-4o
      │
      ▼
Portkey Dashboard
  ├── 📊 Execution Traces
  ├── 💰 Cost & Token Metrics
  ├── 🔍 Agent Decision Logs
  └── ⏱️  Task Timing & Flow
```

## 🤖 Agents Demonstrated

| Crew | Agent | Role | Task |
|------|-------|------|------|
| Crew 1 | **Software Developer** | Write code on demand | Generate a Hello World HTML page |
| Crew 2 | **Product Manager** | Define software requirements | List features for a Ping Pong game |
| Crew 2 | **Software Developer** | Build from requirements | Write full Ping Pong game code |

## 🔑 Key Concepts

### Portkey Integration
All LLM calls are routed through Portkey by configuring the `LLM` object in CrewAI:

```python
from crewai import LLM
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL

gpt_llm = LLM(
    model="gpt-4o",
    base_url=PORTKEY_GATEWAY_URL,       # Route through Portkey
    api_key="dummy",                     # Actual auth via Virtual Key
    extra_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        virtual_key="YOUR_VIRTUAL_KEY",  # Maps to your OpenAI key in Portkey
    )
)
```

### What Portkey Tracks
- **Agent decision-making process** — every reasoning step
- **Task execution flow and timing** — how long each task takes
- **Tool usage details** — which tools were called and with what inputs
- **Token consumption & cost** — per agent, per task, per run

## 🚀 Getting Started

### Prerequisites

- Python >= 3.10 and <= 3.13
- [Portkey account](https://app.portkey.ai) (free tier available)
- OpenAI API Key added as a **Virtual Key** in Portkey

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/Evaluating-CrewAI-Agents-Portkey.git
cd Evaluating-CrewAI-Agents-Portkey

# Install dependencies
pip install -r requirements.txt
```

### Setup Portkey

1. Sign up at [app.portkey.ai](https://app.portkey.ai)
2. Go to **Virtual Keys** → Add your OpenAI API Key → give it a name (e.g., `my-openai`)
3. Go to **Settings** → Copy your **Portkey API Key**

### Environment Variables

Create a `.env` file:

```env
PORTKEY_API_KEY=your_portkey_api_key_here
PORTKEY_VIRTUAL_KEY=your_virtual_key_name_here
```

> ⚠️ **Never commit your `.env` file or hardcode keys in the notebook!**

### Update the Notebook

Replace the hardcoded keys in the notebook with environment variables:

```python
import os
from dotenv import load_dotenv
load_dotenv()

gpt_llm = LLM(
    model="gpt-4o",
    base_url=PORTKEY_GATEWAY_URL,
    api_key="dummy",
    extra_headers=createHeaders(
        api_key=os.getenv("PORTKEY_API_KEY"),
        virtual_key=os.getenv("PORTKEY_VIRTUAL_KEY"),
    )
)
```

### Run the Notebook

Open `Evaluating_CrewAI_Agents_using_PortKey_Practice.ipynb` in Jupyter or Google Colab and run all cells.

### View Traces

After running, go to your **Portkey Dashboard → Logs** to see full execution traces for all agent runs.

## 📦 Dependencies

| Package | Purpose |
|---------|---------|
| `crewai` | Multi-agent orchestration framework |
| `portkey-ai` | LLM gateway, observability & tracing |
| `python-dotenv` | Environment variable management |

Install all with:

```bash
pip install -r requirements.txt
```

## 📁 Project Structure

```
Evaluating-CrewAI-Agents-Portkey/
│
├── Evaluating_CrewAI_Agents_using_PortKey_Practice.ipynb  # Main notebook
├── requirements.txt                                         # Python dependencies
├── .env.example                                             # Example env variables
├── .gitignore                                               # Files excluded from Git
└── README.md                                                # Project documentation
```

## 💡 Why Portkey for CrewAI?

| Feature | Without Portkey | With Portkey |
|---------|----------------|--------------|
| Agent traces | ❌ No visibility | ✅ Full step-by-step traces |
| Cost tracking | ❌ Manual | ✅ Automatic per-run cost |
| LLM fallbacks | ❌ Not available | ✅ Built-in retry & fallback |
| Multi-model support | ❌ One provider | ✅ 200+ LLM providers |
| Request logs | ❌ None | ✅ Complete request/response logs |

## ⚠️ Security Note

This notebook originally contained a hardcoded Portkey API key. **Always use environment variables** for API keys and never commit them to version control. The `.gitignore` in this repo excludes `.env` files automatically.

## 📜 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
