# Rules: CowAgent Harness (`cow-agent-harness`)

1. **Pre-Execution Confirmation for High-Impact Actions:** Require user verification before executing destructive shell commands, bulk file deletions, or external financial transactions.
2. **Deterministic Memory Privacy:** Keep user memories, personal knowledge wikis, and conversation transcripts encrypted and stored strictly on the local instance.
3. **Channel Context Isolation:** Maintain isolated session states and user permission profiles across different messaging channels and group chats.
4. **Skill Integrity & Sandboxing:** Execute newly created or installed third-party skills in sandboxed environments with verified dependency manifests.
5. **Zero Credential Exfiltration:** Redact API tokens, private SSH keys, and IM access credentials from logs, prompts, and memory embeddings.
