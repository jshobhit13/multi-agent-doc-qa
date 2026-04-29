# Multi-Agent Document Q&A System

A Retrieval-Augmented Generation (RAG) powered application that enables intelligent question answering across multiple documents using LangChain, ChromaDB, and OpenAI's language models. Upload any documents and ask natural language questions — the system retrieves the most relevant context and generates accurate, grounded answers.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.10+ |
| API Framework | FastAPI |
| AI / LLM | LangChain, OpenAI API (GPT-3.5 / GPT-4) |
| Vector Database | ChromaDB |
| Embeddings | OpenAI Embeddings |
| Document Parsing | PyMuPDF, python-docx |
| Environment | python-dotenv |

---

## Features

- Upload multiple documents (PDF, DOCX, TXT) through REST API endpoints
- Automatic text chunking and embedding storage in ChromaDB vector database
- Semantic search retrieves the most relevant document chunks for any query
- LangChain orchestrates a modular multi-agent pipeline for context-aware responses
- RESTful API built with FastAPI for easy integration with any frontend
- Clean separation of ingestion, retrieval, and generation agents for extensibility

---

## Project Structure

```
multi-agent-doc-qa/
├── app/
│   ├── main.py              # FastAPI app entry point
│   ├── agents/
│   │   ├── ingestion.py     # Document loading and chunking agent
│   │   ├── retrieval.py     # ChromaDB semantic search agent
│   │   └── generation.py    # LLM response generation agent
│   ├── routes/
│   │   ├── upload.py        # /upload endpoint
│   │   └── query.py         # /query endpoint
│   └── utils/
│       └── chunker.py       # Text splitting utilities
├── chroma_store/            # Local ChromaDB persistence
├── requirements.txt
├── .env.example
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.10 or above
- OpenAI API key

### Installation

```bash
# Clone the repository
git clone https://github.com/jshobhit13/multi-agent-doc-qa.git
cd multi-agent-doc-qa

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
cp .env.example .env
# Add your OpenAI API key in .env
```

### Environment Variables

Create a `.env` file in the root directory:

```
OPENAI_API_KEY=your_openai_api_key_here
CHROMA_PERSIST_DIR=./chroma_store
CHUNK_SIZE=500
CHUNK_OVERLAP=50
```

### Running the Application

```bash
uvicorn app.main:app --reload
```

API will be available at `http://localhost:8000`

Interactive docs at `http://localhost:8000/docs`

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/upload` | Upload a document for ingestion |
| POST | `/query` | Ask a question against ingested documents |
| GET | `/documents` | List all ingested documents |
| DELETE | `/documents/{id}` | Remove a document from the store |

### Example Request

```bash
# Upload a document
curl -X POST "http://localhost:8000/upload" \
  -F "file=@your_document.pdf"

# Query the system
curl -X POST "http://localhost:8000/query" \
  -H "Content-Type: application/json" \
  -d '{"question": "What are the key findings in the document?"}'
```

### Example Response

```json
{
  "answer": "The key findings include...",
  "sources": ["document_1.pdf - page 3", "document_1.pdf - page 7"],
  "confidence": "high"
}
```

---

## How It Works

1. **Ingestion Agent** — Parses uploaded documents, splits text into overlapping chunks using LangChain's RecursiveCharacterTextSplitter, and stores embeddings in ChromaDB
2. **Retrieval Agent** — On each query, performs semantic similarity search in ChromaDB to fetch the top-k most relevant chunks
3. **Generation Agent** — Passes retrieved context along with the user query to OpenAI LLM via LangChain's RetrievalQA chain to produce a grounded, accurate answer

---

## Requirements

```
fastapi
uvicorn
langchain
langchain-openai
chromadb
openai
pymupdf
python-docx
python-dotenv
tiktoken
```

---

## Author

**Shobhit Jain**
GitHub: [jshobhit13](https://github.com/jshobhit13)
Email: jshobhit8172@gmail.com
