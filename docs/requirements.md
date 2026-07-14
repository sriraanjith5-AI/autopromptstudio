# Requirements Document

## Introduction

This sprint establishes the **documentation foundation** for AutoPrompt Studio — a production-grade, web-based AI platform that enables users to create prompt optimization experiments, compare multiple optimization engines, evaluate prompt quality, and automatically recommend a production-ready prompt.

The documentation foundation is itself a deliverable. It constitutes the single source of truth that will guide human developers, AI coding assistants, and future product decision-making throughout the platform's lifecycle. No application code (Python, FastAPI, React, DSPy) is produced in this sprint; only professional, internally consistent documentation artefacts are produced.

The nine documents to be created are:

| Artefact | Path |
|---|---|
| Product Requirements Document | `docs/PRD.md` |
| Architecture Document | `docs/ARCHITECTURE.md` |
| Roadmap | `docs/ROADMAP.md` |
| Project Guide | `docs/PROJECT_GUIDE.md` |
| Architecture Decision Record 001 | `docs/adr/ADR-001-Architecture.md` |
| Sprint Specification 001 | `specs/001_Project_Setup.md` |
| Skills Index | `.skills/skills.md` |
| Coding Rules | `.skills/coding_rules.md` |
| Root README | `README.md` |

---

## Glossary

- **AutoPrompt_Studio**: The web-based AI Prompt Optimization Platform being documented.
- **Documentation_Set**: The complete collection of nine artefacts produced in this sprint.
- **PRD**: Product Requirements Document — captures business goals, user personas, functional and non-functional requirements.
- **ARCHITECTURE**: Architecture Document — captures system structure, component responsibilities, and technology decisions.
- **ADR**: Architecture Decision Record — a time-stamped record of a single significant architectural decision and its rationale.
- **ROADMAP**: Time-phased plan that organises features and milestones into prioritised sprints or phases.
- **PROJECT_GUIDE**: Operational reference covering development setup, contribution conventions, branching strategy, and CI/CD workflow.
- **Sprint_Spec**: A numbered specification file that defines the scope, acceptance criteria, and success metrics for one sprint.
- **Skills_Index**: A machine-readable index used by AI coding assistants to discover available skill files.
- **Coding_Rules**: A standing document that defines mandatory code-quality standards, style conventions, and prohibited patterns.
- **README**: Root-level entry point document visible to anyone who opens the repository.
- **AI_Coding_Assistant**: An LLM-based tool (e.g., Kiro, Copilot) that reads documentation artefacts to assist with code generation.
- **DSPy**: Open-source framework for algorithmically optimising LLM prompts and pipelines; the primary prompt-optimisation engine.
- **FastAPI**: Python web framework used for the AutoPrompt Studio backend API.
- **React**: JavaScript library used for the AutoPrompt Studio frontend.
- **Spec-Driven_Development**: A methodology in which documentation artefacts (specs, ADRs, PRDs) are produced and ratified before implementation begins.
- **Experiment**: A named, versioned execution unit in AutoPrompt Studio that pairs a dataset, one or more optimisation engines, a base prompt, and an evaluation metric to produce a ranked set of optimised prompts.
- **Experiment_Lifecycle**: The ordered set of states an Experiment may occupy from creation to archival, together with the valid transitions between those states.
- **Production_Prompt**: The single, system-selected optimised prompt that AutoPrompt Studio designates as the recommended candidate for deployment after a successful Experiment.
- **Optimizer_Plugin**: A self-contained software component that implements the AutoPrompt Studio optimizer interface, enabling a new optimisation engine to be registered and executed without modifying platform core code.
- **Prompt_Library**: The persistent, searchable collection of Production Prompts that have been promoted from completed Experiments and are available for reuse.
- **Reproducibility_Metadata**: The complete set of configuration and runtime parameters captured alongside an Experiment result, sufficient to re-execute the Experiment and obtain an equivalent outcome.
- **Audit_Log**: An append-only, timestamped record of system-level and user-initiated events, retained for observability, compliance, and debugging purposes.
- **Dataset**: A structured collection of input–output examples uploaded by a user and used as the training or evaluation corpus for an Experiment.
- **AI_Development_Workflow**: The ordered set of artefacts and tooling conventions that govern how AutoPrompt Studio itself is designed, coded, and reviewed using AI-first engineering practices.

---

## Requirements

---

### Requirement 1: Product Requirements Document (`docs/PRD.md`)

