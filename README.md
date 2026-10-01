# AURA — AI Business Knowledge & Operations Agent

> A multi-tenant AI business assistant designed to help organizations search internal knowledge, interact with business data, and automate authorized business operations.

🚧 **Status: In Development**

AURA is a portfolio and learning project focused on building a production-oriented AI application using **Node.js, RAG, LangChain, LangGraph, PostgreSQL/pgvector, Redis, BullMQ, and OpenRouter**.

The goal is not to build another "chat with PDF" application. AURA is being designed as an AI system that can **retrieve knowledge, query structured business data, use tools, execute authorized actions, and request human approval for sensitive operations.**

---

## 🎯 Project Goal

Modern businesses have information spread across:

* PDFs
* Documentation
* SOPs
* FAQs
* Pricing documents
* HR policies
* Customer records
* Orders
* Support tickets
* Databases
* External APIs

AURA aims to provide a single AI interface that can understand a user's request and determine whether it should:

```text
Search company knowledge
        ↓
Query structured business data
        ↓
Call an external API
        ↓
Execute a business tool
        ↓
Request human approval
```

---

## 💡 Example

A support employee could ask:

> "John Smith says his order hasn't arrived. Check his order and tell me what I should do."

Instead of simply generating an answer, AURA can eventually perform a workflow like:

```text
User Request
     ↓
Understand Intent
     ↓
Find Customer
     ↓
Get Order
     ↓
Check Delivery Status
     ↓
Search Shipping Policy
     ↓
Generate Recommended Action
     ↓
Respond With Sources
```

For an action request:

> "Create a support ticket for John regarding the delayed order."

The agent can:

```text
Find Customer
      ↓
Find Order
      ↓
Validate Request
      ↓
Check Permissions
      ↓
createSupportTicket()
      ↓
Audit Action
      ↓
Return Result
```

---

# ✨ Planned Features

## Knowledge Management

* [ ] PDF document upload
* [ ] DOCX document upload
* [ ] TXT/Markdown support
* [ ] Document versioning
* [ ] Document metadata
* [ ] Document processing status
* [ ] Knowledge-base management

## RAG

* [ ] Text extraction
* [ ] Intelligent chunking
* [ ] Embeddings
* [ ] PostgreSQL + pgvector
* [ ] Semantic search
* [ ] Metadata filtering
* [ ] Hybrid search
* [ ] Reranking
* [ ] Query rewriting
* [ ] Source citations

## AI

* [ ] OpenRouter integration
* [ ] Configurable LLM provider
* [ ] Configurable models
* [ ] Streaming responses
* [ ] Structured outputs
* [ ] Tool/function calling
* [ ] Prompt management
* [ ] AI evaluation

## Agents

* [ ] LangChain integration
* [ ] LangGraph workflows
* [ ] Stateful agent execution
* [ ] Business tools
* [ ] Tool authorization
* [ ] Human-in-the-loop approval
* [ ] Agent execution history
* [ ] Agent traces

## Business Operations

* [ ] Customers
* [ ] Orders
* [ ] Support tickets
* [ ] Tasks
* [ ] Business data queries

## Security

* [ ] JWT authentication
* [ ] Refresh tokens
* [ ] RBAC
* [ ] Permission system
* [ ] Multi-tenancy
* [ ] Tenant isolation
* [ ] Rate limiting
* [ ] Prompt injection defenses
* [ ] Tool authorization
* [ ] Audit logs

## Infrastructure

* [ ] PostgreSQL
* [ ] pgvector
* [ ] Redis
* [ ] BullMQ
* [ ] Background workers
* [ ] Docker
* [ ] Docker Compose
* [ ] GitHub Actions
* [ ] AWS deployment

---

# 🏗️ Architecture

The planned architecture looks like:

