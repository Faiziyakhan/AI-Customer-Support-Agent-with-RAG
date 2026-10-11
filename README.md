# AI Customer Support Agent with RAG

An autonomous, retrieval-augmented customer support email automation system built with **n8n**, **Google Gemini**, and **LangChain** in-memory vector storage.

This system monitors incoming customer inquiries, extracts structured complaint entities via AI, searches an internal company policy knowledge base using semantic embeddings, and generates verified, empathetic, policy-compliant email replies directly in Gmail threads.

---

## Overview

High-volume support operations often suffer from long response latencies, inconsistent policy enforcement, and generic or hallucinated automated replies.

This platform addresses these operational challenges through a two-stage multi-agent architecture:

1. **Extraction Agent:** Parses unstructured email inquiries into clean, typed business parameters with issue categorization and priority classification.
2. **Support Specialist Agent:** Queries an in-memory RAG vector store indexing company manuals, shipping rules, return procedures, and warranty terms before drafting context-aware email responses.
3. **Automated Response Loop:** Dispatches threaded Gmail replies automatically without human intervention while safely flagging unresolved or missing information for manual escalation.

---

## Key Capabilities

- **Automated Gmail Ingestion:** Real-time polling trigger capturing unread customer support inquiries.
- **Structured Intent & Priority Extraction:** Identifies order numbers, damaged items, return/refund intents, and assigns priority (Critical, High, Medium, Low).
- **Retrieval-Augmented Generation (RAG):** Uses Gemini embeddings paired with an in-memory vector store to ground responses strictly in internal company guidelines.
- **Zero-Hallucination Policy:** Agent prompts strictly restrict policy assertions to verified vector database chunks; unsupported claims are gracefully escalated to human staff.
- **Threaded Customer Reply:** Directly responds to the original email ID via Gmail OAuth API to maintain message history.

---

## Workflow Architecture

```text
Incoming Customer Email (Gmail Trigger)
                 ↓
      Fetch Full Email Message
                 ↓
      [Agent 1: Email Parser]
 (Gemini Model + Structured Output Parser)
                 ↓
    Extracts Structured Metadata:
    • Order Number
    • Product Name
    • Customer Intent
    • Complaint Category
    • Priority Level
                 ↓
 [Agent 2: AI Customer Support Agent]
                 ↑
   [RAG Retrieval Tool] ← Vector Store (Gemini Embeddings)
                 │         └─ Internal Knowledge Base
                 │            (Policies, FAQ, Warranty, Shipping)
                 ↓
 Generates Grounded, Empathetic Reply
                 ↓
  Automated Threaded Gmail Reply
```

---

## Internal Knowledge Base Indexing (RAG Pipeline)

The workflow dynamically indexes local documentation files (`*.txt`) into an in-memory vector database:

- **Data Ingestion:** Reads disk files through the Read/Write Files from Disk node and chunks documents via LangChain's Default Data Loader.
- **Embedding Generation:** Embeddings are produced using `@n8n/n8n-nodes-langchain.embeddingsGoogleGemini`.
- **Vector Storage & Tool Hook:** Stored in `@n8n/n8n-nodes-langchain.vectorStoreInMemory` and exposed directly to the Support Agent as an autonomous retrieval tool (`Simple Vector Store1`).

**Indexed Documentation Coverage:**

- Return & Refund Policy (inspection requirements, photo evidence rules, return shipping coverage)
- Shipping Policy (standard delivery times: 3–7 business days; express: 1–3 business days)
- Warranty Policy & Troubleshooting Guides
- Frequently Asked Questions (FAQ)

---

## Structured Extraction Schema

Every inbound email passes through a LangChain Structured Output Parser enforcing the following JSON contract:

```json
{
  "order_number": "TG-45821",
  "product_name": "Laptop",
  "customer_intent": "Replacement or Refund",
  "complaint_category": "Damaged Product",
  "priority_level": "High",
  "complaint_summary": "Customer received a damaged laptop with a cracked screen and requested a replacement or refund."
}
```

**Priority Classification Rules:**

| Priority | Criteria |
|----------|----------|
| **Critical** | Safety risks, double charges, business-halting product failures. |
| **High** | Damaged items, missing shipments, wrong item delivered, warranty claims. |
| **Medium** | Shipping status inquiries, return/refund requests, order modifications. |
| **Low** | General inquiries, product availability, pre-sales questions. |

---

## Technology Stack

- **Workflow Engine:** n8n (LangChain nodes integration)
- **AI Models:** Google Gemini Chat Model (`@n8n/n8n-nodes-langchain.lmChatGoogleGemini`)
- **Embeddings:** Google Gemini Embeddings (`@n8n/n8n-nodes-langchain.embeddingsGoogleGemini`)
- **Vector Database:** In-Memory Vector Store (`@n8n/n8n-nodes-langchain.vectorStoreInMemory`)
- **Document Loading:** LangChain Default Data Loader
- **Email Gateway:** Gmail OAuth2 integration (Trigger, Message fetch, Threaded reply)

---

## Evidence & Verified Visual Proof

This repository includes authentic execution records, workflow canvas layouts, and verified customer email replies.

### 1. Workflow Canvas & Execution Performance

**Verified Successful Execution (ID#384 · 1.244s)**

![Successful Execution](./screenshots/execution-success.png)

**Full Workflow Canvas Architecture**

![Workflow Canvas](./screenshots/workflow-canvas.png)

### 2. Multi-Agent RAG Topology Detail

**In-Memory Vector Store & Agent Connectivity**

![RAG Topology](./screenshots/rag-topology.png)

### 3. Live Customer Support Resolution Threads (Gmail Verification)

**Damaged Product Claim & Policy Resolution**

![Damaged Product Reply](./screenshots/gmail-damaged-product.png)

**Shipping Status Inquiry & Policy SLA**

![Shipping Inquiry Reply](./screenshots/gmail-shipping-inquiry.png)

**Warranty Claim Inquiry & Customer Communication**

![Warranty Claim Reply](./screenshots/gmail-warranty-claim.png)

---

## Data & Demo Notice

Email samples, order IDs (e.g., `TG-78945`, `TG-45821`, `TG-908726`), customer names, and demonstration files shown in screenshots represent **test scenarios** executed within a staging environment for validation purposes. No proprietary client data is exposed.

---

## Security

API credentials, Google Palm/Gemini API account keys, and OAuth access tokens are removed from the shared workflow configuration.

---

## Project Status

**Completed — Portfolio Project**

---

## Author

**Faiziya Khan** 

AI Automation Engineer | n8n | AI Agents | RAG | Workflow Automation

📧 Email: khanfaiziya2@gmail.com
