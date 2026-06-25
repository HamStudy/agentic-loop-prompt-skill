# agentic-loop-prompt-skill

An agentic skill for improving prompts, tool contracts, runtime controls, and
evaluation plans for LLM applications that run agentic loops.

The skill is designed for agent runtimes that support loadable instruction
packs or "skills" with a `SKILL.md` file and supporting references. It is
not tied to one provider or framework. It covers Vercel AI SDK
ToolLoopAgent-style loops, LangGraph/ReAct-style agents, MCP/tool-calling
agents, browser/computer-use agents, code/shell agents, multi-agent systems,
structured-output workflows, reasoning traces, verification/judge patterns,
and agents with memory or RAG.

## Purpose

Agentic loops fail in ways that ordinary prompt-writing advice does not catch:
models call the wrong tool, repeat the same tool forever, produce valid JSON
that is not usable, trust hostile web/email/file content, leak data through
side effects, or keep working after the task is already complete.

This skill gives an agent a compact field guide for reviewing and improving
that whole loop contract:

- what belongs in the system/developer prompt
- what belongs in tool descriptions and schemas
- what must be enforced by runtime code
- how to make completion observable
- how to prevent repeated no-progress loops
- how to use structured outputs and repair loops safely
- how to handle prompt injection and excessive agency
- how to expose reasoning traces without leaking private chain-of-thought
- how to add verifier models, consensus, postconditions, and evals

The core principle is:

> Prompt for behavior; enforce invariants in code.

## Repo layout

The skill itself lives in the [`improve-agentic-prompts/`](improve-agentic-prompts/)
directory of this repo. That directory is what you load into your agent
runtime.

```
agentic-loop-prompt-skill/             <- this repo
|-- README.md                          <- you are here
|-- LICENSE
`-- improve-agentic-prompts/           <- the skill folder
    |-- SKILL.md
    `-- references/
        |-- prompt-contracts.md
        |-- tool-interfaces.md
        |-- structured-outputs.md
        |-- loop-controls.md
        |-- verification-and-consensus.md
        |-- reasoning-traces.md
        |-- security-boundaries.md
        |-- model-adaptation.md
        `-- evals.md
```

## What it covers

A short top-level `improve-agentic-prompts/SKILL.md` routes the agent into
focused reference pages:

| Reference | Topic |
|---|---|
| `prompt-contracts.md` | Main system/developer prompts, authority order, trust boundaries, completion states, examples, and prompt anti-patterns |
| `tool-interfaces.md` | What belongs in prompts vs structured tool definitions, tool names/descriptions/schemas, results, provenance, prepare/commit, and tool anti-patterns |
| `structured-outputs.md` | Tool calls vs final output, JSON/schema reliability, constrained output, parser repair, and semantic validation |
| `loop-controls.md` | State-machine design, active tools, tool choice, explicit done states, stop conditions, no-progress detection, memory, approvals, and idempotency |
| `verification-and-consensus.md` | Usable-result checks, postconditions, verifier models, critics, self-consistency, debate, model juries, and escalation |
| `reasoning-traces.md` | Hidden reasoning vs visible summaries, provider thinking blocks, scratchpads, trace logging, context retention, redaction, and UI exposure |
| `security-boundaries.md` | Direct/indirect prompt injection, excessive agency, tool poisoning, memory poisoning, cross-agent risk, egress, secrets, MCP hardening, and approvals |
| `model-adaptation.md` | Adjustments for frontier, reasoning, small, text-only, open/local, RAG, multimodal, code/shell, multi-agent, realtime/voice, and provider-specific behavior |
| `evals.md` | Functional tests, adversarial injection matrices, tool-risk cases, loop cases, metrics, regression practice, and trace-based iteration |

## Installing

Install the nested skill folder, not the repository root.

For Codex-style skill installation, use the GitHub repo plus path:

```bash
install-skill-from-github.py --repo HamStudy/agentic-loop-prompt-skill --path improve-agentic-prompts
```

Or use a GitHub tree URL if your runtime supports it:

```text
https://github.com/HamStudy/agentic-loop-prompt-skill/tree/main/improve-agentic-prompts
```

After installing, restart or reload your agent runtime so it picks up the new
skill metadata.

## Status and currency

This skill is framework-agnostic guidance, but provider and SDK behavior
changes. For exact API names and current behavior, always check the live docs
for the framework/provider you are using, especially around tool-calling,
structured output, reasoning/thinking traces, and loop-control settings.

## License

MIT. See [LICENSE](LICENSE).
