# Multimodal Fraud Investigation Assistant

An AI-powered **Multimodal Fraud Investigation Assistant** that combines **Retrieval-Augmented Generation (RAG)**, **structured transaction analysis**, **PDF/document retrieval**, and **image understanding** to assist investigators in analyzing potentially fraudulent transactions.

The system retrieves relevant fraud policies, historical fraud cases, transaction records, and supporting images, then uses an LLM to generate a contextual investigation report.

---

## 🚀 Project Overview

Fraud investigation often requires analyzing information from multiple sources such as:

* Transaction records
* Historical fraud reports
* Fraud policies and guidelines
* Invoices and screenshots
* Structured databases
* Unstructured documents

Manually analyzing all these sources can be time-consuming.

This project provides a **single AI-powered interface** where a user can ask questions about a transaction or fraud case. The system retrieves relevant evidence from multiple sources and generates an investigation-oriented response.

### Example Questions

```text
Is transaction TXN10025 suspicious?

Why was transaction TXN10025 flagged?

Are there similar fraud cases in the historical reports?

What fraud policy applies to this transaction?

What unusual patterns exist in this customer's transactions?

Summarize the evidence related to transaction TXN10025.
```

---

# 🎯 Objectives

The main objectives of this project are:

1. Build a **multimodal RAG pipeline** for fraud investigation.
2. Retrieve relevant information from fraud reports and policy documents.
3. Query structured transaction data using Pandas/SQL.
4. Process supporting images such as invoices and screenshots.
5. Combine structured and unstructured evidence.
6. Use an LLM to generate a contextual investigation report.
7. Provide an easy-to-use interface through Streamlit.
8. Reduce the manual effort required during initial fraud investigation.

---

# 🏗️ System Architecture

```text
                    ┌──────────────────────┐
                    │       User           │
                    │  Fraud Investigation │
                    │      Question        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │        Streamlit     │
                    │                      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    RAG Pipeline      │
                    │    pipeline.py       │
                    └──────────┬───────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
       ┌───────────────────┐       ┌───────────────────┐
       │ Semantic Retrieval│       │ Structured Query  │
       │                   │       │                   │
       │ ChromaDB          │       │ Pandas / SQLite   │
       │ Fraud Reports     │       │ Transactions      │
       │ Policy Documents  │       │                   │
       └─────────┬─────────┘       └─────────┬─────────┘
                 │                           │
                 └─────────────┬─────────────┘
                               ▼
                    ┌──────────────────────┐
                    │   Evidence Fusion    │
                    │      fusion.py       │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      Gemini LLM      │
                    │   Text + Vision      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Investigation Report │
                    │  Evidence + Analysis │
                    └──────────────────────┘
```

---

# 🔄 Project Workflow

The project follows the following pipeline:

```text
Data Generation
      ↓
Data Ingestion
      ↓
Document Extraction
      ↓
Chunking
      ↓
Embedding Generation
      ↓
ChromaDB
      ↓
User Question
      ↓
Semantic Retrieval + Structured Query
      ↓
Evidence Fusion
      ↓
Prompt Construction
      ↓
Gemini LLM
      ↓
Investigation Report
```

---

# 📂 Project Structure

```text
fraud_investigation_assistant/
│
├── main.py
│   └── CLI entry point for asking fraud investigation questions
│
├── app.py
│   └── Streamlit web application
│
├── setup_knowledge_base.py
│   └── Generates synthetic data and builds the ChromaDB knowledge base
│
├── requirements.txt
│   └── Python dependencies
│
├── .env.example
│   └── Example environment variables
│
├── .gitignore
│   └── Files and folders excluded from Git
│
├── data/
│   │
│   ├── transactions/
│   │   ├── transactions.csv
│   │   └── transactions.db
│   │
│   ├── fraud_reports/
│   │   └── Synthetic historical fraud case PDFs
│   │
│   ├── policies/
│   │   └── Synthetic fraud policy and guideline PDFs
│   │
│   ├── images/
│   │   └── Synthetic invoice and screenshot images
│   │
│   └── chroma_db/
│       └── Persistent ChromaDB vector store
│
└── src/
    │
    ├── config.py
    │   └── Application configuration and model settings
    │
    ├── data_generation/
    │   ├── generate_transactions.py
    │   ├── generate_fraud_reports.py
    │   ├── generate_policy_docs.py
    │   └── generate_images.py
    │
    ├── ingestion/
    │   ├── chunking.py
    │   ├── pdf_ingest.py
    │   └── image_ingest.py
    │
    ├── llm/
    │   ├── embeddings.py
    │   └── gemini_client.py
    │
    ├── retrieval/
    │   ├── vector_store.py
    │   ├── structured_query.py
    │   └── fusion.py
    │
    └── rag/
        └── pipeline.py
```

