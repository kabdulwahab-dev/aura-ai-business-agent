# AURA Backend Phase Plan

Project: AURA-SERVER

## Purpose

Build the backend API, persistence, AI integration, document ingestion, agent workflows, authorization, approvals, and audit trail for AURA.

The backend should support the frontend in phases. Early phases provide stable contracts and mock-compatible data. Later phases replace stubs with real persistence, RAG, tool calling, and multi-tenant security.

## Backend Domains

### Platform And Health

Responsibilities:
- Express API bootstrapping.
- Environment validation.
- Health checks.
- Error handling.
- Request logging.
- CORS.
- Docker runtime support.

### AI Settings

Responsibilities:
- Store OpenRouter API key.
- Mask key status for frontend.
- Test provider connectivity.
- Store default model settings.
- Prepare for future provider abstraction.

### Documents

Responsibilities:
- Accept file uploads.
- Store document metadata.
- Store raw files locally first, object storage later.
- Track processing status.
- Support reprocessing and deletion.

### Ingestion Pipeline

Responsibilities:
- Extract text.
- Split documents into chunks.
- Generate embeddings.
- Store chunks and vectors.
- Track processing errors.
- Run asynchronously through workers later.

### Chat And Conversations

Responsibilities:
- Store conversations.
- Store messages.
- Build prompt context.
- Call OpenRouter.
- Return answer payloads.
- Stream responses later.

### RAG

Responsibilities:
- Embed user questions.
- Search document chunks.
- Build grounded context.
- Return citations.
- Support metadata filters later.
- Support hybrid search and reranking later.

### Business Data

Responsibilities:
- Provide sample customer, order, ticket, and task records.
- Support structured lookups.
- Expose read APIs for frontend.
- Become tool-accessible for the agent.

### Agent Tools And Runs

Responsibilities:
- Define tool interface.
- Record agent runs.
- Record run steps.
- Record tool calls.
- Separate read-only tools from mutating tools.
- Prepare LangChain and LangGraph integration.

### Approvals

Responsibilities:
- Create approval requests for sensitive actions.
- Pause tool execution until approved.
- Record approve and reject decisions.
- Continue or cancel agent action after decision.

### Audit Logs

Responsibilities:
- Record user actions.
- Record agent actions.
- Record API key changes.
- Record document changes.
- Record approval decisions.

### Auth, RBAC, And Tenancy

Responsibilities:
- Authenticate users.
- Issue and refresh tokens.
- Enforce roles and permissions.
- Isolate tenant data.
- Protect sensitive operations.

## Phase 1: API Foundation And Contracts

Goal: Create a stable backend foundation that frontend can integrate against.

Backend scope:
- Add environment config validation.
- Add centralized error handling.
- Add request logging.
- Add `/health`.
- Add route structure by domain.
- Add response shape conventions.
- Add Zod validation pattern.
- Add in-memory or file-backed mock data for early frontend integration.

Endpoints:
- `GET /health`
- `GET /status`

Acceptance criteria:
- API starts locally and in Docker.
- Health endpoint returns service status.
- Invalid requests return consistent validation errors.
- Route structure is ready for domain endpoints.
- Server build passes.

Frontend parallel:
- Frontend Phase 1 can use mock data without waiting.

## Phase 2: AI Settings And OpenRouter Key Management

Goal: Let users configure AI access.

Backend scope:
- Add AI settings model.
- Store API key in a local placeholder store first.
- Return only masked key metadata.
- Test OpenRouter key with a lightweight provider request.
- Store default chat model.
- Add validation for model and key inputs.

Endpoints:
- `GET /settings/ai`
- `PUT /settings/ai/api-key`
- `POST /settings/ai/api-key/test`
- `PUT /settings/ai/default-model`

Acceptance criteria:
- API key can be saved.
- API key is never returned raw.
- Test endpoint reports success or provider error.
- Default model can be updated.

Frontend parallel:
- Frontend Phase 2 consumes these endpoints.

## Phase 3: Persistence Foundation

Goal: Add durable storage before document and chat data grows.

