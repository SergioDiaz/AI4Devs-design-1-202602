# LTI ATS v1 - Product and System Design

## 1. Product Definition

### 1.1 Brief Description
LTI ATS is an AI-native applicant tracking system for recruiting teams that want to hire faster without losing quality.  
The first version focuses on three outcomes:
- Reduce time-to-fill through automation and AI-assisted screening.
- Improve hiring quality through structured evaluation and explainable rankings.
- Improve team alignment through real-time collaboration between recruiters and hiring managers.

### 1.2 Value Added and Competitive Advantages
- AI copilot across the full hiring funnel, not only resume ranking.
- Real-time collaborative workflows with comments, shared scorecards, and decision history.
- Automation engine for repetitive HR tasks (status changes, reminders, interview logistics).
- Explainable AI recommendations with transparent scoring factors and audit logs.
- API-first integration model for job boards, calendars, email, and HRIS platforms.

### 1.3 Main Functions (v1 Scope)
- Job requisition creation, approval, and publication to external job boards.
- Candidate intake from multiple channels and unified applicant profile.
- AI-assisted CV parsing, skill extraction, candidate-job matching, and shortlist proposals.
- Pipeline management with configurable stages and SLA alerts.
- Interview scheduling with calendar sync and interview kit distribution.
- Structured feedback, hiring scorecards, and final decision workflow.
- Offer generation and status tracking.
- KPI dashboards: time-to-hire, conversion by stage, source quality, and interviewer turnaround.

### 1.4 Lean Canvas Diagram
```mermaid
flowchart TB
    UVP["Unique Value Proposition<br/>AI-first collaborative ATS that reduces time-to-hire and improves decision quality with explainable automation"]

    Problem["Problem<br/>- Slow hiring cycles<br/>- Manual repetitive tasks<br/>- Low alignment across HR and managers"]
    Segment["Customer Segments<br/>- Mid-size companies (100-2000 employees)<br/>- High-growth startups<br/>- Enterprise HR teams"]
    Solution["Solution<br/>- Intelligent pipeline automation<br/>- Explainable candidate ranking<br/>- Real-time collaborative hiring workspace"]
    Channels["Channels<br/>- Direct sales<br/>- HR tech partners<br/>- Content marketing and webinars"]
    Revenue["Revenue Streams<br/>- Subscription per recruiter seat<br/>- Usage-based AI credits<br/>- Premium integration packages"]
    Costs["Cost Structure<br/>- Cloud and storage<br/>- LLM/API usage<br/>- Product and customer success teams"]
    Metrics["Key Metrics<br/>- Time-to-fill<br/>- Stage conversion rate<br/>- Offer acceptance rate<br/>- Hiring manager satisfaction"]
    Advantage["Unfair Advantage<br/>- Proprietary hiring signal dataset<br/>- Explainable AI + human-in-the-loop controls<br/>- Fast integration templates"]

    Problem --> UVP
    Segment --> UVP
    Solution --> UVP
    Channels --> Segment
    Revenue --> UVP
    Costs --> Solution
    Metrics --> UVP
    Advantage --> UVP
```

## 2. Three Main Use Cases

### UC-01: Create and Publish a Job Requisition
**Primary actors:** Recruiter, Hiring Manager  
**Goal:** Publish a validated job opening to selected channels with minimum manual steps.

**Main flow**
1. Recruiter creates a new requisition with role details, required skills, and budget range.
2. System routes requisition to Hiring Manager for approval.
3. Hiring Manager reviews and approves (or requests edits).
4. ATS generates optimized job ad copy (AI suggestion).
5. Recruiter selects channels (company site, job boards, social).
6. ATS publishes and tracks posting IDs and delivery status.

**Postconditions**
- Job requisition is approved and published.
- Pipeline stages are initialized.
- Source tracking is enabled for inbound candidates.

```mermaid
sequenceDiagram
    participant R as Recruiter
    participant HM as Hiring Manager
    participant ATS as LTI ATS
    participant JB as Job Boards

    R->>ATS: Create requisition
    ATS->>HM: Request approval
    HM->>ATS: Approve requisition
    ATS->>R: Suggest AI-optimized job description
    R->>ATS: Confirm channels and publish
    ATS->>JB: Push job posts
    JB-->>ATS: Return posting identifiers
    ATS-->>R: Publish status and tracking links
```

