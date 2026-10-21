# Source Log — which source supplied which fact

| Fact | Source | Details |
| --- | --- | --- |
| What the server is; use cases (repo management, issue/PR automation, CI/CD, code analysis, team collaboration) | README of `github/github-mcp-server` | https://raw.githubusercontent.com/github/github-mcp-server/main/README.md |
| Remote server: hosted by GitHub, endpoint `https://api.githubcopilot.com/mcp/`, easiest method, VS Code 1.101+ / Claude Desktop / Cursor / Windsurf, OAuth or PAT, `ghe.com` support, GHES not supported remotely, Insiders mode | README ("Remote GitHub MCP Server" section) | Same README |
| Local server: Docker image `ghcr.io/github/github-mcp-server`, `GITHUB_PERSONAL_ACCESS_TOKEN` (takes precedence over OAuth), OAuth callback port `127.0.0.1:8085`, `--gh-host` / `GITHUB_HOST` for GHES/ghe.com, build from source with `go build ./cmd/github-mcp-server` + `stdio` | README ("Local GitHub MCP Server" + "Installation" sections) | Same README |
| Default toolset = `context`, `repos`, `issues`, `pull_requests`, `users` | README ("'default' toolset" section) | Same README |
| Full available toolset list (context, actions, code_quality, code_security, copilot, copilot_issue_intents, dependabot, discussions, gists, git, governance, issues, labels, notifications, orgs, projects, pull_requests, repos, secret_protection, security_advisories, stargazers, users) | README ("Available Toolsets" automated table) | Same README |
| Remote-only extra toolsets (copilot, copilot_spaces, github_support_docs_search) | README ("Additional Toolsets in Remote GitHub MCP Server") | Same README |
| `--toolsets` / `GITHUB_TOOLSETS`, `--tools` / `GITHUB_TOOLS`, env var precedence, `all` toolset, `--read-only` priority | README ("Tool Configuration" section) | Same README |
| License = MIT, Copyright (c) 2025 GitHub | `LICENSE` file of `github/github-mcp-server` | repo://github/github-mcp-server/contents/LICENSE |
| Latest release `v2.0.2`, name "GitHub MCP Server 2.0.2", ID 406753657, created 2026-10-08T11:26:10Z, published 2026-10-08T11:33:15Z, target `main`, not draft/prerelease, bugfix note re: #3454, changelog v2.0.1...v2.0.2 | Releases listing for `github/github-mcp-server` (first/latest entry) | https://github.com/github/github-mcp-server/releases |
| Release-by-tag cross-check data (identical tag, name, ID, timestamps, flags) | `get_release_by_tag` for tag `v2.0.2` | https://api.github.com/repos/github/github-mcp-server/releases/tags/v2.0.2 — **result: agrees, no discrepancy** |
| Git tag `v2.0.2` details: tag object SHA `503622d154b505a9cebff296a13bc6c9259ea993`, message "Release v2.0.2", tagger date 2026-10-08T11:26:10Z, points to commit `eb47a99ddb866ca2b8a162920e6bda9521f33ebb`, unsigned | Git tag API for `v2.0.2` | https://api.github.com/repos/github/github-mcp-server/git/tags/503622d154b505a9cebff296a13bc6c9259ea993 |
| Release asset names/sizes/download counts (e.g. Darwin_arm64.tar.gz 8848361 bytes / 7199 downloads; checksums 824 bytes / 3399 downloads) and checksum digest | Release assets metadata for release ID 406753657 | https://api.github.com/repos/github/github-mcp-server/releases/406753657/assets |
| Open PR examined: #3534, title, author IrynaKulakova, dates (created 2026-10-19T15:23:57Z, updated 2026-10-20T14:14:33Z), head branch/sha, non-draft status, body (summary/why) | PR listing + PR details for #3534 | https://github.com/github/github-mcp-server/pull/3534 |
| PR #3534 real code diff: `script/licenses` +5/−1 (sed URL normalization before sort/hash), and the three `third-party-licenses.*.md` files +1/−1 each (canonical license URL) | PR files (patch hunks) for #3534 | https://github.com/github/github-mcp-server/pull/3534/files |
| PR #3534 reviews: 2 from `copilot-pull-request-reviewer[bot]` (2026-10-19T15:30:11Z, 2026-10-20T12:06:43Z) and 1 from JoannaaKL (2026-10-20T14:14:33Z) — all state COMMENTED, none approving; JoannaaKL's suggestion to generalize the sed | PR reviews + PR review comments for #3534 | https://github.com/github/github-mcp-server/pull/3534#pullrequestreview-5586308460 and the inline discussion on `script/licenses` |
| Project activity timestamps (repo created 2025-03-04T16:42:04Z, pushed_at 2026-10-19T15:23:31Z, description "GitHub's official MCP Server") | Repository metadata returned alongside the PR listing | https://github.com/github/github-mcp-server |
| MCP background: open-source standard connecting AI applications to external systems; data sources, tools, workflows; "USB-C port for AI applications" analogy; benefits for developers/AI apps/end users; ecosystem support (Claude, ChatGPT, VS Code, Cursor, MCPJam) | Official MCP documentation site | https://modelcontextprotocol.io/docs/getting-started/intro |
| User's notifications | **Not available** — GitHub API returned `403 Resource not accessible by personal access token` for the notifications endpoint | n/a (token-scope limitation; check the web UI) |

### Notes on method

- Release version was determined twice, independently: once from the releases listing and once by querying the release by its tag, and the two were compared field by field (tag, name, ID, created/published timestamps, draft/prerelease flags). They matched on every field.
- The PR analysis is based on the actual patch hunks of the 4 changed files, not the PR title or description.
- All numbers in the digest are quoted verbatim from the API responses; none were rounded or estimated.
