# AURA Frontend Phase Plan

Project: AURA-CLIENT

## Purpose

Build a complete internal AI operations console where a business user can configure AI access, upload knowledge, ask the agent questions, inspect agent work, approve sensitive actions, and review business data.

The frontend should be usable before the backend is complete. Early phases use typed mock data and stable API-shaped interfaces so AURA-SERVER can be built in parallel.

## Product Surface

### Dashboard

The dashboard gives the user a quick operational summary.

User options:
- View API health and AI provider status.
- View document ingestion totals.
- View pending document processing jobs.
- View recent agent runs.
- View pending approvals.
- Open quick actions: ask a question, upload documents, add API key.

### Ask AURA

The main agent workspace.

User options:
- Ask questions in a chat interface.
- View streaming or loading responses.
- View cited sources.
- View tool calls used by the agent.
- View business records found by the agent.
- Continue previous conversations.
- Retry failed responses.
- Start a new conversation.

### Knowledge Base

Document and knowledge management.

User options:
- Upload PDF, DOCX, TXT, and Markdown files.
- Drag and drop documents.
- View document list.
- Filter by status, type, and uploader.
- View processing status: queued, processing, indexed, failed.
- View document metadata.
- View extracted chunks later.
- Reprocess a document.
- Delete a document.

### API Keys And AI Settings

AI provider configuration.

User options:
- Add an OpenRouter API key.
- See whether a key is configured.
- See a masked key preview.
- Replace the key.
- Test provider connection.
- Select default chat model.
- Select default embedding model later.
- Configure basic generation settings later.

### Business Data

Business records the agent can inspect and act on.

User options:
- View customers.
- View orders.
- View support tickets.
- View tasks.
- Search and filter tables.
- Open detail views.
- See records linked from an agent answer.

### Agent Runs

Trace and debugging area for agent behavior.

User options:
- View agent run history.
- View run status: running, completed, failed, needs approval.
- View step timeline.
- View retrieved sources.
- View tool calls.
- View tool inputs and outputs.
- Open the related conversation.

### Approvals

Human-in-the-loop control for sensitive actions.

User options:
- View pending approvals.
- Inspect requested action details.
- See the agent's reason.
- See affected records.
- Approve an action.
- Reject an action.
- Add a decision note.
- View approval history.

### Tools

Business tool visibility and control.

User options:
- View available tools.
- See tool category and required permission.
- Enable or disable tools later.
- Test safe read-only tools later.

### Audit Logs

Security and accountability view.

User options:
- View user actions.
- View agent actions.
- View API key changes.
- View document changes.
- View approval decisions.
- Filter by actor, action, and date.

### Settings

Organization and user configuration.

User options:
- View organization profile.
- Manage users later.
- Manage roles later.
- View tenant settings later.

## Navigation

Primary navigation:
- Dashboard
- Ask AURA
- Knowledge Base
- Business Data
- Agent Runs
- Approvals
- Tools
- Audit Logs
- Settings

Business Data subnavigation:
- Customers
- Orders
- Tickets
- Tasks

Settings subnavigation:
- API Keys
- AI Models
- Organization
- Users later
- Roles later

## Phase 1: Application Shell And Mock Console

Goal: Build the complete app shell and make the product feel real with mock data.

Frontend scope:
- Replace the default Next.js starter page.
- Add dashboard layout with sidebar and top bar.
- Add responsive desktop and mobile navigation.
- Add page routes for all primary areas.
- Add typed mock data for dashboard cards, documents, conversations, agent runs, approvals, and business records.
- Add reusable UI primitives used by later phases: button, input, textarea, badge, table, tabs, dialog, drawer, empty state, loading state.
- Add Dashboard page.
- Add Ask AURA mock chat page.
- Add Knowledge Base mock document list and upload panel.
- Add API Keys settings mock page.
- Add Agent Runs mock trace page.

Acceptance criteria:
- User can navigate to every primary area.
- Dashboard shows realistic state from mock data.
- Ask AURA page supports entering a question and showing a mock answer.
- Knowledge Base page shows upload affordance and document statuses.
- API Keys page shows configured and unconfigured states.
- Agent Runs page shows a timeline with tool calls and citations.
- Client build and lint pass.

Backend dependency:
- None. Mock data only.

## Phase 2: API Keys And AI Settings

Goal: Let the user configure and test an OpenRouter API key.

Frontend scope:
- Add API key form with masked input.
- Show saved key status without revealing the full key.
- Add replace key flow.
- Add test connection button.
- Add default chat model selector.
- Add API error states and success states.

