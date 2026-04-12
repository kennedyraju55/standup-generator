# 📊 Standup Generator

Generate professional standup updates, weekly summaries, and sprint reviews from tasks and git activity. Features 4 templates, JIRA ticket linking, team standups, and history tracking.

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB.svg?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Local LLM](https://img.shields.io/badge/Local_LLM-Ollama-000000.svg?style=for-the-badge&logo=ollama&logoColor=white)](https://ollama.com)
[![Privacy-First](https://img.shields.io/badge/100%25-Privacy--First-2ea043.svg?style=for-the-badge&logo=shield&logoColor=white)](#privacy-first)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)
[![FastAPI](https://img.shields.io/badge/FastAPI-REST_API-009688.svg?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Streamlit](https://img.shields.io/badge/Streamlit-Web_UI-FF4B4B.svg?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)

---

## ✨ Features

- **🤖 AI-Powered Generation** - Generate professional standups in seconds with local LLM
- **📝 4 Templates** - Daily, weekly, sprint review, and async update formats
- **📚 Git Integration** - Auto-capture commit history and branch information
- **🔗 JIRA Linking** - Auto-detect and link ticket references (PROJ-123)
- **👥 Team Standups** - Generate combined reports for multiple team members
- **📊 History Tracking** - Save and retrieve past reports with date filtering
- **🔒 100% Local** - All processing happens on your machine, zero cloud dependency
- **⚡ Fast** - REST API and CLI interfaces for programmatic and interactive use
- **🎨 Web UI** - Streamlit-based dashboard for point-and-click operation

---

## 🏗️ Architecture

```
┌────────────────────┐
│   User Input       │
│  (CLI/Web UI/API)  │
└────────────┬───────┘
             │
             ▼
┌────────────────────┐
│  Core Engine       │
│  - Task Parser     │
│  - Git Integration │
│  - JIRA Detector   │
└────────────┬───────┘
             │
             ▼
┌────────────────────┐
│  Local LLM         │
│  (Ollama/Gemma)    │
└────────────┬───────┘
             │
             ▼
┌────────────────────┐
│  Formatted Output  │
│  - JSON Storage    │
│  - Display         │
└────────────────────┘
```

---

## 📋 Project Structure

```
standup-generator/
├── src/standup_gen/
│   ├── __init__.py           # Package initialization
│   ├── core.py               # Core generation & AI logic
│   ├── cli.py                # Click CLI interface
│   ├── api.py                # FastAPI REST endpoints
│   └── web_ui.py             # Streamlit web interface
├── tests/
│   ├── test_core.py          # Unit tests
│   └── __init__.py
├── docs/
│   └── images/               # Architecture & feature diagrams
├── config.yaml               # Application configuration
├── requirements.txt          # Python dependencies
├── docker-compose.yml        # Docker orchestration
├── Dockerfile                # Container build
└── README.md                 # This file
```

---

## 🚀 Quick Start

### Prerequisites

- **Python 3.11+**
- **Ollama** (for local LLM)
- **Gemma 4 model** (via Ollama)

### Installation

```bash
# Clone the repository
git clone https://github.com/kennedyraju55/standup-generator.git
cd standup-generator

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Pull the AI model (first time only, ~2GB download)
ollama pull gemma4

# Verify installation
python -m standup_gen.cli --help
```

### First Run

```bash
# Start Ollama in background
ollama serve &

# Generate a daily standup
python -m standup_gen.cli generate \
  --tasks tasks.json \
  --template daily

# Or launch web UI
streamlit run src/standup_gen/web_ui.py

# Or start REST API
uvicorn src.standup_gen.api:app --reload --port 8000
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Runtime** | Python 3.11+ | Core application |
| **CLI** | Click 8.1+ | Command-line interface |
| **Web** | Streamlit 1.28+ | Web dashboard |
| **API** | FastAPI | REST endpoints |
| **LLM** | Ollama + Gemma 4 | Local AI inference |
| **Data** | JSON/YAML | Config & storage |
| **Testing** | pytest | Unit & integration tests |
| **Deployment** | Docker | Container orchestration |

---

## 📖 CLI Reference

```bash
python -m standup_gen.cli [COMMAND] [OPTIONS]
```

### Available Commands

| Command | Description | Example |
|---------|-------------|---------|
| generate | Generate standup from tasks | --tasks tasks.json --template daily |
| weekly | Weekly summary | --tasks tasks.json |
| sprint | Sprint review | --tasks tasks.json --sprint "Sprint 23" |
| 	eam | Team combined standup | --members alice,bob --dir team_tasks/ |
| history | View past standups | --days 7 |
| git-log | Show git activity | --days 1 --author alice |

### Global Options

```bash
--config PATH       # Path to config.yaml (default: config.yaml)
--verbose, -v      # Enable debug logging
--help             # Show help message
```

---

## 🌐 Web UI

Launch the interactive Streamlit dashboard:

```bash
streamlit run src/standup_gen/web_ui.py
```

Access at **http://localhost:8501**

Features:
- 🎨 Rich text editor for task input
- 📊 Real-time generation results
- 🔧 Configuration panel
- 📱 Responsive mobile-friendly design
- 💾 Standup history browser

---

## ⚡ REST API

Every feature is available via FastAPI REST endpoints.

### Start Server

```bash
uvicorn src.standup_gen.api:app --reload --port 8000
```

Access interactive docs: **http://localhost:8000/docs**

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /health | Health check |
| GET | /docs | Swagger UI |
| POST | /analyze | Generate standup |

### Example Request

```bash
curl -X POST http://localhost:8000/analyze \
  -H "Content-Type: application/json" \
  -d '{
    "tasks": {
      "completed": ["Fixed bug PROJ-42"],
      "today": ["Implement feature"],
      "blockers": []
    },
    "template": "daily"
  }'
```

---

## 🐳 Docker Deployment

Run with Docker — no local Python setup needed!

```bash
# Clone and start
git clone https://github.com/kennedyraju55/standup-generator.git
cd standup-generator

# Start all services
docker compose up

# Access web UI
open http://localhost:8501

# API docs
open http://localhost:8000/docs
```

### Docker Commands

```bash
docker compose up              # Start services
docker compose up -d           # Start in background
docker compose down            # Stop services
docker compose logs -f         # View live logs
docker compose build --no-cache # Rebuild images
```

---

## 🧪 Testing

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=standup_gen --cov-report=term-missing

# Run specific test
pytest tests/test_core.py::test_generate_standup -v

# Generate HTML coverage report
pytest tests/ --cov=standup_gen --cov-report=html
open htmlcov/index.html
```

### Coverage Target

| Module | Coverage |
|--------|----------|
| core.py | 85%+ |
| cli.py | 80%+ |
| pi.py | 80%+ |
| **Overall** | **80%+** |

---

## ⚙️ Configuration

Create a config.yaml in the project root:

```yaml
llm:
  model: "gemma4"
  temperature: 0.4
  max_tokens: 2000

standup:
  default_template: daily
  history_file: standup_history.json
  auto_save: true

git:
  enabled: true
  repo_path: "."
  days: 1
  include_branches: true

ticket:
  pattern: "[A-Z]+-\\d+"
  link_template: "https://jira.example.com/browse/{ticket}"

team:
  members:
    - alice
    - bob
    - carol
```

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| OLLAMA_HOST | Ollama server URL | http://localhost:11434 |
| LOG_LEVEL | Logging verbosity | INFO |
| DATA_DIR | Data storage directory | ./data |

---

## 🔒 Privacy-First

All processing is **100% local** — your data never leaves your machine:

- ✅ No cloud API calls
- ✅ No data collection or telemetry
- ✅ No internet connection required after model download
- ✅ Full control over your AI model
- ✅ GDPR/HIPAA compliant

---

## 📚 Python API

Use the core module directly in your Python code:

```python
from standup_gen.core import generate_standup, get_git_log

# Generate daily standup
tasks = {
    'completed': ['Fixed login bug'],
    'today': ['Implement dashboard'],
    'blockers': ['Waiting for API spec']
}
git_log = get_git_log(days=1)
standup = generate_standup(tasks, git_log=git_log)
print(standup)
```

---

## 🐛 Troubleshooting

**Ollama not connecting?**
```bash
# Check if Ollama is running
curl http://localhost:11434/api/tags

# Start Ollama
ollama serve
```

**Model not found?**
```bash
# Pull the required model
ollama pull gemma4

# List available models
ollama list
```

**Import errors?**
```bash
# Verify package installation
python -c "from standup_gen.core import *; print('OK')"

# Reinstall dependencies
pip install -r requirements.txt --force-reinstall
```

---

## 🤝 Contributing

Contributions welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: git checkout -b feature/amazing-feature
3. Commit changes: git commit -m 'Add amazing feature'
4. Push to branch: git push origin feature/amazing-feature
5. Open a Pull Request

### Development Setup

```bash
git clone https://github.com/YOUR_USERNAME/standup-generator.git
cd standup-generator

python -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt
pip install pytest pytest-cov ruff black

# Run linting
ruff check src/
black src/

# Run tests
pytest tests/ -v
```

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Nrk Raju Guthikonda**
- GitHub: [@kennedyraju55](https://github.com/kennedyraju55)
- Dev.to: [@kennedyraju55](https://dev.to/kennedyraju55)
- LinkedIn: [Nrk Raju Guthikonda](https://linkedin.com/in/nrk-raju-guthikonda)

---

<div align="center">

**Made with ❤️ by kennedyraju55**

[⭐ Star this repo if you found it helpful!](https://github.com/kennedyraju55/standup-generator)

</div>
