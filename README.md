# Simplified Agentic RAG System with Gemini & LangGraph  

This project implements a **lightweight Retrieval-Augmented Generation (RAG) system** with a **self-critique and refinement loop** using:  
- **Gemini** (Google Cloud Vertex AI via `google-genai`) as the LLM and embeddings  
- **LangGraph** for workflow orchestration  
- **Pinecone** as the vector database  

##  Features

- Loads a knowledge base (`self_critique_loop_dataset.json`) of ~30 entries  
- Generates embeddings with `gemini-embedding-001` and stores them in Pinecone  
- LangGraph workflow with 4 nodes:
  1. **Retriever** → fetch top-K snippets  
  2. **LLM Answer** → initial response with citations `[KBxxx]`  
  3. **Critique** → checks if answer covers all required content  
  4. **Refinement** → pulls one more snippet if needed and regenerates answer  
- Returns a **citation-backed final answer**  
- Logs all steps  

---

assignment3

│── agentic_rag_simplified.ipynb # Notebook with LangGraph workflow

│── self_critique_loop_dataset.json # Knowledge base (provided)

│── README.md

---

## Setup & Installation

1. **Clone repo & install dependencies**
```bash
pip install pinecone sentence-transformers langchain-pinecone langchain langchain-huggingface langchain-google-genai --quiet --upgrade
```

2. **Set up API keys**

- Set the following in your .env file in the same folder

```bash
GOOGLE_API_KEY="Your Key"
PINECONE_API_KEY="Your Key"
GOOGLE_GENAI_USE_VERTEXAI=False
GOOGLE_CLOUD_PROJECT="Your Cloud Project"
```

3. **Run the RAG workflow**

Open agentic_rag_simplified.ipynb and run queries.

--

## Notes

- Retriever returns top-5 initially to allow critique to detect missing content.

- Critique compares the user’s question vs. initial answer to decide if refinement is needed.

- Always returns citations like [KB001].

- Uses temperature=0 for consistency via GenerateContentConfig.
