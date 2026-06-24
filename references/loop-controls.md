# Loop Controls

Use this page when improving the agentic loop itself: step policy, tool availability, state machine phases, retries, context management, memory, approvals, or termination.

## Contents

- Mental model
- Phase-gated tools
- Tool choice
- Stop conditions
- Cycle detection
- Parallel calls
- Context management
- Memory writes
- Approvals and idempotency
- Trace diagnosis

## Mental Model

Treat the loop as a state machine, not as an open-ended conversation.

At each step:

1. The model proposes a tool call or final answer.
2. The runtime validates the proposal against the current state.
3. The executor performs the smallest authorized action.
4. The result returns typed status and provenance.
5. The loop advances, asks, waits for approval, fails, or terminates.

## Phase-Gated Tools

Expose only the tools needed for the current phase.

Example phases:

- `understand_request`: no write tools.
- `retrieve_context`: search and lookup tools only.
- `draft_plan`: prepare tools only.
- `await_approval`: no autonomous side effects.
- `commit_action`: only the approved commit tool.
- `finalize`: no tools unless final verification is required.

If the framework supports `activeTools`, `toolChoice`, graph edges, interrupts, or per-step preparation hooks, use them to enforce phases.

## Tool Choice

Use deterministic tool choice when:

- The first step must retrieve or validate context.
- The model should call a specific submit/finalization tool.
- A phase should allow exactly one tool.
- A previous validation error indicates a specific repair action.

Use automatic tool choice when:

- Multiple read-only tools are safe alternatives.
- The model can choose based on context without increasing risk.

Disable tools when:

- The answer should be generated from already available context.
- The loop is in finalization.
- The user needs to approve or clarify.

## Stop Conditions

Add hard stops outside the prompt:

- Max steps.
- Max wall-clock time.
- Max tokens or cost.
- Max retries per tool.
- Max repair attempts.
- Max repeated equivalent tool calls.
- Stop on approval requirement.
- Stop on unauthorized or fatal error.

Prompt the model to stop appropriately, but enforce stops in runtime code.

## Cycle Detection

Detect repeated work by canonicalizing:

- Tool name.
- Normalized arguments.
- Relevant state.
- Error status.

If the same call repeats without new information, stop, ask for clarification, or change strategy. Do not rely on the model to notice loops.

## Parallel Calls

Allow parallel tool calls only when they are:

- Read-only.
- Independent.
- Idempotent.
- Bounded in number.
- Not competing for the same state.

Do not parallelize state-changing calls unless the runtime has explicit transaction and conflict handling.

## Context Management

- Summarize old steps into compact state, not vague prose.
- Keep current objective, constraints, approvals, and validated IDs visible.
- Drop irrelevant raw tool output.
- Preserve provenance for facts that affect later actions.
- Mark untrusted content as untrusted after summarization.
- Avoid mixing memory with current instructions.

## Memory Writes

Persist memory only through an explicit runtime path:

- Define what may be remembered.
- Exclude secrets, raw external content, transient IDs, and unverified facts.
- Require provenance and confidence.
- Consider user review for personal or durable memory.
- Treat retrieved memory as data, not policy.

## Approvals And Idempotency

For side effects:

- Stage a plan with a prepare tool.
- Show the exact target, action, payload, and risk.
- Store approval outside the model.
- Commit using a trusted prepared action ID.
- Generate idempotency keys in code.
- Log the approved payload and actual execution result.

## Trace Diagnosis

When reviewing a failed trace, identify the first bad transition:

- Wrong objective.
- Wrong tool set exposed.
- Wrong tool chosen.
- Bad arguments.
- Bad tool result shape.
- Missing validation.
- Missing approval.
- Bad retry policy.
- Bad context or memory.
- Bad final output.

Fix the earliest layer that would have prevented the failure.