```text
                         ┌─────────────────────┐
                         │      Next.js UI     │
                         │ TypeScript + React  │
                         └──────────┬──────────┘
                                    │
                              REST / SSE
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Node.js API      │
                         │ Express + TypeScript│
                         └──────────┬──────────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
       ┌─────────────┐       ┌─────────────┐       ┌──────────────┐
       │ PostgreSQL  │       │    Redis    │       │ Object Store │
       │  + pgvector │       │  + BullMQ   │       │    / S3      │
       └─────────────┘       └──────┬──────┘       └──────────────┘
                                    │
                                    ▼
                             ┌─────────────┐
                             │   Workers   │
                             └──────┬──────┘
                                    │
                 ┌──────────────────┼──────────────────┐
                 │                  │                  │
                 ▼                  ▼                  ▼
          Document Worker      Embedding Worker    Agent Worker
                                    │
                                    ▼
                            ┌────────────────┐
                            │ AI Application │
                            │     Layer     │
                            └───────┬────────┘
                                    │
                       ┌────────────┼────────────┐
                       ▼            ▼            ▼
                  LangChain     LangGraph    OpenRouter
                       │            │            │
                       ▼            ▼            ▼
                  Retrieval       Agents       LLMs
                       │
                       ▼
                    pgvector
```

---

# 🧠 RAG Architecture

The initial RAG pipeline:

```text
User Question
      ↓
Create Query Embedding
      ↓
Vector Search
      ↓
Retrieve Relevant Chunks
      ↓
Build Context
      ↓
OpenRouter LLM
      ↓
Answer + Citations
```

The planned advanced pipeline:

```text
                    User Question
                          │
                          ▼
                    Query Analysis
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
       Semantic Search           Keyword Search
             │                         │
             └────────────┬────────────┘
                          ▼
                    Merge Results
                          │
                          ▼
                       Reranker
                          │
                          ▼
                  Relevant Chunks
                          │
                          ▼
                         LLM
                          │
                          ▼
                  Answer + Sources
```

---

# 🤖 Agent Architecture

AURA should eventually distinguish between different types of requests.

```text
                         User Request
                              │
                              ▼
                       Intent Analysis
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
        Knowledge         Business Data      Action
           Query              Query           Request
              │               │                │
              ▼               ▼                ▼
            RAG            DB Tool        Authorized Tool
              │               │                │
              └───────────────┼────────────────┘
                              ▼
                       Generate Response
```

For complex workflows, LangGraph will manage the agent state and execution flow.

---

# 🔧 Planned Tools

Initial tools:

```text
searchKnowledge()
findCustomer()
getCustomer()
getOrder()
getCustomerOrders()
createSupportTicket()
updateSupportTicket()
createTask()
sendEmail()
```

Potential future tools:

```text
searchWeb()
createCalendarEvent()
sendSlackMessage()
```

Tools will include:

* Input validation
* Authorization
* Error handling
* Audit logging
* Idempotency where required

---

# 🛡️ Human-in-the-Loop

Sensitive actions should not automatically execute.

Example:

```text
User
 ↓
Agent
 ↓
Propose Refund
 ↓
Check Permission
 ↓
Approval Required
 ↓
Human Approves
 ↓
Execute Tool
 ↓
Audit Log
```

This allows AURA to demonstrate controlled AI automation rather than unrestricted agent execution.

---

# 🏢 Multi-Tenancy

AURA is designed to support multiple organizations.

```text
AURA
 │
 ├── Company A
 │    ├── Users
 │    ├── Documents
 │    ├── Customers
 │    └── Knowledge
 │
 ├── Company B
 │    ├── Users
 │    ├── Documents
 │    ├── Customers
 │    └── Knowledge
 │
 └── Company C
      ├── Users
      ├── Documents
      ├── Customers
      └── Knowledge
```

Tenant isolation is a core security requirement.

A request belonging to Company A must never retrieve Company B's documents or business data.

---

# 🧰 Tech Stack

## Frontend

* Next.js
* React
* TypeScript
* Tailwind CSS
* TanStack Query
* Server-Sent Events

