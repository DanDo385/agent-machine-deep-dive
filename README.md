# The 5-Part Agent Machine — Deep Dive

A single-page, self-contained deep dive explaining the universal architecture that every AI agent runs on. Strip away the branding and feature lists, and every agent — Claude Code, AutoGPT, LangChain agents, your custom bot — is the same machine with the same five parts.

> Every AI agent ever built runs on this skeleton. No exceptions.

## The Five Parts

| Part | Role | Job |
|------|------|-----|
| **Platform Input** | The Ears | Receives and validates incoming messages; authorization lives here |
| **Session Hydration** | The Memory Recall | Loads prior context within a token budget |
| **Agent Loop** | The Brain | Iterates on the `messages` array, deciding what to do next |
| **Tool Dispatch** | The Hands | Routes tool calls through a registry and executes them |
| **Persistence** | The Diary | Records outcomes so the next session can recall them |

The feedback loop between the Agent Loop and Tool Dispatch is what makes it an *agent* rather than a chatbot: a chatbot fires once and stops, while an agent loops until the model decides it's done.

## What's Inside

For each of the five parts, the document covers:

- **What it is** and a real-world analogy (the running example is a 911 dispatch center)
- **How it works**, with code examples
- **How to evaluate it** — what a healthy vs. unhealthy implementation looks like
- **Individual failure examples** — exactly what breaks and how to fix it, including:
  - Duplicate message processing (Platform Input)
  - Context overflow with no token budget (Session Hydration)
  - Infinite tool loops and the `maxTurns` guard (Agent Loop)
  - Ambiguous schemas producing wrong arguments (Tool Dispatch)
  - Race conditions on concurrent sessions (Persistence)

It closes with **Overlap Zones** (where the parts blur together), a full-system **Poor Coordination** failure walkthrough, and the mental model to keep.

## Viewing It

The entire site is a single, dependency-free `index.html` file. No build step, no package manager, no server required.

- **Simplest:** open `index.html` directly in any browser.
- **Local server** (optional):

  ```bash
  python3 -m http.server 8000
  # then visit http://localhost:8000
  ```

Fonts are loaded from Google Fonts, so an internet connection gives the intended typography but is not required for the content.

## Structure

```
agent-machine-deep-dive/
└── index.html    # The complete deep-dive site (markup, styles, and content)
```
