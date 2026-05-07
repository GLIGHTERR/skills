---
name: srs-from-design
description: >
  Generate a Software Requirements Specification (SRS) document from design inputs
  such as UI mockups, wireframes, user flow diagrams, or design descriptions.
  Use this skill whenever the user uploads a design file (Figma export, wireframe image,
  flow diagram), pastes a design description, or asks to "write SRS", "generate requirements",
  "document requirements from design", "turn this design into a spec", or similar.
  Also trigger when the user mentions IEEE 830, functional requirements, non-functional
  requirements, use cases, or system requirements in the context of a product or feature.
  Do NOT use for pure code documentation (use docstring skills instead) or
  for writing test plans (use QA skills instead).
---

# SRS From Design

Skill to generate a professional Software Requirements Specification (SRS) document
by analyzing design artifacts (UI mockups, wireframes, flow diagrams, or textual design
descriptions) provided by the user.

The output follows the **IEEE 830** standard structure and is delivered as a
well-formatted `.md` file.

---

## Inputs Accepted

- **Images**: Screenshots, wireframes, Figma exports, hand-drawn sketches (uploaded as PNG/JPG)
- **PDFs**: Design decks, flow diagrams, UI specs
- **Text**: Pasted design descriptions, user story lists, feature outlines
- **Mixed**: Any combination of the above

If no input is provided, ask the user to supply at least one design artifact or description
before proceeding.

---

## Workflow

### Step 1 — Understand the Design

Carefully analyze all provided design artifacts. Extract:

- **Purpose & Scope**: What system/product/feature does this design represent?
- **Actors / Users**: Who interacts with the system? (end users, admins, third-party systems)
- **Screens / Pages / States**: List all distinct UI states visible in the design
- **User Flows**: What sequences of actions does each actor perform?
- **Data Entities**: What data is created, read, updated, or deleted?
- **Business Rules**: Any visible constraints, validations, or conditions
- **Integrations**: External services, APIs, or systems referenced

If critical information is ambiguous, ask targeted clarifying questions (max 3 at a time)
before writing the SRS.

### Step 2 — Structure the SRS

Organize content into the standard IEEE 830 outline:

```
1. Introduction
   1.1 Purpose
   1.2 Scope
   1.3 Definitions, Acronyms, Abbreviations
   1.4 References
   1.5 Overview

2. Overall Description
   2.1 Product Perspective
   2.2 Product Functions (high-level summary)
   2.3 User Classes and Characteristics
   2.4 Operating Environment
   2.5 Design and Implementation Constraints
   2.6 Assumptions and Dependencies

3. System Features (Functional Requirements)
   3.x [Feature Name]
       3.x.1 Description and Priority
       3.x.2 Stimulus / Response Sequences
       3.x.3 Functional Requirements (FR-XXX)

4. External Interface Requirements
   4.1 User Interfaces
   4.2 Hardware Interfaces
   4.3 Software Interfaces
   4.4 Communication Interfaces

5. Non-Functional Requirements
   5.1 Performance Requirements
   5.2 Safety Requirements
   5.3 Security Requirements
   5.4 Software Quality Attributes
   5.5 Business Rules

6. Other Requirements (optional)

Appendix A: Glossary
Appendix B: Analysis Models (use case diagrams, data flow diagrams if applicable)
```

### Step 3 — Write Requirements

Each functional requirement must follow this format:

| Field | Content |
|---|---|
| **ID** | FR-001 (sequential, unique) |
| **Title** | Short descriptive title |
| **Description** | "The system SHALL …" (use SHALL for mandatory, SHOULD for recommended) |
| **Priority** | High / Medium / Low |
| **Source** | Which screen or flow this was derived from |
| **Acceptance Criteria** | Measurable condition to verify the requirement is met |

**Non-functional requirements** use the same table with ID prefix `NFR-`.

**Writing rules:**
- Use active voice and present tense
- One requirement per statement — never combine two SHALLs in one entry
- Avoid implementation details (describe *what*, not *how*)
- Every screen in the design must map to at least one FR

### Step 4 — Produce the Document

Read `SKILL.md` before generating the output file.

Generate a `.md` file with:
- Cover page: project name, version, date, authors (use placeholders if unknown)
- Table of contents (auto-generated)
- All sections from the outline above
- Requirements tables with alternating row shading
- Consistent Heading 1 / Heading 2 / Heading 3 styles
- Footer with document version and page numbers

Save the final file to `/srs/<FunctionName>/SRS_<FunctionName>_v1.0.md`
and present it to the user using `present_files`.

---

## Quality Checklist

Before delivering the document, verify:

- [ ] Every UI screen/state from the design has at least one corresponding FR
- [ ] All FRs use "SHALL" language
- [ ] No two FRs share the same ID
- [ ] NFRs cover at minimum: performance, security, and usability
- [ ] Actors defined in Section 2 are referenced in Section 3 use cases
- [ ] Acceptance criteria are measurable (no vague terms like "fast" or "user-friendly")
- [ ] Assumptions section lists anything inferred but not shown in the design

---

## Example Requirement Entry

> **FR-012** — User Login with Email
> - **Description**: The system SHALL allow registered users to log in using their email address and password.
> - **Priority**: High
> - **Source**: Screen: Login Page (Wireframe p.3)
> - **Acceptance Criteria**: Given valid credentials, the user is redirected to the Dashboard within 2 seconds. Given invalid credentials, an error message "Invalid email or password" is displayed and no session is created.

---

## Notes & Constraints

- If the design is for a **mobile app**, add a section 5.6 for platform-specific constraints (iOS/Android versions, screen sizes).
- If the design references **third-party APIs** (e.g., payment gateways, maps), create stub entries under Section 4.3 and flag them for the user to complete.
- If the user provides **multiple design versions**, note discrepancies in the Assumptions section rather than silently picking one.
- Version the document as `v0.1` if the design is clearly a draft/wireframe; use `v1.0` only for high-fidelity or signed-off designs.