**User Story:** As a product manager or developer joining the project, I want a single document that captures why AutoPrompt Studio exists, who it serves, and what it must do, so that all future implementation decisions can be traced back to validated business and user needs.

#### Acceptance Criteria

1. THE `PRD.md` SHALL contain a product vision statement that describes AutoPrompt Studio's purpose in three sentences or fewer.
2. THE `PRD.md` SHALL define at least three distinct user personas, each with a name, role, primary goal, and key pain point.
3. THE `PRD.md` SHALL enumerate functional requirements grouped by feature area (e.g., Experiment Management, Optimisation Engine, Evaluation, Recommendation), with at least two functional requirements listed per feature area.
4. THE `PRD.md` SHALL enumerate non-functional requirements covering performance, security, availability, scalability, and accessibility, with at least one entry per NFR category and each entry providing a measurable threshold or explicit constraint.
5. THE `PRD.md` SHALL list at least two explicit out-of-scope items for the initial release to prevent scope creep.
6. THE `PRD.md` SHALL include a success metrics section with at least three measurable KPIs, each specifying a metric name, measurement method, and a target or baseline value (e.g., time-to-first-optimised-prompt measured via session telemetry with a target of under 10 minutes).
7. IF a requirement in `PRD.md` references a technology choice, architecture pattern, or external dependency, THEN THE `PRD.md` SHALL cite the corresponding ADR by identifier (e.g., ADR-001).
8. THE `PRD.md` SHALL follow consistent heading hierarchy (H1 → H2 → H3) with no orphaned sections.

---

### Requirement 2: Architecture Document (`docs/ARCHITECTURE.md`)

**User Story:** As a software architect or senior developer, I want a living architecture document that describes the system's structure, component interactions, and technology choices, so that I can make implementation decisions that are consistent with the overall design.

#### Acceptance Criteria

1. THE `ARCHITECTURE.md` SHALL include a high-level system diagram described in prose and/or ASCII art that shows the frontend, backend API, optimisation engine layer, database, and external LLM providers.
2. THE `ARCHITECTURE.md` SHALL describe each major component (Frontend, API Gateway, Optimisation Service, Evaluation Service, Prompt Store, LLM Adapter) with its responsibility, inputs, and outputs.
3. THE `ARCHITECTURE.md` SHALL specify the primary technology for each component: React (frontend), FastAPI (backend), DSPy (optimisation engine), and the specific persistent store technology named by name (e.g., PostgreSQL, SQLite).
4. THE `ARCHITECTURE.md` SHALL describe the data flow for the core use case: a user submits a prompt optimisation experiment and receives a ranked recommendation.
5. THE `ARCHITECTURE.md` SHALL define API design conventions as RESTful endpoints versioned under `/api/v1/`, with additional conventions (e.g., pagination, error envelope format) documented explicitly.
6. THE `ARCHITECTURE.md` SHALL include a security architecture section covering: (a) authentication — the mechanism used (e.g., JWT, OAuth2); (b) authorisation — the access control model (e.g., RBAC); and (c) secrets management — where secrets are stored and how they are injected at runtime.
7. THE `ARCHITECTURE.md` SHALL reference ADR-001 by name and relative file path for the rationale behind the chosen architectural pattern.
8. IF a component introduces a third-party dependency, THEN THE `ARCHITECTURE.md` SHALL name that dependency and state: (a) the problem it solves within AutoPrompt Studio, and (b) at least one alternative that was considered and why it was not chosen.

---

### Requirement 3: Architecture Decision Record 001 (`docs/adr/ADR-001-Architecture.md`)

**User Story:** As a developer or architect, I want a structured record of the foundational architecture decision, so that future contributors understand why the system is built the way it is and can make coherent changes.

#### Acceptance Criteria

1. THE `ADR-001-Architecture.md` SHALL follow the standard ADR template with sections: Title, Status, Date, Context, Decision, Consequences. The Date field SHALL use ISO 8601 format (YYYY-MM-DD).
2. THE `ADR-001-Architecture.md` SHALL record the Status as `Accepted`.
3. THE `ADR-001-Architecture.md` SHALL describe the architectural style chosen (e.g., layered monolith, microservices, modular monolith) and at least two alternatives that were considered.
4. THE `ADR-001-Architecture.md` SHALL state at least one distinct reason per rejected alternative explaining why the chosen style was selected over it.
5. THE `ADR-001-Architecture.md` SHALL enumerate at least one positive consequence and at least one accepted trade-off of the decision.
6. WHEN the ADR references a technology by name, THE `ADR-001-Architecture.md` SHALL include a one-sentence description of that technology's role within AutoPrompt Studio.

