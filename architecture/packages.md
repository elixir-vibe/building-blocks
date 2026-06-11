# Package graph

*Status: draft — edges verified against mix.exs/package.json files at
draft time; corrections welcome.*

Every published package, grouped by boundary, with its load-bearing
dependencies inside the ecosystem. External deps (Phoenix, LiveView, OTP)
are omitted for clarity — see each repo's mix.exs.

```mermaid
graph TD
    subgraph DESIGN["Design — layers/design.md"]
        openpencil["open-pencil"]
        figma-use["figma-use"]
        iconify_pkg["iconify"]
        phoenix_iconify["phoenix_iconify"]
        kiwi_codec["kiwi_codec"]
        fig_kiwi["fig_kiwi"]
        skia_ex["skia_ex"]
    end

    subgraph TIME["Time — layers/time.md"]
        phoenix_replay["phoenix_replay"]
    end

    subgraph CAUSALITY["Causality — layers/causality.md"]
        reach["reach"]
    end

    subgraph QUALITY["Quality — layers/quality.md"]
        ex_ast["ex_ast"]
        ex_dna["ex_dna"]
        ex_slop["ex_slop"]
        vibe_kit["vibe_kit"]
    end

    subgraph BUILD["Build — layers/build.md"]
        volt["volt"]
        oxc["oxc"]
        oxide["oxide"]
        npm_ex["npm"]
        npm_semver["npm_semver"]
    end

    subgraph RUNTIME["Runtime — layers/runtime.md"]
        quickbeam["quickbeam"]
    end

    subgraph ONEPROGRAM["One program — layers/one-program.md"]
        phoenix_vapor["phoenix_vapor"]
        vize["vize"]
    end

    subgraph KNOWLEDGE["Knowledge — layers/knowledge.md"]
        exograph["exograph"]
        quackdb["quackdb"]
    end

    subgraph AGENT["Agent — layers/agent.md"]
        pi_elixir["pi-elixir / pi_bridge"]
        vibe["vibe"]
    end

    subgraph DEPLOY["Deploy — layers/deploy.md"]
        xamal["xamal"]
        xamal_proxy["xamal_proxy"]
        livery["livery"]
        systemdkit["systemdkit"]
        unitctl["unitctl"]
    end

    fig_kiwi --> kiwi_codec
    phoenix_iconify --> iconify_pkg
    vibe_kit --> ex_dna
    vibe_kit --> ex_slop
    vibe_kit --> reach
    ex_slop -.-> ex_ast
    ex_dna -.-> ex_ast
    volt --> oxc
    volt --> oxide
    volt --> quickbeam
    volt --> npm_ex
    npm_ex --> npm_semver
    phoenix_vapor --> vize
    phoenix_vapor --> oxc
    phoenix_vapor --> quickbeam
    phoenix_vapor -.-> volt
    exograph --> ex_ast
    exograph --> quackdb
    exograph -.-> reach
    pi_elixir --> ex_ast
    pi_elixir -.-> reach
    pi_elixir -.-> quackdb
    vibe -.-> reach
    xamal_proxy --> xamal
    xamal -.-> systemdkit
    unitctl --> systemdkit
```

Solid = dependency. Dashed = optional/feeds.

## Repository index

| Package | Boundary | Repo | Hex |
|---|---|---|---|
| open-pencil | Design | [open-pencil/open-pencil](https://github.com/open-pencil/open-pencil) | — |
| figma-use | Design | [dannote/figma-use](https://github.com/dannote/figma-use) | — |
| iconify / phoenix_iconify | Design·Build | [elixir-volt](https://github.com/elixir-volt) | ✓ |
| kiwi_codec / fig_kiwi | Design | elixir-vibe | [verify] |
| skia_ex | Design | elixir-vibe | [verify] |
| phoenix_replay | Time | [elixir-vibe/phoenix_replay](https://github.com/elixir-vibe/phoenix_replay) | ✓ |
| reach | Causality | [elixir-vibe/reach](https://github.com/elixir-vibe/reach) | ✓ |
| ex_ast / ex_dna / ex_slop / vibe_kit | Quality | [elixir-vibe](https://github.com/elixir-vibe) | ✓ |
| volt / oxc / oxide / npm / npm_semver | Build | [elixir-volt](https://github.com/elixir-volt) | ✓ |
| quickbeam | Runtime | [elixir-volt/quickbeam](https://github.com/elixir-volt/quickbeam) | ✓ |
| phoenix_vapor / vize | One program | [elixir-volt](https://github.com/elixir-volt) | ✓ |
| exograph / quackdb | Knowledge | [elixir-vibe](https://github.com/elixir-vibe) | ✓ |
| pi-elixir / vibe | Agent | [elixir-vibe](https://github.com/elixir-vibe) | ✓ |
| xamal / xamal_proxy / livery / systemdkit / unitctl | Deploy | [verify orgs] | [verify] |
