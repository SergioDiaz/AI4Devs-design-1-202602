# Prompts Used

## Prompt 1 - Product Discovery and Positioning
```text
You are a senior product strategist and HR-tech expert.
Design a first version of an AI-native Applicant Tracking System (ATS) for a startup named LTI.
Target users: recruiters and hiring managers in growing companies.
Deliver:
1) Product vision and value proposition
2) Top competitive advantages
3) Prioritized v1 feature list with business impact
Keep the output concise, practical, and execution-focused.
```

## Prompt 2 - Lean Canvas
```text
Create a Lean Canvas for LTI ATS.
Include: Problem, Customer Segments, Unique Value Proposition, Solution, Channels, Revenue Streams, Cost Structure, Key Metrics, Unfair Advantage.
Make assumptions explicit and tailored to B2B SaaS HR software.
```

## Prompt 3 - Use Cases
```text
Define the 3 most important use cases for an ATS v1 that emphasizes automation and AI assistance.
For each use case include:
- Primary actors
- Preconditions
- Main success scenario
- Postconditions
Focus on operational workflows used weekly by recruiting teams.
```

## Prompt 4 - Data Model
```text
Design a relational data model for the ATS use cases.
Return:
- Core entities
- Attributes with data types
- Cardinality between entities
Prioritize multi-tenant SaaS concerns, auditability, and explainable AI scoring.
```

## Prompt 5 - High-Level Architecture
```text
Design a high-level system architecture for LTI ATS v1.
Include:
- Frontend and backend boundaries
- Domain services
- AI components
- Storage choices
- External integrations (job boards, email/calendar, HRIS, LLM provider)
Explain key tradeoffs for scalability, speed, and maintainability.
```

## Prompt 6 - C4 Decomposition
```text
Create a C4 decomposition for the ATS and go deep into one strategic component.
Provide:
- System context (Level 1)
- Container view (Level 2)
- Component view (Level 3) of AI Screening Service
Highlight interfaces and responsibilities clearly.
```

## Prompt 7 - Quality Check
```text
Review the whole ATS design for gaps and risks.
Check for:
- Missing workflows
- Data model inconsistencies
- Compliance and fairness concerns in AI-assisted hiring
- Operational risks in integrations
Return a prioritized list of issues and mitigations.
```