## Backend

* Node.js
* TypeScript
* Express.js
* REST API
* Zod

## AI

* OpenRouter API
* LangChain.js
* LangGraph.js
* Configurable LLMs
* Embeddings
* Tool calling
* Structured outputs

## Database

* PostgreSQL
* pgvector

## Infrastructure

* Redis
* BullMQ
* Docker
* Docker Compose
* GitHub Actions

---

# 📁 Project Structure

```text
.
├── application
│   ├── client      # AURA-CLIENT: Next.js frontend
│   └── server      # AURA-SERVER: Express API
├── specs           # Product specs and phase plans
├── docker-compose.dev.yml
├── docker-compose.prod.yml
└── .env.example
```

Planning docs:

* [Product overview](specs/AURA_AI_Business_Knowledge_Operations_Agent_Plan.md)
* [Frontend phase plan](specs/frontend-phase-plan.md)
* [Backend phase plan](specs/backend-phase-plan.md)
* [Parallel delivery roadmap](specs/parallel-delivery-roadmap.md)

---

# 🚀 Running The Project

## Prerequisites

Install:

* Node.js
* npm
* Docker Desktop
* Docker Compose

On WSL2, enable Docker Desktop WSL integration for this distro before running Docker commands.

## Environment

Create a local environment file from the example:

```zsh
cp .env.example .env
```

Current local defaults:

```env
NODE_ENV=development
PORT=4000
AURA_API_URL=http://localhost:4000
NEXT_PUBLIC_API_URL=http://localhost:4000
```

## Run With Docker For Development

From the workspace root:

```zsh
docker compose -f docker-compose.dev.yml up --build
```

This starts:

* Frontend: `http://localhost:3000`
* Backend API: `http://localhost:4000`

The development Compose file mounts local source files into the containers so frontend and backend changes reload during development.

Stop the containers:

```zsh
docker compose -f docker-compose.dev.yml down
```

## Run With Docker For Production-Style Build

From the workspace root:

```zsh
docker compose -f docker-compose.prod.yml up --build
```

This builds production images for:

* Next.js standalone frontend container
* Express API container

Stop the production-style containers:

```zsh
docker compose -f docker-compose.prod.yml down
```

## Run Without Docker

Install and start the backend:

```zsh
cd application/server
npm install
npm run dev
```

In another terminal, install and start the frontend:

```zsh
cd application/client
npm install
npm run dev
```

Local URLs:

* Frontend: `http://localhost:3000`
* Backend API: `http://localhost:4000`

## Build Checks

Backend:

```zsh
cd application/server
npm run build
```

Frontend:

```zsh
cd application/client
npm run build
npm run lint
```

---

## Storage

Development:

* Local storage / MinIO

Production:

* Amazon S3

## Deployment

Planned:

* AWS
* RDS PostgreSQL
* ElastiCache Redis
* S3
* CloudFront
* Route 53
* Load Balancer

---

# 🤖 AI Model Strategy

AURA will use **OpenRouter** as the initial AI model gateway.

This allows the project to switch between different models without tightly coupling the application to one provider.

```text
AURA
  ↓
AI Service
  ↓
OpenRouter
  ↓
Selected Model
```

The model should be configured through environment variables rather than hard-coded.

Example:

```env
OPENROUTER_API_KEY=
OPENROUTER_MODEL=
```

During development, free models can be used where appropriate.

Because free model availability, rate limits, and capabilities can change, the project will treat the model as configurable infrastructure rather than building around a single model.

---

# 📚 Learning Objectives

This project is also a structured learning project.

By completing it, the goal is to become comfortable with:

### Backend

* Node.js
* TypeScript
* Express
* REST APIs
* Middleware
* Authentication
* Authorization
* RBAC
* PostgreSQL
* Transactions
* Indexes
* Redis
* BullMQ
* Workers
* Retries
* Idempotency
* WebSockets/SSE
* Caching

### AI

