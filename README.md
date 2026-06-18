RAG-Chatbot

```markdown
 🤖 PDF Chatbot (Production-Ready RAG Pipeline)

An enterprise-grade, localized Retrieval-Augmented Generation (RAG) chatbot that securely ingests private PDF data, extracts text structures, indexes vector representations using Gemini Embeddings into a Chroma vector database, and exposes an intuitive UI for context-aware interactions.
I've used Google's Gemini API but you can go with any LLM of your choice and it will work just fine.


 🚀 Key Features

* **Dynamic Data Ingestion:** Instantly turns unstructured local PDF documents into an isolated private vector knowledge base.
* **Next-Gen Embedding & Inference Layers:** Utilizes `gemini-embedding-2-preview` for deep semantic tokenization and `gemini-2.5-flash` for rapid, cost-effective, deterministic answers.
* **Fully Formatted Web-GUI:** Built with a clean, responsive web interface using Gradio Blocks and customized inline CSS.
* **Production-Ready LCEL Architecture:** Uses LangChain Expression Language (LCEL) pipelines for seamless internal variable routing.



 🛠️ Installation & Setup

1. Clone the Repository:
   bash-
   git clone [(https://github.com/Om-Bhavsar-6/RAG-Chatbot.git)](https://github.com/Om-Bhavsar-6/RAG-Chatbot.git)
   cd rag-chatbot

```

2. Install Required Dependencies:
```bash
pip install langchain-google-genai pypdf gradio chromadb langchain-core

```


3. **Configure API Credentials:**
Set up your Google AI Studio API key as an environment variable:
```bash
export GOOGLE_API_KEY="your_actual_gemini_api_key_here"

```


4. **Run the Application:**
```bash
python app.py

```


Open the local URL generated in your terminal (typically `http://127.0.0.1:7860`).

---

## 🗺️ RAG Blueprint: Architecture Breakdown

```
[ Local PDF ] ➔ [ PyPDFLoader ] ➔ [ Chunking (Recursive) ] ➔ [ LLM Embeddings ] ➔ [ Authorized Vector DB ]
                                                                                                 |
                                                                                         (Semantic Match)
                                                                                                 |
[ User Query ] ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ [ Prompt Injection Layer ] ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ ➔ [  LLM Inference ]

```

### 1. Ingestion & Tokenization (Retrieval)

When a PDF is dropped into the interface, `PyPDFLoader` strips the document layers. The `RecursiveCharacterTextSplitter` breaks down text structures into 1000-character slices with a 200-character semantic overlap. This overlap ensures contextual information is not severed at boundaries.

### 2. Mathematical Vectorization

The text slices are routed to `GenerativeAIEmbeddings` which convert natural string syntax into high-dimensional vector arrays. These arrays are stored in a database (`Chroma`) optimized for cosine similarity or Euclidean distance calculations.

### 3. Contextual Injection & Generation (Augmentation)

When a user asks a question, the pipeline extracts the top 4 most vector-similar text nodes from the database, stitches them together as string literals, drops them into a deterministic system prompt template as `{context}`, and forces `gemini-2.5-flash` to answer **strictly** from your data.

---

## 🛡️ Production Readiness: Securing the Vector Layer

While this repository executes an ephemeral, memory-isolated Chroma database for local prototyping, moving this RAG pipeline to a production environment requires structural shifts—chiefly around **Vector Database Authorization and Access Control**.

In an enterprise environment, private documents represent sensitive corporate data. Exposing an unauthenticated vector database to an application layer is a severe vulnerability.

### Production Architectural Shift:

```
[ User Chatfront ] ➔ [ API Gateway / Auth Layer ] ➔ [ Backend RAG Service (LangChain) ] 
                                                                 |
                                                    (Secure TLS with API Token/gRPC)
                                                                 |
                                                    [ Private cloud Vector DB ]
                                                    (e.g., Pinecone, Qdrant, Milvus)

```

1. **Moving to Persistent Clouds:** In production, Chroma is replaced with distributed, managed cloud vector databases like Pinecone, Qdrant, or enterprise-hosted instances of Milvus/Vespa.
2. **Placing Databases Behind Isolation Layers:** Production vector databases are housed in private subnets inside a Virtual Private Cloud (VPC). They are never exposed directly to the open web or the client-facing GUI code.
3. **Strict Authorization & RBAC (Role-Based Access Control):** * Connections between your backend RAG service (LangChain) and the vector engine must use TLS encryption.
* Access requires strict bearer tokens or cryptographic API keys passed via secure network headers.
* Fine-grained authorization models ensure that if "User A" logs into your chatbot, the backend queries the vector store utilizing metadata filters configured with User A's specific organizational read privileges (preventing data leaks across departments).


