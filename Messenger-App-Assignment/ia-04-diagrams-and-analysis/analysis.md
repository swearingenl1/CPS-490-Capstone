# System Diagrams and Engineering Analysis: Messenger System

## 1. Data Flow Diagram (DFD) Analysis

*   **Diagram Source Path:** `docs/diagrams/data-flow.puml` [3].
*   **Engineering Question Answered:** *Where does information come from, where does it go, what transforms it, and what information must persist?* [28, 29]

### Structural &amp; Behavioral Insights

1.  **Associated Requirements:** `FR-01`, `FR-02`, `FR-03`, `FR-04`, `NFR-02` [29, 30].
2.  **Design Responsibilities &amp; Flows:** Traces how unvalidated user inputs pass through validation processes, transform into structured payload entities, and persist in data stores (`DS-1: Message Store`) prior to distribution [28, 29].
3.  **Discovered Ambiguities / Missing Requirements:** Construction of the DFD revealed an unstated data store cleanup requirement for orphaned group messages. Added `FR-05` to specify message retention/deletion behavior [3, 30].

---

## 2. Sequence Diagram Analysis

*   **Diagram Source Path:** `docs/diagrams/sequence.puml` [3].
*   **Engineering Question Answered:** *Who communicates with whom, in what order, and what happens when the scenario does not follow only the ideal path?* [29, 31]

### Structural &amp; Behavioral Insights

1.  **Associated Requirements:** `FR-02`, `FR-03`, `NFR-01` [29, 30].
2.  **Design Responsibilities &amp; Interaction Sequences:** Models the asynchronous handshake between Client A, API Messaging Broker, and Client B, exposing explicit queue mechanisms when Client B is offline [29, 31].
3.  **Discovered Ambiguities / Alternative Paths:** Highlighted potential concurrency issues when a user is removed from a group while actively dispatching a message. Defined alternative failure response frames for un-authorization errors [30, 31].