* LLM APIs
* OpenRouter
* Prompt engineering
* Structured output
* Tool calling
* Embeddings
* Vector databases
* RAG
* Hybrid retrieval
* Reranking
* LangChain
* LangGraph
* Agent state
* Human-in-the-loop
* AI evaluation

### Production Engineering

* Docker
* Testing
* CI/CD
* Logging
* Health checks
* Monitoring
* AWS
* Security
* Multi-tenancy

---

# 🗺️ Development Roadmap

## Milestone 0 — Project Planning

* [ ] Repository setup
* [ ] Architecture documentation
* [ ] Docker Compose
* [ ] Environment configuration
* [ ] Initial project structure

---

## Milestone 1 — Backend Foundation

* [ ] Node.js
* [ ] TypeScript
* [ ] Express
* [ ] PostgreSQL
* [ ] Migrations
* [ ] Error handling
* [ ] Zod validation
* [ ] Health endpoint
* [ ] Module architecture

---

## Milestone 2 — Authentication & Multi-Tenancy

* [ ] Registration
* [ ] Login
* [ ] JWT access token
* [ ] Refresh token
* [ ] Refresh token rotation
* [ ] Tenant/company creation
* [ ] RBAC
* [ ] Permissions
* [ ] Tenant isolation

---

## Milestone 3 — Document Management

* [ ] File uploads
* [ ] PDF support
* [ ] DOCX support
* [ ] TXT/Markdown support
* [ ] Document metadata
* [ ] Document versions
* [ ] Document lifecycle states

---

## Milestone 4 — Async Document Processing

* [ ] Redis
* [ ] BullMQ
* [ ] Document worker
* [ ] Text extraction
* [ ] Job retries
* [ ] Backoff
* [ ] Concurrency
* [ ] Failure handling
* [ ] Idempotent processing

---

## Milestone 5 — Embeddings & Vector Search

* [ ] Text chunking
* [ ] Embedding generation
* [ ] pgvector
* [ ] Vector storage
* [ ] Similarity search
* [ ] Metadata filtering
* [ ] Knowledge search endpoint

---

## Milestone 6 — Basic RAG

* [ ] Retrieval
* [ ] Prompt construction
* [ ] OpenRouter integration
* [ ] Grounded answers
* [ ] Source citations
* [ ] Streaming response

---

## Milestone 7 — Advanced RAG

* [ ] Metadata filtering
* [ ] Hybrid search
* [ ] Query rewriting
* [ ] Reranking
* [ ] Context optimization
* [ ] Retrieval evaluation

---

## Milestone 8 — Business Data & Tools

* [ ] Customers
* [ ] Orders
* [ ] Support tickets
* [ ] Tasks
* [ ] Customer lookup tool
* [ ] Order lookup tool
* [ ] Ticket creation tool
* [ ] Task creation tool
* [ ] Tool authorization

---

## Milestone 9 — LangChain

* [ ] LangChain document loaders
* [ ] Text splitters
* [ ] Retrievers
* [ ] Prompt templates
* [ ] Structured output
* [ ] Tool interfaces
* [ ] Retrieval pipelines

---

## Milestone 10 — LangGraph Agent

* [ ] Agent state
* [ ] Nodes
* [ ] Edges
* [ ] Conditional routing
* [ ] Tool loops
* [ ] Checkpoints
* [ ] Persistent state
* [ ] Multi-step workflows

---

## Milestone 11 — Human Approval

* [ ] Approval requests
* [ ] Agent interrupts
* [ ] Approve/reject UI
* [ ] Sensitive-action policies
* [ ] Action audit trail

---

## Milestone 12 — Agent Observability

* [ ] Agent run tracking
* [ ] Agent steps
* [ ] Tool call tracking
* [ ] Latency tracking
* [ ] Token usage
* [ ] Error tracking
* [ ] Agent activity dashboard

---

## Milestone 13 — Security & Guardrails

