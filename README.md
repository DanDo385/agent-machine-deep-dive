# Agent Runtime

A single-page, self-contained explainer of how two production agent runtimes implement the same five-part tool-using loop:

- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) — Python gateway, `AIAgent`, SQLite session storage
- [openclaw/openclaw](https://github.com/openclaw/openclaw) — TypeScript packages, `CoreAgentHarness`, JSONL session tree

Each section maps a standard agent runtime model to real source files and public APIs, with Python-shaped Hermes excerpts and TypeScript-shaped OpenClaw excerpts. The examples are compact reconstructions that preserve filenames, class and function names, and data flow while omitting product-specific branches and provider details.

## Runtime Model

Every tool-using agent tends to follow the same runtime loop:

```text
Inbound event
  -> 1. platform input
  -> 2. session hydration
  -> 3. agent loop
  -> 4. tool dispatch
  -> 5. persistence
  -> outbound response
```

This page walks that pipeline side by side in Hermes and OpenClaw.

## The Five Parts

| Part | Hermes Agent | OpenClaw |
|------|--------------|----------|
| **Platform Input** | `SessionSource`, `BasePlatformAdapter`, gateway allowlist handling | ACP runtime handles, turn input contracts, session store |
| **Session Hydration** | `run_conversation(...)`, conversation history, compression hooks | `Session.buildContext()`, branch replay, compaction summaries |
| **Agent Loop** | `agent.conversation_loop.run_conversation` with `max_iterations` | `agentLoop` / `runLoop` in `@openclaw/agent-core` |
| **Tool Dispatch** | `tools.registry`, `agent.tool_executor` | `executeToolCalls`, before/after tool hooks |
| **Persistence** | SQLite `SessionDB.append_message` plus JSON logs | Append-only JSONL session tree storage |

## What's on the Page

- **Source anchors** — entry-point files in each repo for each runtime part
- **Side-by-side code maps** — Hermes Python vs OpenClaw TypeScript for all five parts
- **Coordination summary** — comparison table plus the main architectural difference (monolithic Python runtime vs factored TypeScript packages)
- **Agent mode exports** — `llms.txt` and `agent-runtime-map.json` for agent-readable summaries
- **Sticky sidebar navigation** with jump links to each section
- **Dark and light themes** with preference saved in `localStorage`
- **Responsive layout** for smaller screens

### Source Anchors

| Hermes (Python) | OpenClaw (TypeScript) |
|-----------------|----------------------|
| `run_agent.py` | `packages/agent-core/src/agent-loop.ts` |
| `agent/conversation_loop.py` | `packages/agent-core/src/harness/agent-harness.ts` |
| `agent/tool_executor.py` | `packages/agent-core/src/harness/session/session.ts` |
| `tools/registry.py` | `packages/agent-core/src/harness/session/jsonl-storage.ts` |
| `gateway/session.py`, `gateway/platforms/base.py` | `packages/acp-core/src/session.ts`, `runtime/types.ts` |

## Viewing It

The site is a single static `index.html` file. No build step, package manager, or server is required.

- Open `index.html` directly in a browser.
- Optional local server:

  ```bash
  python3 -m http.server 8000
  # then visit http://localhost:8000
  ```

Fonts are loaded from Google Fonts, so an internet connection gives the intended typography but is not required for the content.

## Structure

```text
agent-runtime/
├── agent-runtime-map.json # Machine-readable runtime map
├── index.html             # Complete comparison page (markup, styles, content)
├── llms.txt               # Agent-readable text summary
└── README.md
```
