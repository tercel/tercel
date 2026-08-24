**English** · [中文](https://github.com/tercel/tercel/blob/main/README-zh.md)

# Tercel

**Governed capability standards for agent-callable software · schema-enforced modules · protocol-neutral execution boundaries**

I work on the boundary where an agent stops reasoning and starts *causing things to happen*.

> **MCP tells an agent what it can call. [apcore](https://github.com/aiperceivable/apcore) decides whether this call — with these arguments, in this environment, by this identity — should happen at all, and leaves evidence that it did.**

Most of the agent-tooling stack today is a discovery problem solved twice and an authorization problem solved zero times. A tool schema tells a model how to format a call. It says nothing about who may make it, what it may touch, whether a human must approve it first, or what record survives afterwards. Those questions do not belong in the model's reasoning loop — they belong at the execution boundary, enforced identically no matter which protocol carried the request.

That is the position [**apcore**](https://github.com/aiperceivable/apcore) takes:

```text
                  ┌──────────────────────────────────────────┐
                  │     Agent  ·  Human  ·  Application       │
                  └────────────────────┬─────────────────────┘
                                       │
          ┌──────────┬──────────┬──────┴────┬──────────┬──────────┐
          ▼          ▼          ▼           ▼          ▼          ▼
       ┌─────┐   ┌───────┐  ┌───────┐   ┌──────┐   ┌───────┐  ┌────────┐
       │ MCP │   │  A2A  │  │  CLI  │   │ HTTP │   │OpenAI │  │ direct │
       └──┬──┘   └───┬───┘  └───┬───┘   └──┬───┘   └───┬───┘  └───┬────┘
          └──────────┴──────────┴─────┬────┴───────────┴──────────┘
                                      ▼
              ╔═══════════════════════════════════════════════╗
              ║              apcore runtime                   ║
              ║   schema · ACL · approval gate · middleware    ║
              ║          observability · audit trail          ║
              ╚═══════════════════════┬═══════════════════════╝
                                      ▼
                  ┌──────────────────────────────────────────┐
                  │      your existing business logic        │
                  │           (unchanged)                    │
                  └──────────────────────────────────────────┘
```

Define a governed capability once. Expose it through any surface. The guarantees do not change when the transport does.

---

## The ecosystem

Everything below lives under [**@aiperceivable**](https://github.com/aiperceivable).

**Core standard** — a protocol specification with three independent implementations, held together by cross-language conformance fixtures rather than by a shared codebase. [Full documentation →](https://apcore.aiperceivable.com/)

| | Repository | Install |
|---|---|---|
| Specification | [apcore](https://github.com/aiperceivable/apcore) | — |
| Python | [apcore-python](https://github.com/aiperceivable/apcore-python) | `pip install apcore` |
| TypeScript | [apcore-typescript](https://github.com/aiperceivable/apcore-typescript) | `npm install apcore-js` |
| Rust | [apcore-rust](https://github.com/aiperceivable/apcore-rust) | `cargo add apcore` |

**Surface adapters** — the same module, projected onto a different protocol. Each is specified once and implemented three times, with a shared conformance suite pinning the behavior that must match.

| Surface | Spec | Python | TypeScript | Rust |
|---|---|---|---|---|
| Model Context Protocol | [apcore-mcp](https://github.com/aiperceivable/apcore-mcp) | [py](https://github.com/aiperceivable/apcore-mcp-python) | [ts](https://github.com/aiperceivable/apcore-mcp-typescript) | [rs](https://github.com/aiperceivable/apcore-mcp-rust) |
| Agent2Agent | [apcore-a2a](https://github.com/aiperceivable/apcore-a2a) | [py](https://github.com/aiperceivable/apcore-a2a-python) | [ts](https://github.com/aiperceivable/apcore-a2a-typescript) | [rs](https://github.com/aiperceivable/apcore-a2a-rust) |
| Command line | [apcore-cli](https://github.com/aiperceivable/apcore-cli) | [py](https://github.com/aiperceivable/apcore-cli-python) | [ts](https://github.com/aiperceivable/apcore-cli-typescript) | [rs](https://github.com/aiperceivable/apcore-cli-rust) |

**Meeting code where it already is** — integrations that scan an existing codebase and expose what is already there, rather than asking anyone to rewrite it.

[fastapi-apcore](https://github.com/aiperceivable/fastapi-apcore) · [django-apcore](https://github.com/aiperceivable/django-apcore) · [flask-apcore](https://github.com/aiperceivable/flask-apcore) · [nestjs-apcore](https://github.com/aiperceivable/nestjs-apcore) · [axum-apcore](https://github.com/aiperceivable/axum-apcore) · [tiptap-apcore](https://github.com/aiperceivable/tiptap-apcore)

And for the code that has no framework at all — [**apexe**](https://github.com/aiperceivable/apexe) wraps an existing CLI binary into a governed module by scanning its help output, so a forty-year-old tool arrives at an agent with the same ACL and approval semantics as a hand-written one.

**Orchestration** — [apflow](https://github.com/aiperceivable/apflow), distributed task orchestration where every capability in the graph is itself an apcore module.

---

## Developer tooling

Separately, a set of Claude Code skills for the parts of engineering that reward a checklist over improvisation:

| | |
|---|---|
| [**code-forge**](https://github.com/tercel/code-forge) | TDD-driven implementation — plan, execute, debug, review, worktree and branch lifecycle, parallel agent dispatch |
| [**spec-forge**](https://github.com/tercel/spec-forge) | PRD, SRS, technical design, and test plans — standalone, or as one traceability chain |
| [**theory-forge**](https://github.com/tercel/theory-forge) | Academic rigor auditing — citation truth, falsifiability, Toulmin argument structure, scope discipline, counter-argument engagement |
| [**research-forge**](https://github.com/tercel/research-forge) | Technical and business due diligence on open-source projects and products |
| [**prompt-coach**](https://github.com/tercel/prompt-coach) | Real-time prompt and target-language coaching, as a `UserPromptSubmit` hook |
| [**agent-skill-bundler**](https://github.com/tercel/agent-skill-bundler) | Port Claude Code skills to other agent platforms |
| [**tercel-claude-plugins**](https://github.com/tercel/tercel-claude-plugins) | The marketplace that ships them |

---

## Elsewhere

[aiperceivable.com](https://aiperceivable.com/) · [@tercelyi](https://x.com/tercelyi) · [LinkedIn](https://www.linkedin.com/in/tercel-yi)

Issues and design discussions are welcome on any repository above — particularly the hard parts: authorization semantics, cross-language conformance, and what an agent is entitled to assume when a call comes back denied.
