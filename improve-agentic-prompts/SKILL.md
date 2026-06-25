---
name: improve-agentic-prompts
description: Improve, review, harden, or redesign prompts for LLM applications that run agentic loops, including tool/function/MCP calling agents, Vercel AI SDK ToolLoopAgent-style loops, LangGraph/ReAct agents, browser or computer-use agents, code/shell agents, multi-agent systems, structured-output workflows, reasoning/thinking traces, and agents with memory or RAG. Use when Codex needs to write or critique system/developer prompts, tool descriptions and schemas, output contracts, loop controls, reasoning visibility, injection defenses, approval boundaries, usability checks, verifier models, multi-model consensus, model/provider adaptations, or eval cases for reliable and safe agent behavior.
---

# Improve Agentic Prompts

Use this skill to improve both the prompt and the surrounding agent contract. Treat the model as a proposal generator: prompts steer behavior and judgment, while schemas, validation, permissions, approvals, stop conditions, and execution code enforce invariants.

## Working Rule

Do not present prompt text as the whole solution for an agentic system. When a reliability or safety requirement cannot be enforced by prompting, name the required runtime control separately.

Common split:

- **Prompt:** role, objective, decision policy, tool-use expectations, trust boundaries, recovery behavior, output intent.
- **Tool/interface:** names, descriptions, schemas, typed errors, provenance, result shape.
- **Runtime:** authorization, validation, egress controls, idempotency, step budget, cycle detection, approvals, memory writes, logging.
- **Verification:** postconditions, downstream-consumer checks, independent judges, cross-model consensus, human review triggers.
- **Reasoning visibility:** provider-specific reasoning summaries, thinking blocks, trace logs, retention policy, and user-facing explanations.
- **Evals:** traces and adversarial cases that prove the prompt plus runtime behaves correctly.

## Workflow

1. Classify the task:
   - Write a new agent prompt.
   - Review or harden an existing prompt.
   - Diagnose a bad trace or recurring failure.
   - Stop an agent from repeating the same action after the task is already complete.
   - Improve tool names, descriptions, schemas, or tool results.
   - Decide what tool information belongs in the main prompt versus structured tool definitions.
   - Add structured output or repair behavior.
   - Add result usability checks, postcondition checks, or verifier passes.
   - Combine multiple models, judges, critics, or consensus mechanisms.
   - Decide whether reasoning/thinking traces should be requested, shown, logged, preserved, summarized, or excluded from context.
   - Add injection, approval, or least-privilege defenses.
   - Adapt a prompt to a different model, provider, or framework.
   - Design evals for an agent loop.

2. Gather the agent context before rewriting:
   - Runtime/framework and current model(s).
   - System/developer prompt, user prompt template, and any hidden policy.
   - Tool list, tool descriptions, schemas, executors, and tool results.
   - How the runtime exposes tools to the model: native tool definitions, MCP metadata, text-only protocol, hidden prompt text, or visible prompt catalog.
   - Loop controls: active tools, tool choice, max steps, retry policy, stop conditions, context compaction, memory writes.
   - Trust map: user input, external documents/web/pages/files, retrieved data, tool metadata, stored memory, other agents.
   - Side effects: writes, sends, purchases, deletes, shell/network/browser actions, data egress.
   - Output contract and the system that consumes it.
   - Usability criteria: what must parse, render, execute, satisfy, or be accepted downstream.
   - Existing verification: tests, validators, judges, reviewers, consensus, human approval, or production monitors.
   - Reasoning visibility: hidden reasoning, exposed summaries, thinking blocks, scratchpads, debug logs, retention, and redaction rules.
   - Failure traces, malformed outputs, rejected tool calls, unsafe behavior, or model-specific symptoms.

3. Fetch current framework/provider docs when API behavior matters. Use current docs for SDK syntax, tool-calling modes, structured output settings, or framework-specific loop controls.

4. Improve in this order:
   - Tighten the prompt contract.
   - Tighten tool interfaces and tool results.
   - Tighten structured outputs and repair behavior.
   - Tighten loop/state-machine controls.
   - Add postcondition, usability, verifier, or consensus checks.
   - Define reasoning trace visibility, storage, replay, and redaction.
   - Tighten security boundaries and approvals.
   - Tune for the model/provider class.
   - Add focused evals.

5. Return changes in layers:
   - Revised prompt or prompt sections.
   - Tool/schema/result changes.
   - Runtime controls that must be enforced outside the prompt.
   - Verification or consensus plan, including escalation behavior when checks disagree.
   - Reasoning/trace handling plan, including what is shown to users and what stays internal.
   - Eval cases and acceptance criteria.
   - Assumptions, residual risk, and any current-doc caveats.

