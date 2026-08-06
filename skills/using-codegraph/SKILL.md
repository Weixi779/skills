---
name: using-codegraph
description: >
  Route structural repository exploration through a configured CodeGraph MCP
  server. Use when locating symbols, callers, or callees; tracing a flow;
  assessing change impact; mapping a subsystem or architecture; or gathering
  cross-file context before a non-trivial change. Trigger only when CodeGraph
  tools are available or may be configured. Do not use for literal-text
  searches, trivial single-file work, or correctness validation.
---

# Using CodeGraph

Use this skill as a routing layer. Let the installed CodeGraph MCP server own
its version-specific tool manual; do not reconstruct or preserve that manual
here.

## Route the Task

1. Discover the available `codegraph_*` tools when they are not already
   visible.
2. Select the tool by the user's intent and follow its current description.
   Prefer task context for subsystem questions, trace for an end-to-end path,
   symbol search for definitions, and impact analysis before changing a
   boundary.
3. Answer from the graph directly. Do not delegate the same exploration or
   repeat it with a broad grep-and-read loop.
4. Use `rg` or focused file reads for literal text, a detail the graph does not
   cover, or files explicitly identified as pending re-index.

## Handle Availability

- If CodeGraph is unavailable and the task can proceed cheaply, use native
  repository tools without ceremony.
- If CodeGraph would materially help but the repository is not initialized,
  ask before running `codegraph init -i`.
- Treat fresh graph results as structural evidence. Use the compiler, tests,
  and linters—not CodeGraph—to validate correctness after a change.
