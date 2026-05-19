# DESIGN_BRIEF

## STRICT_UI_SCOPE_CONTRACT
- Design only UI that maps to one or more PRODUCT_SURFACES below.
- Do not invent modules, workflows, dashboards, marketing pages, admin areas, ecommerce flows, docs, PRD pages, or settings outside the Product Surfaces.
- Physical screen count, routing, tabs, modals, drawers, and component hierarchy are Stitch decisions, but every generated screen must be traceable to a SURF_* id.
- Every permitted action must have a plausible visible control or platform-appropriate interaction.
- Empty, loading, validation, and error states may be included only inside the declared Product Surfaces.
- All visible user-facing text must be in English.
- Keep metadata, screen titles, and technical identifiers in English.
- Target device type: DESKTOP.

## PRODUCT_SURFACES
1. SURF_WORKSPACE - Workspace
   Purpose: Give the user the main operational view for inspecting, searching, filtering, and acting on Tickets data.
   Data: Tickets, ActivityEvent, Preference
   Core content: summary metrics, primary list/board/table, filters, search, selected item preview, empty/loading/error states.
   Actions: ACT_SEARCH_RECORDS (search_input_persistent), ACT_CREATE_RECORD (primary_button), ACT_SELECT_RECORD (inline_edit), ACT_RETRY_LOAD (secondary_button)
   Entry/exit: direct_url -> If data is unavailable, stay on the same surface and show retry/clear actions.
   Guidance: Dense but calm product UI; avoid marketing hero composition and unrelated admin/reporting modules.

2. SURF_RECORD_EDITOR - Record Editor
   Purpose: Let the user create, edit, validate, save, cancel, and recover Tickets changes.
   Data: Tickets, ValidationError
   Core content: form fields, required/optional indicators, validation messages, save/cancel controls, unsaved-state feedback.
   Actions: ACT_SAVE_RECORD (form_submit), ACT_CANCEL_EDIT (secondary_button)
   Entry/exit: SURF_WORKSPACE -> Save returns to SURF_WORKSPACE with persisted changes; cancel preserves existing data and closes the editor.
   Guidance: Form layout must be clear and task-specific; do not invent payment, onboarding, or unrelated identity forms.

3. SURF_INSIGHTS - Insights
   Purpose: Show useful summaries, trends, and status signals derived from Tickets data without becoming a separate analytics product.
   Data: Tickets, ActivityEvent
   Core content: small metrics, recent activity, state distribution, actionable follow-up hints, empty/error state.
   Actions: ACT_FILTER_INSIGHTS (context_menu), ACT_EXPORT_SUMMARY (secondary_button)
   Entry/exit: SURF_WORKSPACE -> No external BI/admin area; returns to SURF_WORKSPACE.
   Guidance: Keep insight content project-relevant and compact; no generic charts unrelated to the requested domain.

## UI_OUT_OF_SCOPE
- No PRD/requirements/sitemap/documentation screens.
- No generic admin, pricing, checkout, blog, onboarding, account, or profile areas unless declared as a Product Surface.
- No local placeholder/wireframe design.

## FULL_PRD_APPENDIX
The full PRD below is passive background context. If it conflicts with PRODUCT_SURFACES, PRODUCT_SURFACES wins.

# Surfacegate Desk Product Contract

## 1. Context And Goals
- Overview: Surfacegate Desk turns the user's request into a directly usable product workflow. The first experience must be the actual tool/game/API/CLI behavior, not a marketing landing page or placeholder demo.
- Target Audience: Users who need the requested web product to work immediately with clear feedback, recovery paths, and deterministic verification hooks.
- UI Language: English. Pipeline metadata, action IDs, surface IDs, story titles, technical reports, and file identifiers remain English.
- Core Objectives:
- FR-001: Build a compact browser service desk app called SurfaceGate Desk. It should manage tickets, queues, agents, SLA status, insights, settings, empty and error states, and every visible action should update real app state.
- Business Goals: reduce ambiguity for downstream agents, preserve the requested domain, and keep unrelated modules out of scope.
- User Goals: inspect current state, take primary actions, understand validation/recovery feedback, and return to a stable state after failures.
- Primary Workflows: load product state, perform the main action, recover from validation/system errors, and verify final state through the platform-appropriate test surface.
- Non-Functional: first usable state under 2s for local/frontend apps, WCAG 2.1 AA for UI platforms, deterministic test handles, and responsive behavior for UI platforms.
- External Dependencies: none unless explicitly listed in the task or System Contracts.

## 2. Data And State Contract
### Entities
- Tickets:
  - Fields: id:string required, title:string required, status:enum required, createdAt:timestamp required, updatedAt:timestamp required, metadata:json optional.
  - Relations: ActivityEvent belongs to the primary entity when activity/history is visible.
- ActivityEvent:
  - Fields: id:string required, entityId:string required, label:string required, timestamp:timestamp required, severity:enum optional.