### UC-02: AI-Assisted Screening and Shortlisting
**Primary actors:** Recruiter  
**Goal:** Prioritize candidates using explainable AI while keeping human control.

**Main flow**
1. Candidate applications enter the pipeline.
2. ATS parses resumes and extracts structured signals (skills, experience, certifications).
3. AI Screening Service computes fit score per candidate-job pair.
4. System generates explanation cards and risk flags.
5. Recruiter reviews ranked list and confirms shortlist.
6. Selected candidates move to interview stage.

**Postconditions**
- Shortlist is created with explainable rationale.
- Rejected or parked candidates are documented with reason codes.

```mermaid
sequenceDiagram
    participant ATS as LTI ATS
    participant AI as AI Screening Service
    participant R as Recruiter

    ATS->>AI: Send candidate profile + job requirements
    AI->>AI: Parse CV and compute fit score
    AI-->>ATS: Return ranking + explanation
    ATS-->>R: Show prioritized shortlist
    R->>ATS: Approve shortlist and move stage
    ATS-->>R: Stage updates and audit trail
```

### UC-03: Collaborative Interview and Hiring Decision
**Primary actors:** Recruiter, Interviewers, Hiring Manager  
**Goal:** Run structured interviews and reach a decision quickly with full transparency.

**Main flow**
1. Recruiter schedules panel interview slots.
2. ATS sends calendar invites and interview kits.
3. Interviewers submit structured feedback and scorecards.
4. ATS aggregates feedback and highlights disagreements.
5. Hiring Manager and Recruiter run decision review.
6. ATS records decision (offer/reject/talent pool) and triggers next actions.

**Postconditions**
- Decision is recorded with traceable evidence.
- Offer workflow starts automatically for approved candidates.

```mermaid
sequenceDiagram
    participant R as Recruiter
    participant ATS as LTI ATS
    participant I as Interviewers
    participant HM as Hiring Manager

    R->>ATS: Schedule interviews
    ATS->>I: Send invites and interview kits
    I->>ATS: Submit scorecards and feedback
    ATS->>HM: Provide consolidated recommendation
    HM->>R: Decision alignment
    R->>ATS: Confirm final decision
    ATS-->>R: Trigger offer/reject automation
```

## 3. Data Model

