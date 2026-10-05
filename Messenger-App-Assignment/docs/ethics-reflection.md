# Ethics and Social Impact Reflection: Messenger System

## 1. Design Impact Analysis

| Dimension | Positive Impacts | Negative Impacts | Mitigations / Impact Justification |
| :--- | :--- | :--- | :--- |
| **1. Cultural** | Enhances communication across diverse groups [32]. | Cultural misunderstandings in unmoderated group chats [32]. | Provide group administrator moderation tools [32, 33]. |
| **2. Economic** | Free messaging reduces communication expense [32]. | Minimal macroeconomic impact. | *Justification:* Academic scope limits direct market disruption [33]. |
| **3. Environmental** | Replaces paper-based written communication [32]. | Energy consumption from server infrastructure [32]. | Optimize payload size and indexing to reduce CPU cycles [33]. |
| **4. Global** | Supports cross-border real-time collaboration [32]. | Latency variation across global networks [32]. | Implement lightweight message caching [33]. |
| **5. Public Health** | Facilitates remote peer support channels [32]. | Screen fatigue and late-night notification disruption [32]. | Implement customizable "Do Not Disturb" quiet hours [33]. |
| **6. Public Safety** | Enables rapid group emergency alerting [32]. | Misuse for coordinated harassment or illicit activities [32]. | Build blocking features and message reporting tools [27, 33]. |
| **7. Public Welfare** | Connects educational communities [32]. | Digital divide excluding users with low bandwidth [32]. | Keep baseline protocol bandwidth minimal [33]. |
| **8. Social** | Strengthens interpersonal relationships [32]. | Risk of toxic behavior or harassment in direct messages [32]. | Build user-level blocklists and privacy controls [27, 33]. |

## 2. Connection to Engineering

### Negative Impact 1: User Harassment via Direct Messages

*   **Affected Parties:** Registered Messenger users [27].
*   **Potential Harm:** Emotional distress resulting from unwanted or toxic direct messaging [27].
*   **Proposed Mitigation:** Implement blocklists enabling users to prevent messages from specified accounts [27].
*   **Engineering Influence:** Directly introduces requirement `FR-06 (User Blocking)` and modifies user relationship tables in the database schema [27].
*   **Requirement Tracing:** Traced to `FR-06` and updated in `docs/requirements.md` [27].

### Negative Impact 2: Account Hijacking &amp; Unencrypted Interception

*   **Affected Parties:** All communication participants [27].
*   **Potential Harm:** Exposure of confidential user conversations and account credentials [27].
*   **Proposed Mitigation:** Mandate TLS for data in transit and salted hashing for credentials at rest [27].
*   **Engineering Influence:** Shapes non-functional requirement `NFR-03` and enforces secure storage configurations [27].
*   **Requirement Tracing:** Traced to `NFR-03` in `docs/requirements.md` [27].

## AI-Assisted Work

[Document any AI tool usage (e.g., ChatGPT, Claude, Gemini, Copilot) across the assignment sequence [34]. Identify the assistance received, give a concrete example of an output requiring human verification/correction, and explain your review process [4, 34]. If no AI tools were used, state so explicitly and describe the human review process used] [4].