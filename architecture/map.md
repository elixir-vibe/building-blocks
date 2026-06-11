# The boundary map

*Status: draft. The one-screen picture. (A designed graphic will replace
the Mermaid rendering for the site; this stays the diffable source.)*

```mermaid
graph TD
    DESIGN["DESIGN\na design is a node tree,\nnot a picture"]
    TIME["TIME\nwhat happened, replayable"]
    CAUSALITY["CAUSALITY\nwhat depends on what,\nwith proof"]
    BUILD["BUILD\nthe toolchain lives\ninside the app"]
    RUNTIME["RUNTIME\nJavaScript inside the\nobservable VM"]
    ONEPROGRAM["ONE PROGRAM\nno client/server\nagreement to break"]
    QUALITY["QUALITY\nmachine-generated code,\nmachine-checked"]
    KNOWLEDGE["KNOWLEDGE\nthe ecosystem,\nsearchable by shape"]
    AGENT["AGENT\nthe worker is a\nsupervised system"]
    DEPLOY["DEPLOY\na supervised path\nto a Linux box"]

    DESIGN --> ONEPROGRAM
    ONEPROGRAM --> RUNTIME
    BUILD --> RUNTIME
    ONEPROGRAM --> TIME
    CAUSALITY --- RUNTIME
    AGENT --> CAUSALITY
    AGENT --> QUALITY
    AGENT --> KNOWLEDGE
    AGENT --> TIME
    ONEPROGRAM --> DEPLOY
    QUALITY --> KNOWLEDGE

    click DESIGN "../layers/design.md"
    click TIME "../layers/time.md"
    click CAUSALITY "../layers/causality.md"
    click BUILD "../layers/build.md"
    click RUNTIME "../layers/runtime.md"
    click ONEPROGRAM "../layers/one-program.md"
    click QUALITY "../layers/quality.md"
    click KNOWLEDGE "../layers/knowledge.md"
    click AGENT "../layers/agent.md"
    click DEPLOY "../layers/deploy.md"
```

Each node links to its layer page. The [package graph](packages.md) shows
the same picture at repository granularity; [the chain](chain.md) shows the
main artifact's path through it; [feedback loops](loops.md) shows how
failures travel back.
