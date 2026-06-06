# Students_AI
# 🎓 Student AI

### A Privacy-Aware, Hallucination-Free RAG Assistant for Documents & Videos

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![LangChain](https://img.shields.io/badge/LangChain-Orchestration-1C3C3C?style=for-the-badge&logo=chainlink&logoColor=white)](https://langchain.com)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-Gemma--1.1--2b-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co)
[![FAISS](https://img.shields.io/badge/FAISS-Vector_Search-0075FF?style=for-the-badge)](https://faiss.ai)
[![Gradio](https://img.shields.io/badge/Gradio-UI-FF7C00?style=for-the-badge&logo=gradio&logoColor=white)](https://gradio.app)

*Query your PDFs and YouTube videos with natural language — no hallucinations, no data leaks.*

</div>

---

## 📌 Overview

**Student AI** is a context-grounded AI assistant built on the **RAG (Retrieval-Augmented Generation)** pipeline. It lets you upload any PDF or paste a YouTube video link, then ask questions — and it answers strictly from the content you provided.

Unlike generic LLMs that confidently make things up, Student AI explicitly tells you when something isn't in the source material. Privacy is baked in: no data is persisted beyond your session.

---

## ✨ Features

| Feature | Description |
|---|---|
| 📄 **PDF Q&A** | Upload research papers, notes, or textbooks and query them instantly |
| 🎬 **YouTube Video Q&A** | Paste a YouTube link — the bot transcribes and indexes it automatically |
| 🚫 **Zero Hallucination** | Out-of-scope questions are flagged, not fabricated |
| 🌍 **Multilingual Support** | Understands and responds in multiple languages |
| ⚡ **Fast Responses** | Average response time of ~3.32 seconds |
| 🔒 **Privacy-First** | Documents processed locally; nothing stored after the session |

---

## 🏗️ Architecture

```
User Input (PDF / YouTube URL / Query)
          │
          ▼
┌─────────────────────────┐
│   Gradio Chat Interface  │
└─────────────┬───────────┘
              │
    ┌─────────▼──────────┐
    │  Content Ingestion  │
    │  PDF → LlamaIndex   │
    │  YouTube → Whisper  │
    │   → ReportLab PDF   │
    └─────────┬──────────┘
              │
    ┌─────────▼──────────┐
    │  Embedding Layer    │
    │  sentence-transformers│
    │  all-mpnet-base-v2  │
    └─────────┬──────────┘
              │
    ┌─────────▼──────────┐
    │   FAISS Vector DB   │
    │  (in-memory index)  │
    └─────────┬──────────┘
              │
    ┌─────────▼──────────┐
    │   RAG Pipeline      │
    │  Top-k chunk recall │
    │  + LLM Generation   │
    └─────────┬──────────┘
              │
    ┌─────────▼──────────┐
    │  HuggingFace LLM   │
    │  Gemma-1.1-2b-it   │
    │  4-bit quantized    │
    └─────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **LLM** | `google/gemma-1.1-2b-it` via HuggingFace (4-bit BitsAndBytes quantization) |
| **Embeddings** | `sentence-transformers/all-mpnet-base-v2` |
| **Vector Store** | FAISS (in-memory) |
| **RAG Orchestration** | LlamaIndex + LangChain |
| **Speech-to-Text** | OpenAI Whisper (`base` model) |
| **PDF Generation** | ReportLab + WeasyPrint |
| **UI** | Gradio ChatInterface |
| **Runtime** | Google Colab (GPU recommended) |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- GPU with CUDA (recommended — runs on Colab T4 free tier)
- HuggingFace account with access to Gemma models

### Installation

Run in a Colab notebook or local environment:

```bash
pip install -q pypdf transformers einops accelerate langchain bitsandbytes \
    sentence_transformers llama_index llama-index-llms-huggingface \
    gradio pytube jiwer reportlab weasyprint

pip install -q git+https://github.com/openai/whisper.git
```

### HuggingFace Login

```python
from huggingface_hub import notebook_login
notebook_login()
```

> ⚠️ You need to accept the [Gemma model terms](https://huggingface.co/google/gemma-1.1-2b-it) on HuggingFace before use.

### Run

Open `Student_AI.ipynb` in Google Colab and run all cells. The Gradio interface will launch with a public share link.

---

## 💬 Usage

**Ask questions from a PDF:**
```
> What are the key contributions of the Attention Is All You Need paper?
```

**Query a YouTube video:**
```
> https://www.youtube.com/watch?v=u47GtXwePms
[Video transcribed and indexed]
> What topics were covered in this lecture?
```

**Out-of-scope question (hallucination-safe):**
```
> What's 2+2?
< This question is outside the scope of the provided documents.
```

---

## 📁 Project Structure

```
Student_AI/
├── Student_AI.ipynb     # Main notebook — full pipeline
└── README.md            # This file
```

---

## ⚙️ Configuration

Key parameters you can tune inside the notebook:

| Parameter | Default | Description |
|---|---|---|
| `chunk_size` | `1024` | Token size per document chunk |
| `max_new_tokens` | `256` | Max tokens in LLM response |
| `context_window` | `4096` | LLM context window |
| `temperature` | `0` | Deterministic outputs (set >0 for creativity) |
| `whisper model` | `base` | Change to `small`/`medium` for better transcription |

---

## 📊 Performance

- ⚡ Average response time: **~3.32 seconds**
- ✅ Rated useful by **>95% of test users**
- 🔢 Embedding model: **768-dimensional** semantic vectors (all-mpnet-base-v2)
- 🧮 Quantization: **4-bit** (BitsAndBytes) — fits in Colab free-tier GPU

---

## 🔮 Roadmap

- [ ] Persistent FAISS index across sessions
- [ ] Multi-document cross-referencing
- [ ] Support for `.docx`, `.txt`, `.pptx` input formats
- [ ] LangChain memory for multi-turn conversation
- [ ] Deployable Flask/FastAPI backend

---

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you'd like to change.

---

## 📄 License

This project is open-source. Feel free to fork, modify, and build on it.

---

<div align="center">

Built with ❤️ using LangChain · HuggingFace · FAISS · Gradio

</div>

