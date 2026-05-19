# 🤖 Contract Intel Engine: Enterprise GenAI & LLM Data Pipeline

## 🎯 Executive Summary & Business Value
**The Problem:** Corporate legal and procurement departments spend thousands of manual hours reviewing vendor contracts to find hidden renewal dates, liability clauses, and SLA frameworks. Because this data is locked inside unstructured PDFs, it cannot be queried, leading to missed deadlines and regulatory compliance risks.

**The Solution:** As the Lead Product Owner, I designed a scalable **Retrieval-Augmented Generation (RAG)** pipeline that automates the ingestion, chunking, embedding, and storage of legal contracts. By converting raw text into semantic vector embeddings, business users can query thousands of legal documents simultaneously using natural language, reducing manual audit cycles from hours to seconds.

---

## 🏗️ Technical Architecture Diagram
[ Raw Contracts: PDF / Word ]
│
▼ (Document Ingestion Engine)
┌────────────────────────────────────────────────────────┐
│                 GENAI EXTRACTION PIPELINE              │
│                                                        │
│   1. TEXT EXTRACTION: OCR & Structural Markdown Parser │
│        │                                               │
│        ▼ (Fixed-Size Chunking with 10% Overlap)        │
│                                                        │
│   2. CHUNKING ENGINE: Tokenization & Text Segments     │
│        │                                               │
│        ▼ (Passage Vectorization via Embedding Model)   │
│                                                        │
│   3. EMBEDDING PIPELINE: Text converted to Vectors    │
└────────────────────────────────────────────────────────┘
│
▼ (Payload Storage)
[ Vector Database: Pinecone / Milvus ]
│
┌───────────┴───────────┐
▼ (Semantic Query)      ▼ (Contextual Injection)
[ User Search Prompt ] ──► [ Foundation LLM ] ──► [ Actionable Legal Answer ]


---

## 🛠️ Tech Stack Utilized
* **Orchestration & Frameworks:** LangChain / LlamaIndex / Python
* **Vector Infrastructure:** Pinecone / Azure AI Search
* **Foundational Models:** OpenAI text-embedding-3 / GPT-4o
* **Cloud Infrastructure:** Azure AI Foundry / GitHub Actions

---

## 📋 AI Platform Specifications & PM Artifacts

### 1. Document Chunking & Ingestion Strategy
To prevent information loss across long-form legal documents, I established a strict token-boundary strategy for our chunking algorithms. Below is a production-ready Python specification template illustrating how text is segmented and vectorized:

```python
# Processing Layer: Chunking and Embedding Generation Specification
from langchain_text_splitters import RecursiveCharacterTextSplitter

def process_contract_text(raw_legal_text):
    # Defining token guardrails: 1000 character chunks with 100 character overlap
    text_splitter = RecursiveCharacterTextSplitter(
        chunk_size=1000,
        chunk_overlap=100,
        length_function=len
    )
    
    # Generate structural text segments
    contract_chunks = text_splitter.split_text(raw_legal_text)
    return contract_chunks

# Output maps directly to the Vector DB payload schema

2. User Experience Validation (Happy Path Scenario)
To ensure the model output remains accurate and legally safe, I designed the user query lifecycle with strict relevance thresholds:

The Persona: A Procurement Manager evaluating vendor terms.

The Natural Language Query: "What is the termination notice period for this contract?"

The Pipeline Guardrail: The system performs a semantic similarity search against the vector database, extracts the exact clause block, passes it to the LLM as immutable context, and demands a citation. If the cosine similarity score is below 0.75, the system gracefully outputs a fallback message rather than hallucinating a false date.

3. Data Governance & Model Safety
Tenant Isolation: Contract metadata tags ensure users can only query documents belonging to their specific department domain.

Hallucination Mitigation: Grounded system prompts explicitly forbid the LLM from utilizing out-of-context parametric memory when answering legal queries.


4. Scroll to the bottom of the editor page, click **Commit changes...**, select **Commit directly to the main branch**, and click the green **Commit changes** button.

---

## 📂 Step 3: Add the RAG Prompt Template Artifact

To prove your expertise in controlling LLM behaviors, let's add an explicit prompt engineering configuration asset to this project.

1. Back on the main screen of your new repository, click **Add file** -> **Create new file**.
2. Name the file: `system_prompt_config.json`
3. Paste this production-grade prompt configuration template inside:
   ```json
   {
     "config_name": "legal_rag_extraction_v1",
     "temperature": 0.0,
     "max_tokens": 500,
     "system_instruction": "You are an elite corporate legal counsel assistant. Your task is to analyze the provided contract context segments and answer the user query with 100% precision. Rule 1: Rely ONLY on the provided context. Do not use external knowledge. Rule 2: If the text context does not contain the answer, reply explicitly with 'Information not found in source documents'. Rule 3: Always provide a direct quotation from the text to justify your response."
   }
Scroll to the bottom and click the green Commit changes button.
