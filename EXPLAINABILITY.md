# EXPLAINABILITY — CowAgent Harness

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* CowAgent Harness (`cow-agent-harness`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Autonomous Multi-Agent Harness & Proactive Task Runtime  

---

## 1. Overview & Operational Purpose

The **CowAgent Harness** (`cow-agent-harness`) is an autonomous personal AI assistant and Agent Harness reference implementation. Built to operate continuously 24/7 on local machines or cloud servers, CowAgent combines autonomous task planning, a three-tier memory architecture (Context → Daily → Core) with Deep Dream distillation, an evolving Markdown knowledge wiki and graph, custom skill creation, and multi-channel messaging gateway connectivity (WeChat, Feishu, DingTalk, WeCom, Telegram, Slack, and Web).

By integrating deterministic tool calling with proactive self-evolution, CowAgent autonomously accomplishes open-ended workflows while keeping user data strictly local, sovereign, and auditable.

---

## 2. How the Agent Decides (Decision-Making Logic)

CowAgent Harness operates across a deterministic, multi-stage decision pipeline:

```
[Inbound Multi-Channel Message] ──> [Task Planning & Intent Decomp] ──> [Three-Tier Memory Query]
                                                                                  │
                                                                                  ▼
[Outbound Channel Delivery] <── [State & Wiki Commit] <── [Tool Exec & Confirmation]
```

### 2.1 Proactive Task Planning & Autonomous Looping
- **Decision:** Determines whether a user request requires single-turn execution or multi-step iterative planning with tool loops.
- **Rules:**
  - Evaluates user goal complexity; generates a structured step-by-step execution plan for multi-action tasks.
  - Loops over available tools (terminal, file I/O, browser automation, web search) until the termination condition is satisfied.
  - Re-evaluates plan status after each tool execution; dynamically adjusts remaining steps upon encountering errors.

### 2.2 Three-Tier Memory Distillation & Retrieval
- **Decision:** Decides which memories to retain, promote, or query during conversation turns.
- **Rules:**
  - Buffers short-term conversation turns in active working memory.
  - Summarizes day-end interactions into daily memory logs and triggers Deep Dream distillation to extract permanent core facts.
  - Executes hybrid semantic vector similarity and keyword search across core memory to inject relevant historical context into prompts.

### 2.3 Knowledge Wiki Ingestion & Entity Graph Linking
- **Decision:** Evaluates incoming documents, URLs, and conversation insights to update the structured knowledge wiki.
- **Rules:**
  - Analyzes content structure and assigns files to appropriate subdirectories under `knowledge/`.
  - Extracts key entities, relationships, and metadata tags to update the global knowledge graph index.
  - Automatically updates `knowledge/index.md` to preserve navigation hierarchy.

### 2.4 Multi-Channel Message Routing & Format Adaptation
- **Decision:** Selects appropriate message formatting, media transcoding, and delivery protocol based on the originating channel.
- **Rules:**
  - Formats rich text, Markdown, or card layouts according to target channel capabilities (Feishu cards vs. WeChat text vs. Telegram markdown).
  - Transcodes audio voice messages via speech-to-text (STT) and text-to-speech (TTS) engines when voice channels are engaged.
  - Enforces channel-specific permission boundaries to prevent unauthorized users in group chats from invoking administrative commands.

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
| :--- | :--- | :--- | :--- |
| User Inquiries & Commands | IM channels (WeChat, Feishu, Slack) / Web UI | Captures user requests, tasks, and conversation turns | Ephemeral processing, stored in local database |
| Three-Tier Memory Logs | Local SQLite / vector storage (`memory/`) | Supplies personalized context and historical recall | Encrypted at rest, never transmitted externally |
| Knowledge Base Files | Markdown documents in `knowledge/` directory | Grounding knowledge and wiki cross-referencing | Maintained locally on disk, full user ownership |
| External Tool Execution Data | Host OS shell, filesystem, web browser | Gathers live system data and executes tasks | Sandboxed execution with command confirmation gates |

CowAgent Harness complies with operational security and privacy standards:
- **No Cloud Data Exfiltration:** All personal knowledge, conversation logs, and memories remain strictly within the user's host environment.
- **Epistemic Isolation:** Memory distillation strictly segregates personal facts from operational configuration rules.
- **Sanitized Model Payloads:** Prompts and shell inputs undergo rigorous sanitization to neutralize command injection vectors before execution.
- **Data Minimization:** Only relevant memory fragments and essential tool schemas are dispatched to LLM inference endpoints.

---

## 4. Known Limitations & Failure Modes

Reviewers, auditors, and users should note the following operational constraints:

1. **Unbounded Autonomous Looping:**
   - *Limitation:* Highly complex or ambiguous tasks might cause the agent to loop endlessly across tools.
   - *Mitigation:* Hard iteration limit (`max_turns: 25`), cycle detection heuristics, and automatic escalation to user prompt.

2. **IM Channel Rate Limiting:**
   - *Limitation:* Rapid message dispatching to third-party IM platforms (e.g. WeChat, Slack) can trigger platform rate limits.
   - *Mitigation:* Outbound message queueing with adaptive token-bucket rate limiting and jittered retries.

3. **Memory Hallucination During Distillation:**
   - *Limitation:* Automatic Deep Dream summarization could misinterpret nuances from casual conversations as core facts.
   - *Mitigation:* Human-editable Markdown memory files (`memory/core.md`) allowing users to review, edit, or purge memories.

4. **Multi-Agent Coordination Deadlocks:**
   - *Limitation:* Multiple specialized agents debating sub-tasks in a shared conversation may deadlock.
   - *Mitigation:* Designated supervisor agent role with override capability and turn-taking timeouts.

---

## 5. Verification, Safety & Human Oversight

- **Real-Time Human Approval Gate:** Potentially destructive operations (file deletion, terminal execution, system modifications) mandate explicit human authorization before execution.
- **Emergency Session Interrupt:** Users can immediately abort running task loops at any time via `/stop`, terminal signals, or emergency kill commands.
- **Step Quota Guardrails:** Strict execution quotas limit maximum autonomous turns and prevent runaway recursive invocation loops.
- **Structured Audit Logging:** Every executed shell command, tool payload, memory update, and incoming channel message is captured in immutable local audit logs.