Expected backend endpoints:
- `GET /settings/ai`
- `PUT /settings/ai/api-key`
- `POST /settings/ai/api-key/test`
- `PUT /settings/ai/default-model`

Acceptance criteria:
- User can save an API key.
- User can test the key.
- User can change the default model.
- UI handles loading, success, validation error, and server error states.

## Phase 3: Document Upload And Knowledge Base

Goal: Let the user upload documents and track ingestion status.

Frontend scope:
- Add drag and drop upload.
- Add file validation before upload.
- Add upload progress state.
- Add document table backed by API data.
- Add filters by status and type.
- Add document detail drawer.
- Add delete document action.
- Add reprocess document action.

Expected backend endpoints:
- `GET /documents`
- `POST /documents`
- `GET /documents/:id`
- `DELETE /documents/:id`
- `POST /documents/:id/reprocess`

Acceptance criteria:
- User can upload supported files.
- User can see uploaded documents.
- User can track processing status.
- User can delete or reprocess a document.
- Failed uploads show clear errors.

## Phase 4: Ask AURA With Real API

Goal: Connect the chat interface to AURA-SERVER.

Frontend scope:
- Replace mock chat response with API call.
- Add conversation creation.
- Add conversation history.
- Add message loading state.
- Add answer citations.
- Add tool result cards.
- Add retry failed message.

Expected backend endpoints:
- `GET /conversations`
- `POST /conversations`
- `GET /conversations/:id`
- `POST /conversations/:id/messages`

Acceptance criteria:
- User can create a conversation.
- User can ask a question.
- User receives an answer from the server.
- User can view citations when present.
- User can return to previous conversations.

## Phase 5: Streaming, Sources, And Retrieval Debugging

Goal: Make RAG answers feel responsive and explainable.

Frontend scope:
- Add streaming response support.
- Add source cards with document name, page, chunk preview, and score.
- Add source preview drawer.
- Add retrieval debug panel for development mode.
- Add copy answer action.

Expected backend endpoints:
- `GET /conversations/:id/messages/stream` or `POST /chat/stream`
- `GET /documents/:id/sources/:sourceId`

Acceptance criteria:
- User sees response text appear progressively.
- Citations are visible and clickable.
- Source preview opens without leaving the chat.
- Retrieval debug data is hidden from normal users when needed.

## Phase 6: Business Data And Agent Tool Results

Goal: Show business records and tool results used by the agent.

Frontend scope:
- Add Customers page.
- Add Orders page.
- Add Tickets page.
- Add Tasks page.
- Add detail drawers for records.
- Add linked business record cards in chat.
- Add tool call display in agent runs.

Expected backend endpoints:
- `GET /customers`
- `GET /customers/:id`
- `GET /orders`
- `GET /orders/:id`
- `GET /tickets`
- `GET /tickets/:id`
- `GET /tasks`
- `GET /tasks/:id`

Acceptance criteria:
- User can browse business records.
- User can open record details.
- Agent answers can link to business records.
- Agent run traces show tool calls clearly.

## Phase 7: Approvals And Audit Logs

Goal: Support human approval for sensitive operations.

Frontend scope:
- Add approval inbox.
- Add approval detail page or drawer.
- Add approve and reject flows.
- Add decision note field.
- Add audit log table.
- Add audit filters.

Expected backend endpoints:
- `GET /approvals`
- `GET /approvals/:id`
- `POST /approvals/:id/approve`
- `POST /approvals/:id/reject`
- `GET /audit-logs`

Acceptance criteria:
- User can view pending approvals.
- User can approve or reject an action.
- Approval decisions appear in audit logs.
- Agent run status updates after approval decision.

## Phase 8: Auth, Roles, And Tenant Readiness

Goal: Make the UI production-shaped for authenticated organizations.

Frontend scope:
- Add login page.
- Add session-aware layout.
- Add logout.
- Add role-aware navigation.
- Add user management page.
- Add role management page.
- Add organization settings page.

Expected backend endpoints:
- `POST /auth/login`
- `POST /auth/logout`
- `POST /auth/refresh`
- `GET /auth/me`
- `GET /users`
- `GET /roles`
- `GET /organization`

Acceptance criteria:
- Unauthenticated users cannot access app pages.
- Authenticated users see their organization context.
- Navigation respects permissions.
- Session refresh works without losing app state.

## Frontend Implementation Notes

- Keep frontend API clients typed and grouped by domain.
- During mock phases, keep mock data shaped like expected API responses.
- Do not block frontend layout work on backend persistence.
- Prefer dense, work-focused UI. This is an operations console, not a landing page.
- Keep cards for repeated items and tool panels; avoid marketing-style hero layouts.
- Every async page needs loading, empty, error, and success states.

