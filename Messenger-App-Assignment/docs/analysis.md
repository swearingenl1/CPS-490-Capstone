# Analysis: Messenger System

**Project:** CPS 490 Capstone I — Messenger System  
**Author:** Lucas Swearingen  
**Faculty Mentor:** Nick Stiffler  
**Target Submission Path:** `docs/analysis.md`  
**Assignment Due Date:** October 12, 2026  
**Final System Delivery Deadline:** November 2, 2026  

## 1. Data Flow Diagram (DFD) Analysis

* **Source File Path:** `docs/diagrams/data-flow.puml`
* **Target Engineering Question:** *Where does information come from, where does it go, what transforms it, and what information must persist?*

### 1.1 Represented Requirements

The Data Flow Diagram directly models the information transformations and data stores supporting the following system requirements:

* **`FR-01` (Account Registration):** Input of user registration credentials from the `Registered User` external entity into Process `1.0 Authenticate Session & Access Control`.
* **`FR-02` (User Authentication &amp; Session Management):** Verification of submitted username and password against `DS-1: User Credential Store`.
* **`FR-04` (Direct Message Transmission) &amp; `FR-05` (Message Delivery &amp; Storage):** Routing 1-on-1 text payloads through Process `2.0 Process &amp; Route Direct Message` into `DS-3: Persistent Message Store` and dispatching to recipient clients.
* **`FR-06` (Group Channel Creation &amp; Roster Management):** Processing channel creation requests and roster modifications through Process `3.0 Manage Group Channel Roster`, writing membership records to `DS-2: Group Roster Store`.
* **`FR-07` (Group Message Broadcasting):** Reading group membership lists from `DS-2` and broadcasting message payloads to active members via Process `4.0 Broadcast Group Message`.
* **`FR-08` (Durable Storage Persistence) &amp; `NFR-02` (Persistence Integrity):** Committing all account records, group rosters, and chat logs to persistent disk stores (`DS-1`, `DS-2`, `DS-3`).

### 1.2 Design Responsibilities &amp; Subsystem Boundaries

1. **Separation of Authentication from Message Routing:** Constructing the DFD made visible the need to separate user session authentication (Process `1.0`) from messaging engines (Processes `2.0` and `3.0`). Unauthenticated data flows are rejected at Process `1.0` before reaching message routing logic.
2. **Dedicated Persistence Stores:** Rather than treating the database as a single block, the DFD explicitly separates persistence into three logical stores:  
  * `DS-1: User Credential Store` (User ID, username, password hash credentials).
  * `DS-2: Group Roster Store` (Group ID, Group Creator ID, member roster list).
  * `DS-3: Persistent Message Store` (Message ID, sender ID, recipient/group ID, payload, timestamp).
3. **Group Membership Verification Boundary:** Process `4.0` must query `DS-2: Group Roster Store` to verify sender membership *before* accepting a group message payload for broadcast, ensuring non-members cannot inject messages into private channels.

### 1.3 Discovered Ambiguities &amp; Missing Requirements

* **Discovery (Unbounded Message History vs. Disk Capacity):** Modeling the data flow into `DS-3: Persistent Message Store` highlighted an unconstrained data flow. Inactive offline recipients will cause undelivered and historical messages to accumulate indefinitely in `DS-3`, creating a potential storage bottleneck on local server disks.
* **Engineering Resolution:** Identified the need to establish an offline queue retention limit or FIFO message pruning policy.

---

## 2. Sequence Diagram Analysis

* **Source File Path:** `docs/diagrams/sequence.puml`
* **Scenario Modeled:** **Direct 1-on-1 Message Dispatch &amp; Delivery**
* **Target Engineering Question:** *Who communicates with whom, in what order, and what happens when the scenario does not follow only the ideal path?*

### 2.1 Represented Requirements

The Sequence Diagram models real-time interaction over time across `Sender Client (User A)`, `Messaging Broker API`, `Session &amp; Auth Manager`, `Persistent Data Store`, and `Recipient Client (User B)`, tracing:

* **`FR-02` (Session Verification):** Validating active session token prior to accepting message payload.
* **`FR-04` (Direct Message Transmission):** Processing 1-on-1 text payload dispatch.
* **`FR-05` (Message Delivery &amp; Persistence):** Handling dual-path message delivery (real-time push to online recipients vs. persistent storage queuing for offline recipients).
* **`FR-08` (Durable Storage Persistence):** Synchronous write to persistent database.
* **`NFR-01` (Delivery Latency):** Enforcing the 1-second real-time delivery benchmark for online recipients.

### 2.2 Key Subsystem Interactions &amp; Alternative Paths

The sequence model incorporates explicit combined fragments (`alt` / `else`) to govern alternative operational paths:

1. **Main Success Path (Online Recipient):**  
  * `Sender Client` transmits `sendDirectMessage(recipientID, payload)`.
  * `Messaging Broker API` verifies active sender session with `Session Manager`.
  * Broker writes message record to `Persistent Data Store` (`FR-08`).
  * Broker pushes real-time socket payload to `Recipient Client (User B)` within 1 second (`NFR-01`).
  * Broker returns `200 OK (Message Sent)` confirmation to Sender.
2. **Alternative Path (Offline Recipient):**  
  * If `Recipient Client` is offline/disconnected, `Messaging Broker API` catches the socket delivery timeout.
  * Message remains persisted in `Persistent Data Store` marked as `UNREAD`.
  * When Recipient authenticates (`FR-02`), the client issues `fetchUnreadMessages()`, and the Broker returns historical unread logs in chronological sequence (`OBJ-04`).
3. **Failure Path (Non-Existent Recipient):**  
  * If `recipientID` is not found in `User Credential Store`, `Messaging Broker API` rejects the transaction, returns `404 RECIPIENT_NOT_FOUND`, and halts database persistence.

### 2.3 Discovered Ambiguities &amp; Missing Requirements

* **Discovery (Socket Disconnect &amp; Duplicate Delivery Risk):** Modeling real-time socket delivery revealed a race condition: if a recipient disconnects immediately after receiving a message but before sending an ACK frame back to the Broker, the Broker might incorrectly assume delivery failed and re-queue the message, resulting in duplicate client displays upon reconnect.

---

## Consistency and Traceability

| Requirement / Decision | Statement of Work (`statement-of-work.md`) | Requirements Spec (`requirements.md`) | Visual Diagram (`.puml`) | 
| :--- | :--- | :--- | :--- | 
| **User Identity &amp; Authentication** | `OBJ-01` / In-Scope Feature 1 | `FR-01`, `FR-02`, `FR-03` | DFD Process `1.0` &amp; Data Store `DS-1` | 
| **Direct 1-on-1 Messaging** | `OBJ-02` / In-Scope Feature 2 | `FR-04` | DFD Process `2.0` &amp; Sequence Diagram | 
| **Group Channels (50 Cap)** | `OBJ-03` / `ASM-01` (50 Users) | `FR-05`, `FR-06`, `FR-07` | DFD Processes `3.0`, `4.0` &amp; Data Store `DS-2` | 
| **Message Persistence &amp; History** | `OBJ-04` / In-Scope Feature 4 | `FR-08`, `NFR-02` | DFD Data Store `DS-3` &amp; Sequence DB Writes | 
| **1-Second Delivery Latency** | Acceptance Criteria `AC-02` | `NFR-01` | Sequence Diagram Latency Constraint (&lt;1s) | 
| **Transport Layer Security** | Security &amp; Context Boundary | `NFR-03` | Network Transport Layer Protocol | 
| **Zero Paid External Dependencies** | Course Constraint | `NFR-04` | System Configuration &amp; Build Scripts |