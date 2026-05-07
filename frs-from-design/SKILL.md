---
name: frs-from-design
description: >
  Generate a Functional Requirements Specification (FRS) document from design inputs
  such as UI mockups, wireframes, user flow diagrams, or design descriptions.
  Use this skill whenever the user uploads a design file (Figma export, wireframe image,
  flow diagram), pastes a design description, or asks to "write FRS", "generate functional spec",
  "document functional requirements from design", "turn this design into a functional spec", or similar.
  Also trigger when the user mentions functional specification, feature specification,
  functional design document, or FRS in the context of a product or feature.
  Do NOT use when the user asks for SRS (use srs-from-design skill instead),
  for pure code documentation (use docstring skills instead),
  or for writing test plans (use QA skills instead).
---

# FRS From Design

Skill to generate a professional Functional Requirements Specification (FRS) document
by analyzing design artifacts (UI mockups, wireframes, flow diagrams, or textual design
descriptions) provided by the user.

Unlike SRS (which covers both functional and non-functional requirements at a system level),
the FRS focuses exclusively on **what the system must do** — describing each feature and
function in detail from the user's perspective, with precise input/output/behavior definitions.

The output is delivered as a well-formatted `.md` file (convertible to `.docx` via Pandoc).

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
- **Inputs & Outputs**: For each screen/function, what data goes in and what comes out?
- **Business Rules**: Visible constraints, validations, conditions, and decision logic
- **Edge Cases**: Error states, empty states, boundary conditions visible in the design
- **Integrations**: External services, APIs, or systems referenced

If critical information is ambiguous, ask targeted clarifying questions (max 3 at a time)
before writing the FRS.

### Step 2 — Structure the FRS

Organize content into the following outline:

```
1. Introduction
   1.1 Purpose
   1.2 Scope
   1.3 Definitions, Acronyms, Abbreviations
   1.4 References
   1.5 Document Conventions

2. Overall Description
   2.1 Product Overview
   2.2 User Classes and Characteristics
   2.3 Assumptions and Dependencies
   2.4 Out of Scope

3. Functional Requirements
   3.x [Feature / Module Name]
       3.x.1 Feature Overview
       3.x.2 User Interactions
       3.x.3 Functional Specifications (FS-XXX)
           - Basic Flow
           - Alternate Flows (if any)
           - Exception Flows (error handling)
           - Business Rules
           - Non-functional requirements (if directly tied to this feature)

4. Screen Descriptions & UI Requirements
   4.1 Screen Inventory
   4.2 Screen Descriptions
       4.2.x [Screen Name]
           - Purpose: What this screen is for
           - Actors: Who accesses this screen
           - Entry Points: How users reach this screen
           - UI Components: List of all visible elements (buttons, fields, labels, icons)
           - Default State: What the screen looks like on first load
           - States: Loading / Empty / Populated / Error states
           - Exit Points: Where users can navigate to from this screen
           - Annotations: Any design notes or conditional visibility rules
   4.3 Navigation & Flow
   4.4 UI Behavior Rules (loading states, validation messages, empty states)

5. Data Requirements
   5.1 Data Entities and Attributes
   5.2 Data Validation Rules
   5.3 Data Relationships

6. Integration Requirements
   6.x [Integration Name]
       - Purpose
       - Trigger
       - Request / Response contract (if visible in design)

Appendix A: Glossary
Appendix B: Screen Inventory with Annotations
Appendix C: User Flow Diagrams
```

### Step 3 — Write Functional Specifications

Each functional specification entry must follow this format:

| Field | Content |
|---|---|
| **ID** | FS-001 (sequential, unique) |
| **Feature** | Name of the parent feature/module |
| **Title** | Short descriptive title |
| **Description** | "The system SHALL …" (use SHALL for mandatory, SHOULD for recommended) |
| **Actor** | Who triggers this function |
| **Priority** | High / Medium / Low |
| **Trigger** | What event initiates this function |
| **Precondition** | What must be true for this function to be invoked |
| **Postcondition** | What must be true after this function completes |
| **Basic Flow** | The standard sequence of steps for this function |
| **Alternate Flows** | Alternative sequences of steps (if any) |
| **Exception Flows** | Sequences for handling errors or exceptional conditions |
| **Business Rules** | Constraints, validations, or decision logic |
| **Non-functional Requirements** | Performance, security, usability, or other non-functional criteria |
| **Source** | Which screen or flow this was derived from |
| **Related Screen(s)** | Which screen(s) this function is associated with |

**Writing rules:**
- Use active voice and present tense
- Use SHALL for mandatory behavior, SHOULD for recommended
- One behavior per FS entry — never combine two SHALLs in one entry
- Be explicit about basic flows, alternate flows, exception flows, business rules, and non-functional requirements — these are the key difference from SRS
- Every screen state (including error and empty states) must map to at least one FS entry
- Avoid implementation details (describe *what*, not *how*)

### Step 3b — Write Screen Descriptions

For **every distinct screen or UI state** identified in the design, produce a
Screen Description block in Section 4.2 using this format:

| Field | Content | 
|---|---|
| **Screen ID** | SCR-001 (sequential, unique) |
| **Screen Name** | Human-readable name (e.g., "Login Page", "Dashboard") |
| **Purpose** | One sentence describing what this screen is for |
| **Actors** | Who can access this screen |
| **Entry Points** | List of screens/events that lead here |
| **UI Components** | All visible elements: buttons, input fields, labels, icons, tables, modals |
| **Default State** | Layout and content on first load (before user interaction) |
| **States** | Loading / Empty / Populated / Error — describe each visible state |
| **Exit Points** | All navigation destinations reachable from this screen |
| **Linked FS IDs** | FS entries that describe behavior on this screen (e.g., FS-007, FS-008) |
| **Annotations** | Conditional visibility rules, tooltips, or design notes |

