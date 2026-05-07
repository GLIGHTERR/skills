---
name: screen-description-from-design
description: >
  Generate a Use Case Description document and a Screen Description document from design inputs
  such as UI mockups, wireframes, user flow diagrams, or design descriptions.
  Use this skill whenever the user uploads a design file (Figma export, wireframe image,
---

# Screen Description From Design

Skill to generate a professional Use Case Description document and a professional Screen Description document by analyzing design artifacts (UI mockups, wireframes, flow diagrams, or textual design descriptions) provided by the user.

The output is delivered as a
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

### Step 2 — Structure the Screen Description

Organize content into the following outline:

```
1. Use Case Description
2. Screen Description
```

### Step 3 — Write Use Case Descriptions

Each use case must follow this format:

| Field | Content |
|---|---|
| **ID** | UC-001 (sequential, unique) |
| **Title** | Short descriptive title |
| **Description** | "The system SHALL …" (use SHALL for mandatory, SHOULD for recommended) |
| **Priority** | High / Medium / Low |
| **Source** | Which screen or flow this was derived from |
| **Acceptance Criteria** | Measurable condition to verify the requirement is met |
| **Basic Flow** | Step-by-step user interaction flow (if applicable) |
| **Alternate Flows** | Variations or exceptions to the basic flow (if applicable) with ID prefix `AL-` |
| **Exception Flows** | Error conditions and how they are handled (if applicable) with ID prefix `EX-` |
| **Business Rules** | Any relevant rules or constraints (if applicable) with ID prefix `BR-` |
| **Non-functional Requirements** | Performance, security, usability, etc. (if applicable) with ID prefix `NFR-` |

**Writing rules:**
- Use active voice and present tense
- One use case per statement — never combine two SHALLs in one entry
- Avoid implementation details (describe *what*, not *how*)
- Every screen in the design must map to at least one UC

### Step 4 — Write Screen Descriptions

Each screen description must follow this format:

| Field | Data Type | Required | Editable| Default | Content |
|---|---|---|---|---|---|
| **UI Components** | Data Type | Required | Editable| Default | What is the meaning of this component?, which values are allowed? |

**Writing rules:**
- Use active voice and present tense
- One component per statement — never combine two in one row
- One screen description may describe multiple screens if they are in the same flow (e.g., sign-up flow with 3 screens described 3 steps)
- Describe the screen like "Image 1. Sign-up Screen: Step 1", followed by an image reference and then a table describing the components on that screen
- Every screen in the design must map to at least one UC

### Step 5 — Produce the Document

Read `SKILL.md` before generating the output file.

Generate a `.md` file with:
- Cover page: project name, version, date, authors (use placeholders if unknown)
- Table of contents (auto-generated)
- All sections from the outline above
- Requirements tables with alternating row shading
- Consistent Heading 1 / Heading 2 / Heading 3 styles
- Footer with document version and page numbers

Save the final file to `/screen-descriptions/<FunctionName>/SCREEN_DESCRIPTION_<FunctionName>_v1.0.md`
and present it to the user using `present_files`.

---

## Quality Checklist

Before delivering the document, verify:

- [ ] Every UI screen/state from the design has at least one corresponding SC
- [ ] All SCs use "SHALL" language
- [ ] No two SCs share the same ID
- [ ] NFRs cover at minimum: performance, security, and usability
- [ ] Actors defined in Section 2 are referenced in Section 3 use cases
- [ ] Acceptance criteria are measurable (no vague terms like "fast" or "user-friendly")
- [ ] Assumptions section lists anything inferred but not shown in the design

---

## Example Requirement Entry

> **SC-012** — User Login with Email
> - **Description**: The system SHALL allow registered users to log in using their email address and password.
> - **Priority**: High
> - **Source**: Screen: Login Page (Wireframe p.3)
> - **Acceptance Criteria**: Given valid credentials, the user is redirected to the Dashboard within 2 seconds. Given invalid credentials, an error message "Invalid email or password" is displayed and no session is created.
> - **Basic Flow**:
>   1. User navigates to the Login Page.
>   2. User enters email and password.
>   3. User clicks the "Login" button.
>   4. System validates credentials.
>   5. If valid, system creates a session and redirects to Dashboard.
>   6. If invalid, system displays error message and remains on Login Page.
> - **Alternate Flows**:
>   - **AL-012a**: If the user clicks "Forgot Password", they are taken to the Password Recovery screen.
> - **AL-012b**: If the user clicks "Login with Google", they are redirected to the Google OAuth flow.
> - **Exception Flows**:
>   - **EX-012a**: If the authentication service is down, the system displays "Login service is currently unavailable. Please try again later." and does not attempt to validate credentials.
> - **Business Rules**:
>   - **BR-012a**: Passwords must be at least 8 characters long and contain at least one number and one special character.
> - **Non-functional Requirements**:
>   - **NFR-012a**: The login process must complete within 2 seconds under normal load conditions.
>   - **NFR-012b**: The login page must be accessible and meet WCAG 2.1 AA standards.

---

## Notes & Constraints

- If the design is for a **mobile app**, add a section 5.6 for platform-specific constraints (iOS/Android versions, screen sizes).
- If the design references **third-party APIs** (e.g., payment gateways, maps), create stub entries under Section 4.3 and flag them for the user to complete.
- If the user provides **multiple design versions**, note discrepancies in the Assumptions section rather than silently picking one.
- Version the document as `v0.1` if the design is clearly a draft/wireframe; use `v1.0` only for high-fidelity or signed-off designs.