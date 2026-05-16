# 🧪 AI Home Lab

> Self-hosted AI engineering lab — local LLMs, agentic workflows, and automation built by a senior data leader to stay hands-on with the stack.

I'm a VP of Data & Analytics with 18+ years leading data strategy across global organisations. This repo documents my personal AI engineering lab — built not to become a developer, but to stay genuinely close to the technology I commission, govern, and lead at scale.

The philosophy: **a data leader who builds thinks differently from one who only buys.**

---

## 🖥️ Lab Setup

### Always-On Server — IBM ThinkCentre
| Component | Detail |
|-----------|--------|
| CPU | Intel Core i5 |
| RAM | 16GB |
| OS | Windows (Docker via WSL2) |
| Purpose | 24/7 model serving, workflow automation |

**Running:**
- [Ollama](https://ollama.ai) — local LLM runtime
- [Open WebUI](https://github.com/open-webui/open-webui) — browser-based chat interface
- [n8n](https://n8n.io) — workflow automation (Docker)

### Models Installed
| Model | Use Case |
|-------|----------|
| `llama3.2` | Primary reasoning and analysis |
| `phi3` | Lightweight, fast responses |

### Engineering Workstation — MacBook Air M4 (24GB)
Used for AI engineering, development, and building automations. The 24GB unified memory handles local model experimentation and serious AI workloads without cloud dependency.

---

## 🤖 Workflows

### Tech Industry Briefing Agent
**File:** [`workflows/tech-briefing-agent.json`](workflows/tech-briefing-agent.json)

A daily automated intelligence briefing that pulls top technology stories, runs them through a local LLM, and filters for high-relevance insights.

**Pipeline:**
```
Schedule Trigger (7am daily)
    → RSS Feed (Reddit r/technology — top posts)
    → AI Agent (article analysis)
        └── Ollama Chat Model (llama3.2)
    → Filter (high relevance only)
    → Formatted Briefing Output
```

**What the LLM extracts from each article:**
- Key trend
- Business implication for data/technology leaders
- Specific data or AI opportunity
- Relevance score (high / medium / low)

**Why this matters as an engineering exercise:**
This replicates the core pattern of enterprise AI pipelines — scheduled ingestion, unstructured-to-structured transformation via LLM, relevance filtering, and formatted output. The same architecture underpins customer intent classification, content recommendation engines, and market signal detection at scale. Running it locally (zero API cost, full data sovereignty) also mirrors on-premise vs cloud trade-off decisions that data leaders make daily.

**To import into your own n8n:**
1. Download `workflows/tech-briefing-agent.json`
2. In n8n → Workflows → Import from file
3. Reconfigure the Ollama credential to point to your local instance (`http://localhost:11434`)

---

## 🐳 Stack Replication

To replicate the full ThinkCentre stack locally:

### Prerequisites
- Docker Desktop installed
- WSL2 enabled (Windows) or native Linux/macOS

### Quick Start

```bash
# 1. Pull and run Ollama
docker run -d \
  --name ollama \
  -p 11434:11434 \
  -v ollama:/root/.ollama \
  ollama/ollama

# 2. Pull models
docker exec -it ollama ollama pull llama3.2
docker exec -it ollama ollama pull phi3

# 3. Run Open WebUI
docker run -d \
  --name open-webui \
  -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  ghcr.io/open-webui/open-webui:main

# 4. Run n8n
docker volume create n8n_data

docker run -d \
  --name n8n \
  --restart unless-stopped \
  -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

**Access points:**
| Service | URL |
|---------|-----|
| Open WebUI | http://localhost:3000 |
| n8n | http://localhost:5678 |
| Ollama API | http://localhost:11434 |

> **Note:** In n8n, connect to Ollama using `http://host.docker.internal:11434` — this routes correctly from inside the Docker container to the host.

---

## 🗺️ Roadmap

- [ ] Artist affinity classifier (music genre → audience segmentation)
- [ ] Customer intent prediction prototype
- [ ] RAG pipeline using internal documents
- [ ] Automated competitive intelligence dashboard
- [ ] Model evaluation framework (Phi-3 vs Llama 3.2 benchmarks)

---

## 🔗 About

Built by **Sid Shah** — VP Data & Analytics International, DataIQ Top 100 (2022 & 2026), Fractional CDO.

- LinkedIn: [linkedin.com/in/sidshah](https://www.linkedin.com/in/mrsidshah)
- Substack: [substack.com/@mrsidshah](https://substack.com/@mrsidshah)
- Fractional Digital & AI practice: 

---

*This lab exists because I believe senior data leaders who understand the stack — not just the strategy — make better decisions, ask sharper questions, and build more credible teams.*
