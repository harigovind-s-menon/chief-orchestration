# Chief Orchestration

`chief-orchestration` is a reusable Codex skill for coordinating substantial,
multi-part implementation work through a strict agent hierarchy.

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

The package includes the required role definitions in `agents/`:

- `commander.toml`
- `implementer.toml`
- `verifier.toml`

The skill and its plugin manifest are under `skills/chief-orchestration/` and
`.codex-plugin/`. See [INSTALL.md](INSTALL.md) for local setup. The package
does not install agents automatically or perform remote actions.

If this skill helps your work, optional support is welcome at
<https://buymeacoffee.com/harigovindsmenon>; payment is never required to use,
install, or contribute to the package.
