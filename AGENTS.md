# AGENTS.md

This file defines the working instructions for coding agents operating in the AgentForge repository.

AgentForge is an experimental open-source project exploring an **engineering control plane for coding agents**: a portable way for repositories to expose context, capabilities, guardrails, permissions, and evaluation requirements across different agent runtimes.

The repository is intentionally designed to be both:

- **human-readable**
- **agent-explicit**

This file is a behavioral guide for coding agents. Machine-readable repository configuration belongs under `.agentforge/`.

---

# 1. Start here

Before making changes:

1. Read `.agentforge/manifest.yaml`.
2. Read only the documentation relevant to the current task.
3. Check applicable Architecture Decision Records under `docs/decisions/`.
4. Inspect the smallest useful set of source files.
5. Resolve applicable guardrails before performing consequential actions.
6. Identify how the change will be validated.

Do **not** scan the entire repository unless the task genuinely requires repository-wide understanding.

Prefer targeted discovery over exhaustive exploration.

---

# 2. Source of truth

Use the following order when interpreting project intent:

```text
Accepted Architecture Decision Records
        ↓
Architecture documentation
        ↓
.agentforge/ repository contract
        ↓
Project vision and roadmap
        ↓
Capability documentation
        ↓
Implementation
```

If implementation conflicts with an accepted architectural decision, do not silently change the architecture.

Instead:

1. identify the conflict;
2. follow the accepted decision unless the task explicitly requires reconsideration;
3. propose a new or superseding ADR when necessary.

---

# 3. Human-readable. Agent-explicit.

Do not create separate copies of the same knowledge for humans and agents unless there is a strong technical reason.

Prefer:

```text
Human-readable documentation
        ↓
canonical source of truth
        ↓
.agentforge/ references or structured metadata
```

For example:

```text
docs/architecture/context-routing.md
```

may explain context routing, while:

```text
.agentforge/context/routes.yaml
```

may define executable routing configuration.

Avoid duplicating the same policy or architecture description across both locations.

---

# 4. Use the least-complex mechanism

AgentForge follows this principle:

> **Deterministic where possible. Semantic where useful. Generative where necessary.**

Use normal software for deterministic problems.

Examples:

- file discovery
- schema validation
- path matching
- Git metadata
- dependency parsing
- policy evaluation
- permissions
- configuration validation

Use semantic decision-making only where rules would become brittle.

Examples:

- relevance classification
- capability routing
- risk classification
- context prioritization

Use generative reasoning where actual reasoning or creation is required.

Examples:

- code generation
- debugging
- architectural analysis
- documentation
- root-cause investigation
- complex refactoring

Do not use an LLM where a deterministic solution is sufficient.

---

# 5. Skills before agents

Do not create an autonomous agent simply because one can be created.

Prefer this progression:

```text
Instruction
    ↓
Reusable Skill
    ↓
Tool-enabled Skill
    ↓
Specialized Agent
    ↓
Multi-agent Workflow
```

Introduce additional autonomy only where it produces measurable value.

---

# 6. Context is a finite resource

Treat context and tokens as engineering resources.

Prefer:

- targeted file discovery
- progressive disclosure
- task-specific documentation
- relevant ADRs only
- relevant skills only
- selective tool exposure
- bounded historical context

Avoid:

- repeatedly rereading unchanged files
- loading unrelated documentation
- loading every available skill
- loading all repository history
- sending the entire repository to a model without need

When possible, explain why a context source is relevant before loading it.

---

# 7. Prefer repository evidence over inference

Do not infer facts that can be established from repository evidence.

Prefer:

```text
configuration
source code
tests
Git history
ADRs
schemas
runtime evidence
```

over assumptions.

When evidence is incomplete, distinguish clearly between:

```text
Observation
Hypothesis
Supporting Evidence
Contradicting Evidence
Confidence
Next Validation Step
```

A plausible explanation is not a verified conclusion.

---

