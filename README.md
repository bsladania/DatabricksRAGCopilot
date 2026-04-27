# DatabricksRAGCopilot

This project implements a Retrieval-Augmented Generation (RAG) system combined with an agent-based architecture to support data engineering workflows. It enables users to query enterprise runbooks using natural language and receive grounded, context-aware responses. The system can retrieve relevant operational knowledge, generate SQL validation queries, and provide structured troubleshooting guidance. It is built on Databricks using Vector Search, Delta Lake, and large language models.

Overview

The solution transforms static runbook documentation into an intelligent assistant capable of:

Retrieving relevant information from enterprise documents
Generating SQL queries for data validation
Providing troubleshooting steps for pipeline failures
Suggesting data quality checks and validation strategies

The system uses semantic search to locate relevant context and an agent layer to determine how to respond based on the user’s request.

Architecture

User Query > Agent Router (Intent Detection)

--------------------------------
|         Tool Layer          |
|-----------------------------|
| 1. Vector Search (RAG)      |
| 2. SQL Generator            |
| 3. Summarization (LLM)      |
--------------------------------
    ↓
LLM (Databricks Model Serving) > Final Response

Project Structure

notebooks/
01_ingest_parse_pdf.ipynb 

02_chunk_prepare_index.ipynb

03_vector_index_setup.ipynb

04_tools_layer.ipynb

05_agent_query_app.ipynb

How It Works
1. Document Ingestion
- Runbook PDFs are parsed into clean text and stored in a structured format.
2. Chunking Strategy
- Paragraph-based chunking is used to preserve meaning
- Overlapping chunks improve retrieval continuity
- Chunks are stored in a Delta table (rag_chunks)
3. Embedding and Indexing
- Embeddings are generated using databricks-gte-large-en
- Managed by Databricks Vector Search
- Stored internally within the index
4. Retrieval
- Semantic similarity search retrieves relevant chunks
- Results are passed as context to the language model
5. Agent Routing

The agent uses a tool-based architecture:

- search_runbooks() -> retrieves relevant document chunks
- generate_validation_sql() -> creates SQL queries
- summarize_recovery_steps()  -> produces structured responses

Routing logic analyzes the user query and selects the appropriate tool or combination of tools.

Example Queries

- `ask_agent("Give me a row count validation query")`
- `ask_agent("What validations should I run after pipeline load?")`
- `ask_agent("How do I fix pipeline authentication failure?")`

Technologies Used
- Databricks (Unity Catalog and Vector Search)
- PySpark
- Delta Lake
- Databricks Model Serving (LLM)
- Python

Key Design Decisions
- Overlapping chunking improves retrieval quality
- Managed embeddings simplify implementation
- Tool-based agent design allows flexible responses
- Structured prompts align output with user intent

Conclusion
This project demonstrates how RAG and agent-based systems can be applied to real-world data engineering problems. By combining semantic retrieval with intelligent tool selection, the system provides practical, context-aware assistance for troubleshooting, validation, and SQL generation.