---

### Requirement 4: Roadmap (`docs/ROADMAP.md`)

**User Story:** As a product manager or engineering lead, I want a time-phased roadmap that organises work into clearly scoped phases, so that the team can plan sprints, communicate progress to stakeholders, and avoid unplanned scope expansion.

#### Acceptance Criteria

1. THE `ROADMAP.md` SHALL organise work into at least three phases: Foundation, Core Features, and Production Readiness.
2. THE `ROADMAP.md` SHALL list for each phase at least one goal, at least one key deliverable, and a numeric estimated sprint count.
3. THE `ROADMAP.md` SHALL assign Sprint 001 (Documentation Foundation) to Phase 1 with a single-sentence description naming its primary deliverable.
4. THE `ROADMAP.md` SHALL mark every listed feature entry with exactly one of three statuses: `Planned`, `In Progress`, or `Completed`.
5. THE `ROADMAP.md` SHALL include a future considerations section for features explicitly deferred beyond the initial roadmap.
6. THE `ROADMAP.md` SHALL remain free of hard calendar dates — defined as any day number, month name, year number, or ISO-formatted date string — using sprint numbers or relative time references instead.

---

### Requirement 5: Project Guide (`docs/PROJECT_GUIDE.md`)

**User Story:** As a developer onboarding to the project, I want a single operational reference that tells me how to set up my environment, run the project, and contribute code, so that I am productive within one working session.

#### Acceptance Criteria

1. THE `PROJECT_GUIDE.md` SHALL include a prerequisites section listing required tools and their minimum versions (e.g., Python 3.11+, Node.js 20+, Docker).
2. THE `PROJECT_GUIDE.md` SHALL provide step-by-step local development setup instructions for both the backend (FastAPI) and frontend (React).
3. THE `PROJECT_GUIDE.md` SHALL define the branching strategy (e.g., trunk-based, GitFlow) and the naming convention for feature, fix, and release branches.
4. THE `PROJECT_GUIDE.md` SHALL describe the pull request process, including required reviewers, CI checks that must pass, and the merge strategy.
5. THE `PROJECT_GUIDE.md` SHALL describe the CI/CD pipeline stages (lint, unit test, integration test, build, deploy).
6. THE `PROJECT_GUIDE.md` SHALL include a section on environment variable management, stating which variables are required, where they must be defined (e.g., `.env.local`), and that secrets must never be committed to version control.
7. IF a developer encounters a common setup problem, THEN THE `PROJECT_GUIDE.md` SHALL provide a Troubleshooting section with at least three documented solutions.

---

### Requirement 6: Sprint Specification 001 (`specs/001_Project_Setup.md`)

**User Story:** As a developer or AI coding assistant executing Sprint 001, I want a precise specification that defines the sprint's scope, tasks, and acceptance criteria, so that the sprint can be executed and verified without ambiguity.

#### Acceptance Criteria

1. THE `001_Project_Setup.md` SHALL state the sprint number, title, and goal in the document header.
2. THE `001_Project_Setup.md` SHALL list every artefact to be produced in this sprint with the exact file path for each.
3. THE `001_Project_Setup.md` SHALL provide acceptance criteria for each artefact deliverable.
4. THE `001_Project_Setup.md` SHALL explicitly state that no application code is produced in Sprint 001.
5. THE `001_Project_Setup.md` SHALL define the definition of done as: all nine artefacts created, peer-reviewed, and committed to the repository.
6. THE `001_Project_Setup.md` SHALL list dependencies (none for Sprint 001) and risks with corresponding mitigations.

---

### Requirement 7: Skills Index (`.skills/skills.md`)

**User Story:** As an AI coding assistant operating on the AutoPrompt Studio codebase, I want a structured index of available skill files, so that I can discover and apply the correct conventions and rules when generating or reviewing code.

#### Acceptance Criteria

1. THE `skills.md` SHALL list each skill file in the `.skills/` directory with its file name and a one-sentence description of its purpose.
2. THE `skills.md` SHALL specify the intended audience for each skill (e.g., AI coding assistant, human developer, or both).
3. THE `skills.md` SHALL describe when each skill should be applied (e.g., "apply `coding_rules.md` before generating any source code").
4. THE `skills.md` SHALL be structured so that an AI coding assistant can parse it without ambiguity — using a consistent table or list format.
5. WHEN a new skill file is added to `.skills/`, THE `skills.md` SHALL be updated to include an entry for that file.

