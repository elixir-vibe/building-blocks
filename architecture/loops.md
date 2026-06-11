# Feedback loops

*Status: draft. The thesis in motion: how a failure anywhere in the stack
travels back to the agent as a witness — where, why, and when possible,
how to fix.*

```mermaid
flowchart TD
    agent(("Agent"))

    agent -->|"edits code"| code["Source"]
    code -->|"compile + gates"| checks["Quality gates\ncompile · tests · clones\nslop · architecture"]
    checks ==>|"finding + location\n+ suggested fix"| agent

    code -->|"runs"| app["Live app\n(BEAM)"]
    app ==>|"eval: state, logs,\nprocesses, queries"| agent

    app -->|"renders in"| browser["Browser"]
    browser ==>|"console errors\n→ server logs"| agent

    app -->|"records"| replay["Session\nrecordings"]
    replay ==>|"assigns timeline:\nexact divergent event"| agent

    code -->|"indexed by"| graph["Dependence\ngraph"]
    graph ==>|"impact set, taint paths,\narchitecture violations"| agent

    ecosystem["Public ecosystem\nindex"] ==>|"existing implementations,\nAPI shapes"| agent

    style agent fill:#fff4e0
```

Thin arrows: the agent's actions and the system's normal operation. Thick
arrows: witnesses flowing back. The design goal, stated in the
[manifesto](../manifesto/README.md#3--the-environment-thesis): every thick
arrow carries *where, why, and when possible how to fix* — never a bare
"failed".

The unusual edges, and where they're explained:

- **Browser console → server logs** without any browser tooling: the build
  toolchain lives in the app's VM ([Build](../layers/build.md)).
- **Replay → exact divergent event**: render purity makes recordings typed
  data, not video ([Time](../layers/time.md)).
- **Live app → eval**: the agent holds a stateful session inside the
  runtime it is debugging ([Agent](../layers/agent.md)).
- **Ecosystem → agent**: structural search over all published code, against
  hand-rolling ([Knowledge](../layers/knowledge.md)).
