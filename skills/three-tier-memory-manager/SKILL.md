---
name: "three-tier-memory-manager"
description: "Coordinates context, daily logs, and core memory with Deep Dream distillation."
---

# Three-Tier Memory Manager Skill

## Overview
Manages long-term personal memory across three hierarchical tiers:
1. **Working Context:** Short-term conversational buffer for immediate turn coherence.
2. **Daily Memory:** Chronological daily interaction summaries capturing day-to-day events.
3. **Core Memory:** Permanent distilled profile facts, preferences, and long-term user context.

## Workflow
- Automatically logs significant conversation points to daily notes.
- Runs nocturnal Deep Dream distillation to extract enduring core facts.
- Performs hybrid keyword + vector retrieval to augment active prompts.
