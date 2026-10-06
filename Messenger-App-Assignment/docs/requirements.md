# Requirements Engineering Specification: Messenger System

**Project:** CPS 490 Capstone I — Messenger  
**Author:** Lucas Swearingen
**Faculty Mentor:** Nick Stiffler  
**Target Submission Path:** `docs/requirements.md`  
**Assignment Due Date:** October 12, 2026  
**Final System Delivery Deadline:** November 2, 2026

## 1. System Context

*   **System Boundary:** The Messenger system is a lightweight, secure text communication system that enables authenticated direct and multi-user group messaging. The system boundary encompasses:
    * User Client Interface: The application interface through which end-users write, send, and view messages.
    * Data Persistence Subsystem: The persistent data store containing user account records, passwords, group membership rosters, and chronological chat message logs.

*   **Actors and External Systems:**

| Actor / External Entity | Category | Description & System Interaction Boundary |
| :--- | :--- | :--- |
| **Registered User** | Human Actor | Primary operator who registers an account, authenticates, sends/receives direct text messages, creates/joins group channels, and views message history. |
| **Group Creator / Admin** | Human Actor | A registered user who creates a group messaging channel, manages member invitations/removals, and holds channel administration privileges. |
| **Data Store** | Internal Subsystem | Storage layer that maintains records of credentials, rosters, and message archives across system restarts. |

## 2. Functional Requirements

*   **FR-01 (User Registration):** The system shall enable new users to create an account with a unique identifier [24].
*   **FR-02 (Direct Messaging):** The system shall allow a registered user to transmit a direct text message to another registered user [24, 25].
*   **FR-03 (Group Messaging):** The system shall allow registered users to create messaging groups and send messages to all group members [24, 25].
*   **FR-04 (Message Retrieval):** The system shall allow users to retrieve historical direct and group messages [24].

## 3. Non-Functional Requirements

*   **NFR-01 (Delivery Latency):** The system shall deliver direct messages to online recipients within 500ms under normal operating conditions [24].
*   **NFR-02 (Data Persistence):** User account records and chat histories shall persist across application restarts [24].
*   **NFR-03 (Credential Security):** The system shall store user credentials using salted password hashing [24].

## 4. Acceptance Criteria

| Requirement ID | Acceptance Criterion | Verification Method |
| :--- | :--- | :--- |
| **FR-02** | User A sends "Hello" to User B. User B's interface displays "Hello" with sender ID and timestamp within 500ms. | Automated Test / Demonstration [24, 26] |
| **FR-03** | User A posts to Group X. All active members in Group X receive the message in their group feed. | Integration Test / Demonstration [24, 26] |
| **NFR-03** | Inspection of the database confirms zero plaintext passwords exist. | Security Audit / Code Review [24, 26] |

## 5. Assumptions and Unresolved Questions

*   **Assumptions:** [e.g., Users possess steady network connectivity during active sessions] [26].
*   **Unresolved Stakeholder Questions:** [e.g., What is the maximum permitted member capacity per group?] [26].
*   **Ambiguities / Conflicts:** [e.g., Balancing offline message retention limits against storage constraints] [26].

## 6. Traceability

| Requirement ID | Source / Rationale | Target Subsystem / Component |
| :--- | :--- | :--- |
| **FR-01** | User Access Control Objective | Authentication Module [2] |
| **FR-02** | Statement of Work In-Scope Capability [12] | Private Messaging Engine [2] |
| **FR-03** | Statement of Work In-Scope Capability [12] | Group Channel Module [2] |
| **NFR-03** | Security Constraint &amp; Ethical Risk Mitigation [27] | Auth &amp; Data Persistence Layer [2] |