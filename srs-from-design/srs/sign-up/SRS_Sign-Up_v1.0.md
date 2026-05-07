# Software Requirements Specification (SRS) - Hotel Sign-Up Process

**Version:** 1.0  
**Date:** May 3, 2026  
**Status:** Draft  

---

## 1. Introduction

### 1.1 Purpose
The purpose of this document is to define the functional and non-functional requirements for the multi-step sign-up process of the Hotel Management Application. This process allows hotel owners or managers to register their account and provide initial hotel and room configuration details.

### 1.2 Scope
This SRS covers the following stages of the sign-up flow:
1. User Account Registration.
2. Email Verification via OTP.
3. Hotel Profile Information (Name, Location, Rating, Media).
4. Room Type Configuration (Details, Amenities, Media).

### 1.3 Definitions, Acronyms, Abbreviations
- **OTP**: One-Time Password.
- **SRS**: Software Requirements Specification.
- **FR**: Functional Requirement.
- **NFR**: Non-Functional Requirement.

### 1.4 References
- Design Files: `step_1.png` through `step_7.png`.

---

## 2. Overall Description

### 2.1 Product Perspective
The sign-up process is the entry point for hotel partners into the management platform. It is designed as a guided, multi-step mobile interface to ensure data completeness.

### 2.2 Product Functions
- User registration via Email or Social Auth.
- Security verification via OTP.
- Onboarding of hotel property details.
- Definition of initial room inventory and amenities.

### 2.3 User Classes and Characteristics
- **Hotel Owner/Manager**: Non-technical or semi-technical users who need to list their property.

### 2.4 Operating Environment
- Mobile application (iOS/Android) or mobile-responsive web browser.

### 2.5 Design and Implementation Constraints
- The flow must be completed sequentially.
- Image uploads must meet specific resolution and size requirements.

---

## 3. System Features (Functional Requirements)

### 3.1 User Account Registration
| Field | Content |
|---|---|
| **ID** | FR-001 |
| **Title** | Manual Account Registration |
| **Description** | The system SHALL allow users to register by providing Full Name, E-mail, and Password. |
| **Priority** | High |
| **Source** | `step_1.png` |
| **Acceptance Criteria** | Successful submission redirects user to the OTP verification screen. |

| Field | Content |
|---|---|
| **ID** | FR-002 |
| **Title** | Social Authentication |
| **Description** | The system SHALL provide options to sign up using Google, Apple, or Facebook accounts. |
| **Priority** | Medium |
| **Source** | `step_1.png` |
| **Acceptance Criteria** | User can successfully authenticate via third-party providers. |

| Field | Content |
|---|---|
| **ID** | FR-003 |
| **Title** | Password Visibility Toggle |
| **Description** | The system SHALL allow users to toggle the visibility of the password during entry. |
| **Priority** | Low |
| **Source** | `step_1.png` |
| **Acceptance Criteria** | Clicking the eye icon masks/unmasks the password text. |

### 3.2 OTP Verification
| Field | Content |
|---|---|
| **ID** | FR-004 |
| **Title** | Email OTP Verification |
| **Description** | The system SHALL require a 4-digit OTP sent to the user's email for account activation. |
| **Priority** | High |
| **Source** | `step_2.png` |
| **Acceptance Criteria** | Entering the correct 4-digit code allows the user to proceed to Hotel Info registration. |

| Field | Content |
|---|---|
| **ID** | FR-005 |
| **Title** | Resend OTP |
| **Description** | The system SHALL provide a "Resend" option if the user does not receive the code. |
| **Priority** | Medium |
| **Source** | `step_2.png` |
| **Acceptance Criteria** | Clicking "Gửi lại" triggers a new OTP email. |

### 3.3 Hotel Information Registration
| Field | Content |
|---|---|
| **ID** | FR-006 |
| **Title** | Hotel Profile Details |
| **Description** | The system SHALL collect Hotel Name, Area (dropdown), Detailed Address, and Star Rating. |
| **Priority** | High |
| **Source** | `step_3.png`, `step_4.png` |
| **Acceptance Criteria** | Address field provides search suggestions as the user types. |

| Field | Content |
|---|---|
| **ID** | FR-007 |
| **Title** | Hotel Media and Description |
| **Description** | The system SHALL allow users to upload hotel images and provide a basic introduction. |
| **Priority** | High |
| **Source** | `step_5.png` |
| **Acceptance Criteria** | Image upload validates minimum width (1600px) and maximum size (10MB). |

### 3.4 Room Type Configuration
| Field | Content |
|---|---|
| **ID** | FR-008 |
| **Title** | Room Type Details |
| **Description** | The system SHALL collect Room Type Name, Quantity, Bed Type, Area (sqm), and Standard Capacity. |
| **Priority** | High |
| **Source** | `step_6.png` |
| **Acceptance Criteria** | Numeric fields (Quantity, Area, Capacity) only accept valid numeric input. |

| Field | Content |
|---|---|
| **ID** | FR-009 |
| **Title** | Room Amenities and Promotions |
| **Description** | The system SHALL allow users to select room amenities from a predefined list and apply promotions. |
| **Priority** | Medium |
| **Source** | `step_7.png` |
| **Acceptance Criteria** | Amenity icons change state when selected/deselected. |

---

## 4. External Interface Requirements

### 4.1 User Interfaces
- The interface SHALL follow the visual design provided in the mockups, using the primary green brand color for action buttons.
- Navigation SHALL support "Back" actions at each step.

---

## 5. Non-Functional Requirements

### 5.1 Security Requirements
| Field | Content |
|---|---|
| **ID** | NFR-001 |
| **Title** | Data Transmission Security |
| **Description** | All data transmitted during the sign-up process SHALL be encrypted via HTTPS. |
| **Priority** | High |
| **Acceptance Criteria** | SSL certificate is valid and active. |

### 5.2 Usability Requirements
| Field | Content |
|---|---|
| **ID** | NFR-002 |
| **Title** | Multi-step Progress Persistence |
| **Description** | The system SHOULD save the user's progress if they navigate away from the sign-up flow. |
| **Priority** | Medium |
| **Acceptance Criteria** | Returning users start at the last completed step. |

---

## Appendix A: Glossary
- **Đăng ký**: Sign up / Register.
- **Tiếp tục**: Continue.
- **Khu vực**: Area / Region.
- **Hạng khách sạn**: Hotel star rating.
- **Tiện ích**: Amenities.
