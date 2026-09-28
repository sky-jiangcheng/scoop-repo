# scoop-repo

Scoop bucket for [RepoNest](https://github.com/sky-jiangcheng/RepoNest) — the
local-first code project context base.

## What is in here

| Manifest | Package | Artifact |
|----------|---------|----------|
| [`bucket/reponest.json`](bucket/reponest.json) | `reponest` | `reponest-windows-amd64.zip` — the Wails desktop app |
| [`bucket/reponest-mcp.json`](bucket/reponest-mcp.json) | `reponest-mcp` | `reponest-mcp-windows-amd64.zip` — the MCP stdio server |

## Install

```powershell
scoop bucket add repo https://github.com/sky-jiangcheng/scoop-repo

# Desktop app
scoop install repo/reponest

# MCP server
scoop install repo/reponest-mcp
```

After installing the MCP server, register it with an AI client:

```powershell
claude mcp add reponest -- (Get-Command reponest-mcp).Source
```

The desktop app is **not** required to use the MCP server. Both read the same
local SQLite database, and both share one `internal/service` implementation, so
their behaviour cannot drift.

## Updates

Each manifest carries a `checkver` that reads the newest release tag from the
GitHub API and an `autoupdate` block that rewrites the download URL from it. To
pull in a new version:

```powershell
scoop update
```

Scoop hashes the downloaded file locally when `autoupdate` has no `hash` block,
so a manifest whose `hash` has not been refreshed will still update cleanly.

## Maintenance

The manifests in `bucket/` are generated from
[`packaging/scoop/`](https://github.com/sky-jiangcheng/RepoNest/tree/master/packaging/scoop)
in the main repository. **Edit them there**, then re-publish; editing here will
be overwritten on the next release.

`hash` values come from the assets GitHub actually serves, not from a local
build of the same version — a binary compiled on a different host is a
different file, and a digest taken from it would reject every real download.