Backend scope:
- Add PostgreSQL to Docker Compose.
- Add migration tooling.
- Add database connection module.
- Create initial tables for organizations, users placeholder, settings, documents, conversations, messages, agent runs, approvals, and audit logs.
- Add repository pattern or clear domain data access modules.
- Add seed data for business records.

Tables:
- `organizations`
- `users`
- `ai_settings`
- `documents`
- `document_chunks`
- `conversations`
- `messages`
- `agent_runs`
- `agent_run_steps`
- `approvals`
- `audit_logs`
- `customers`
- `orders`
- `support_tickets`
- `tasks`

Acceptance criteria:
- Database starts through Docker Compose.
- Migrations run cleanly from empty DB.
- Seed data loads for local development.
- Existing Phase 2 endpoints use persistence.

Frontend parallel:
- Frontend continues using Phase 2 APIs and mock business pages.

## Phase 4: Document Upload And Metadata

Goal: Support document upload and lifecycle tracking.

Backend scope:
- Add multipart upload handling.
- Validate file type and size.
- Store uploaded file locally.
- Create document metadata record.
- Return document list and details.
- Add delete document.
- Add reprocess action that resets status.

Endpoints:
- `GET /documents`
- `POST /documents`
- `GET /documents/:id`
- `DELETE /documents/:id`
- `POST /documents/:id/reprocess`

Acceptance criteria:
- Supported documents upload successfully.
- Invalid files are rejected clearly.
- Documents have status values.
- Documents can be listed, viewed, deleted, and queued for reprocess.

Frontend parallel:
- Frontend Phase 3 consumes these endpoints.

## Phase 5: Document Processing Pipeline

Goal: Extract searchable text and prepare document chunks.

Backend scope:
- Add text extraction for TXT and Markdown first.
- Add PDF and DOCX extraction after basic path works.
- Add chunking service.
- Store chunks in database.
- Track processing status and errors.
- Add background worker path later with BullMQ.

Status model:
- `queued`
- `processing`
- `indexed`
- `failed`

Acceptance criteria:
- Uploaded TXT or Markdown documents become indexed.
- Extracted chunks are stored.
- Processing failures are visible.
- Reprocess repeats extraction and chunking.

Frontend parallel:
- Frontend Phase 3 can show real processing status.

## Phase 6: Basic Chat With OpenRouter

Goal: Answer user questions through the configured model.

Backend scope:
- Add conversation and message persistence.
- Add non-RAG chat endpoint.
- Use configured OpenRouter API key and default model.
- Store user and assistant messages.
- Return consistent answer payload.

Endpoints:
- `GET /conversations`
- `POST /conversations`
- `GET /conversations/:id`
- `POST /conversations/:id/messages`

Acceptance criteria:
- User can create a conversation.
- User can send a message.
- API returns an AI answer.
- Conversation history persists.
- Missing API key returns a clear setup error.

Frontend parallel:
- Frontend Phase 4 consumes these endpoints.

## Phase 7: RAG Search And Citations

Goal: Ground chat answers in uploaded documents.

Backend scope:
- Add pgvector extension.
- Add embedding generation.
- Store embedding vectors for chunks.
- Embed user questions.
- Retrieve relevant chunks.
- Build context prompt.
- Return answer with citations.
- Include source document, page when available, chunk preview, and score.

Acceptance criteria:
- Questions retrieve relevant chunks.
- Answers include citations.
- Citations reference document and chunk data.
- Empty retrieval produces a clear "not enough knowledge" answer.

Frontend parallel:
- Frontend Phase 5 shows sources and retrieval detail.

## Phase 8: Streaming Responses

Goal: Improve chat responsiveness.

Backend scope:
- Add SSE streaming endpoint.
- Stream model output tokens.
- Stream tool and retrieval events later.
- Persist final assistant message after stream completion.
- Handle client disconnects.

Endpoints:
- `POST /conversations/:id/messages/stream`

Acceptance criteria:
- Frontend receives progressive answer text.
- Final message is persisted.
- Errors during stream are reported clearly.
- Disconnect does not corrupt conversation state.

Frontend parallel:
- Frontend Phase 5 uses streaming.

## Phase 9: Business Data APIs

Goal: Expose structured business records.

Backend scope:
- Add customers endpoints.
- Add orders endpoints.
- Add support tickets endpoints.
- Add tasks endpoints.
- Add filtering and search.
- Add detail endpoints.

