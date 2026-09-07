# Claude Code plugin guide

This repository contains a native Claude Code plugin in `claude-plugin/`.
Claude Code plugins keep `.claude-plugin/plugin.json` at the plugin root and
discover `skills/` and `agents/` beside it. The manifest is pinned to version
`1.0.0`; review changes to that version and the complete plugin diff before
updating a deployed copy.

The [official plugin guide](https://code.claude.com/docs/en/plugins) explains
the layout and local development flow. Validate the package and load it for a
session with:

```bash
git clone https://github.com/harigovind-s-menon/chief-orchestration.git
cd chief-orchestration
claude plugin validate ./claude-plugin
claude --plugin-dir ./claude-plugin
```

Inside Claude Code, use `/chief-orchestration:chief-orchestration` for the
workflow skill. Plugin skills are namespaced by the manifest name. The agent
definitions are available as `@agent-chief-orchestration:commander`,
`@agent-chief-orchestration:implementer`, and
`@agent-chief-orchestration:verifier`; a main session can also start the
Commander with `claude --agent chief-orchestration:commander`. After local
edits, run `/reload-plugins`. See the [official plugin reference](https://code.claude.com/docs/en/plugins-reference)
and [subagent guide](https://code.claude.com/docs/en/sub-agents) for the
current command and frontmatter behavior.

## Installation scopes

For a personal installation, use the Claude Code plugin manager at user scope
(`~/.claude/settings.json`, the default). For a project installation shared by
a team, use project scope (`.claude/settings.json`) and commit the resulting
`enabledPlugins` entry after reviewing it. Local scope
(`.claude/settings.local.json`) is suitable for a developer-only checkout and
should normally remain gitignored. The [official settings guide](https://code.claude.com/docs/en/settings)
and [plugin installation guide](https://code.claude.com/docs/en/plugins)
describe these scopes and `enabledPlugins` configuration.

For a team or local marketplace, add a marketplace entry that points to a
reviewed repository or path, then install the pinned plugin through Claude
Code. For example, the official CLI route is:

```bash
claude plugin marketplace add ./my-marketplace --scope project
claude plugin install chief-orchestration@my-marketplace --scope project
```

The [official marketplace guide](https://code.claude.com/docs/en/plugin-marketplaces)
documents the marketplace schema, validation, local paths, GitHub sources, and
project scope. This repository does not publish or imply a marketplace listing.

## GitHub workflow

Keep project-specific instructions in that project's `CLAUDE.md` (and commit
them with the project when they are team guidance). The plugin does not rely on
a plugin-root `CLAUDE.md`; reusable workflow instructions live in its skill and
agent files.

For a change, start from an up-to-date branch, open or reference a GitHub
issue, create a focused branch, and keep the issue, branch, and pull request
scope aligned. Run `claude plugin validate ./claude-plugin` and the repository's
other checks locally. Review `git diff --check` and the complete diff, update
the pinned plugin version when releasing a change, and describe validation and
any limitations in the pull request. A maintainer reviews the pull request
before merging to `main`.

The plugin's role hierarchy is a process contract: Human ↔ Chief ↔ Commander ↔
Implementer / Verifier. A Commander owns one Chief-level task and delegates
bounded edits and checks. The Verifier independently reviews read-only and
returns PASS or actionable FAIL findings. Keep the implementation, independent
review, correction, and final project-wide gate in that order. Genuine product,
architecture, security, and requirements decisions move upward through the
same chain.

## Trust and permissions

Install only from a source you trust and review the plugin diff before enabling
it. A Claude Code plugin can supply prompts, skills, agents, hooks, and other
components; this package intentionally relies on skills and agents and does
not require hooks or MCP servers. Review the [official plugin security and
validation guidance](https://code.claude.com/docs/en/plugins-reference) and
your organization's Claude Code settings before enabling third-party content.

Users still need Claude Code installed, authenticated, and trusted for the
working directory. The `verifier` agent is restricted to read-oriented tools
and explicitly must not edit or run mutating commands. The `implementer` can
edit only within the Commander assignment, subject to the active session's
permissions and project policies.

Installing or loading this plugin does not grant GitHub credentials, repository
write access, network access, marketplace publication rights, or permissions in
any external service. Configure and authorize those separately, and inspect
commands before approving external actions.