### 3.1 Entity and Attribute Model
```mermaid
erDiagram
    ORGANIZATION {
        uuid organization_id PK
        string name
        string industry
        string plan_tier
        datetime created_at
    }

    USER {
        uuid user_id PK
        uuid organization_id FK
        string full_name
        string email
        string role
        boolean is_active
        datetime created_at
    }

    JOB_REQUISITION {
        uuid requisition_id PK
        uuid organization_id FK
        uuid hiring_manager_id FK
        string title
        string department
        string location
        string employment_type
        decimal salary_min
        decimal salary_max
        string status
        datetime created_at
    }

    JOB_POSTING {
        uuid posting_id PK
        uuid requisition_id FK
        string channel
        string external_post_id
        string status
        datetime published_at
    }

    CANDIDATE {
        uuid candidate_id PK
        uuid organization_id FK
        string full_name
        string email
        string phone
        string current_location
        string source
        datetime created_at
    }

    RESUME {
        uuid resume_id PK
        uuid candidate_id FK
        string file_url
        string parsed_text_checksum
        int total_experience_months
        datetime uploaded_at
    }

    APPLICATION {
        uuid application_id PK
        uuid candidate_id FK
        uuid requisition_id FK
        string current_stage
        decimal ai_fit_score
        string decision_status
        datetime applied_at
        datetime updated_at
    }

    STAGE_DEFINITION {
        uuid stage_id PK
        uuid requisition_id FK
        string stage_name
        int stage_order
        int sla_hours
    }

    APPLICATION_STAGE_EVENT {
        uuid event_id PK
        uuid application_id FK
        uuid stage_id FK
        uuid changed_by_user_id FK
        string from_stage
        string to_stage
        string reason_code
        datetime changed_at
    }

    INTERVIEW {
        uuid interview_id PK
        uuid application_id FK
        string interview_type
        datetime scheduled_start
        datetime scheduled_end
        string meeting_link
        string status
    }

    INTERVIEW_FEEDBACK {
        uuid feedback_id PK
        uuid interview_id FK
        uuid interviewer_id FK
        int score
        string recommendation
        text notes
        datetime submitted_at
    }

    OFFER {
        uuid offer_id PK
        uuid application_id FK
        decimal base_salary
        decimal variable_salary
        date start_date
        string status
        datetime created_at
    }

    COMMENT {
        uuid comment_id PK
        uuid application_id FK
        uuid author_user_id FK
        text body
        boolean is_private
        datetime created_at
    }

    AUTOMATION_RULE {
        uuid rule_id PK
        uuid organization_id FK
        string name
        string trigger_event
        string action_type
        json action_config
        boolean is_enabled
    }

    NOTIFICATION {
        uuid notification_id PK
        uuid user_id FK
        string channel
        string title
        string body
        string status
        datetime sent_at
    }

    AUDIT_LOG {
        uuid audit_id PK
        uuid organization_id FK
        uuid actor_user_id FK
        string entity_type
        uuid entity_id
        string action
        json metadata
        datetime created_at
    }

    ORGANIZATION ||--o{ USER : has
    ORGANIZATION ||--o{ JOB_REQUISITION : owns
    ORGANIZATION ||--o{ CANDIDATE : stores
    ORGANIZATION ||--o{ AUTOMATION_RULE : configures
    ORGANIZATION ||--o{ AUDIT_LOG : records
    USER ||--o{ COMMENT : writes
    USER ||--o{ NOTIFICATION : receives
    JOB_REQUISITION ||--o{ JOB_POSTING : publishes
    JOB_REQUISITION ||--o{ APPLICATION : receives
    JOB_REQUISITION ||--o{ STAGE_DEFINITION : defines
    CANDIDATE ||--o{ RESUME : uploads
    CANDIDATE ||--o{ APPLICATION : submits
    APPLICATION ||--o{ APPLICATION_STAGE_EVENT : tracks
    STAGE_DEFINITION ||--o{ APPLICATION_STAGE_EVENT : classifies
    APPLICATION ||--o{ INTERVIEW : includes
    INTERVIEW ||--o{ INTERVIEW_FEEDBACK : collects
    APPLICATION ||--o| OFFER : may_receive
    APPLICATION ||--o{ COMMENT : contains
```

### 3.2 Relationship Notes
- One candidate can apply to multiple requisitions.
- Each application belongs to exactly one requisition and one candidate.
- Pipeline history is stored as immutable stage events for traceability.
- Offer is optional and only created for approved applications.
- Audit logs are cross-cutting and capture sensitive changes for compliance.

## 4. High-Level System Design

### 4.1 Architecture Explanation
LTI ATS v1 follows a modular, API-first architecture:
- **Client layer:** Recruiter and Hiring Manager web app with role-based UI.
- **API layer:** Single entry point for auth, validation, rate limits, and routing.
- **Domain services:** Requisition, Candidate Pipeline, Collaboration, Interview Scheduling, Offers, Notifications.
- **AI services:** Resume parsing, matching, ranking, and explanation generation.
- **Automation layer:** Event-driven rules engine for status transitions and reminders.
- **Data layer:** PostgreSQL for transactional data, object storage for resumes, cache for sessions/jobs, search index for fast candidate lookup.
- **Integration layer:** Job boards, calendar/email providers, HRIS connectors, and LLM provider.