* [ ] Rate limiting
* [ ] Tenant isolation tests
* [ ] Tool permission checks
* [ ] Prompt injection defenses
* [ ] Input validation
* [ ] Output validation
* [ ] Sensitive-data controls
* [ ] Audit logging

---

## Milestone 14 — AI Evaluation

* [ ] RAG evaluation dataset
* [ ] Retrieval evaluation
* [ ] Citation evaluation
* [ ] Answer correctness
* [ ] Tool selection evaluation
* [ ] Tool argument evaluation
* [ ] Regression tests
* [ ] Latency/token tracking

---

## Milestone 15 — Testing

* [ ] Unit tests
* [ ] Integration tests
* [ ] API tests
* [ ] Agent tests
* [ ] RAG tests
* [ ] E2E tests
* [ ] Security tests

---

## Milestone 16 — Production Readiness

* [ ] Docker images
* [ ] Docker Compose
* [ ] GitHub Actions
* [ ] Health checks
* [ ] Structured logging
* [ ] Graceful shutdown
* [ ] Environment management
* [ ] Production configuration

---

## Milestone 17 — AWS Deployment

Planned deployment:

```text
                    Route 53
                       │
                       ▼
                   CloudFront
                       │
                       ▼
                 Load Balancer
                       │
              ┌────────┴────────┐
              ▼                 ▼
          API Server          Workers
              │                 │
              ├───────┬─────────┘
              ▼       ▼
             RDS   ElastiCache
              │
              ▼
          PostgreSQL
              +
           pgvector

              S3
          Documents
```

---

# 🧪 Testing Strategy

## Unit Tests

Test:

* Services
* Validators
* Permissions
* Business rules
* Tool schemas

## Integration Tests

Test:

```text
API
 ↓
PostgreSQL
 ↓
Redis
 ↓
BullMQ
```

## E2E Tests

Example:

```text
Register
 ↓
Create Company
 ↓
Upload Document
 ↓
Process Document
 ↓
Ask Question
 ↓
Retrieve Answer
 ↓
Create Ticket
 ↓
Audit Action
```

## AI Tests

Test:

```text
Question
 ↓
Expected Retrieval
 ↓
Expected Tool
 ↓
Expected Response
```

---

# 🔐 Security Principles

AURA will follow these principles:

### 1. Retrieved content is untrusted data

Documents should never automatically become system instructions.

### 2. Backend authorization is mandatory

Frontend permissions are not sufficient.

### 3. Tools are privileged operations

Every tool must have authorization rules.

### 4. Tenant isolation is mandatory

Every tenant-scoped operation must enforce the current tenant.

### 5. Sensitive actions require approval

The AI should not automatically perform high-impact operations.

### 6. AI output should be validated

Do not blindly trust LLM-generated structured data or tool arguments.

---

# 📊 Agent Observability

AURA should eventually provide an agent activity view such as:

```text
Agent Run #9281

Intent:
Customer Support

Steps:

✓ Understand request
✓ Find customer
✓ Get order
✓ Search knowledge
✓ Validate action
✓ Create ticket
✓ Generate response

Status:
COMPLETED

Latency:
2.8s

Tools:
5

Model:
Configured OpenRouter model
```

---

# 📁 Planned Repository Structure

```text
aura-ai-business-agent/
│
├── apps/
│   ├── web/
│   ├── api/
│   └── workers/
│
├── packages/
│   ├── shared/
│   ├── database/
│   ├── ai/
│   ├── config/
│   └── types/
│
├── docs/
│   ├── architecture/
│   ├── api/
│   ├── ai/
│   └── decisions/
│
├── tests/
│   ├── integration/
│   ├── e2e/
│   └── evaluation/
│
├── infrastructure/
│   ├── docker/
│   └── aws/
│
├── docker-compose.yml
├── package.json
├── .env.example
└── README.md
```

This structure may evolve as the project grows.

---

# 🧠 Engineering Decisions

Important architecture decisions will be documented in:

