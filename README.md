# CourseMate AI - RAG Book Assistant

## Project Overview

**CourseMate AI** is an intelligent document analysis application that uses Retrieval Augmented Generation (RAG) to enable users to ask questions about PDF documents. The application combines LLMs, vector embeddings, and semantic search to provide accurate, context-aware answers directly from your documents.

---

## 🎯 Features

- 📄 **PDF Upload & Processing** - Upload any PDF document for analysis
- 🔍 **Semantic Search** - Find relevant content using MMR (Maximal Marginal Relevance) retrieval
- 🤖 **AI-Powered QA** - Ask natural language questions and get accurate answers based on document content
- ⚡ **Performance Optimized** - Cached models and embeddings for fast response times
- 💾 **Persistent Storage** - Vector database persists across sessions
- 🔐 **API Security** - Uses environment variables for secure API key management

---

## 🏗️ Architecture & Tech Stack

### Core Technologies
- **LLM**: Mistral AI (`mistral-small-2506`)
- **Embeddings**: HuggingFace (sentence-transformers)
- **Vector Database**: ChromaDB
- **Framework**: LangChain
- **UI**: Streamlit
- **Language**: Python 3.10

### RAG Pipeline Flow
```
User PDF
    ↓
PDF Loader (PyPDF)
    ↓
Text Splitter (1000 chunks, 200 overlap)
    ↓
HuggingFace Embeddings
    ↓
ChromaDB Vector Store (Persistent)
    ↓
MMR Retriever (k=4, fetch_k=10)
    ↓
Mistral LLM + Prompt Template
    ↓
AI Response
```

---

## 📁 Project Structure

```
CourseMate/
├── app.py                      # Main Streamlit application
├── main.py                     # Alternative entry point with RAG chain
├── create_database.py          # Database initialization script
├── requirements.txt            # Python dependencies
├── .env                        # API keys (not in git)
├── .gitignore                  # Git ignore rules
├── chroma_db/                  # Persistent vector database
│   ├── chroma.sqlite3
│   └── [embedding collections]
├── document loaders/           # PDF and document handling
│   ├── pdf.py                 # PDF loader implementation
│   ├── page.py                # Page handling
│   ├── test.py                # Testing utilities
│   ├── notes.txt              # Sample text data
│   └── deeplearning.pdf       # Sample PDF (optional)
├── vector store/               # Vector database utilities
│   └── DB.py                  # Vector store operations
└── retrievers/                # Retrieval strategies
    ├── mmr.py                 # Maximal Marginal Relevance
    ├── multiquery.py          # Multi-query retrieval
    └── arixv.py               # Additional retrievers
```

---

## 🚀 Setup Instructions

### Prerequisites
- Python 3.10+
- pip package manager
- Git

### Step 1: Clone/Initialize Project
```bash
cd d:\GENAI\CourseMate
```

### Step 2: Create Virtual Environment
```bash
python -m venv venv
```

### Step 3: Activate Virtual Environment
```bash
# Windows PowerShell
.\venv\Scripts\Activate.ps1

# Windows CMD
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

### Step 4: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 5: Configure API Keys
Create `.env` file with:
```
MISTRAL_API_KEY="your_mistral_api_key_here"
HUGGINGFACEHUB_API_TOKEN="your_huggingface_token_here"
```

---

## 💻 Usage

### Run Streamlit App
```bash
streamlit run app.py
```

The app will open at `http://localhost:8501`

### Using the Application

1. **Upload PDF**: Click "Browse files" or drag-drop a PDF
2. **Create Vector DB**: Click the button to process and embed the document
3. **Ask Questions**: Type your questions in the text input
4. **Get Answers**: The AI will search the document and provide context-aware responses

### Alternative: Using main.py
```bash
python main.py
```

This runs the RAG chain directly without the Streamlit UI.

### Initialize Database from Scratch
```bash
python create_database.py
```

---

## ⚙️ Configuration