Endpoints:
- `GET /customers`
- `GET /customers/:id`
- `GET /orders`
- `GET /orders/:id`
- `GET /tickets`
- `GET /tickets/:id`
- `GET /tasks`
- `GET /tasks/:id`

Acceptance criteria:
- Frontend can browse seeded business records.
- Search and filters work for core fields.
- Detail endpoints return related records.

Frontend parallel:
- Frontend Phase 6 consumes these endpoints.

## Phase 10: Agent Runs And Tools

Goal: Let the agent inspect business data and record its reasoning steps.

Backend scope:
- Define tool registry.
- Add read-only tools: find customer, get order, search tickets, search knowledge.
- Record agent run.
- Record each step and tool call.
- Return tool results in chat response.
- Add initial LangChain integration.
- Add LangGraph workflow after simple tool path works.

Endpoints:
- `GET /agent-runs`
- `GET /agent-runs/:id`

Acceptance criteria:
- Agent can call read-only tools.
- Agent run history records steps.
- Tool inputs and outputs are inspectable.
- Chat response can include business data references.

Frontend parallel:
- Frontend Phase 6 shows tool results and run traces.

## Phase 11: Approvals And Mutating Tools

Goal: Require human approval before sensitive actions.

Backend scope:
- Add mutating tool pattern.
- Add create support ticket tool.
- Add approval request creation.
- Pause action until approval.
- Execute approved action.
- Cancel rejected action.
- Record approval decision.

Endpoints:
- `GET /approvals`
- `GET /approvals/:id`
- `POST /approvals/:id/approve`
- `POST /approvals/:id/reject`

Acceptance criteria:
- Sensitive action creates an approval instead of executing immediately.
- Approved action executes and updates agent run state.
- Rejected action is cancelled and recorded.
- All decisions create audit log records.

Frontend parallel:
- Frontend Phase 7 consumes approval endpoints.

## Phase 12: Audit Logs

Goal: Provide accountability across users, agents, and system actions.

Backend scope:
- Add audit service.
- Record API key changes.
- Record document uploads and deletes.
- Record chat action requests.
- Record tool calls.
- Record approval decisions.
- Add audit list endpoint with filters.

Endpoints:
- `GET /audit-logs`

Acceptance criteria:
- Important actions create audit entries.
- Audit list can filter by actor, action, entity, and date.
- Approval decisions are visible in audit logs.

Frontend parallel:
- Frontend Phase 7 shows audit logs.

## Phase 13: Auth, RBAC, And Tenant Isolation

Goal: Make the system production-shaped.

Backend scope:
- Add login.
- Add refresh tokens.
- Add authenticated request middleware.
- Add user and role tables.
- Add permission checks.
- Add tenant scoping to all domain queries.
- Add organization settings.

Endpoints:
- `POST /auth/login`
- `POST /auth/logout`
- `POST /auth/refresh`
- `GET /auth/me`
- `GET /users`
- `GET /roles`
- `GET /organization`

Acceptance criteria:
- Anonymous users cannot access protected APIs.
- Users can log in and refresh sessions.
- Data is scoped by tenant.
- Roles restrict sensitive actions.

Frontend parallel:
- Frontend Phase 8 consumes auth and role data.

## Phase 14: Hardening And Evaluation

Goal: Improve safety, quality, and production readiness.

Backend scope:
- Add rate limiting.
- Add prompt injection checks for documents.
- Add retrieval evaluation fixtures.
- Add agent behavior evaluations.
- Add test coverage for core domains.
- Add CI checks.
- Add production environment documentation.

Acceptance criteria:
- Core endpoints have tests.
- RAG behavior has repeatable evaluation cases.
- Sensitive tool paths are covered by tests.
- CI runs build, lint, and tests.

## Backend Implementation Notes

- Keep route handlers thin.
- Keep domain logic in services.
- Validate inputs with Zod at API boundaries.
- Return stable response shapes from the start.
- Treat AI provider calls as replaceable adapters.
- Treat tool calls as auditable business actions, not hidden implementation details.
- Add persistence before complex agent logic.
- Add streaming only after non-streaming chat works.