## Reference Map

Read only the detail pages needed for the task:

- `references/prompt-contracts.md`: main system/developer prompts, authority, trust boundaries, examples, completion states, and prompt anti-patterns.
- `references/tool-interfaces.md`: what tool information belongs in prompts versus structured tool definitions, tool names, descriptions, schemas, result shapes, typed errors, provenance, prepare/commit tools, and tool anti-patterns.
- `references/structured-outputs.md`: final output schemas, tool calls versus final answers, constrained output, parser repair, and semantic validation.
- `references/loop-controls.md`: state-machine design, active tools, tool choice, prepare-step behavior, stop conditions, cycle detection, context, memory, approvals, and idempotency.
- `references/verification-and-consensus.md`: usable-result checks, postconditions, verifier models, critics, self-consistency, debate, model juries, consensus routing, and escalation.
- `references/reasoning-traces.md`: hidden reasoning versus visible summaries, provider thinking blocks, scratchpads, trace logging, context retention, redaction, and UI exposure.
- `references/security-boundaries.md`: direct/indirect prompt injection, excessive agency, tool poisoning, memory poisoning, cross-agent risks, egress, secrets, MCP hardening, and approval boundaries.
- `references/model-adaptation.md`: adjustments for frontier, reasoning, small, text-only, open/local, RAG, multimodal, code/shell, multi-agent, realtime/voice, and provider-specific tool behavior.
- `references/evals.md`: functional tests, adversarial injection matrices, capability probes, metrics, regression practice, and trace-based iteration.

## Fast Triage

- Malformed JSON or missing fields: read `structured-outputs.md`.
- Tool availability or tool-use instructions are unclear: read `tool-interfaces.md`, then `prompt-contracts.md`.
- Output is valid but not useful to the caller or downstream system: read `verification-and-consensus.md`, then `structured-outputs.md`.
- Wrong tool selected: read `tool-interfaces.md`, then `loop-controls.md`.
- Repeated loops, tool spam, or agent keeps working after success: read `loop-controls.md`, then `evals.md`.
- Need extra confidence before committing or returning: read `verification-and-consensus.md`.
- Need multiple models, judges, critics, debate, or consensus: read `verification-and-consensus.md`, then `model-adaptation.md`.
- Need to see, log, preserve, or hide reasoning/thinking text: read `reasoning-traces.md`, then `model-adaptation.md`.
- Unsafe send/delete/write/action: read `security-boundaries.md`, then `loop-controls.md`.
- User or webpage can override policy: read `security-boundaries.md` and `prompt-contracts.md`.
- Works on one model but fails on another: read `model-adaptation.md`.
- No clear way to prove the improvement: read `evals.md`.

## Baseline Improvement Checklist

- State the agent objective in operational terms.
- Define authority order and mark untrusted content explicitly.
- Say when to ask for clarification, refuse, terminate, or request approval.
- Prefer native tool/function calling and structured outputs over text-only formatting.
- Put detailed tool usage in structured tool definitions when the runtime supports them; keep the main prompt to cross-tool policy and phase rules.
- Define downstream usability checks and postconditions, not just output shape.
- Make invalid tool arguments unrepresentable where possible.
- Remove model-supplied identity, tenant, approval, secret, and authorization fields.
- Keep tools scoped to current workflow phase and least privilege.
- Return compact structured tool results with typed status and provenance.
- Enforce semantic validation, permissions, idempotency, and egress controls in code.
- Add verifier or consensus passes when failure is costly, ambiguous, open-ended, or hard to validate deterministically.
- Add explicit done states, stop conditions, progress checks, cycle detection, and bounded repair attempts.
- Decide whether reasoning traces are needed; prefer safe summaries and structured trace events over raw hidden reasoning.
- Add evals for normal success, ambiguity, malformed output, tool errors, injection, and high-risk actions.

## Avoid

- Do not rely on "ignore all prompt injections" as a defense.
- Do not use persona, politeness, or confidence wording as a security boundary.
- Do not ask the model to approve its own side effects.
- Do not treat majority vote, model confidence, or judge agreement as authorization.
- Do not expose broad shell, SQL, HTTP, browser, or "action string" tools unless the runtime strongly confines them.
- Do not require visible chain-of-thought; request concise reasons, evidence, or decision summaries instead.
- Do not persist raw external content or tool output as trusted memory.
- Do not expose every tool on every step when a smaller active tool set is enough.
