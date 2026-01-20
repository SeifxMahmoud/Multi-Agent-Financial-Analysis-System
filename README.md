# Multi-Agent Financial Analysis System

This repository contains the workflows, ingestion pipeline, and supporting documentation for a multi-agent RAG system designed to answer financial questions about First Abu Dhabi Bank’s quarterly performance. The system uses metadata-enriched vector search, deterministic numerical reasoning, and multi-agent orchestration through n8n.

Two documents accompany this project:

- **Architecture_Document.pdf** — system design, agents, workflow, ingestion pipeline, and reasoning framework  
- **Evaluation_Report.pdf** — results of 23 evaluation queries across retrieval, multi-hop reasoning, numeric calculation, temporal analysis, and guardrail behavior


## Overview

The system ingests FAB’s publicly available quarterly PDFs (QEP, QFS, QRC), parses them into structured content, embeds the segments using OpenAI embeddings, and stores them in a Pinecone vector database. Queries are processed through a multi-agent workflow that performs retrieval, calculation, temporal reasoning, and safe response construction.

The project includes:

- Multi-agent orchestration workflow (`multiagent_workflow.json`)
- Knowledge base ingestion workflow (`ingestion_pipeline.json`)
- Parsed metadata logic for quarter/year/report-type classification
- Deterministic calculator agent for financial ratios and deltas
- Guardrail and clarification mechanisms


## Features

- **Metadata-aware vector retrieval** (quarter, year, section, report type)
- **Deterministic numeric calculations** (LDR, CIR, YoY, QoQ, multi-period deltas)
- **Trend analysis and temporal reasoning**
- **Multi-document aggregation**
- **Hallucination prevention and out-of-scope handling**
- **Streamlined ingestion pipeline with LlamaParse**
- **n8n-based agent orchestration**


## Project Structure
/
├── workflows/
│ ├── multiagent_workflow.json
│ ├── ingestion_pipeline.json
│
├── docs/
│ ├── Architecture_Document.pdf
│ ├── Evaluation_Report.pdf
│
├── data/
│ └── (FAB quarterly PDFs)
│
└── README.md



## Workflows

### Multi-Agent Workflow
Handles:
- Query classification  
- Retrieval from Pinecone  
- Numeric reasoning  
- Temporal/trend processing  
- Guardrails & safe output formatting  

Agents used:
1. Orchestrator Agent  
2. Retrieval Agent  
3. Calculator Agent  
4. Clarification Agent  
5. Responder Agent  

### Ingestion Workflow
Handles:
- PDF detection  
- Metadata extraction from filenames  
- PDF parsing via LlamaParse  
- Page-level structuring  
- Text chunking  
- Embedding and Pinecone indexing  


## Running the System

### Requirements
- Docker  
- n8n  
- OpenAI API key  
- Pinecone API key  
- LlamaParse API key  

### Setup
```bash
docker compose up -d
1. Open n8n (http://localhost:5678)

2. Import both workflow JSON files

3. Add PDFs into the configured data folder

4. Execute the ingestion workflow

5. Trigger the multi-agent workflow to run queries

Example Queries

“What was FAB’s net profit in Q3 2023?”

“Compare operating expenses between Q1 2023 and Q1 2024.”

“What is the LDR for Q1 2023? Show the calculation.”

“Explain the NIM trend from Q1 2022 to Q1 2024.”

“What was net profit in Q2 2024?” (guardrail example)

“Show a 6-quarter RoTE trend.” (graceful failure case)

Evaluation Summary

23 evaluation queries were executed across five categories.
Results are provided in Evaluation_Report.pdf.

Metrics:

Retrieval Precision: 100%

Retrieval Recall: ~85%

Numerical Accuracy: ~92%

Citation Accuracy: 100%

Avg Latency: 30–45 seconds

Cost per Query: $0.002–$0.006

Two trend queries returned partial results due to missing quarters in the dataset.
Hallucination prevention worked as intended.

Limitations

Incomplete quarter coverage for some trend queries

Structural variations between QEP/QFS/QRC documents

No vision-based chart extraction

Static retriever configuration

Metadata extraction from filenames uses heuristic rules

Future Improvements

Full ingestion of all quarterly reports 2022–2024

Chart/table extraction using multimodal models

Auto-indexing based on file updates

Parallel agent execution for lower latency

Integration with LangSmith/Langfuse for observability

License

This project is provided as part of the FAB AI Engineer assignment and is intended for evaluation purposes only.

