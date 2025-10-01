# RAG OLLAMA - Advanced Document Q&A System

A sophisticated Retrieval-Augmented Generation (RAG) system built with Streamlit, Ollama, and ChromaDB for intelligent document analysis and question-answering.

## 🌟 Features

### Core Functionality
- **Document Processing**: Upload and process PDF documents with intelligent text chunking
- **Advanced RAG**: Retrieval-Augmented Generation using local Ollama models
- **Vector Search**: Semantic document search using ChromaDB vector database
- **Conversation Management**: Persistent chat history with export capabilities
- **Real-time Analytics**: Document and conversation analytics dashboard

### User Interface
- **Modern Streamlit UI**: Clean, responsive interface with intuitive navigation
- **Multi-page Application**: Documents, Chat, Settings, and Analytics pages
- **Document Library**: Comprehensive document management with status tracking
- **Export Capabilities**: Export conversations in JSON, TXT, or Markdown formats

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Streamlit Frontend                      │
├─────────────────────────────────────────────────────────────┤
│  📄 Documents  │  💬 Chat  │  ⚙️ Settings  │  📊 Analytics │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   Application Layer                        │
├─────────────────┬─────────────────┬─────────────────────────┤
│ Document Manager│  RAG Engine     │ Conversation Manager    │
│ - Upload docs   │ - Vector search │ - Chat history         │
│ - Text extract  │ - LLM query     │ - Export/Import        │
│ - Chunk text    │ - Context gen   │ - Analytics           │
└─────────────────┴─────────────────┴─────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   Storage Layer                            │
├─────────────────┬─────────────────┬─────────────────────────┤
│   ChromaDB      │    File System  │    Ollama LLM          │
│ - Vector store  │ - Documents     │ - Local models         │
│ - Embeddings    │ - Conversations │ - Text generation      │
│ - Similarity    │ - Metadata      │ - Embeddings           │
└─────────────────┴─────────────────┴─────────────────────────┘
```

## 🔧 Working Principle

### 1. Document Processing Pipeline
```
PDF Upload → Text Extraction → Chunking → Embedding → Vector Store
```

1. **Upload**: Users upload PDF documents through the Streamlit interface
2. **Extraction**: PyPDF2 extracts text content from PDF files
3. **Chunking**: Text is split into manageable chunks (default: 1000 chars, 200 overlap)
4. **Embedding**: Ollama generates vector embeddings for each chunk
5. **Storage**: Vectors stored in ChromaDB for semantic search

### 2. RAG Query Process
```
User Question → Embedding → Vector Search → Context Retrieval → LLM Response
```

1. **Question Processing**: User question is converted to vector embedding
2. **Similarity Search**: ChromaDB finds most relevant document chunks
3. **Context Assembly**: Retrieved chunks are assembled as context
4. **LLM Generation**: Ollama model generates response using context
5. **Response Delivery**: Formatted response with source references

## 📁 Project Structure

```
RAGOLLAMA/
├── app/
│   ├── app.py                    # Main Streamlit application
│   ├── config.py                 # Configuration management
│   ├── document_manager.py       # Document processing & storage
│   ├── rag_engine.py            # RAG implementation
│   ├── conversation_manager.py   # Chat history management
│   ├── session_init.py          # Session initialization
│   └── pages/
│       ├── documents.py         # Document management page
│       ├── chat.py              # Chat interface
│       ├── settings.py          # Configuration page
│       └── analytics.py         # Analytics dashboard
├── data/                        # Sample documents
├── chroma_db/                   # Vector database storage
├── conversations/               # Chat history storage
├── uploaded_documents/          # User uploaded files
├── requirements.txt             # Python dependencies
└── README.md                    # This file
```

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- Ollama installed and running
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/DavidR-CSIC/RAG_OLLAMA_ReviewPaper.git
   cd RAGOLLAMA
   ```

2. **Create virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Install and start Ollama**
   ```bash
   # Install Ollama (visit https://ollama.ai for installation instructions)
   
   # Pull required models
   ollama pull qwen2.5:1.5b
   ollama pull nomic-embed-text  # For embeddings
   ```

5. **Run the application**
   ```bash
   cd app
   streamlit run app.py
   ```

6. **Access the application**
   Open your browser and navigate to `http://localhost:8501`

## 📖 Usage Guide

### Initial Setup
1. **Initialize RAG System**: Go to Settings → Click "Initialize RAG System"
2. **Upload Documents**: Navigate to Documents → Upload → Select PDF files
3. **Wait for Processing**: Monitor document processing status in the Library tab

### Using the Chat Interface
1. **Navigate to Chat**: Click on the Chat page
2. **Ask Questions**: Type questions about your uploaded documents
3. **Review Responses**: View AI responses with source references
4. **Export Conversations**: Save chat history in various formats

### Configuration Options
- **Model Selection**: Choose different Ollama models (Settings page)
- **Chunk Settings**: Adjust document chunking parameters
- **Retrieval Parameters**: Modify number of retrieved documents
- **UI Preferences**: Customize interface settings

## ⚙️ Configuration

### Key Configuration Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `llm_model` | qwen2.5:1.5b | Ollama model for text generation |
| `embedding_model` | nomic-embed-text | Model for document embeddings |
| `chunk_size` | 1000 | Size of text chunks in characters |
| `chunk_overlap` | 200 | Overlap between chunks |
| `retrieval_k` | 8 | Number of documents to retrieve |

### Environment Variables
```bash
# Optional: Set custom paths
export RAGOLLAMA_DATA_DIR="/path/to/documents"
export RAGOLLAMA_DB_DIR="/path/to/chroma_db"
```

## 🔧 Development

### Running in Development Mode
```bash
# Enable debug mode
streamlit run app.py --server.runOnSave true

# View logs
tail -f ~/.streamlit/logs/streamlit.log
```

### Adding New Features
1. **New Pages**: Add to `app/pages/` directory
2. **New Models**: Update `config.py` model definitions
3. **Custom Processors**: Extend `document_manager.py`

## 🐛 Troubleshooting

### Common Issues

**1. "Model not found" Error**
```bash
# Ensure Ollama is running
ollama serve

# Pull required models
ollama pull qwen2.5:1.5b
ollama pull nomic-embed-text
```

**2. "Database readonly" Error**
```bash
# Fix permissions
chmod -R 777 chroma_db/
# Or delete and reinitialize
rm -rf chroma_db/
```

**3. "RAG system not initialized"**
- Go to Settings page
- Click "Initialize RAG System"
- Wait for confirmation message

**4. Documents not uploading**
- Check file permissions in `uploaded_documents/`
- Ensure documents are valid PDFs
- Check Streamlit logs for detailed errors

### Performance Optimization
- Use smaller models for faster responses
- Reduce chunk size for faster processing
- Adjust retrieval_k based on document corpus size

## 📊 Analytics & Monitoring

The application provides comprehensive analytics:
- **Document Statistics**: Upload trends, processing times
- **Conversation Metrics**: Question types, response quality
- **System Performance**: Response times, error rates
- **Usage Patterns**: Most accessed documents, popular queries

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit changes (`git commit -am 'Add new feature'`)
4. Push to branch (`git push origin feature/new-feature`)
5. Create Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- **Ollama**: Local LLM inference
- **ChromaDB**: Vector database
- **Streamlit**: Web interface framework
- **LangChain**: RAG implementation framework

## 📞 Support

For issues and questions:
- Create an issue on GitHub
- Check the troubleshooting section
- Review Streamlit and Ollama documentation

---

**Built with ❤️ for intelligent document analysis**