### Chunk Size Settings (in `RecursiveCharacterTextSplitter`)
- `chunk_size`: 1000 tokens per chunk
- `chunk_overlap`: 200 tokens (for context continuity)

### Retrieval Settings (in `MMR Retriever`)
- `k`: 4 - Number of documents to retrieve
- `fetch_k`: 10 - Candidates to fetch before diversity filtering
- `lambda_mult`: 0.5 - Balance between relevance (1.0) and diversity (0.0)

### Caching Strategy
- `@st.cache_resource` for:
  - HuggingFaceEmbeddings (model loads once)
  - ChatMistralAI (LLM initializes once)
  - ChatPromptTemplate (template created once)

---

## 🔄 How RAG Works

### Step 1: Document Ingestion
- PDF is loaded and split into chunks
- Each chunk is converted to numerical embeddings using HuggingFace

### Step 2: Vector Storage
- Embeddings are stored in ChromaDB with metadata
- Database persists in `chroma_db/` directory

### Step 3: Query Processing
- User query is embedded using the same model
- MMR search finds most relevant chunks (balances relevance + diversity)

### Step 4: LLM Response Generation
- Retrieved chunks become the "context"
- User question + context sent to Mistral LLM
- System prompt ensures answers come only from context
- Response returned to user

---

## 📦 Dependencies

| Package | Purpose |
|---------|---------|
| `langchain` | RAG framework & orchestration |
| `langchain-mistralai` | Mistral LLM integration |
| `langchain-huggingface` | HuggingFace embeddings |
| `chromadb` | Vector database |
| `streamlit` | Web UI |
| `pypdf` | PDF loading |
| `sentence-transformers` | Embedding models |
| `python-dotenv` | Environment variable management |

---

## 🎓 Example Questions

For a Deep Learning PDF:
- "What is a neural network?"
- "Explain backpropagation"
- "What are activation functions?"
- "How do convolutional networks work?"

---

## ⚡ Performance Tips

1. **First Run**: Model downloads (~300-500MB) - takes 1-2 minutes
2. **Subsequent Runs**: Uses cached models - much faster
3. **Large PDFs**: Will process slower - consider splitting into sections
4. **Chunk Size**: Smaller = more searches, Larger = less precise

---

## 🔒 Security Notes

- ✅ API keys stored in `.env` (not in version control)
- ✅ `.gitignore` excludes sensitive files
- ✅ Uploaded PDFs stored temporarily only
- ⚠️ Never commit `.env` file with real keys

---

## 🐛 Troubleshooting

### "NameError: OpenAIEmbeddings not defined"
- Already fixed - using HuggingFaceEmbeddings instead

### Slow processing on first run
- Model downloading - wait for completion, will cache afterward

### ChromaDB not found
- Run `python create_database.py` to initialize

### API Key errors
- Verify `.env` file has correct keys
- Reload environment: `load_dotenv()`

---

## 📚 Resources

- [LangChain Documentation](https://python.langchain.com/)
- [Mistral AI API](https://mistral.ai/)
- [HuggingFace Hub](https://huggingface.co/)
- [ChromaDB Documentation](https://docs.trychroma.com/)
- [Streamlit Documentation](https://docs.streamlit.io/)

---

## 👨‍💻 Development

### Adding Custom Retrievers
Edit `retrievers/` files to implement:
- `mmr.py` - Maximal Marginal Relevance (default)
- `multiquery.py` - Multiple query strategies
- `arixv.py` - Custom retrieval methods

### Extending Document Loaders
Add support for more formats in `document loaders/`:
- `.txt`, `.docx`, `.md`, etc.

### Customizing LLM
Replace in `main.py` or `app.py`:
```python
llm = ChatMistralAI(model="mistral-large-2506")  # Different model
```

---

## 📄 License

Private Project

---

## 📧 Notes

- Project built with LangChain for flexible RAG implementation
- Optimized for accuracy and performance
- Easy to extend and customize