---

### Requirement 8: Coding Rules (`.skills/coding_rules.md`)

**User Story:** As a developer or AI coding assistant, I want a definitive set of coding rules and style conventions for AutoPrompt Studio, so that all code produced — whether by humans or AI — is consistent, maintainable, and production-grade.

#### Acceptance Criteria

1. THE `coding_rules.md` SHALL define rules for Python code covering: type hints (mandatory), docstring format (Google style), maximum function length (40 lines), import ordering (isort-compatible), and error handling patterns.
2. THE `coding_rules.md` SHALL define rules for TypeScript/React code covering: component naming (PascalCase), hook naming (`use` prefix), prop typing (explicit interface required), and forbidden patterns (e.g., `any` type, inline styles).
3. THE `coding_rules.md` SHALL specify the test coverage requirement: a minimum of 80% line coverage for all new modules.
4. THE `coding_rules.md` SHALL list security-specific rules including: no hardcoded secrets, all user inputs must be validated before use, and all external API calls must handle failures explicitly.
5. THE `coding_rules.md` SHALL define commit message format (e.g., Conventional Commits: `type(scope): description`).
6. THE `coding_rules.md` SHALL state that AI coding assistants MUST read `coding_rules.md` before generating any source code, and that any generated code violating these rules is considered non-compliant and must be regenerated.
7. THE `coding_rules.md` SHALL include a section listing explicitly prohibited patterns with a rationale for each prohibition.

---

### Requirement 9: Root README (`README.md`)

**User Story:** As any person who opens the AutoPrompt Studio repository — developer, evaluator, or stakeholder — I want a clear, welcoming README that tells me what the project is, how to get started, and where to find more information, so that I can orient myself in under five minutes.

#### Acceptance Criteria

1. THE `README.md` SHALL open with a one-paragraph description of AutoPrompt Studio that covers what it is, who it is for, and what problem it solves.
2. THE `README.md` SHALL include a technology stack section listing the primary technologies with a one-line description of each role.
3. THE `README.md` SHALL include a quick-start section that references `docs/PROJECT_GUIDE.md` for full setup instructions and provides a minimal "get running" command sequence.
4. THE `README.md` SHALL include a documentation index section with links to all eight supporting documents in the Documentation_Set.
5. THE `README.md` SHALL include a contributing section that references `docs/PROJECT_GUIDE.md` and states the code of conduct expectation.
6. THE `README.md` SHALL include a current project status badge or section indicating the platform is in active development / pre-release.
7. THE `README.md` SHALL be free of placeholder text (e.g., "TODO", "TBD", "coming soon") except within the roadmap status section.

---

### Requirement 10: Cross-Document Consistency

**User Story:** As a developer or AI coding assistant consuming multiple documents, I want all documents in the Documentation_Set to use consistent terminology, version references, and cross-links, so that there are no contradictions that would cause implementation errors or confusion.

#### Acceptance Criteria

1. THE Documentation_Set SHALL use consistent technology version references: when a technology version is mentioned in more than one document, it MUST be identical across all documents.
2. THE Documentation_Set SHALL use consistent terminology: every term defined in the Glossary of any document SHALL be used with the same meaning in all other documents.
3. WHEN one document references another (e.g., `ARCHITECTURE.md` references ADR-001), THE referencing document SHALL include the exact relative file path or a named link.
4. THE Documentation_Set SHALL contain no contradictory statements — for example, if `ARCHITECTURE.md` designates FastAPI as the backend framework, no other document SHALL designate a different backend framework.
5. THE Documentation_Set SHALL not reference any application code files that do not yet exist, except within the `ROADMAP.md` future considerations section and `specs/001_Project_Setup.md` planned deliverables.

---

### Requirement 11: Documentation Quality Standards

**User Story:** As a technical writer, engineering lead, or AI coding assistant, I want every document in the Documentation_Set to meet professional quality standards so that the documentation can serve as an authoritative and durable source of truth.

#### Acceptance Criteria

1. THE Documentation_Set SHALL use correct English grammar and spelling throughout, with no sentence fragments used as standalone paragraphs.
2. WHEN a document uses a technical term for the first time, THE document SHALL define it inline or reference the Glossary.
3. THE Documentation_Set SHALL use Markdown formatting correctly: headings use `#` syntax, code references use backticks, file paths use inline code formatting.
4. THE Documentation_Set SHALL contain no broken relative links between documents within the repository.
5. THE Documentation_Set SHALL not contain opinionated statements presented as fact (e.g., "DSPy is the best framework") — comparative claims must be qualified with context.
6. WHILE a document is a living artefact, THE document SHALL include a `Last Updated` date field in its header or front-matter so that readers can assess its currency.

