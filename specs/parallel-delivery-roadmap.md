# AURA Parallel Delivery Roadmap

Projects: AURA-CLIENT and AURA-SERVER

## Purpose

This roadmap aligns frontend and backend work so the app can be built in parallel. The frontend should not wait for every backend feature. It should start with mock data shaped like the planned API, then replace mocks phase by phase as backend endpoints become available.

## Delivery Strategy

- Frontend owns product flow, navigation, UX states, and typed API clients.
- Backend owns contracts, persistence, AI integration, ingestion, agent execution, approvals, and audit.
- Each phase should end with a working vertical slice when possible.
- Mock data is allowed only before the matching backend endpoint exists.
- Once an endpoint exists, the frontend should switch from mock data to API data for that domain.

## Phase Map

| Phase | Frontend Track | Backend Track | Integration Result |
| ----- | -------------- | ------------- | ------------------ |
| 1 | App shell, navigation, mock dashboard, mock chat, mock knowledge base, mock settings, mock agent runs | API foundation, health, route structure, error shape | UI and API can run independently |
| 2 | API key settings UI | AI settings and OpenRouter key management | User can save and test API key |
| 3 | Upload UI and document table | PostgreSQL, migrations, base persistence | Durable app state foundation |
| 4 | Real document upload integration | Document upload and metadata APIs | User can upload and manage documents |
| 5 | Processing status and document detail UX | Text extraction and chunking pipeline | Documents move through queued, processing, indexed, failed |
| 6 | Real chat integration | Conversations and non-RAG OpenRouter chat | User can ask AURA questions |
| 7 | Citation cards and source preview | RAG search, embeddings, pgvector, citations | Answers include grounded sources |
| 8 | Streaming chat UI | SSE streaming responses | User sees progressive answers |
| 9 | Business Data pages | Customers, orders, tickets, tasks APIs | User can browse operational records |
| 10 | Tool result cards and run traces | Agent tools and run recording | Agent work is inspectable |
| 11 | Approval inbox and decisions | Approval workflow and mutating tools | Sensitive actions require human approval |
| 12 | Audit logs UI | Audit log service and filters | User can review system activity |
| 13 | Login and role-aware UI | Auth, RBAC, tenant isolation | App is protected and tenant-ready |
| 14 | Polish, accessibility, empty/error states | Hardening, tests, evaluations, CI | Production-shaped portfolio app |

## Recommended Build Sequence

### Step 1: Build Frontend Phase 1

Reason:
- It defines the product shape.
- It gives backend a clear target.
- It can be built without waiting for persistence or AI services.

Deliverables:
- Complete dashboard shell.
- Mock pages for core workflows.
- Typed mock data matching planned API objects.

### Step 2: Build Backend Phase 1 And Phase 2

Reason:
- Health and AI settings are low-risk starting points.
- API key setup unlocks real AI work.

Deliverables:
- Health route.
- AI settings endpoints.
- OpenRouter key test endpoint.

### Step 3: Connect Frontend Phase 2 To Backend Phase 2

Reason:
- This creates the first real frontend/backend vertical slice.

Deliverables:
- API key form saves real data.
- Test connection works.
- Model setting updates.

### Step 4: Build Backend Persistence Before Uploads

Reason:
- Documents, conversations, agent runs, approvals, and audit all need durable state.

Deliverables:
- PostgreSQL in Compose.
- Migrations.
- Seed data.
- Data access modules.

### Step 5: Build Document Upload Slice

Reason:
- Knowledge ingestion is central to AURA.
- Upload status gives the frontend an important real workflow before RAG is complete.

Deliverables:
- Upload documents.
- List documents.
- Track statuses.
- Delete and reprocess.

### Step 6: Build Chat Without RAG

Reason:
- This validates OpenRouter, conversations, and message persistence before retrieval complexity.

Deliverables:
- Ask question.
- Store conversation.
- Return model answer.

### Step 7: Add RAG And Sources

Reason:
- This turns AURA from general chat into company knowledge assistant.

Deliverables:
- Chunk search.
- Embeddings.
- Citations.
- Source preview.

### Step 8: Add Business Tools And Agent Runs

Reason:
- This turns AURA from knowledge assistant into operations agent.

Deliverables:
- Business data APIs.
- Read-only tool calls.
- Agent run timeline.
- Tool result display.

### Step 9: Add Approvals And Audit

Reason:
- Sensitive actions need trust and accountability before mutating tools expand.

Deliverables:
- Approval inbox.
- Approve/reject flow.
- Audit log records.

### Step 10: Add Auth, RBAC, Tenant Isolation

Reason:
- This is easier once the core domain flows are visible, but it must happen before production-style use.

Deliverables:
- Login.
- Session handling.
- Role-aware UI.
- Tenant-scoped backend queries.

## API Contract Rules

Use stable response envelopes:

```json
{
  "data": {},
  "meta": {},
  "error": null
}
```

For errors:

```json
{
  "data": null,
  "meta": {},
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request is invalid.",
    "details": []
  }
}
```

Use these shared concepts across frontend and backend:
- `DocumentStatus`: `queued`, `processing`, `indexed`, `failed`
- `AgentRunStatus`: `running`, `completed`, `failed`, `needs_approval`
- `ApprovalStatus`: `pending`, `approved`, `rejected`, `cancelled`
- `ToolKind`: `read`, `write`
- `ActorKind`: `user`, `agent`, `system`

## Work Split Guidance

Frontend can start immediately:
- App shell.
- Mock dashboard.
- Mock chat.
- Mock upload.
- Mock settings.
- Mock agent run traces.

Backend can start immediately:
- Route structure.
- Health endpoint.
- Environment validation.
- AI settings endpoints.
- OpenRouter test call.

Do not start with:
- Full LangGraph workflows.
- Multi-tenant RBAC.
- Complex RAG evaluation.
- Advanced document versioning.

Those are important, but they should come after the first real upload and chat slices work.

## Definition Of Done Per Phase

Each phase is done when:
- The target user flow works in the browser or through the API.
- Loading, empty, and error states exist for frontend work.
- API inputs are validated for backend work.
- Build passes for the touched project.
- The next phase can start without rewriting the current phase.

