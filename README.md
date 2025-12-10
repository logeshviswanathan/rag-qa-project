# RAG Q&A System

A production-ready Retrieval-Augmented Generation (RAG) question-answering system built with FastAPI, LangChain, and Qdrant Cloud.

## Features

- **Document Ingestion**: Upload PDF, TXT, and CSV files
- **AI-Powered Q&A**: Ask questions and get answers based on your documents
- **Source Attribution**: See which documents were used to generate answers
- **Streaming Responses**: Real-time response streaming
- **RESTful API**: FastAPI with automatic Swagger documentation
- **Containerized**: Docker support for easy deployment
- **CI/CD Ready**: GitHub Actions pipeline included

## Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Python 3.12 |
| Web Framework | FastAPI |
| RAG Framework | LangChain |
| Vector Database | Qdrant Cloud |
| Embeddings | OpenAI text-embedding-3-small |
| LLM | OpenAI gpt-4o-mini |
| Containerization | Docker |
| CI/CD | GitHub Actions |

## Project Structure

```
rag-qa-project/
├── app/
│   ├── main.py              # FastAPI application
│   ├── config.py            # Configuration management
│   ├── api/
│   │   ├── routes/          # API endpoints
│   │   └── schemas.py       # Pydantic models
│   ├── core/                # Business logic
│   │   ├── document_processor.py
│   │   ├── embeddings.py
│   │   ├── vector_store.py
│   │   └── rag_chain.py
│   └── utils/
│       └── logger.py        # Logging configuration
├── tests/                   # Test suite
├── .github/workflows/       # CI/CD pipeline
├── Dockerfile
├── docker-compose.yml
└── requirements.txt
```

## Quick Start

### Prerequisites

- Python 3.12+
- OpenAI API key
- Qdrant Cloud account (URL and API key)

### 1. Clone the Repository

```bash
git clone <repository-url>
cd rag-qa-project
```

### 2. Set Up Environment

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Configure Environment Variables

```bash
# Copy example environment file
cp .env.example .env

# Edit .env with your credentials
```

Required variables:
```
OPENAI_API_KEY=sk-proj-your-key-here
QDRANT_URL=https://your-cluster.qdrant.io
QDRANT_API_KEY=your-qdrant-api-key
```

### 4. Run the Application

```bash
# Development mode
uvicorn app.main:app --reload

# Or with Python
python -m app.main
```

### 5. Access the API

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc
- **API Base**: http://localhost:8000

## Docker Deployment

### Using Docker Compose

```bash
# Build and run
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f
```

### Using Docker Directly

```bash
# Build image
docker build -t rag-qa-system .

# Run container
docker run -p 8000:8000 --env-file .env rag-qa-system
```

## API Usage

### Upload a Document

```bash
curl -X POST "http://localhost:8000/documents/upload" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@document.pdf"
```

### Ask a Question

```bash
curl -X POST "http://localhost:8000/query" \
  -H "Content-Type: application/json" \
  -d '{"question": "What is RAG?", "include_sources": true}'
```

### Check Collection Status

```bash
curl "http://localhost:8000/documents/info"
```

### Health Check

```bash
curl "http://localhost:8000/health"
```

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Basic health check |
| `/health/ready` | GET | Readiness check with DB status |
| `/documents/upload` | POST | Upload and ingest a document |
| `/documents/info` | GET | Get collection information |
| `/documents/collection` | DELETE | Delete entire collection |
| `/query` | POST | Ask a question |
| `/query/stream` | POST | Ask with streaming response |
| `/query/search` | POST | Search documents only |

## Running Tests

```bash
# Install dev dependencies
pip install -r requirements-dev.txt

# Run all tests
pytest

# Run with coverage
pytest --cov=app --cov-report=html

# Run specific test file
pytest tests/test_query.py -v
```

## Configuration Options

| Variable | Default | Description |
|----------|---------|-------------|
| `OPENAI_API_KEY` | - | OpenAI API key (required) |
| `QDRANT_URL` | - | Qdrant Cloud URL (required) |
| `QDRANT_API_KEY` | - | Qdrant API key (required) |
| `COLLECTION_NAME` | `rag_documents` | Qdrant collection name |
| `CHUNK_SIZE` | `1000` | Document chunk size |
| `CHUNK_OVERLAP` | `200` | Chunk overlap |
| `EMBEDDING_MODEL` | `text-embedding-3-small` | OpenAI embedding model |
| `LLM_MODEL` | `gpt-4o-mini` | OpenAI LLM model |
| `RETRIEVAL_K` | `4` | Number of documents to retrieve |
| `LOG_LEVEL` | `INFO` | Logging level |

## CI/CD Pipeline

The GitHub Actions pipeline includes:

### CI Pipeline (`ci.yml`)
1. **Lint & Format**: Ruff linter and Black formatter
2. **Unit Tests**: pytest with coverage reporting
3. **Docker Build**: Build and test Docker image
4. **Security Scan**: Bandit and Safety checks

### CD Pipeline (`deploy.yml`)
1. **Build & Push**: Build Docker image and push to AWS ECR
2. **Deploy**: Deploy to AWS App Runner
3. **Health Check**: Verify deployment

## AWS Deployment

Deploy to AWS using ECR + App Runner:

```
GitHub → ECR (Container Registry) → App Runner (Hosting)
```

### Quick Setup

1. Create ECR repository
2. Create App Runner service
3. Configure GitHub Secrets:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `APP_RUNNER_ECR_ACCESS_ROLE_ARN`
   - `OPENAI_API_KEY`
   - `QDRANT_URL`
   - `QDRANT_API_KEY`

4. Push to `main` branch - auto deploys!

📖 **Full Guide**: See [docs/AWS_DEPLOYMENT_GUIDE.md](docs/AWS_DEPLOYMENT_GUIDE.md)

## Development

### Code Style

```bash
# Format code
black app/ tests/

# Lint code
ruff check app/ tests/

# Fix linting issues
ruff check app/ tests/ --fix
```

### Adding New Features

1. Create feature branch
2. Add tests for new functionality
3. Implement the feature
4. Ensure all tests pass
5. Submit pull request

## License

MIT License

## Contributing

Contributions are welcome! Please read the contributing guidelines first.
