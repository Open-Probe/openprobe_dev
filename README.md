# OpenProbe - Advanced Agent-Based Search System

OpenProbe is an advanced agent-based search system that performs deep web searches to answer complex questions. It uses a graph-based approach with LangGraph to orchestrate different components and deliver comprehensive answers.

## ✨ Features

- **Deep Web Search**: Multi-step search process with intelligent planning and execution.
- **Automated Planning**: Breaks down complex queries into multiple sub-queries for efficient searching.
- **Adaptive Replanning**: Revises search strategies when initial plans fall short (up to 2 replans).
- **Reflection**: Explains why previous plans failed and how they were improved.
- **Web Search Integration**: Seamlessly integrates with multiple search APIs for information retrieval.
- **Intelligent Reranking**: Jina-powered result reranking for enhanced relevance.
- **Evaluation Framework**: Built-in evaluation system for testing search quality.
- **CLI Interface**: Command-line tool for easy interaction.
- **Caching System**: Persistent caching for improved performance.
- **Configuration Management**: Flexible configuration with persistent settings.

## 🏗️ Architecture

### Current Architecture
![image](https://github.com/user-attachments/assets/4e6d22b7-2dcc-446a-a129-8f1ba5abf1cd)

### Core Components

- **Search Engine**: Main orchestration system in `src/deepsearch/graph.py`
- **Web Search**: Search functionality in `src/deepsearch/web_search/`
- **State Management**: Agent state management in `src/deepsearch/state.py`
- **Evaluation System**: Testing framework in `evals/`
- **Configuration**: Settings management with persistent storage

### LangGraph Workflow

The system uses LangGraph to orchestrate the search process with these key nodes:

1. **Master Node**: Central decision-making component
2. **Plan Node**: Generates structured research plans
3. **Search Node**: Executes web searches and processes results
4. **Decide Action**: Routes between nodes based on current state

## 🛠️ Installation

### Prerequisites

- Python 3.8+
- API keys for:
  - Google Gemini API
  - Serper.dev API (for web search)
  - Jina API (for reranking)

### Setup Steps

1. **Clone and Install Dependencies**
   ```bash
   git clone <repository-url>
   cd openprobe_dev
   pip install -r requirements.txt
   ```

2. **Development Installation**
   ```bash
   pip install -e .
   ```

3. **Set Up API Keys**
   
   Set environment variables:
   ```bash
   # Google Gemini API key for LLM
   export GOOGLE_API_KEY=your_api_key
   
   # Serper.dev API key for web search
   export WEB_SEARCH_API_KEY=your_api_key
   
   # Jina API key for reranking
   export JINA_API_KEY=your_api_key
   ```
   
   Or create a `.env` file in the project root:
   ```env
   GOOGLE_API_KEY=your_api_key
   WEB_SEARCH_API_KEY=your_api_key
   JINA_API_KEY=your_api_key
   ```

## 🚀 Quick Start

### Using the Test Script
```bash
python test_deepsearch.py
```

### Using the CLI

The CLI provides comprehensive interaction with the DeepSearch system:

```bash
# Run a direct search
python -m src.deepsearch.cli search "What are the latest developments in quantum computing?"

# Run with custom parameters
python -m src.deepsearch.cli search --max-replan 2 "Who won the most recent Olympic games?"

# Run in interactive mode
python -m src.deepsearch.cli search --interactive

# Configure settings
python -m src.deepsearch.cli config --set max_sources=5 verbose=true

# Show current configuration
python -m src.deepsearch.cli config --show

# Reset configuration to defaults
python -m src.deepsearch.cli config --reset

# Show version information
python -m src.deepsearch.cli version
```

## ⚙️ Configuration

OpenProbe uses a flexible configuration system with persistent settings:

### Key Settings

| Setting | Description | Default |
|---------|-------------|---------|
| `max_sources` | Maximum sources to process | 3 |
| `max_iterations` | Maximum search iterations | 5 |
| `web_search_provider` | Search provider | "serper" |
| `reranker` | Result reranker | "jina" |
| `model_name` | Language model | "gemini-2.5-pro-preview-05-06" |
| `cache_enabled` | Enable result caching | true |
| `cache_max_age_days` | Cache expiration days | 7 |
| `verbose` | Enable detailed logging | false |

### Configuration Usage

```python
from src.deepsearch.config import config

# Set values
config.set("max_sources", 5)
config.set("verbose", True)

# Get values
max_sources = config.get("max_sources")
```

Configuration is automatically saved to `~/.openprobe/config.json`.

## 🔍 How It Works

### Search Pipeline

1. **Query Analysis**: The system analyzes the user's question
2. **Plan Generation**: Creates a search plan with multiple sub-queries
3. **Search Execution**: Executes searches based on the plan using:
   - SERP Search via Serper.dev
   - Content extraction and processing
   - Intelligent reranking with Jina
4. **Result Processing**: Aggregates and processes search results
5. **Adaptive Replanning**: If results are insufficient, replans with improved queries (up to 2 times)
6. **Answer Synthesis**: Synthesizes all information into a comprehensive answer

### Web Search Components

The web search system includes:

- **SERP Search**: Serper.dev API integration
- **Content Processing**: Web scraping and content cleaning
- **Chunking**: Text processing for manageable chunks
- **Reranking**: Jina-powered result enhancement
- **Context Building**: Comprehensive context aggregation
- **Caching**: Persistent result caching

## 📊 Evaluation System

OpenProbe includes a comprehensive evaluation framework in the `evals/` directory:

- **Accuracy Testing**: Automated accuracy evaluation
- **Dataset Management**: Test datasets for consistent evaluation
- **Grading System**: Automated grading of search results
- **Performance Metrics**: Detailed performance analysis

### Running Evaluations

```bash
# Run accuracy evaluations
python evals/accuracy.py

# Auto-grade results
python evals/autograde_df.py
```

## 🔧 System Limitations

- Maximum of 5 search attempts per session
- Maximum of 2 replanning attempts for any query
- After the replan limit is reached, the system must answer with available information
- Cache location: `~/.openprobe/cache/`

## 📁 Project Structure

```
openprobe_dev/
├── src/deepsearch/           # Core DeepSearch module
│   ├── graph.py             # LangGraph workflow definition
│   ├── state.py             # State management
│   ├── prompt.py            # System prompts
│   ├── utils.py             # Utility functions
│   ├── cli.py               # Command-line interface
│   └── web_search/          # Web search components
│       ├── web_search.py    # Main search interface
│       ├── serp_search.py   # SERP API integration
│       ├── context_builder.py # Context aggregation
│       ├── jina_reranker.py # Result reranking
│       └── ...              # Other search components
├── evals/                   # Evaluation framework
├── data/                    # Data storage
├── test_deepsearch.py       # Test script
└── requirements.txt         # Dependencies
```

## 🔑 Key Dependencies

- **Google Gemini API**: Language model capabilities
- **Serper.dev**: Web search functionality
- **Jina**: Search result reranking
- **LangGraph**: Workflow orchestration
- **LangChain**: LLM framework integration

## 📄 License

This project is licensed under the [Apache License Version 2.0](LICENSE). You are free to use, modify, and distribute this code, subject to the terms of the license.

---

## 🧩 References and Acknowledgements

This project builds upon and integrates ideas and code from various open-source projects, including:

* [LangChain](https://github.com/langchain-ai/langchain)
* [LangGraph](https://github.com/langchain-ai/langgraph)
* [LlamaIndex](https://github.com/jerryjliu/llama_index) — For data connectors and query engines.
* [Serper API](https://serper.dev/) — For web search capabilities.
* [Jina AI](https://github.com/jina-ai/jina) — For computing text embeddings.
* [Mistral](https://mistral.ai) — For LLM-based grading and evaluation.
* [LangGraph ReWOO implementation](https://langchain-ai.github.io/langgraph/tutorials/rewoo/rewoo) - Reference implemenation of ReWOO.
* [OpenDeepSearch](https://github.com/sentient-agi/OpenDeepSearch) - For implementing the web search tool and FRAMES evaluation.

Many thanks to these projects and their communities for making this work possible!

---

## 👥 Contributors

We'd like to thank the following people for their contributions to this project:

* **[Kuo-Hsin Tu](https://github.com/NTU-P04922004)** — Team Lead, Developer, Researcher
* **[Suryansh Singh Rawat](https://github.com/xsuryanshx)** — Developer
* **[Jean Yu](https://github.com/jeanyu-habana)** — Developer
* **[Ankit Basu](https://github.com/AnkitXP)** — Researcher