---

### Requirement 12: Experiment Lifecycle

**User Story:** As a developer or AI coding assistant implementing Experiment execution, I want the platform to define and enforce a complete, deterministic lifecycle for every Experiment, so that users and operators always know the current state of an Experiment and can reason about what has happened and what will happen next.

#### Acceptance Criteria

1. THE system SHALL assign every Experiment exactly one of the following states at any point in time: `Created`, `Configured`, `Queued`, `Running`, `Completed`, `Failed`, `Cancelled`, `Archived`.
2. THE system SHALL enforce the following valid state transitions and SHALL reject any transition not listed here:
   - `Created` → `Configured`
   - `Configured` → `Queued`
   - `Queued` → `Running`
   - `Running` → `Completed`
   - `Running` → `Failed`
   - `Queued` → `Cancelled`
   - `Running` → `Cancelled`
   - `Completed` → `Archived`
   - `Failed` → `Archived`
   - `Failed` → `Configured` (retry path)
3. THE system SHALL persist a state-transition log entry for every state change, capturing: Experiment identifier, previous state, new state, actor (user or system), and an ISO 8601 timestamp.
4. THE system SHALL persist the full result payload of every Experiment that reaches `Completed` state, including all ranked prompt candidates and evaluation scores, without time-bound expiry.
5. WHEN an Experiment transitions to `Failed` state, THE system SHALL capture and persist the error type, error message, and the stack trace or structured error detail that caused the failure.
6. IF an Experiment in `Failed` state is retried by a user, THEN THE system SHALL transition the Experiment back to `Configured` state, preserve the original failure log entry, and create a new state-transition log entry recording the retry action.
7. THE system SHALL expose the current state and the full state-transition history of an Experiment via the API, so that clients can display lifecycle progress without polling internal logs.

---

### Requirement 13: Production Prompt Recommendation

**User Story:** As a user who has run a successful Experiment, I want the platform to automatically identify and present exactly one Production Prompt as the recommended candidate for deployment, so that I do not have to manually compare all optimised variants to determine which one to use.

#### Acceptance Criteria

1. WHEN an Experiment transitions to `Completed` state, THE system SHALL automatically designate exactly one optimised prompt as the Production Prompt for that Experiment.
2. THE Production Prompt SHALL be selected by ranking all candidate prompts using the evaluation score produced by the Experiment's configured evaluation metric; in the event of a tie, the candidate produced by the highest-priority configured optimizer SHALL be selected.
3. THE Production Prompt record SHALL contain the following fields: prompt text, selected optimizer name, model name, model configuration snapshot (including temperature and any sampler parameters), evaluation score, and a confidence level expressed as a numeric value between 0.0 and 1.0 inclusive.
4. THE system SHALL associate exactly one Production Prompt with each Experiment that reaches `Completed` state; no Experiment SHALL have zero or more than one designated Production Prompt.
5. THE platform UI SHALL display the Production Prompt on the Experiment Results screen with a clearly labelled "Production Prompt" section, distinct from the ranked list of all candidates.
6. THE platform UI SHALL provide a copy-to-clipboard control on the Production Prompt section that copies the prompt text to the system clipboard without additional user interaction.
7. THE system SHALL provide a download control that exports the Production Prompt as a standalone `.txt` file and as a structured `.json` file containing all fields specified in criterion 3.
8. THE system SHALL provide an export control that saves the Production Prompt directly to the Prompt Library (see Requirement 17), setting its initial status to `Active`.

---

### Requirement 14: Optimizer Plugin Architecture

**User Story:** As a platform engineer or third-party contributor, I want the optimisation engine layer to expose a stable plugin interface, so that new optimization engines can be added to AutoPrompt Studio without modifying existing platform code.

#### Acceptance Criteria

