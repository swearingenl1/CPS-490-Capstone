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

*   **FR-01 (Account Registration):** The system shall enable new users to register a user account by providing a unique username and a secure password.
*   **FR-02 (User Authentication &amp; Session Management):** The system shall authenticate returning users by verifying submitted passwords against stored passwords prior to granting access to messaging functions.
*   **FR-03 (Session Logout):** The system shall allow an authenticated user to terminate their active session.
*   **FR-04 (Direct Message Transmission):** The system shall allow an authenticated user to transmit a direct message to another designated registered user.
*   **FR-06 (Group Channel Creation):** The system shall allow an authenticated user to create a named group messaging channel and automatically assign that user as the Group Creator.
*   **FR-06 (Group Membership Management):** The system shall allow the Group Creator to add or remove registered users from the group channel roster.
*   **FR-07 (Group Message Broadcasting):** The system shall distribute messages posted to a group channel to all current active members of that group.
*   **FR-08 (Durable Storage Persistence):** The system shall commit all user records, group rosters, and transmitted messages to persistent disk storage immediately upon transaction confirmation.

## 3. Non-Functional Requirements

*   **NFR-01 (Delivery Latency):** Under normal operating conditions, the system shall deliver direct and group messages to online recipients within **1 second** of sending.
*   **NFR-02 (Data Persistence &amp; Recovery Integrity):** All registered user account profiles, group membership rosters, and chat logs shall persist across system reboots with **0% message loss or data corruption**.
*   **NFR-03 (Transport Layer Security):** All network communication between messaging clients and the messaging broker shall utilize transport layer encryption (TLS / SSL) to prevent plaintext packet sniffing.
*   **NFR-04 (Zero External Paid Service Dependency):** The system implementation shall rely entirely on open-source libraries and local infrastructure without requiring paid subscriptions or third-party proprietary services.

## 4. Acceptance Criteria

**User Registration:**
    Given: Username `"alice"` does not exist in the user database.
    When: A user submits a registration request with username `"alice"` and password `"SecurePass123!"`.
    Then: The system creates the account, stores the password in the database, returns a `SUCCESS` status, and allows `"Alice"` to authenticate.

**Duplicate User Prevention:**
    Given: Username `"alice"` is already registered in the system database.
    When: A new user attempts to register with username `"alice"`.
    Then: The system rejects the registration, returns a `DUPLICATE_USER` error, and does not create a database record.

**Group Channel Creation &amp; Message Transmission:**
    *Given:** `"alice"` creates a group, `"CS Capstone"`, with members `"bob"`, and `"charlie"`. User `"eve"` is not a member.
    **When:** `"alice"` posts `"Code freeze tonight"` to `"CS Capstone"`.
    **Then:** Both `"bob"` and `"charlie"` receive the message in their group feeds, while `"eve"` receives zero notifications and cannot view the message.

**Message Latency &amp; Message Store:**
    **Given:** Authenticated users `"alice"` and `"bob"` have an active session.
    **When:** `"alice"` sends direct message `"Meeting at 4PM"` to `"bob"`.
    **Then:** The message appears in `"bob"`'s client feed with `"alice"`'s ID within 1 second, and is logged to persistent storage.

## 5. Assumptions and Unresolved Questions

    **Assumptions:** [e.g., Users possess steady network connectivity during active sessions] [26].
        * 
    **Unresolved Stakeholder Questions:** [e.g., What is the maximum permitted member capacity per group?] [26].
    **Ambiguities / Conflicts:** [e.g., Balancing offline message retention limits against storage constraints] [26].

## 6. Traceability

| Requirement ID | Source / Rationale | Target Subsystem / Component |
| :--- | :--- | :--- |
| **FR-01** | User Access Control Objective | Authentication Module [2] |
| **FR-02** | Statement of Work In-Scope Capability [12] | Private Messaging Engine [2] |
| **FR-03** | Statement of Work In-Scope Capability [12] | Group Channel Module [2] |
| **NFR-03** | Security Constraint &amp; Ethical Risk Mitigation [27] | Auth &amp; Data Persistence Layer [2] |