- Preference:
  - Fields: key:string required, value:json required, updatedAt:timestamp required.
### State Architecture
- Server State: none for local-only apps unless later setup introduces a backend.
- Client/Local State: active surface, selected entity, drafts, filters, loading flags, lastError, and transient feedback.
- URL / Router State: active surface or selected item may be reflected in route/query when the platform supports it.
- Persisted State: local preferences and local-only records when DB_REQUIRED=none; server persistence when DB_REQUIRED is postgres/sqlite.
- Transient UI State: modal/drawer open state, hover/focus, optimistic flags, and retry timers.
### Data Flow
- Read Path: load seed/current data into a single source of truth, derive visible lists/metrics from it, and expose deterministic test state.
- Write Path: validate input, update local/server state once, then refresh derived views without duplicate state copies.
- Error Path: preserve last good data, set lastError/storageStatus, show retry/clear actions, and keep drafts when safe.
- Side Effects: persistence writes, analytics/logging only when explicitly in scope, and no hidden network calls for local-only products.

## 3. Behavioral And Action Contract
### ACTION: ACT_SEARCH_RECORDS
- Surface Bound: SURF_WORKSPACE
- Trigger: User types in search or changes filters.
- Preconditions & Auth: Tickets data is loaded or recoverable.
- Async Behavior: Debounced local filter or server query; visible loading when remote.
- Success Effect: Results and empty state update deterministically.
- Failure Effect: Keep previous results and show retryable error.
- Navigation After Success: target same, method replace.
- Navigation After Failure: target same, preserve_form_data true.
- State Changes: query, filters, visible result set.
- Persistence Effects: Optional persisted preference for last filters.
- User Feedback: Result count and active filter badges update.
- Required Role: any.
- Unauthorized Effect: Redirect or show auth call-to-action only if auth is in scope.

### ACTION: ACT_CREATE_RECORD
- Surface Bound: SURF_WORKSPACE
- Trigger: User clicks the primary create action.
- Preconditions & Auth: User can create Tickets records.
- Async Behavior: Open editor immediately; no network wait.
- Success Effect: SURF_RECORD_EDITOR opens with blank/default fields.
- Failure Effect: Show unavailable-state message if creation is blocked.
- Navigation After Success: target SURF_RECORD_EDITOR, method modal.
- Navigation After Failure: target same, preserve_form_data true.
- State Changes: active editor draft created.
- Persistence Effects: None until save.
- User Feedback: Editor appears with focus on first required field.
- Required Role: any.
- Unauthorized Effect: Show auth or permission message when auth is in scope.

### ACTION: ACT_SAVE_RECORD
- Surface Bound: SURF_RECORD_EDITOR
- Trigger: User submits the editor form.
- Preconditions & Auth: Required fields pass validation and user can save Tickets.
- Async Behavior: Disable submit, show loading, timeout after 10 seconds with retry.
- Success Effect: Persist changes and return to SURF_WORKSPACE with the updated row/card visible.
- Failure Effect: Inline field errors for validation; banner/toast for system failure; draft is preserved.
- Navigation After Success: target SURF_WORKSPACE, method replace.
- Navigation After Failure: target same, preserve_form_data true.
- State Changes: Tickets collection and active selection update.
- Persistence Effects: Write to localStorage or selected database.
- User Feedback: Success confirmation and updated timestamp visible.
- Required Role: any.
- Unauthorized Effect: Show permission message without dropping the draft.

### ACTION: ACT_RETRY_LOAD
- Surface Bound: SURF_WORKSPACE
- Trigger: User clicks Retry from loading/error/empty recovery UI.
- Preconditions & Auth: Data source can be retried.
- Async Behavior: Show retry loading state and prevent duplicate submissions.
- Success Effect: Replace error with current Tickets data or a helpful empty state.
- Failure Effect: Keep retry visible and explain next step.
- Navigation After Success: target same, method replace.
- Navigation After Failure: target same, preserve_form_data true.
- State Changes: loading, lastError, storageStatus.
- Persistence Effects: None except recovery metadata.
- User Feedback: Inline status changes visibly.
- Required Role: any.
- Unauthorized Effect: Show auth or permission message when auth is in scope.

### ACTION: ACT_FILTER_INSIGHTS
- Surface Bound: SURF_INSIGHTS
- Trigger: User changes insight filter/menu.
- Preconditions & Auth: Insight source data exists or empty state is valid.
- Async Behavior: Local update or short loading state for remote aggregation.
- Success Effect: Metrics and recent activity reflect the chosen filter.
- Failure Effect: Keep previous metrics and show retry message.
- Navigation After Success: target same, method replace.
- Navigation After Failure: target same, preserve_form_data true.
- State Changes: insight filter and derived metrics.
- Persistence Effects: Optional last-used insight filter.
- User Feedback: Metric labels and timestamps update.
- Required Role: any.
- Unauthorized Effect: Hide or disable restricted metrics if auth is in scope.

