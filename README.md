# Chief Orchestration

`chief-orchestration` is a reusable workflow package for coordinating
substantial, multi-part implementation work through a strict agent hierarchy.
This repository provides both the existing Codex package and a native Claude
Code plugin with the same role boundaries.

The workflow is:

```text
Chief -> Commander -> Implementer
                    \-> Verifier
```

The Chief owns planning, task decomposition, dependencies, safe concurrency,
and the final completion gate. A Commander owns one bounded task and delegates
execution to Implementers, then requests independent review from a Verifier for
substantial work. Implementers stay within scope; Verifiers inspect and report
without fixing the implementation.

The Codex package includes the required role definitions in `agents/`:

- `commander.toml`
- `implementer.toml`
- `verifier.toml`

The skill and its plugin manifest are under `skills/chief-orchestration/` and
`.codex-plugin/`. See [INSTALL.md](INSTALL.md) for local setup. The package
does not install agents automatically or perform remote actions.

## Claude Code plugin

The native Claude Code package is under [`claude-plugin/`](claude-plugin/):

```text
claude-plugin/
├── .claude-plugin/plugin.json
├── agents/
│   ├── commander.md
│   ├── implementer.md
│   └── verifier.md
└── skills/chief-orchestration/SKILL.md
```

It uses Claude-compatible YAML frontmatter and model aliases. Test the local
plugin from this repository without installing it:

```bash
claude plugin validate ./claude-plugin
claude --plugin-dir ./claude-plugin
```

Then invoke `/chief-orchestration:chief-orchestration`, mention
`@agent-chief-orchestration:commander`, or launch the configured agent with
`claude --agent chief-orchestration:commander`. Run `/reload-plugins` after
editing a plugin in an active session. See [CLAUDE-CODE.md](CLAUDE-CODE.md) for
project installation scopes, GitHub workflow, security notes, and official
Claude Code documentation links.

The repository does not claim publication in a marketplace. A team may add the
plugin to a local or private marketplace when it has reviewed the source and
configured its own distribution process.

If this skill helps your work, optional support is welcome at
<https://buymeacoffee.com/harigovindsmenon>; payment is never required to use,
install, or contribute to the package.
