# Ethics Reflection: Messenger System

**Project:** CPS 490 Capstone I — Messenger System  
**Author:** Lucas Swearingen  
**Faculty Mentor:** Nick Stiffler  
**Target Submission Path:** `docs/ethics-reflection.md`  
**Assignment Due Date:** October 12, 2026  
**Final System Delivery Deadline:** November 2, 2026  

## 1. Design Impact Analysis

### 1.1 Cultural Impact

* **Positive Impacts:** Enables inclusive, structured peer-to-peer and group collaboration in academic and team settings. The lightweight text interface removes visual clutter, fostering focused discussion around shared goals.
* **Negative Impacts:** Unmoderated group communication can host toxic discourse, exclusionary language, or cultural misunderstandings without automated content filters.
* **Mitigation Strategy:** Provide clear administrative group controls (`FR-05`, `FR-06`) allowing Group Creators to manage membership rosters and remove abusive accounts from private group channels.

### 1.2 Economic Impact

* **Positive Impacts:** Implements zero external paid cloud API dependencies (`NFR-04`), providing a completely free, open-source communication tool for students and small institutions without subscription overhead.
* **Negative Impacts:** Minimal economic impact. However, inefficient server resource utilization could impose host server operational costs on academic infrastructure.
* **Impact Justification &amp; Mitigation:** *Justified Low Impact.* The application's scope is strictly bounded to a 50-user capacity per group (`ASM-01`) and small-to-medium deployment scale.

### 1.3 Environmental Impact

* **Positive Impacts:** Bounded exclusively to plain-text communication, consuming less network bandwidth and data center energy than video/audio streaming platforms.
* **Negative Impacts:** Accumulation of unconstrained message histories in persistent local disk storage (`DS-3`) creates unnecessary disk consumption and host CPU overhead during database queries.
* **Mitigation Strategy:** Address storage bounds by establishing an offline message queue retention limit and efficient relational database indexing to minimize CPU cycles during history retrieval (`FR-08`).

### 1.4 Global Impact

* **Positive Impacts:** Adopts standard text encoding, enabling cross-border internationalization and multi-language character set communication.
* **Negative Impacts:** High network latency variations for overseas participants if hosted on a single centralized host server without distributed Content Delivery Networks (CDNs).
* **Impact Justification &amp; Mitigation:** *Justified Low Impact.* Messenger is explicitly defined as a local desktop/intranet system designed for academic capstone deployment rather than a globally distributed cloud service.

### 1.5 Public Health Impact

* **Positive Impacts:** Supports remote academic collaboration and peer support networks, reducing social isolation during remote learning periods.
* **Negative Impacts:** Continuous message notifications can contribute to screen fatigue, stress, and late-night sleep disruption.
* **Mitigation Strategy:** Implement clean session logout capabilities (`FR-03`), allowing users to terminate active socket connections and disconnect from incoming real-time notifications at will.

### 1.6 Public Safety Impact

* **Positive Impacts:** Enables rapid emergency group broadcasting (&lt;1 second delivery under `NFR-01`) for swift information transmission among registered project team members.
* **Negative Impacts:** Unencrypted network transport on public Wi-Fi networks exposes private conversations or user credentials to malicious packet sniffing and eavesdropping.
* **Mitigation Strategy:** Enforce mandatory Transport Layer Security (TLS/SSL encryption under `NFR-03`) across all client-broker communications to preserve data confidentiality.

### 1.7 Public Welfare Impact

* **Positive Impacts:** Standardizes access to functional communication software by running efficiently on low-spec hardware without high-bandwidth requirements or paid licenses (`NFR-04`).
* **Negative Impacts:** Account hijacking or credential exposure resulting from plaintext credential storage in server databases, compromising user digital welfare.
* **Mitigation Strategy:** Mandate salted password hashing (`NFR-05`) and unique username constraints (`FR-01`) to prevent credential theft and unauthorized impersonation.

### 1.8 Social Impact

* **Positive Impacts:** Strengthens interpersonal connections and team cohesion among student groups through direct 1-on-1 private messaging (`FR-04`) and organized group channels (`FR-05`).
* **Negative Impacts:** Potential for targeted toxic direct messaging or harassment between individuals in private 1-on-1 threads.
* **Mitigation Strategy &amp; Tradeoff Analysis:** *Analyzed Scope Tradeoff.* To avoid scope creep and preserve a defensible core capstone application (`ia-02`), complex server moderation trees were excluded. Social risk is managed by maintaining strict account authentication (`FR-02`), user session termination (`FR-03`), and Group Creator roster authority (`FR-06`).

## 2. Connection to Engineering

### 2.1 Ethical Risk 1: Unencrypted Network Interception &amp; Credential Theft

* **Affected Parties:** All registered Messenger users transmitting credentials or private messages over shared or public network infrastructure (e.g., campus Wi-Fi).
* **Potential Harm:** An attacker eavesdropping on unencrypted TCP socket traffic could capture plaintext user credentials during login, hijack active user sessions, or read confidential private direct messages, violating user privacy and public safety.
* **Engineering Mitigation:**  
  1. Enforce mandatory **Transport Layer Security (TLS/SSL)** for all socket and HTTP traffic between messaging clients and the messaging broker.
  2. Implement salted cryptographic password hashing for user credentials in the database.
* **Engineering Influence:** Directly answers non-functional requirements **`NFR-03` (Transport Layer Security)** and **`NFR-05` (Credential Security)** in `docs/requirements.md`.
* **Verification Method:** Verified during testing via Wireshark and database table inspection (confirming zero plaintext passwords stored).

### 2.2 Ethical Risk 2: Unauthorized Message Injection &amp; Unmoderated Group Spooling

* **Affected Parties:** Members of private group communication channels.
* **Potential Harm:** Unverified or non-member accounts injecting unauthorized, deceptive, or toxic messages into private group broadcasts, eroding trust, disrupting team coordination, and causing group harassment.
* **Engineering Mitigation:** Mandate strict server-side group membership verification within Process `4.0 Broadcast Group Message` (`docs/diagrams/data-flow.puml`), querying `DS-2: Group Roster Store` to validate sender authorization *before* broadcasting payloads.
* **Engineering Influence:** Directly completes functional requirements **`FR-06` (Group Membership Management)** and **`FR-07` (Group Message Broadcasting)** in `docs/requirements.md`.
* **Verification Method:** Verified via multi-client integration testing, proving that a non-member client (`"eve"`) attempting to post to a private group (`"CS Capstone"`) receives an authorization rejection and zero messages reach group members.

## AI-Assisted Work

* **Assistance Received:** Gemini Notebook was utilized as an engineering tool to format Markdown document templates, organize traceability matrices across all artifacts, and structure PlantUML syntax for system diagrams.
* **Human Review &amp; Revision Example:** During initial brainstorming, Gemini suggested adding commercial features such as multi-tier server hierarchies, and user account blocking tables. I explicitly rejected these suggestions because they violated the core scope boundaries which focuses on lightweight text messaging without commercial bloat. 
* **Verification Process:** All generated requirement identifiers (`FR-01` through `FR-08`, `NFR-01` through `NFR-05`) and acceptance criteria were verified directly against my source files (`docs/requirements.md` and `docs/analysis.md`). I confirmed that every claim in these documents accurately reflect my personal system design vision. I filtered and omitted unnecessary language and AI fluff.