### 4.2 High-Level Architecture Diagram
```mermaid
flowchart LR
    subgraph Clients
        Web["Recruiter/Hiring Manager Web App"]
    end

    subgraph Platform["LTI ATS Platform"]
        APIGW["API Gateway / BFF"]
        Auth["Auth and Organization Service"]
        Req["Requisition Service"]
        Pipe["Candidate Pipeline Service"]
        Collab["Collaboration Service"]
        Sched["Interview Scheduling Service"]
        OfferSvc["Offer Service"]
        Auto["Automation Engine"]
        AISvc["AI Screening Service"]
        Notify["Notification Service"]
        Integ["Integration Hub"]
    end

    subgraph Data
        PG["PostgreSQL"]
        Obj["Object Storage (CVs, attachments)"]
        Redis["Redis Cache / Queue"]
        Search["Search Index"]
    end

    subgraph External
        JobBoards["Job Boards APIs"]
        CalMail["Calendar and Email APIs"]
        HRIS["HRIS / Payroll Systems"]
        LLM["LLM Provider"]
    end

    Web --> APIGW
    APIGW --> Auth
    APIGW --> Req
    APIGW --> Pipe
    APIGW --> Collab
    APIGW --> Sched
    APIGW --> OfferSvc
    APIGW --> AISvc
    APIGW --> Notify
    APIGW --> Integ
    Req --> PG
    Pipe --> PG
    Collab --> PG
    Sched --> PG
    OfferSvc --> PG
    Pipe --> Obj
    Pipe --> Search
    Auto --> Redis
    Notify --> Redis
    AISvc --> LLM
    AISvc --> PG
    Integ --> JobBoards
    Integ --> CalMail
    Integ --> HRIS
```

## 5. C4 Diagram Deep Dive

### 5.1 Target Component
Selected component: **AI Screening Service** (critical for differentiation and hiring efficiency).

### 5.2 C4 Level 1 - System Context
```mermaid
flowchart LR
    Recruiter["Person: Recruiter"]
    HiringManager["Person: Hiring Manager"]
    Candidate["Person: Candidate"]
    LTI["System: LTI ATS"]
    JobBoards["External System: Job Boards"]
    Calendar["External System: Calendar/Email"]
    LLM["External System: LLM Provider"]

    Recruiter --> LTI
    HiringManager --> LTI
    Candidate --> LTI
    LTI --> JobBoards
    LTI --> Calendar
    LTI --> LLM
```

### 5.3 C4 Level 2 - Container View
```mermaid
flowchart LR
    User["Person: Recruiter/Hiring Manager"]

    subgraph LTI["System: LTI ATS"]
        Web["Container: Web Application (React)"]
        API["Container: API Backend (Node/Java/.NET)"]
        AIS["Container: AI Screening Service"]
        DB["Container: PostgreSQL"]
        Blob["Container: Object Storage"]
    end

    LLM["External Container: LLM Provider API"]
    Boards["External Container: Job Boards API"]

    User --> Web
    Web --> API
    API --> DB
    API --> Blob
    API --> AIS
    API --> Boards
    AIS --> LLM
    AIS --> DB
```

### 5.4 C4 Level 3 - Component View (Inside AI Screening Service)
```mermaid
flowchart TB
    API["Inbound API Adapter"]
    Parser["Resume Parser Component"]
    ReqNorm["Job Requirement Normalizer"]
    Feature["Feature Builder"]
    Match["Matching and Scoring Engine"]
    Explain["Explainability Generator"]
    Guard["Bias and Policy Guardrails"]
    RankAPI["Ranking Response Composer"]
    Vector["Embedding/Vector Client"]
    Store["Screening Feature Store"]
    PromptRepo["Prompt Template Repository"]

    API --> Parser
    API --> ReqNorm
    Parser --> Feature
    ReqNorm --> Feature
    Feature --> Match
    Match --> Vector
    Match --> Guard
    Guard --> Explain
    Explain --> RankAPI
    Match --> RankAPI
    RankAPI --> Store
    PromptRepo --> Explain
```

### 5.5 Component Responsibilities
- **Resume Parser:** Extracts structured candidate signals from CV files.
- **Job Requirement Normalizer:** Converts requisition text into weighted requirement vectors.
- **Feature Builder:** Creates model-ready features from candidate and job inputs.
- **Matching and Scoring Engine:** Computes fit scores and ranking confidence.
- **Bias and Policy Guardrails:** Applies fairness checks and business constraints.
- **Explainability Generator:** Produces human-readable reasons behind ranking.
- **Ranking Response Composer:** Returns shortlist output to the Pipeline Service.

## 6. v1 Delivery Notes
- Multi-tenant boundaries are enforced at organization level across all entities.
- Human decision remains final for all hiring outcomes (AI is assistive, not autonomous).
- Architecture is designed to support future modules (referral, onboarding, internal mobility).