1. THE system SHALL define and publish a formal Optimizer Plugin interface specifying the minimum contract every optimizer must implement, including: a unique optimizer identifier, a human-readable display name, an `optimize(dataset, base_prompt, config) → List[CandidatePrompt]` method signature, and a `supported_config_schema()` method that returns a JSON Schema object describing the optimizer's configuration parameters.
2. THE system SHALL support dynamic discovery of Optimizer Plugins at application start-up by scanning a designated plugin directory or entry-point registry, without requiring changes to platform core code.
3. THE system SHALL maintain an internal plugin registry that records each discovered optimizer's identifier, display name, version, and registration timestamp; this registry SHALL be queryable via the API.
4. IF an Optimizer Plugin raises an unhandled exception during execution, THEN THE system SHALL isolate the failure to that plugin, log the error with the plugin identifier and full error detail, transition the affected Experiment to `Failed` state, and continue operating normally for all other active Experiments.
5. THE system SHALL guarantee backward compatibility for all plugins conforming to a given major version of the Optimizer Plugin interface; a change to the interface that breaks existing plugins SHALL increment the major version and be documented in a new ADR.
6. THE built-in DSPy optimizer SHALL be implemented as a conforming Optimizer Plugin and SHALL serve as the reference implementation of the plugin interface.

---

### Requirement 15: Dataset Validation

**User Story:** As a user uploading a dataset for an Experiment, I want the platform to validate my dataset immediately on upload and return specific, actionable error messages if the dataset is invalid, so that I do not discover data quality problems only after an Experiment has started running.

#### Acceptance Criteria

1. THE system SHALL accept dataset uploads in the following formats: CSV (UTF-8 encoded, comma-delimited), JSON Lines (one JSON object per line, UTF-8 encoded), and JSON array (a top-level array of objects, UTF-8 encoded). THE system SHALL reject uploads in any other format and return an error identifying the unsupported format.
2. THE system SHALL validate that each record in the uploaded dataset contains all fields declared as required by the Experiment's configured dataset schema, and SHALL reject the upload if any record is missing a required field, returning the field name and the index of the first offending record.
3. THE system SHALL reject an upload where the dataset contains zero records after parsing, returning an error stating that an empty dataset is not permitted.
4. THE system SHALL enforce a maximum upload size of 100 MB per dataset file; uploads exceeding this limit SHALL be rejected before parsing begins, with an error stating the limit and the actual size of the rejected file.
5. THE system SHALL detect exact-duplicate records within an uploaded dataset (defined as two records whose field values are identical across all fields) and SHALL surface a warning listing the count of duplicate records; the upload SHALL NOT be blocked by duplicates alone, but the warning SHALL be recorded in the Experiment's state-transition log.
6. THE system SHALL detect and reject dataset files that are not valid UTF-8 encoded text, returning an error identifying the encoding issue and the byte offset of the first invalid sequence.
7. WHEN a dataset upload is rejected for any reason listed in criteria 1 through 6, THE system SHALL return a structured error response (see Requirement 21) that identifies: the validation rule violated, a human-readable description of the problem, and, where applicable, the location within the file (record index or byte offset) where the problem was detected.

---

### Requirement 16: Prompt Library

**User Story:** As a user or team, I want a centralised Prompt Library that stores all Production Prompts promoted from completed Experiments, so that I can discover, reuse, and manage optimised prompts across projects without re-running Experiments unnecessarily.

#### Acceptance Criteria

1. THE system SHALL maintain a Prompt Library that persists all Production Prompts that have been exported to it, retaining each entry indefinitely unless explicitly deleted or archived by a user.
2. THE Prompt Library SHALL support full-text search across prompt text, display name, and tags, returning results ranked by relevance score descending.
3. THE Prompt Library SHALL support filter operations on the following dimensions: optimizer name, model name, evaluation score range (minimum and maximum), status (`Active` or `Archived`), and one or more tags.
4. THE system SHALL allow users to associate zero or more free-text tags with each Prompt Library entry; tags SHALL be stored, searched, and filtered as described in criteria 2 and 3.
5. THE system SHALL maintain a version history for each Prompt Library entry, recording each update to the prompt text or metadata as a new immutable version with an ISO 8601 timestamp and the identifier of the user who made the change; prior versions SHALL remain readable.
6. THE system SHALL allow users to archive a Prompt Library entry, setting its status to `Archived` and excluding it from default search and filter results while preserving all version history.
7. THE system SHALL provide an export control for each Prompt Library entry that downloads the current version as a `.txt` file and as a structured `.json` file containing prompt text, tags, version number, optimizer name, model name, evaluation score, and confidence level.
8. THE system SHALL allow users to initiate a new Experiment directly from a Prompt Library entry, pre-populating the new Experiment's base prompt field with the selected entry's prompt text.

---

### Requirement 17: Reproducibility Metadata

