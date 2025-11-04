# Lightweight RAG with LangChain + Groq + Neo4j

This notebook demonstrates a **lightweight Retrieval-Augmented Generation (RAG)** system built using:
- **LangChain** (core + community + text-splitters)
- **Groq Llama-3.1-8B-Instant** model for low-latency responses
- **Neo4j** as both a vector store and chat memory graph
- **Sentence Transformers (BAAI/bge-small-en-v1.5)** for embeddings

The goal is to provide a **minimal, fully functional RAG pipeline** that can run in **Google Colab** with tiny models and limited resources.

---

##  Architecture Overview

```text
User → Chat Interface → RAG Chain
                       ├── Neo4j (Vector Store)
                       ├── Groq LLM (Reasoning)
                       └── HuggingFace Embeddings (BAAI/bge-small-en-v1.5)
```

### Key Features

- ✅ Tiny models for fast local experimentation (30MB embeddings, 2B LLM)
- ✅ PDF or TXT document ingestion (up to 2 pages)
- ✅ Text chunking and vector indexing in Neo4j
- ✅ Lightweight chat history stored and retrieved from Neo4j
- ✅ Short, contextually aware RAG-style conversations
- ✅ Cleanup and retention logic for old chat sessions

---

##  Tech Stack

| Component | Library | Purpose |
|------------|----------|----------|
| **LLM** | `langchain-groq` | Access Groq Llama-3.1 models |
| **Embeddings** | `sentence-transformers` | Vectorize text for retrieval |
| **Vector DB** | `Neo4j` | Store and query chunks + chat history |
| **Framework** | `LangChain` | Orchestrate RAG pipeline |
| **File Loader** | `langchain_community.document_loaders` | Handle PDF/TXT input |

---

##  Setup Instructions

### 1️⃣ Install Required Packages

```bash
!pip install -qU   "langchain-groq>=0.2.0"   "langchain>=0.2.0"   "langchain-community>=0.2.0"   "langchain-core>=0.2.0"   "langchain-text-splitters>=0.2.0"   neo4j   pypdf   sentence-transformers
```

### 2️⃣ Add Secrets in Colab

Store your credentials securely under **Colab > Secrets**:

- `GROQ_API_KEY`
- `NEO4J_URI`
- `NEO4J_USERNAME`
- `NEO4J_PASSWORD`

### 3️⃣ Upload a File

Run the upload cell and select a **small PDF or TXT file** (≤ 2 pages).  
The script automatically chunks and embeds it into Neo4j.

### 4️⃣ Start Chatting

Once ingestion completes, the terminal-style chat interface appears.  
You can ask questions about your uploaded file or previous chat context.

Example:

```text
You: Summarize the uploaded document.
Assistant: The file discusses lightweight RAG using LangChain and Neo4j...
```

Commands:

- `exit` → quit session
- `cleanup` → delete old chat history

---

##  File Structure

```text
.
├── cell1_rag_pipeline_chat_history.ipynb     

```

---

## How It Works

1. **Ingestion** → Split and embed uploaded document → store vectors in Neo4j.  
2. **Query** → Retrieve top-k relevant chunks from Neo4j.  
3. **Prompt** → Combine retrieved context + past chat into a prompt.  
4. **Generate** → Send prompt to Groq LLM → get concise, contextual answer.  
5. **Store** → Save question and answer embeddings into Neo4j as message nodes.

---

##  Maintenance

- Automatic cleanup of chat history older than 90 days.  
- You can trigger manual cleanup using `cleanup` command.

---

##  Next Steps

- Replace Groq with other LangChain-compatible LLMs.  
- Add document metadata filters (title, date, tags).  
- Enable multiple-session memory and embeddings per user.  
- Integrate UI front-end (e.g., Streamlit or Gradio).

---

## 📄 License

This example is provided for educational purposes under the MIT License.