---

# 🧩 Key Components

## 1. Synthetic Data Generation

The project uses synthetic data instead of real customer or financial information.

The data generation modules create:

### Transaction Data

```text
generate_transactions.py
```

Generates transaction records containing information such as:

* Transaction ID
* Customer ID
* Transaction amount
* Transaction type
* Location
* Timestamp
* Merchant
* Failed transaction count
* Risk-related attributes
* Fraud label

The generated transactions are stored in:

```text
data/transactions/transactions.csv
data/transactions/transactions.db
```

---

## 2. Fraud Reports

```text
generate_fraud_reports.py
```

Generates synthetic historical fraud investigation reports.

These documents provide historical context that can be retrieved when investigating a new transaction.

Example information:

```text
Case ID
Transaction ID
Fraud Type
Investigation Findings
Evidence
Resolution
Investigator Notes
```

---

## 3. Fraud Policies

```text
generate_policy_docs.py
```

Generates synthetic fraud detection policies and investigation guidelines.

For example:

```text
High-value transaction policy
Multiple failed transaction policy
Unusual location policy
Account takeover guidelines
Suspicious transaction investigation procedure
```

These documents are indexed so that the RAG system can retrieve the relevant policy for a user query.

---

# 📄 4. PDF Ingestion

The PDF ingestion pipeline extracts text from fraud reports and policy documents.

```text
PDF
 ↓
Text Extraction
 ↓
Cleaning
 ↓
Chunking
 ↓
Embedding
 ↓
ChromaDB
```

This allows the system to retrieve only the relevant sections rather than passing entire documents to the LLM.

---

# ✂️ 5. Document Chunking

Large documents are divided into smaller chunks before embedding.

```text
Large PDF
    ↓
Text Extraction
    ↓
Chunking
    ↓
Individual Text Chunks
    ↓
Embeddings
    ↓
ChromaDB
```

### Why chunking?

Passing an entire document to an LLM for every question can:

* Increase token usage
* Increase latency
* Include irrelevant information
* Make retrieval less precise
* Increase processing cost

Chunking allows the system to retrieve the most relevant pieces of information.

---

# 🖼️ 6. Image Processing

The project also handles supporting images such as:

* Invoices
* Payment screenshots
* Transaction screenshots
* Other synthetic fraud evidence

The image ingestion pipeline prepares these images for multimodal analysis.

The Gemini model can then use both **textual evidence and visual information** when generating an investigation response.

---

# 🔢 7. Embeddings

Text chunks are converted into numerical vectors using an embedding model.

```text
Text
 ↓
Embedding Model
 ↓
Vector
 ↓
ChromaDB
```

Embeddings allow the system to perform **semantic search**.

For example:

```text
User:
"Find cases involving repeated failed payments."

```

The system can retrieve documents discussing:

```text
Multiple failed transactions
Repeated payment attempts
Suspicious payment retries
```

even when the exact words are different.

---

# 🗄️ 8. ChromaDB

ChromaDB is used as the project's vector database.

It stores:

```text
Embedding
+
Document Chunk
+
Metadata
```

Example:

```text
Document:
fraud_case_102.pdf

Chunk:
"Customer performed multiple high-value transactions..."

Metadata:
case_id = 102
document_type = fraud_report
```

ChromaDB performs similarity search to retrieve relevant evidence.

---

# 🧾 9. Structured Transaction Retrieval

Not all fraud information should be stored in a vector database.

Transaction records are structured data, so the project uses:

```text
Pandas
+
SQLite
```

for structured queries.

Example:

