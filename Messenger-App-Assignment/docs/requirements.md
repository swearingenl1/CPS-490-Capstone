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
    * Data Persistence Subsystem: The persistent data store containing user account records, salted password hashes, group membership rosters, and chronological chat message logs.

*   **Actors and External Systems:**

| Actor / External Entity | Category | Description & System Interaction Boundary |
| :--- | :--- | :--- |
| **Registered User** | Human Actor | Primary operator who registers an account, authenticates, sends/receives direct text messages, creates/joins group channels, and views message history. |
| **Group Creator / Admin** | Human Actor | A registered user who creates a group messaging channel, manages member invitations/removals, and holds channel administration privileges. |
| **Data Store** | Internal Subsystem | Storage layer that maintains records of credentials, rosters, and message archives across system restarts. |

## 2. Functional Requirements

*   **FR-01 (Account Registration):** The system shall enable new users to register a user account by providing a unique username and a secure password.
*   **FR-02 (User Authentication &amp; Session Management):** The system shall authenticate returning users by verifying submitted passwords against stored salted password hashes prior to granting access to messaging functions.
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
*   **NFR-05 (Credential Security):** User passwords shall never be stored or logged in plain text. Credentials at rest must be protected using a salted cryptographic hashing algorithm.

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
    **When:** `"alice"` posts `"Code due tonight"` to `"CS Capstone"`.
    **Then:** Both `"bob"` and `"charlie"` receive the message in their group feeds, while `"eve"` receives zero notifications and cannot view the message.

**Message Latency &amp; Message Store:**
    **Given:** Authenticated users `"alice"` and `"bob"` have an active session.
    **When:** `"alice"` sends direct message `"Meeting at 4PM"` to `"bob"`.
    **Then:** The message appears in `"bob"`'s client feed with `"alice"`'s ID within 1 second, and is logged to persistent storage.

## 5. Assumptions and Unresolved Questions

**Assumptions:**
    * **Internal Authentication:** User identities and passwords are managed internally in the local database without external campus log in integration.
    * **Network Connectivity:** End users possess steady network connectivity (TCP/IP socket connection) during active messaging sessions.
**Unresolved Stakeholder Questions:**
    * **Maximum Group Capacity:** What is the exact maximum permitted member capacity per group channel (currently assumed at 100 users)?
    * **Group Creator Departure Policy:** If a Group Creator deletes their account or leaves a group channel, should channel administration transfer automatically to the oldest remaining member, or should the channel lock into read-only mode?
    * **Offline Message Expiration Window:** Should unread offline messages persist indefinitely in the database, or should unretrieved messages expire after a fixed retention window (e.g., 30 days)?
**Ambiguities / Conflicts:** 
    * **Disk Storage Constraints:** Retaining unlimited message history for users will conflict with local database storage constraints on host servers.

## 6. Traceability

| Requirement ID | Requirement Name | Primary Source / Rationale | Target System Subsystem | Verification Method | 
| :--- | :--- | :--- | :--- | :--- | 
| **FR-01** | Account Registration | SOW `OBJ-01` (Secure Identity) &amp; In-Scope Feature 1 | Authentication Engine | Automated Unit Test &amp; Database Audit | 
| **FR-02** | User Authentication &amp; Session Management | SOW `OBJ-01` &amp; In-Scope Feature 1 | Authentication Engine | Automated Authentication Test | 
| **FR-03** | Session Logout | User Security / Session Boundary | Client Session Manager | Integration Test | 
| **FR-04** | Direct Message Transmission | SOW `OBJ-02` (Direct Private Messaging) &amp; In-Scope Feature 2 | Direct Messaging Engine | Automated End-to-End Test | 
| **FR-05** | Group Channel Creation | SOW `OBJ-03` (Group-Based Messaging Channels) &amp; In-Scope Feature 3 | Group Channel Subsystem | Automated Unit Test | 
| **FR-06** | Group Membership Management | SOW `OBJ-03` &amp; `ASM-01` (50-User Capacity Cap) | Group Channel Subsystem | Roster Boundary Test (51st User) | 
| **FR-07** | Group Message Broadcasting | SOW `OBJ-03` &amp; In-Scope Feature 3 | Group Channel Subsystem | Multi-Client Integration Test | 
| **FR-08** | Durable Storage Persistence | SOW `OBJ-04` (Message Persistence &amp; History) &amp; In-Scope Feature 4 | Persistent Data Store | System Reboot Recovery Test | 
| **NFR-01** | Delivery Latency (&lt;1s) | SOW Acceptance Criteria `AC-02` / Performance Quality | Network Broker / Transport | Automated Latency Benchmark (&lt;1s) | 
| **NFR-02** | Data Persistence &amp; Recovery Integrity | SOW Reliability Requirement &amp; Acceptance Criteria `AC-04` | Persistent Data Store | Hard Crash &amp; Reboot Recovery Audit | 
| **NFR-03** | Transport Layer Security | SOW Security &amp; System Context Boundary | Network Protocol Layer | Wireshark / Packet Inspection Audit | 
| **NFR-04** | Zero External Paid Service Dependency | Course Constraint / No Paid Services Requirement | System Dependencies | Open-Source License &amp; Dependency Audit |
| **NFR-05** | Credential Security | SOW Security &amp; Ethics | Credential Hashing Module | Hash Inspection &amp; DB Audit |