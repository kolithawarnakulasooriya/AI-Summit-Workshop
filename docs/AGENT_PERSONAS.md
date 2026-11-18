# Agent Personas

These project-scoped Codex custom-agent definitions support implementation, review, and testing. Creating the definitions does not start agents or implement the backlog.

| Persona | Definition | Responsibility | Expected output |
| --- | --- | --- | --- |
| `creator` | [creator.toml](../.codex/agents/creator.toml) | Build the delegated application scope and meaningful tests | Changed files, acceptance coverage, actual validation, limitations |
| `reviewer` | [reviewer.toml](../.codex/agents/reviewer.toml) | Inspect requirements and code for concrete defects; do not edit | Findings by severity with file/line, reproduction, impact, and remedy |
| `tester` | [tester.toml](../.codex/agents/tester.toml) | Author/run assigned tests and check behavior | Acceptance IDs with pass/fail/not-run evidence and reproducible failures |

Each persona reads [AGENTS.md](../AGENTS.md) and the relevant specifications. All follow the HTML5/CSS3/JavaScript and jQuery stack, static-only architecture, absence of app authentication, free endpoint restriction, and local-snapshot definition of History.

## Configuration

Definitions use standalone `.codex/agents/*.toml` files with `name`, `description`, and `developer_instructions`, following the [official custom-agent format](https://learn.chatgpt.com/docs/agent-configuration/subagents#custom-agents). No model, reasoning level, provider, MCP server, or permission override is configured; agents inherit the parent session's settings. The reviewer's no-edit behavior is an instruction, not an enforced read-only sandbox.

During planning, the Codex CLI was not available on `PATH`, so the definitions were checked for TOML syntax and required fields, but native client discovery was not exercised. A supporting Codex client can discover the project-scoped definitions. If the active client does not expose custom agents, use a definition's instructions as the delegated role prompt.

## Suggested handoff

1. Delegate WX-001 to `creator`, including the intended scope and file ownership.
2. After an implementation exists, delegate acceptance verification to `tester` and a read-only review to `reviewer`.
3. Give confirmed defects to `creator`, rerun affected checks, and resolve release-blocking findings before completion.
4. The coordinating agent consolidates evidence and updates the single backlog task only when its Definition of Done is met.

Prefer sequential edits. A reviewer can inspect without editing while tests run, but coordinate file ownership before any concurrent writers. Further delegation requires an explicit user or applicable project instruction.

## Example requests

```text
Use creator to implement WX-001 from docs/BACKLOG.md. Follow AGENTS.md and
all three feature specifications. Return the implementation and validation evidence.
```

```text
Use reviewer to inspect the current changes against the PRD, architecture,
and design. Do not edit files. Return concrete defects and validation gaps.
```

```text
Use tester to verify the implementation against S-01–S-07, W-01–W-08,
and H-01–H-08. Mock all automated API calls and report pass/fail/not-run evidence.
```

While the repository contains only planning files, review/test work should check documentation and definition consistency. Application acceptance remains not run until implementation exists.
