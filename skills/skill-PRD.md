# Skill: Product Requirements Document (PRD) Creator

## Description
Instructs the agent on how to gather, structure, and validate product requirements into a standardized, execution-ready PRD. This skill prevents scope creep and ensures technical alignment before coding begins.

## Trigger Conditions
- When an agent workflow explicitly loads this module, or when a user requests to "write a PRD", "create a spec", "define requirements", or "document a new feature".

## Core Instructions

### 1. The Discovery Rule
Before writing the document, you must evaluate if you have enough information. If the user's request is vague (e.g., "Make a checkout page"), you must ask clarifying questions about user roles, data storage, and edge cases before outputting the PRD.

### 2. Execution Guidelines
- **Be Prescriptive:** Do not use vague terms like "the system should be fast." Define measurable metrics (e.g., "Page load under 2 seconds").
- **Enforce Scope Boundaries:** Explicitly separate what is included in the current version from what is pushed to future iterations.
- **Maintain Markdown Structure:** Always output the document using the exact headers specified in the structural blueprint below.

---

## Required PRD Structural Blueprint

Your outputted PRD must contain the following default sections:

### 1. Document Metadata
- **Project Name:** [Name]
- **Target Launch Date:** [Date or Timeline]
- **Status:** [Draft / Under Review / Approved]
- **Authors:** [Names/Roles]

### 2. Executive Summary & Goals
- **Product Vision:** A 2-3 sentence overview of what is being built and why it matters.
- **Business Objectives:** What specific business metric does this feature drive?
- **Success Metrics (KPIs):** Clear, quantifiable goals (e.g., "Increase sign-ups by 15%").

### 3. Target Audience & User Personas
- **Primary User:** Who is the main user of this feature?
- **User Pain Points:** What specific frustration is this feature solving for them?

### 4. Functional Requirements (The "What")
*Format this section as a structured table with unique IDs for easy reference:*

| Req ID | User Role | Action / Capability | Acceptance Criteria (Definition of Done) | Priority (P0/P1) |
| :--- | :--- | :--- | :--- | :--- |
| FR-01 | Guest User | Can sign up using email and password | Password must be >8 characters with 1 number. Sends verification email. | P0 |
| FR-02 | Admin | Can view user registration metrics | Dashboard updates in real-time with total user counts. | P1 |

### 5. Non-Functional Requirements
- **Performance:** Response times, uptime, and scaling needs.
- **Security & Privacy:** Authentication standards, data encryption, and compliance (e.g., GDPR).
- **UX/UI Constraints:** Layout guidelines, theme/style adherence, or responsive design requirements.

### 6. Out of Scope (Future Phases)
- List features, platforms, or enhancements explicitly excluded from this current release to prevent scope creep.

### 7. Known Risks & Open Questions
- Technical dependencies, third-party API limitations, or design choices that still need a final decision.
