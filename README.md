# agent-marketplace

The published plugin marketplace for
[imtf-group/agent-control](https://github.com/imtf-group/agent-control).

> [!WARNING]
> Everything in this repository is **generated**. `plugins/`, both
> `marketplace.json` files, `.claude-plugin/` and `.agents/` are rewritten on
> every merge to `agent-control`'s `main` -- edit them by hand and the next
> publish silently overwrites you. Change the source in `agent-control`
> instead.

> Internal to IMTF. See [LICENSE](LICENSE).

## Why a separate repository

A plugin host clones this repo and reads each plugin's `source` path *as
committed* -- it never runs `apm install` and never runs any APM tooling
against this repo. This repository carries **no APM configuration of its
own**: no `apm.yml`, no lockfile, no version-check gate. All of that lives in
`agent-control`, where validation happens before anything ever reaches here.

## Consuming

```text
/plugin marketplace add imtf-group/agent-marketplace
/plugin install example@agent-marketplace
```

The repository is private, so the host needs a GitHub credential that can read
`imtf-group` -- an SSH source, or a PAT the host is configured with.

## What a bundle contains

| Path | What it is |
|---|---|
| `.claude-plugin/plugin.json` | Manifest for Claude Code / Desktop |
| `.codex-plugin/plugin.json` | Manifest for Codex / ChatGPT Desktop |
| `skills/<name>/SKILL.md` | The skills, with their `references/` and `scripts/` |
| `commands/<name>.md` | Prompts, converted to slash commands |
| `LICENSE`, `LICENSES/` | The terms, copied from the source package |

## Versioning

Each package versions independently, on a calendar scheme: `YYYY.MM.DD`, or
`YYYY.MM.DD.N` if the same package published more than once on the same day.
There is no semver here -- a package's version means "when this last changed",
not "how compatible this is with the last release".

## Regenerating

Never done by hand here. From a checkout of `agent-control` with this
repository as a sibling:

```bash
cd ../agent-control
python scripts/publish.py --repo . --marketplace ../agent-marketplace --push
```

In practice this runs automatically, via `agent-control`'s `publish.yml`
workflow, on every merge to its `main`.

| Generated file | Read by |
|---|---|
| `.claude-plugin/marketplace.json` | Claude Code, Claude Desktop |
| `.agents/plugins/marketplace.json` | Codex, ChatGPT Desktop |

## Releases land by direct push

`main` here has no branch protection: the `publish.yml` workflow in
`agent-control` is the only writer this repository has, authenticated with a
PAT held in that repo's `MARKETPLACE_TOKEN` secret. There is no gate here to
bypass -- everything reaching this repository has already passed
`agent-control`'s pre-merge checks.