**User Story:** As a researcher, engineer, or auditor, I want every Experiment result to carry a complete metadata snapshot of the conditions under which the optimisation was performed, so that I can reproduce or audit any result independently of the original runtime environment.

#### Acceptance Criteria

1. THE system SHALL attach a Reproducibility Metadata record to every Experiment that reaches `Completed` or `Failed` state.
2. THE Reproducibility Metadata record SHALL contain the following fields: model name and version, optimizer name and version, dataset identifier and content hash (SHA-256), temperature setting, random seed (if the optimizer or model supports a seed parameter), number of optimisation iterations executed, Experiment start timestamp (ISO 8601), Experiment end timestamp (ISO 8601), and a complete snapshot of the optimizer configuration as a JSON object.
3. THE system SHALL compute and store the SHA-256 content hash of the dataset at the time the Experiment enters `Running` state, so that dataset modifications after execution do not invalidate the reproducibility record.
4. IF the optimizer or model does not support a random seed parameter, THEN THE Reproducibility Metadata SHALL record the seed field as `null` with a note stating "not supported by this optimizer/model combination".
5. THE Reproducibility Metadata SHALL be included in the Experiment's API response payload and SHALL be included in the Production Prompt export (see Requirement 13, criterion 7).
6. THE system SHALL ensure that the Reproducibility Metadata record for a `Completed` Experiment is immutable after the Experiment reaches `Completed` state; no field in the record SHALL be modifiable by any user or system process after that point.

---

### Requirement 18: Observability and Audit Logging

**User Story:** As a platform operator, security auditor, or developer debugging a production incident, I want the platform to emit structured, timestamped logs for all significant system and user-initiated events, so that I can trace the sequence of actions that led to any given system state or error condition.

#### Acceptance Criteria

1. THE system SHALL emit a structured log entry for every state transition in the Experiment Lifecycle (see Requirement 12), including: Experiment identifier, previous state, new state, actor, and ISO 8601 timestamp.
2. THE system SHALL emit a structured log entry for each optimisation iteration executed by an Optimizer Plugin, including: Experiment identifier, optimizer name, iteration number, duration in milliseconds, and the evaluation score of the candidate produced.
3. THE system SHALL emit a structured log entry for every inbound API request and its corresponding response, including: HTTP method, endpoint path, response status code, response latency in milliseconds, and an anonymised or pseudonymised request identifier; personally identifiable information SHALL NOT be logged in request or response log entries.
4. THE system SHALL maintain an append-only audit trail that records every user-initiated action that creates, modifies, or deletes a platform resource, capturing: the action type, the resource type and identifier, the user identifier, and an ISO 8601 timestamp. This audit trail SHALL NOT be deletable through any user-facing API endpoint.
5. THE system SHALL emit a structured error log entry whenever an unhandled exception or system error occurs, capturing: error type, error message, component name, and full stack trace or structured error detail.
6. THE system SHALL retain all log entries described in criteria 1 through 5 for a minimum of 90 days from the date of creation.
7. THE system SHALL expose Experiment-scoped log entries (criteria 1 and 2) through the API, so that clients can display execution progress and per-iteration results without direct access to infrastructure log streams.

---

### Requirement 19: UI Requirements

**User Story:** As a user of AutoPrompt Studio, I want a coherent, task-oriented web application that guides me from creating an Experiment through to retrieving a Production Prompt recommendation, so that I can complete the full optimisation workflow without requiring knowledge of the underlying API.

#### Acceptance Criteria

1. THE application SHALL provide a Dashboard screen that displays: a summary count of Experiments by state, a list of the five most recently active Experiments with their current state and last-updated timestamp, and a prominent call-to-action control to create a new Experiment.
2. THE application SHALL provide a Create Experiment screen that collects: Experiment name, base prompt text, dataset upload control (subject to Requirement 15), optimizer selection (populated from the plugin registry defined in Requirement 14), model selection, and evaluation metric selection.
3. THE application SHALL provide an Experiment Details screen that displays: current state, state-transition history with timestamps, Reproducibility Metadata (Requirement 17), and controls to start, cancel, retry, or archive the Experiment according to the valid transitions defined in Requirement 12.
4. THE application SHALL provide a Dataset Upload component, accessible from the Create Experiment screen, that presents inline validation feedback matching the error messages specified in Requirement 15 before the user submits the Experiment.
5. THE application SHALL provide a Running Experiment view that displays real-time or near-real-time progress, including: current iteration count, elapsed time, and the evaluation score of the best candidate identified so far, updated at an interval of no more than 5 seconds.
6. THE application SHALL provide an Experiment Results screen that presents: the designated Production Prompt section (per Requirement 13), the full ranked list of all candidate prompts with their evaluation scores, and export controls for the Production Prompt.
7. THE application SHALL provide a Prompt Library screen that exposes the search, filter, tag, version history, archive, export, and reuse controls defined in Requirement 16.
8. THE application SHALL provide a Settings screen that allows authorised users to manage: API key configuration for LLM providers (display masked values only), default model selection, and default evaluation metric selection.
9. THE application SHALL display non-blocking error notification banners for all API errors and validation failures, each banner containing a human-readable message and a dismissal control; banners SHALL NOT block the user from navigating away from the current screen.