# 8. Enterprise guardrails are authoritative

Coding-agent safety controls do not replace enterprise policy.

Before performing an action with meaningful side effects, determine:

1. what resource is being accessed;
2. what action is being performed;
3. which environment is affected;
4. which guardrails apply;
5. whether human approval is required.

Expected policy outcomes are:

```text
ALLOW
DENY
REQUIRE_APPROVAL
```

Do not attempt to weaken or bypass applicable guardrails.

A repository-level policy must not silently weaken a mandatory higher-level enterprise policy.

---

# 9. Guardrail resolution may be external

Enterprise or organization-wide policies may not live directly in this repository.

Guardrails may be resolved from:

- secure internal policy services
- private repositories
- artifact registries
- organization-managed configuration
- locally defined repository policy

Do not assume the absence of a local policy means no policy exists.

If required guardrails cannot be resolved, fail safely according to the applicable policy model.

Do not expose credentials, policy secrets, authentication tokens, or sensitive policy data in generated output.

---

# 10. Human approval at consequential boundaries

Agents may generally inspect, analyze, propose, generate, and test.

Actions such as the following should require explicit authorization unless an approved repository policy says otherwise:

- merging to protected branches
- deploying software
- modifying production systems
- changing enterprise policy
- destructive operations
- modifying production data
- broad permission changes

Do not interpret successful tool access as authorization.

Capability and permission are not the same thing.

---

# 11. Keep the core vendor-neutral

Core AgentForge abstractions should not depend unnecessarily on:

- one coding agent
- one model provider
- one IDE
- one observability vendor
- one cloud provider

Provider-specific behavior should usually live behind adapters.

For example:

```text
ObservabilityProvider
        ↓
Datadog Adapter
Splunk Adapter
Dynatrace Adapter
Elastic Adapter
Prometheus Adapter
```

Prefer portable contracts over provider-specific assumptions.

---

# 12. Adapters should remain thin

Agent-specific integration files are compatibility layers.

Examples may include:

```text
AGENTS.md
CLAUDE.md
.github/copilot-instructions.md
other runtime-specific shims
```

These files should point to the canonical AgentForge repository contract rather than duplicating large amounts of configuration.

Avoid maintaining separate copies of policy or architecture for each coding agent.

---

# 13. Architectural changes require deliberation

A change should generally be considered architectural if it materially changes:

- a core abstraction
- repository contract semantics
- capability boundaries
- context-routing behavior
- guardrail semantics
- permission models
- evaluation architecture
- integration boundaries
- state-management strategy
- provider portability

Architectural decisions with meaningful long-term consequences should be documented under:

```text
docs/decisions/
```

Do not create ADRs for trivial implementation choices.

---

# 14. Capability design

AgentForge capabilities should follow a common contract where practical.

A capability should identify:

```text
Purpose
Inputs
Required Context
Required Skills
Tools
Permissions
Guardrails
Workflow
Expected Output
Failure Modes
Safety Boundaries
Evaluations
```

Do not add fields that provide no useful information.

Prefer explicit capability contracts over hidden assumptions.

---

# 15. Tool use

Expose and invoke only tools relevant to the current task.

Before invoking a tool, consider:

- Is this tool necessary?
- Is there a deterministic alternative?
- Is access permitted?
- Is the scope appropriately narrow?
- Could the result contain sensitive information?
- Is the action reversible?

Avoid unnecessary tool calls.

Tool availability does not imply permission to use it.

---

# 16. Production debugging

Production-debugging workflows must be evidence-driven.

When investigating an incident:

1. establish the incident boundary;
2. construct a timeline;
3. identify affected services;
4. collect relevant logs, traces, metrics, alerts, and deployment information;
5. correlate runtime evidence with source code and Git history;
6. generate explicit hypotheses;
7. gather evidence supporting and contradicting each hypothesis;
8. reproduce when practical;
9. propose remediation;
10. validate the remediation with tests.