## 4. Product Surfaces
### SURFACE: SURF_WORKSPACE
- Name: Workspace
- Purpose: Give the user the main operational view for inspecting, searching, filtering, and acting on Tickets data.
- Data Entities Bound: Tickets, ActivityEvent, Preference
- Core Content: summary metrics, primary list/board/table, filters, search, selected item preview, empty/loading/error states.
- Permitted Actions: ACT_SEARCH_RECORDS (control_hint: search_input_persistent), ACT_CREATE_RECORD (control_hint: primary_button), ACT_SELECT_RECORD (control_hint: inline_edit), ACT_RETRY_LOAD (control_hint: secondary_button)
- Entry Points: direct_url
- Exit & Guard Rules: If data is unavailable, stay on the same surface and show retry/clear actions.
- Auth Required: false
- Design Guidance: Dense but calm product UI; avoid marketing hero composition and unrelated admin/reporting modules.

### SURFACE: SURF_RECORD_EDITOR
- Name: Record Editor
- Purpose: Let the user create, edit, validate, save, cancel, and recover Tickets changes.
- Data Entities Bound: Tickets, ValidationError
- Core Content: form fields, required/optional indicators, validation messages, save/cancel controls, unsaved-state feedback.
- Permitted Actions: ACT_SAVE_RECORD (control_hint: form_submit), ACT_CANCEL_EDIT (control_hint: secondary_button)
- Entry Points: SURF_WORKSPACE
- Exit & Guard Rules: Save returns to SURF_WORKSPACE with persisted changes; cancel preserves existing data and closes the editor.
- Auth Required: false
- Design Guidance: Form layout must be clear and task-specific; do not invent payment, onboarding, or unrelated identity forms.

### SURFACE: SURF_INSIGHTS
- Name: Insights
- Purpose: Show useful summaries, trends, and status signals derived from Tickets data without becoming a separate analytics product.
- Data Entities Bound: Tickets, ActivityEvent
- Core Content: small metrics, recent activity, state distribution, actionable follow-up hints, empty/error state.
- Permitted Actions: ACT_FILTER_INSIGHTS (control_hint: context_menu), ACT_EXPORT_SUMMARY (control_hint: secondary_button)
- Entry Points: SURF_WORKSPACE
- Exit & Guard Rules: No external BI/admin area; returns to SURF_WORKSPACE.
- Auth Required: false
- Design Guidance: Keep insight content project-relevant and compact; no generic charts unrelated to the requested domain.

## 5. Validation And Error Strategy
- Validation Rules: required text fields cannot be empty; status/enum values must be known; dates/timestamps must be parseable; destructive actions require explicit user intent.
- Business Logic Errors: show contextual messages near the action and keep the previous valid state.
- System/Network Errors: show a retryable banner or inline state with lastError details suitable for QA, not a silent reset.
- Error Display Policy: forms use inline errors; global load/persist failures use compact banners or state panels; no blocking alert dialogs unless the platform requires them.

## 6. System Contracts
- Environment Needs: list key names only when an external provider is required; values are supplied by MC/runtime, never by PLAN.
- External Integrations: none by default. If task requests Stripe, Supabase, Firebase, email, maps, or another provider, define provider purpose and failure behavior here.
- Permission Model: anonymous/local by default; if auth is requested, define roles and unauthorized effects per action and surface.
- Security: never expose secrets in client code; persistence errors must be visible; user data should not be silently discarded.

## 7. Platform Contract
- Type: Web
- Rendering Strategy: CSR for Vite React unless the task requests SSR/SEO.
- Auth Storage: local state/localStorage only for non-sensitive local apps; httpOnly cookie if auth/server is introduced.
- Route Guards: client router or surface-level guard state.
- Test Surface: window.app is allowed for deterministic smoke/final-test inspection.

## 8. Testability Contract
- Critical Path TC_LOAD_READY: launch product, observe primary surface or command/API readiness, and verify no placeholder/marketing-only first state.
- Critical Path TC_PRIMARY_ACTION: execute the main action from the task and verify visible or response-state change.
- Critical Path TC_ERROR_RECOVERY: force invalid input or a recoverable load/persist failure and verify user feedback plus retry/clear path.
- Test Handle Policy: every interactive control uses data-testid; expose deterministic window.app state for smoke/final-test when TECH_STACK allows browser globals.
- API Mock Hints: only generate mocks for declared external dependencies or server endpoints.

## 9. Out Of Scope
- No repo paths, branch names, GitHub URLs, run slugs, package names, or hardcoded local/server directories in PLAN.
- No physical screen table, screen-count field, or agent-invented screen list in PLAN.
- No modules outside Product Surfaces, Action Contracts, or explicit task requirements.
- No local fallback design; DESIGN must use Stitch when DESIGN_REQUIRED=true and must block on Stitch failure.