---

### Requirement 20: API Versioning and Governance

**User Story:** As an API consumer or integration developer, I want the AutoPrompt Studio API to follow a published versioning and deprecation policy, so that I can build integrations with confidence that existing endpoints will not be removed or altered without prior notice.

#### Acceptance Criteria

1. THE API SHALL expose all endpoints under the `/api/v1/` path prefix; no production endpoint SHALL be served outside a versioned path prefix.
2. THE API SHALL maintain backward compatibility within a major version: adding new optional request fields, adding new response fields, and adding new endpoints are permitted changes; removing fields, renaming fields, or changing field types are breaking changes and SHALL NOT be made within the same major version.
3. WHEN the platform introduces a breaking change requiring a new major version, THE API SHALL continue to serve the previous major version for a deprecation period of no fewer than two sprints, returning a `Deprecation` response header on every response from the deprecated version that states the version identifier being deprecated and the sprint number after which it will be removed.
4. THE API SHALL return all error responses in the following consistent JSON envelope format: `{"error": {"code": "<string>", "message": "<string>", "details": [<optional array of structured detail objects>]}}`, with the HTTP status code set to the most semantically appropriate 4xx or 5xx value.
5. THE API SHALL document every endpoint, request schema, response schema, and error code in a machine-readable OpenAPI 3.1 specification committed to the repository under `docs/api/openapi.yaml`; this specification SHALL be updated in the same pull request as any API change.

---

### Requirement 21: AI Development Workflow

**User Story:** As a developer or AI coding assistant working on the AutoPrompt Studio codebase, I want the platform documentation to explicitly state the AI-first engineering workflow and the order in which artefacts must be read and applied, so that all implementation work — whether performed by humans or AI agents — is consistent, traceable, and aligned with the approved architecture.

#### Acceptance Criteria

1. THE `docs/PROJECT_GUIDE.md` SHALL include a dedicated "AI Development Workflow" section that states AutoPrompt Studio follows a Spec-Driven Development methodology in which every implementation task is preceded by a documentation review phase.
2. THE "AI Development Workflow" section SHALL specify the following ordered sequence of artefacts that MUST be read before any implementation begins:
   1. `specs/<sprint_number>_<title>.md` — the Sprint Specification governing the current work unit
   2. `docs/PRD.md` — to confirm the feature aligns with validated business and user requirements
   3. `docs/ARCHITECTURE.md` — to confirm the implementation approach conforms to the approved system architecture
   4. `docs/adr/ADR-001-Architecture.md` (and any subsequent ADRs) — to apply the rationale behind architectural decisions
   5. `.skills/skills.md` — to discover which skill files are applicable to the current task
   6. `.skills/coding_rules.md` — to apply mandatory code-quality standards before generating any code
3. THE "AI Development Workflow" section SHALL state that the following AI development tools are the approved toolchain for AutoPrompt Studio: Kiro (primary AI coding assistant), with Headroom, Ponytail, and Caveman available as supplementary AI workflow utilities.
4. THE "AI Development Workflow" section SHALL state that any AI coding assistant MUST read `.skills/coding_rules.md` in full before generating source code, and that generated code that violates any rule in `coding_rules.md` is non-compliant and MUST be regenerated.
5. THE "AI Development Workflow" section SHALL state that all significant architectural decisions arising during implementation MUST be captured as a new ADR in `docs/adr/` before the implementing pull request is merged, so that the decision record remains current with the codebase.
6. THE `docs/PROJECT_GUIDE.md` SHALL include `docs/ARCHITECTURE.md` as a required reading reference within the AI Development Workflow section, so that the architecture document is consumed as part of every implementation cycle, not only during onboarding.
