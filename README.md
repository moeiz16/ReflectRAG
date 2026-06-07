# ReflectRAG

An advanced, self-correcting **Corrective Retrieval-Augmented Generation (CRAG)** engine built on top of low-level **LangGraph** primitives. `ReflectRAG` moves past standard, linear vector lookups by introducing stateful evaluation loops—using structured LLM judges to grade retrieved documents, rewrite sub-optimal queries, and pull live search results as a deterministic fallback when local vector database confidence falls below strict thresholds.

## 🧠 Architectural Overview

Traditional RAG setups process data blindly, rendering answers even if vector queries return noisy or completely irrelevant data fragments. `ReflectRAG` utilizes a low-level cyclic state graph to implement self-reflection:

1. **Retrieve:** Context documents are extracted asynchronously from a high-performance vector index.
2. **Evaluate (Grading Loop):** An LLM acting as a structured judge scores every retrieved document chunk for query relevance.
3. **Route Decisions:** 
   - *High Relevance:* The engine forwards context directly to the final generator node.
   - *Irrelevant/Ambiguous Data:* The engine triggers a **Query Transformer** to optimize the user text and routes execution out to external web search instances.
4. **Generate:** The user receives a hallucination-free answer completely grounded in validated, verified facts.

## 🛠️ Core Tech Stack
- **Orchestration & State Machine:** LangGraph / LangChain
- **Vector Database:** Qdrant Vector DB
- **External Web Search Wrapper:** Tavily API
- **Underlying Intelligence:** OpenAI API Model Ecosystem

## 📁 Repository Blueprint
```text
├── my-agents/
│   ├── requirements.txt      # Core system package distributions
│   ├── graph.py             # LangGraph StateGraph definitions and routing nodes
│   ├── state.py             # GraphState type definitions 
│   └── tools.py             # Tavily web-search and Qdrant connector tools
├── langgraph.json            # Configuration profile for LangGraph Studio
└── README.md                 # Primary system documentation
