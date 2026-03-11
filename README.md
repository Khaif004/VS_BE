# Backend - Pipeline Parser API

This is a FastAPI backend service that analyzes pipeline data to calculate the number of nodes, edges, and determine if the pipeline forms a Directed Acyclic Graph (DAG).

## Features

- **Pipeline Analysis**: Calculates the number of nodes and edges in a pipeline
- **DAG Detection**: Uses Kahn's algorithm for topological sorting to detect if the pipeline is a DAG
- **CORS Support**: Configured to accept requests from the React frontend (localhost:3000)

## Setup Instructions

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Run the Server

```bash
uvicorn main:app --reload
```

The server will start on `http://localhost:8000`

## API Endpoints

### GET /
Health check endpoint
- **Response**: `{"Ping": "Pong"}`

### POST /pipelines/parse
Analyzes a pipeline and returns statistics

- **Request Body**:
```json
{
  "nodes": [
    {"id": "node-1", ...},
    {"id": "node-2", ...}
  ],
  "edges": [
    {"source": "node-1", "target": "node-2", ...}
  ]
}
```

- **Response**:
```json
{
  "num_nodes": 2,
  "num_edges": 1,
  "is_dag": true
}
```

## DAG Detection Algorithm

The backend uses Kahn's algorithm for topological sorting to detect cycles:

1. Calculate in-degree for each node
2. Start with nodes that have 0 in-degree
3. Process nodes and reduce in-degree of neighbors
4. If all nodes are processed, the graph is a DAG
5. If some nodes remain unprocessed, there's a cycle (not a DAG)

## Testing

You can test the endpoint using curl:

```bash
curl -X POST http://localhost:8000/pipelines/parse \
  -H "Content-Type: application/json" \
  -d '{"nodes":[{"id":"1"},{"id":"2"}],"edges":[{"source":"1","target":"2"}]}'
```

Expected response:
```json
{"num_nodes":2,"num_edges":1,"is_dag":true}