```text
docs/architecture/decisions/
```

Planned ADRs:

```text
001-why-postgresql.md
002-why-pgvector.md
003-why-bullmq.md
004-why-langgraph.md
005-why-openrouter.md
006-rag-vs-fine-tuning.md
007-multi-tenancy.md
008-tool-authorization.md
009-human-approval.md
010-model-provider-abstraction.md
```

Each decision should document:

```text
Context
Decision
Alternatives
Reason
Trade-offs
```

---

# 🎯 MVP

The first working version will intentionally be much smaller.

### MVP scope

```text
Authentication
      +
Tenant
      +
Document Upload
      +
PDF Text Extraction
      +
Chunking
      +
Embeddings
      +
pgvector
      +
Basic RAG
      +
OpenRouter
      +
Source Citations
```

Architecture:

```text
Next.js
   ↓
Node.js / Express
   ↓
PostgreSQL + pgvector
   ↓
OpenRouter
```

After the MVP works, add:

```text
Redis
 ↓
BullMQ
 ↓
LangChain
 ↓
Tools
 ↓
LangGraph
 ↓
Human Approval
 ↓
Evaluation
 ↓
Security Hardening
 ↓
AWS
```

---

# 📌 Definition of Done

AURA will be considered portfolio-ready when:

* [ ] Authentication works
* [ ] Multi-tenancy works
* [ ] RBAC works
* [ ] Tenant isolation is enforced
* [ ] Documents can be uploaded
* [ ] Documents process asynchronously
* [ ] Text is chunked
* [ ] Embeddings are generated
* [ ] Vectors are stored
* [ ] RAG works
* [ ] Answers include citations
* [ ] Metadata filtering works
* [ ] Hybrid retrieval works
* [ ] Reranking works
* [ ] Business data can be queried
* [ ] Agent can call tools
* [ ] Tools are authorized
* [ ] Sensitive actions require approval
* [ ] LangGraph manages agent state
* [ ] Agent runs are observable
* [ ] Audit logs exist
* [ ] AI evaluation dataset exists
* [ ] Unit tests exist
* [ ] Integration tests exist
* [ ] E2E tests exist
* [ ] Security tests exist
* [ ] Prompt injection tests exist
* [ ] Docker setup works
* [ ] CI pipeline works
* [ ] Production deployment works
* [ ] Architecture is documented
* [ ] README is complete
* [ ] Demo/screenshots are available

---

# 📚 Learning Order

Do not try to learn everything before starting.

Learn each technology when the corresponding milestone requires it.

```text
Node.js + TypeScript
        ↓
Express
        ↓
PostgreSQL
        ↓
Authentication + RBAC
        ↓
Redis
        ↓
BullMQ
        ↓
Document Processing
        ↓
Embeddings
        ↓
pgvector
        ↓
Basic RAG
        ↓
OpenRouter
        ↓
Tool Calling
        ↓
LangChain
        ↓
LangGraph
        ↓
Human-in-the-Loop
        ↓
AI Evaluation
        ↓
Security
        ↓
Docker
        ↓
CI/CD
        ↓
AWS
```

---

# 🚧 Current Status

**Project initialized.**

Current focus:

> Milestone 0 — Project Planning

Next implementation target:

> **Milestone 1 — Node.js + TypeScript + Express + PostgreSQL backend foundation**

---

# 📄 License

License to be selected before the first public release.

---

## Author

**Abdul Wahab**

Full Stack Developer | Node.js | React | AI Engineering

---

## ⭐ Project Philosophy

The goal of AURA is not to demonstrate how many AI libraries can be added to a project.

The goal is to understand how to build a reliable AI-powered product where:

```text
Traditional Software Engineering
                +
AI Engineering
                +
Data Engineering
                +
Distributed Systems
                +
Security
                =
Production AI Application
```

The project will prioritize **understanding, engineering decisions, reliability, and explainability** over simply adding more technologies.