Do not label a hypothesis as the root cause without adequate evidence.

Production access should be read-only by default unless an applicable policy explicitly permits otherwise.

---

# 17. Testing

Validate changes at the lowest useful level first.

Prefer:

```text
deterministic validation
        ↓
unit tests
        ↓
integration tests
        ↓
end-to-end tests
        ↓
agent/evaluation suites
```

Use expensive validation only when it adds value.

When fixing a defect, add a regression test when practical.

Do not create tests merely to increase test counts.

Tests should verify meaningful behavior.

---

# 18. Evaluation is part of the work

A significant AgentForge capability should eventually have a measurable evaluation strategy.

When modifying agent behavior, ask:

- What behavior are we trying to improve?
- What is the baseline?
- What metric indicates success?
- What could regress?
- What does the new behavior cost?

Where applicable, capture:

```text
Task Success
First-pass Success
Tokens
Latency
Cost
Tool Calls
Retries
Policy Violations
Failure Category
Context Consumed
```

Prefer reproducible experiments over anecdotal improvements.

---

# 19. Efficiency matters

Do not optimize only for task completion.

Also consider:

- tokens
- latency
- tool usage
- model usage
- unnecessary context
- retries
- complexity
- generated code volume

Prefer the smallest reliable solution.

Avoid speculative abstractions before they are needed.

---

# 20. Dependencies

Before introducing a new dependency, consider:

1. Is it required?
2. Does equivalent functionality already exist?
3. Can the standard library or platform solve the problem?
4. Is it actively maintained?
5. Is the license compatible?
6. Does it introduce security or supply-chain risk?
7. Does it materially increase complexity?

Avoid dependency growth without clear value.

---

# 21. Security

Never commit or expose:

- API keys
- credentials
- access tokens
- secrets
- private certificates
- customer data
- production credentials
- sensitive telemetry

Treat logs, traces, incident data, repository metadata, and external guardrails as potentially sensitive.

Use least-privilege access.

---

# 22. Keep changes focused

For each task:

1. understand the requested outcome;
2. discover the minimum relevant context;
3. identify applicable guardrails;
4. make the smallest coherent change;
5. validate it;
6. update relevant documentation;
7. summarize assumptions and limitations.

Do not opportunistically refactor unrelated areas unless necessary.

---

# 23. Documentation discipline

Documentation should explain durable intent.

Prefer documenting:

- why something exists
- what contract it exposes
- important constraints
- design trade-offs
- evaluation strategy

Avoid documenting every internal implementation detail.

Update documentation when behavior, architecture, contracts, or user-facing workflows change.

---

# 24. When uncertain

If several approaches are valid:

1. prefer the simpler solution;
2. prefer fewer irreversible assumptions;
3. prefer portable abstractions;
4. prefer deterministic behavior where possible;
5. prefer solutions that can be evaluated;
6. document consequential trade-offs.

Do not invent requirements unsupported by repository documentation or the task.

---

# 25. Definition of done

A change is not complete merely because code has been generated.

Where applicable, completion includes:

- correct implementation
- appropriate validation
- relevant tests
- documentation updates
- guardrail considerations
- evaluation considerations
- no unnecessary provider coupling
- clear assumptions and limitations

---

# 26. Dogfood AgentForge

AgentForge should use the practices it promotes.

This repository itself is an experimental environment for:

- agent-aware repository design
- portable repository contracts
- context routing
- coding-agent adapters
- reusable skills
- enterprise guardrails
- agent evaluations
- token-efficiency experiments
- production-debugging workflows

Where practical, improvements to AgentForge should also help validate AgentForge's own product hypotheses.

---

# 27. Guiding principle

When choosing between approaches, prefer:

> **Deterministic where possible. Semantic where useful. Generative where necessary.**

And when choosing how much context, tooling, autonomy, or complexity to introduce:

> **Use the minimum required to complete the task reliably, safely, and measurably.**