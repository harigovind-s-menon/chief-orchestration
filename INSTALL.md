# Chief Orchestration

This repository packages the reusable `chief-orchestration` Codex skill and the
role definitions it expects. Maintainer: `harigovind-s-menon`.

## Local installation

1. Copy `skills/chief-orchestration` into your personal Codex skills directory
   (normally `%USERPROFILE%\.codex\skills\chief-orchestration` on Windows or
   `~/.codex/skills/chief-orchestration` on Unix-like systems).
2. Copy the three TOML files in `agents/` into your personal agent directory
   (normally `%USERPROFILE%\.codex\agents` on Windows or `~/.codex/agents` on
   Unix-like systems). Preserve the filenames so the `commander`,
   `implementer`, and `verifier` selectors remain available.
3. Restart Codex or start a new session, then invoke the skill with
   `$chief-orchestration` for substantial multi-part work.

The root `agents/` directory is deliberate: it keeps the personal runtime
configuration together in a clear repository location while the skill's
`skills/chief-orchestration/agents/openai.yaml` remains the plugin UI manifest.
The TOMLs are packaged copies of the current personal role definitions; review
them before installing if your local runtime has changed.

Optional support is welcome at
<https://buymeacoffee.com/harigovindsmenon>; payment is never required to use
or install this package.