```text
Find all transactions for customer C1023.

Find transactions above ₹50,000.

Find transactions with multiple failed attempts.

Find transactions from unusual locations.
```

This provides exact numerical and transactional information.

---

# 🔀 10. Evidence Fusion

One of the important components of this project is:

```text
src/retrieval/fusion.py
```

It combines evidence from different sources.

```text
             User Question
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
 Semantic Retrieval     Structured Query
        │                     │
        ▼                     ▼
 Fraud Reports           Transactions
 Policies                SQLite/Pandas
        │                     │
        └──────────┬──────────┘
                   ▼
             Evidence Fusion
                   │
                   ▼
                Gemini
```

This allows the LLM to reason over multiple types of evidence instead of relying only on document retrieval.

---

# 🤖 11. Gemini LLM

The project uses a Gemini model for generation and multimodal analysis.

The LLM receives:

```text
User Question
+
Retrieved Documents
+
Transaction Information
+
Relevant Policy
+
Image Evidence
```

and generates an investigation-oriented response.

The LLM wrapper is implemented in:

```text
src/llm/gemini_client.py
```

---

# 🧠 12. RAG Pipeline

The main orchestration logic is implemented in:

```text
src/rag/pipeline.py
```

The pipeline performs:

```text
1. Receive user question
        ↓
2. Identify relevant evidence
        ↓
3. Perform semantic retrieval
        ↓
4. Query structured transaction data
        ↓
5. Retrieve relevant fraud policies/reports
        ↓
6. Fuse the evidence
        ↓
7. Build the prompt
        ↓
8. Send evidence to Gemini
        ↓
9. Generate investigation report
```

---

# 🖥️ Streamlit Application

The project provides a web interface using Streamlit.

Run:

```bash
streamlit run app.py
```

The application allows users to interact with the fraud investigation assistant through a simple web interface.

---


# ⚙️ Installation

## 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/fraud_investigation_assistant.git
```

Navigate into the project:

```bash
cd fraud_investigation_assistant
```

---

## 2. Create a Virtual Environment

Windows:

```bash
python -m venv venv
```

Activate:

```bash
venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# 🔐 Environment Variables

Create a `.env` file from `.env.example`.

```bash
copy .env.example .env
```

Add the required API key:

```env
GEMINI_API_KEY=your_api_key_here
```

Do **not** commit your `.env` file to GitHub.

The `.gitignore` file should contain:

```gitignore
.env
venv/
__pycache__/
data/chroma_db/
*.pyc
```

---

# 🏗️ Build the Knowledge Base

Before running the application, generate the synthetic data and create the vector database.

Run:

```bash
python setup_knowledge_base.py
```

This process creates:

```text
Transactions
    ↓
Fraud Reports
    ↓
Policy Documents
    ↓
Images
    ↓
Document Processing
    ↓
Chunking
    ↓
Embeddings
    ↓
ChromaDB
```

The persistent ChromaDB database is automatically created under:

```text
data/chroma_db/
```

---

# ▶️ Run the Application

## Streamlit

```bash
streamlit run app.py
```

Then open the Streamlit URL shown in the terminal.

---

# 🧪 Example Investigation

### User Question

```text
Is transaction TXN10025 suspicious?
```

### System Processing

```text
User Question
      ↓
Transaction Lookup
      ↓
Semantic Search
      ↓
Historical Fraud Cases
      ↓
Fraud Policies
      ↓
Evidence Fusion
      ↓
Gemini
```

### Example Response

```text
Transaction TXN10025 shows several indicators that
require further investigation.

Evidence:
- The transaction amount is significantly higher than
  the customer's previous transactions.
- Multiple failed payment attempts occurred before
  the successful transaction.
- A relevant fraud policy identifies repeated failed
  attempts followed by a high-value transaction as
  an investigation indicator.
- Similar patterns were found in historical fraud cases.

Recommendation:
The transaction should be reviewed using the
organization's standard fraud investigation procedure.
```

> The application is designed to assist investigators by organizing evidence; it does not replace human investigation or make definitive fraud determinations.

---

# 🛠️ Technologies Used

