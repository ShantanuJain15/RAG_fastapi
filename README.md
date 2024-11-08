# RAG_fastapi


Here's a detailed example of a `README.md` file that you can use for your assignment. This file includes an explanation of the code, the setup process, and instructions for using the API.

---

# FastAPI Server for RAG with ChromaDB

This project implements a lightweight **FastAPI** server for Retrieval-Augmented Generation (RAG). The server utilizes **ChromaDB** for persistent storage of embeddings and document metadata. It supports document ingestion (PDF, DOCX, TXT) and querying using **sentence-transformers** embeddings from Hugging Face.

## Features

- **Document Ingestion**: Ingests PDF, DOCX, and TXT files, extracting text content and generating embeddings using `sentence-transformers/all-MiniLM-L6-v2`.
- **Persistent Storage**: Uses **ChromaDB** as a vector database for storing document embeddings.
- **Efficient Querying**: Provides a `/query/` endpoint that accepts a search query and retrieves relevant documents based on similarity.
- **Non-blocking API**: The server uses `async` endpoints for efficient concurrency handling.

## Tech Stack

- **FastAPI**: Python web framework for building APIs.
- **ChromaDB**: Vector database for storing and querying embeddings.
- **Sentence Transformers**: Pre-trained model from Hugging Face for generating embeddings.
- **PyPDF2**: Library for extracting text from PDF files.
- **python-docx**: Library for extracting text from DOCX files.

## Prerequisites

- Python 3.8 or above
- ChromaDB (`chromadb`)
- FastAPI (`fastapi`)
- Uvicorn (`uvicorn`)
- Sentence Transformers (`sentence-transformers`)
- PyPDF2 (`pypdf2`)
- python-docx (`python-docx`)
- aiofiles (`aiofiles`)

## Setup Instructions

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/fastapi-rag-server.git
   cd fastapi-rag-server
   ```

2. **Install Dependencies**:
   Create a virtual environment and install the required Python packages:
   ```
    # On Windows
    genai\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Run the Server**:
   Start the FastAPI server using Uvicorn:
   ```bash
   uvicorn app:app --reload
   ```

   The server will be available at `http://127.0.0.1:8000`.

## API Endpoints

### 1. **Document Ingestion** (`/ingest/`)

- **Method**: `POST`
- **URL**: `/ingest/`
- **Description**: Ingests a list of files (PDF, DOCX, TXT) and stores their embeddings in ChromaDB.

- **Request (Form-Data)**:
  - `files`: Upload multiple files (PDF, DOCX, TXT).

- **Example Request** (using Postman or curl):
  ```bash
  curl -X 'POST' \
    'http://127.0.0.1:8000/ingest/' \
    -F 'files=@example.pdf' \
    -F 'files=@document.docx'
  ```

- **Response**:
  ```json
  {
    "message": "Documents example.pdf, document.docx ingested successfully."
  }
  ```

### 2. **Query Endpoint** (`/query/`)

- **Method**: `POST`
- **URL**: `/query/`
- **Description**: Accepts a search query and retrieves the most relevant documents based on embeddings similarity.

- **Request (JSON)**:
  ```json
  {
    "query": "search text"
  }
  ```

- **Example Request** (using Postman or curl):
  ``
   'POST' \
    'http://127.0.0.1:8000/query/' \
    -H 'Content-Type: application/json' \
    -d '{"query": "your search text here"}'
  ```

- **Response**:
  ```json
  {
    "results": {
      "documents": ["Relevant document text 1", "Relevant document text 2"],
      "metadatas": [{"filename": "example.pdf"}, {"filename": "document.docx"}],
      "distances": [0.12, 0.18]
    }
  }
  ```

## Code Walkthrough

### **1. Document Ingestion (`/ingest/` Endpoint)**

- The `ingest_documents` function handles multiple file uploads.
- Files are processed based on their type (PDF, DOCX, TXT):
  - **PDF Files**: Text is extracted using `PyPDF2`.
  - **DOCX Files**: Text is extracted using `python-docx`.
  - **TXT Files**: Text is read and decoded as UTF-8.
- Embeddings for the extracted text are generated using the `all-MiniLM-L6-v2` model from `sentence-transformers`.
- The embeddings are stored in a ChromaDB collection with the document content and metadata.

### **2. Querying Documents (`/query/` Endpoint)**

- The `query_document` function accepts a JSON body containing the search query.
- The query text is encoded into an embedding using the same model.
- The embedding is used to query ChromaDB for the most similar documents.
- The response includes the matching documents, metadata, and similarity scores.

## Dependencies

- `fastapi`
- `uvicorn`
- `sentence-transformers`
- `chromadb`
- `PyPDF2`
- `python-docx`
- `aiofiles`

Install all dependencies using:
```
pip install -r requirements.txt
```


---

Y