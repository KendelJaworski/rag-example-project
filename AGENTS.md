# AGENTS.md — High-Signal Guidance for This Repository

## Setup & Prerequisites

### Docker Compose Services
This project uses `docker compose` to orchestrate:
- **Application**: Python RAG app (main service)
- **Database**: PostgreSQL vector database
- **LLM/Embedding Models**: Local models hosted on the network

**Before running or developing:**
1. Ensure Docker Desktop is running and connected to your local network
2. Verify you can reach the LLM/embedding model endpoints from within containers
3. Check `docker-compose.yml` for correct service names, ports, and environment variables

### Database Setup
- PostgreSQL is used as a **vector database** (not just relational storage)
- Ensure vector extension (`pgvector`) is installed before inserting embeddings
- Default connection details are in `.env` or `docker-compose.yml`; update if deploying elsewhere

## Common Pitfalls

### 1. Model Connectivity Errors
The app expects to reach local LLM/embedding models via network. If you see:
```
Connection refused / timeout to model endpoint
```
→ Verify Docker Desktop networking and that the host machine is running the model server.

### 2. Vector DB Schema Issues
If embeddings fail to insert or queries return empty:
- Confirm `pgvector` extension is enabled in PostgreSQL
- Check embedding dimension matches between model output and DB schema
- Review table/column names for expected embedding storage (likely `embeddings` table)

### 3. Frontend Visualization
The project includes a frontend to visualize embeddings in 2D reduction with HSL coloring:
- Ensure the frontend service is running before accessing the visualization UI
- Default port/path may be exposed via Docker; check `docker-compose.yml` for host mapping

## Architecture Notes

- **RAG Flow**: Documents → Embeddings (local model) → PostgreSQL vector DB → Retrieval → LLM response
- **Visualization**: 2D reduction of embeddings displayed with HSL color encoding
- **Deployment**: Pure Docker Compose; no external dependencies beyond network-accessible models

## Development Workflow

1. Start services: `docker compose up -d`
2. Run Python scripts from the app container or host (check entrypoint in `docker-compose.yml`)
3. For local debugging, consider mounting source code to the app service volume