| Technology | Purpose                             |
| ---------- | ----------------------------------- |
| Python     | Core programming language           |
| Streamlit  | Web interface                       |
| Gemini     | LLM and multimodal analysis         |
| ChromaDB   | Vector database                     |
| Pandas     | Structured data analysis            |
| SQLite     | Transaction database                |
| Embeddings | Semantic representation             |
| PyPDF      | PDF text extraction                 |
| RAG        | Context-aware information retrieval |
| dotenv     | Environment configuration           |

---

# 📌 Why Multimodal RAG?

Traditional RAG systems primarily work with text documents.

Fraud investigations can contain multiple types of evidence:

```text
                 Fraud Investigation
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
       ▼                 ▼                 ▼
   Documents        Transactions        Images
       │                 │                 │
       ▼                 ▼                 ▼
   PDF Reports       CSV / SQL       Invoice /
   Policies                          Screenshot
       │                 │                 │
       └─────────────────┼─────────────────┘
                         ▼
                    Multimodal RAG
                         │
                         ▼
                  Investigation Report
```

Combining these sources provides a more complete evidence context for investigation.

---

# 🔑 Key Features

* ✅ Multimodal fraud investigation
* ✅ Retrieval-Augmented Generation
* ✅ PDF document retrieval
* ✅ Fraud policy retrieval
* ✅ Historical case retrieval
* ✅ Structured transaction analysis
* ✅ SQLite transaction database
* ✅ Pandas-based querying
* ✅ ChromaDB vector search
* ✅ Semantic search using embeddings
* ✅ Image evidence analysis
* ✅ Evidence fusion
* ✅ Gemini-powered response generation
* ✅ Streamlit interface
* ✅ CLI interface
* ✅ Synthetic data generation
* ✅ Persistent vector database

---

# 🔒 Data Privacy

This project uses **synthetically generated data** for demonstration and educational purposes.

No real customer financial information or personally identifiable financial data should be included in the repository.

For a production system, appropriate security, access control, encryption, data governance, and organizational compliance requirements would be necessary.

---

# ⚠️ Limitations

This project is a prototype designed for learning and demonstration.

Current limitations include:

* Synthetic rather than real-world fraud data
* Limited fraud patterns
* LLM responses can require human verification
* Image analysis depends on image quality
* Retrieval quality depends on chunking and embeddings
* Production-scale authentication and authorization are not implemented
* No production fraud decision engine is included

---

# 🚀 Future Enhancements

Potential future improvements include:

* Real-time transaction monitoring
* Advanced anomaly detection
* Fraud risk scoring
* Reranking models for improved retrieval
* Hybrid search combining keyword and semantic search
* Agentic investigation workflows
* Automated evidence citation
* Investigation history and case management
* Authentication and role-based access control
* Production vector databases such as Qdrant or Pinecone
* Advanced multimodal document understanding
* Human-in-the-loop investigation approval
* RAG evaluation using retrieval and answer-quality metrics

---

# 📚 Learning Concepts Demonstrated

This project demonstrates practical knowledge of:

```text
Generative AI
     ↓
Large Language Models
     ↓
Embeddings
     ↓
Vector Databases
     ↓
Semantic Search
     ↓
RAG
     ↓
Multimodal RAG
     ↓
Structured + Unstructured Retrieval
     ↓
Evidence Fusion
     ↓
LLM-based Generation
```

It also demonstrates software concepts such as:

* Python modular programming
* API integration
* Environment variables
* Database querying
* Data preprocessing
* Document ingestion
* Vector search
* Streamlit application development

---

# 👩‍💻 Author

**Pranayeni Tammana**

B.Tech – Artificial Intelligence and Data Science

---

# ⭐ Project Summary

**Multimodal Fraud Investigation Assistant** is a GenAI-based RAG application designed to assist fraud investigators by combining **structured transaction data, historical fraud reports, fraud policies, and visual evidence**.

The system uses **semantic retrieval, structured querying, evidence fusion, and Gemini-based multimodal generation** to transform scattered fraud-related information into a contextual investigation report.

```text
Multiple Data Sources
        ↓
Data Ingestion
        ↓
Chunking + Embeddings
        ↓
ChromaDB + Structured Database
        ↓
Hybrid Evidence Retrieval
        ↓
Evidence Fusion
        ↓
Gemini
        ↓
Fraud Investigation Assistant
```

---