**Writing rules for Screen Descriptions:**
- Document every state the screen can be in — do not only describe the happy path
- UI Components must list every interactive element by name and type
- Entry Points and Exit Points create a complete navigation map of the product
- Linked FS IDs create a traceable bridge between screens and functional behavior

### Step 4 — Produce the Document

Generate a `.md` file with:
- Cover section: project name, version, date, authors (use placeholders if unknown)
- Table of contents
- All sections from the outline above
- Functional specification tables for every feature (Section 3)
- Screen description tables for every screen and UI state (Section 4.2)
- Screen inventory summary table in Appendix B listing each SCR ID, name, and linked FS IDs

Save the final file as `FRS_<ProjectName>_v1.0.md`.

To convert to Word format:
```bash
pandoc FRS_<ProjectName>_v1.0.md -o FRS_<ProjectName>_v1.0.docx
```

Version the document as `v0.1` if the design is clearly a draft/wireframe;
use `v1.0` only for high-fidelity or signed-off designs.

---

## Quality Checklist

Before delivering the document, verify:

- [ ] Every UI screen/state has at least one corresponding FS entry
- [ ] Every screen has a Screen Description block (SCR-XXX) in Section 4.2
- [ ] Every Screen Description documents all states: Loading, Empty, Populated, Error
- [ ] Every SCR entry lists its Linked FS IDs — no screen is left untraced
- [ ] Every FS entry explicitly defines Input, Processing, Output, and Error Handling
- [ ] All FS entries use "SHALL" language
- [ ] No two FS entries share the same ID
- [ ] All actors defined in Section 2 are referenced in Section 3
- [ ] Error states and edge cases from the design are documented
- [ ] Data validation rules in Section 5.2 are consistent with FS entries
- [ ] Assumptions section lists anything inferred but not shown in the design

---

## Key Difference: FRS vs SRS

| Aspect | SRS | FRS |
|---|---|---|
| Scope | Full system (functional + non-functional) | Functional behavior only |
| Detail level | High-level requirements | Detailed input/processing/output per function |
| Audience | All stakeholders | Developers, QA engineers |
| Non-functional reqs | Included | Excluded (separate NFR document) |
| Error handling | Minimal | Explicit per function |

---

## Example Screen Description Entry

> **SCR-003** — Login Page
>
> | Field | Content |
> |---|---|
> | **Screen ID** | SCR-003 |
> | **Screen Name** | Login Page |
> | **Purpose** | Allow registered users to authenticate and access the system |
> | **Actors** | Registered User |
> | **Entry Points** | App launch (unauthenticated), Logout action, Session expiry redirect |
> | **UI Components** | Email input field, Password input field, "Login" button, "Forgot Password" link, inline field error labels, loading spinner (on submit) |
> | **Default State** | Both fields empty, Login button enabled, no error messages visible |
> | **States** | **Loading**: spinner shown, button disabled. **Error**: inline error below relevant field. **Locked**: all fields disabled, lockout message displayed with countdown timer |
> | **Exit Points** | Dashboard (on success), Forgot Password page (on link click) |
> | **Linked FS IDs** | FS-007, FS-008, FS-009 |
> | **Annotations** | Password field has show/hide toggle. Email field auto-fills from last session if browser remembers credentials. |

---

## Example Functional Specification Entry

> **FS-007** — Login Form Validation
>
> | Field | Content |
> |---|---|
> | **ID** | FS-007 |
> | **Feature** | Authentication |
> | **Title** | Login Form Validation |
> | **Description** | The system SHALL validate user credentials before granting access. |
> | **Actor** | Registered User |
> | **Priority** | High |
> | **Trigger** | User clicks the "Login" button |
> | **Input** | Email (string), Password (string, min 8 chars) |
> | **Processing** | 1. Validate email format. 2. Check credentials against user store. 3. If valid, generate session token. |
> | **Output** | Redirect to Dashboard; session token stored in cookie (expires 24h) |
> | **Precondition** | User is on the Login Page with email and password fields filled |
> | **Postcondition** | User is either authenticated and redirected to Dashboard, or shown appropriate error messages |
> | **Basic Flow** | 1. User clicks "Login". 2. System validates email format. 3. System checks credentials against user store. 4a. If valid, generate session token, store in cookie, and redirect to Dashboard. 4b. If invalid, display error message without creating session. |
> | **Alternate Flows** | 1. If email format is invalid, show inline error "Please enter a valid email address". 2. If password is too short, show inline error "Password must be at least 8 characters". |
> | **Exception Flows** | 1. If there are 3 consecutive failed login attempts, lock the account for 15 minutes and show message "Account locked due to multiple failed attempts. Try again in 15 minutes." |
> | **Business Rules** | 1. Email must follow standard format. 2. Password must be at least 8 characters. 3. Session token expires after 24 hours of inactivity. |
> | **Non-functional Requirements** | 1. Authentication process must complete within 2 seconds under normal load. 2. System must prevent brute-force attacks by locking accounts after 3 failed attempts. 3. Error messages must not reveal whether the email or password was incorrect to avoid information leakage. |
> | **Source** | Screen: Login Page (Wireframe p.3) |
> | **Related Screen(s)** | Login Page (SCR-003) |

---

## Notes & Constraints

- If the design is for a **mobile app**, add platform-specific behavior notes (swipe gestures, back button behavior, keyboard handling) to each relevant FS entry.
- If the design references **third-party APIs** (payment, maps, auth), create stub FS entries under Section 6 and flag them for the user to define the contract.
- If the user provides **multiple design versions**, document each version's behavior separately and note discrepancies in the Assumptions section.
- If an SRS already exists for this project, cross-reference FS IDs to FR IDs from the SRS in an optional Appendix D.