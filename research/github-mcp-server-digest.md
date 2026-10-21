# GitHub MCP Server — Adoption Evaluation Digest

*Compiled from the public `github/github-mcp-server` repository (README, releases, open pull requests) and the official Model Context Protocol documentation site. All figures are reported exactly as returned by the sources at the time of research.*

---

## 1. What it is

The GitHub MCP Server connects AI tools directly to GitHub's platform. It gives AI agents, assistants, and chatbots the ability to read repositories and code files, manage issues and PRs, analyze code, and automate workflows — all through natural language interactions.

README use cases:

- **Repository Management** — browse/query code, search files, analyze commits, understand project structure.
- **Issue & PR Automation** — create, update, manage issues and PRs; triage bugs; review code changes; maintain project boards.
- **CI/CD & Workflow Intelligence** — monitor GitHub Actions runs, analyze build failures, manage releases.
- **Code Analysis** — security findings, Dependabot alerts, code patterns.
- **Team Collaboration** — discussions, notifications, team activity.

Repository: https://github.com/github/github-mcp-server (description: "GitHub's official MCP Server", created 2025-03-04T16:42:04Z).

## 2. The two ways to run it

### 2.1 Remote (hosted by GitHub) — recommended starting point

- The remote GitHub MCP Server is **hosted by GitHub** and provides the easiest method for getting up and running.
- Endpoint: `https://api.githubcopilot.com/mcp/` (HTTP type in host config).
- Prerequisites: a compatible MCP host with remote server support (**VS Code 1.101+**, Claude Desktop, Cursor, Windsurf, etc.) and any applicable policies enabled.
- Authentication: OAuth (hosts must configure a GitHub App or OAuth App) **or** a GitHub PAT via an `Authorization: Bearer` header.
- Also usable from GitHub Enterprise Cloud with data residency (`ghe.com`), e.g. `https://copilot-api.YOURSUBDOMAIN.ghe.com/mcp`.
- **GitHub Enterprise Server does not support remote server hosting** — it must use the local server.
- An **Insiders** version offers early access to new features and experimental tools (`/mcp/insiders` path or `X-MCP-Insiders: true` header).

### 2.2 Local (run it yourself, e.g. in Docker with a PAT)

- Run the public Docker image `ghcr.io/github/github-mcp-server` yourself (Docker required).
- Authentication on github.com needs nothing up front for OAuth — a browser-based one-click login on first use keeps the token **in memory only**; in Docker a fixed callback port is published to loopback (`127.0.0.1:8085`).
- **Or authenticate with a GitHub Personal Access Token** by setting `GITHUB_PERSONAL_ACCESS_TOKEN` (it takes precedence over OAuth).
- Example:

```bash
docker run -i --rm \
  -e GITHUB_PERSONAL_ACCESS_TOKEN=<your-token> \
  -e GITHUB_TOOLSETS="repos,issues,pull_requests,actions,code_security" \
  ghcr.io/github/github-mcp-server
```

- `--gh-host` / `GITHUB_HOST` point the local server at GitHub Enterprise Server or `ghe.com` (HTTPS required and enforced).
- Alternative to Docker: build from source with `go build ./cmd/github-mcp-server` and run `github-mcp-server stdio` with `GITHUB_PERSONAL_ACCESS_TOKEN` set.
- The README also documents PAT handling best practices: store in environment variables, `.gitignore` `.env`, minimum scopes, separate tokens per project, regular rotation.

## 3. Toolsets

Toolsets let you enable/disable groups of functionality via `--toolsets` (or `GITHUB_TOOLSETS`), and individual tools via `--tools` (or `GITHUB_TOOLS`); env vars take precedence over the CLI flag. Enabling only needed toolsets helps the LLM with tool choice and reduces context size. A `--read-only` mode takes priority over explicit tool requests. Toolsets are not limited to tools — relevant MCP Resources and Prompts are included where applicable.

### Default toolset (used when no toolsets are specified)

- `context`
- `repos`
- `issues`
- `pull_requests`
- `users`

To keep the defaults and add more: `GITHUB_TOOLSETS="default,stargazers"`.

### Available toolsets

`context`, `actions`, `code_quality`, `code_security`, `copilot`, `copilot_issue_intents`, `dependabot`, `discussions`, `gists`, `git`, `governance`, `issues`, `labels`, `notifications`, `orgs`, `projects`, `pull_requests`, `repos`, `secret_protection`, `security_advisories`, `stargazers`, `users`

Special toolset `all` enables every toolset regardless of other configuration.

### Additional toolsets in the Remote GitHub MCP Server only

`copilot`, `copilot_spaces`, `github_support_docs_search`

## 4. License

**MIT License**, Copyright (c) 2025 GitHub (per the repository `LICENSE` file).

## 5. Latest release — and the cross-check

- **Latest published release: `v2.0.2`**, release name "GitHub MCP Server 2.0.2".
- Release ID: 406753657; created at 2026-10-08T11:26:10Z; published at 2026-10-08T11:33:15Z; target commitish `main`; not a draft, not a prerelease.
- Release notes: "## Bugfix release — fix(inventory): restore typed tool HTTP header validation by @SamMorrowDrums in #3454"; full changelog `v2.0.1...v2.0.2`.
- Git tag `v2.0.2` (tag object SHA `503622d154b505a9cebff296a13bc6c9259ea993`, message "Release v2.0.2", tagger Sam Morrow, tagger date 2026-10-08T11:26:10Z) points at commit `eb47a99ddb866ca2b8a162920e6bda9521f33ebb`. The tag is unsigned (verification: not verified / unsigned).
- Notable release assets (sizes and download counts exactly as reported by the API at research time; download counts drift continuously, so expect slightly higher numbers by the time you read this):
  - `github-mcp-server_2.0.2_checksums.txt` — 824 bytes, 3407 downloads, digest `sha256:786ee910d74ff2ee8ad328cdad40a29cf1270ec96aae8217b044bb274d09424f`
  - `github-mcp-server_Darwin_arm64.tar.gz` — 8848361 bytes, 7202 downloads
  - `github-mcp-server_Darwin_x86_64.tar.gz` — 9427411 bytes, 483 downloads
  - `github-mcp-server_Linux_arm64.tar.gz` — 8417035 bytes, 1409 downloads
  - `github-mcp-server_Linux_i386.tar.gz` — 8742842 bytes, 45 downloads

### Cross-check result: ✅ AGREE

The version shown on the releases page (`v2.0.2`, the first/latest entry returned when listing releases) **agrees** with the release data GitHub itself reports for that same tag when queried directly by tag name: both return tag `v2.0.2`, name "GitHub MCP Server 2.0.2", release ID 406753657, created at 2026-10-08T11:26:10Z, published at 2026-10-08T11:33:15Z, draft=false, prerelease=false. No discrepancy was found, so there is nothing to flag — if the two had disagreed, that would have been reported loudly as a data-quality problem.

## 6. Activity check — open pull request examined

**PR #3534 — "fix: make jd license URL deterministic before grouping reports"** (open)
https://github.com/github/github-mcp-server/pull/3534

- Author: IrynaKulakova (CONTRIBUTOR); opened 2026-10-19T15:23:57Z; last updated 2026-10-20T14:14:33Z; not a draft.
- Head branch: `github:irynakulakova-pr-3490-feedback` (commit `224cb62ca2474cc1684557d95183b6ff629c9771`).
- Purpose: normalize an inconsistent `jd/v2` license URL before sorting/hashing the architecture license reports, so identical reports group consistently and license checks don't spuriously fail. It addresses review feedback on #3490.

What the real code diff changes (4 files, no MCP tool changes):

1. **`script/licenses` (+5/−1)** — the core change. The report-gathering pipeline previously did `grep -E '^ - [' "${report_file}" | LC_ALL=C sort`. It now pipes through a `sed` that rewrites the version-specific bad URL `https://github.com/josephburnett/jd/blob/v2.5.0/v2/LICENSE` to the canonical `https://github.com/josephburnett/jd/blob/v2.5.0/LICENSE` before the `LC_ALL=C sort` and the subsequent hash computation. A comment explains that `go-licenses` can resolve `jd/v2`'s license to a nonexistent `v2/LICENSE` because at v2.5.0 it lives at the repository root.
2. **`third-party-licenses.darwin.md` (+1/−1)**, **`third-party-licenses.linux.md` (+1/−1)**, **`third-party-licenses.windows.md` (+1/−1)** — each regenerated so the single `github.com/josephburnett/jd/v2` line points at the canonical `blob/v2.5.0/LICENSE` instead of `blob/v2.5.0/v2/LICENSE`.

The PR's own checklist confirms: no tool or API changes, no security/limits impact, license validation failure gate preserved, workflow notification behavior unchanged; `./script/lint` and `./script/test` pass locally.

**Reviews so far: 3, none approving yet.**

- Two from `copilot-pull-request-reviewer[bot]` (2026-10-19T15:30:11Z and 2026-10-20T12:06:43Z), both "Approval recommended / no open issues" (state COMMENTED).
- One human review from JoannaaKL (COLLABORATOR, 2026-10-20T14:14:33Z, state COMMENTED) — the most recent activity. Its inline comment on `script/licenses` pushes back on the pinning approach and suggests making the normalization **generic** (drop any `/vN/` folder from license URLs via a regex) rather than hard-coding `jd` v2.5.0, so the fix doesn't quietly stop matching when `jd` is bumped. That suggestion appears unaddressed as of this research.

Further activity signals: the repository's navigation at research time showed **193 open issues** and **188 open pull requests**; the repo was pushed to at 2026-10-19T15:23:31Z.

Activity read: the project is clearly active — latest release published 2026-10-08, an open PR opened and updated within the last two days, ongoing maintainer review, and automated Copilot review on PRs.

## 7. Background: what is the Model Context Protocol (MCP)?

MCP (Model Context Protocol) is an **open-source standard for connecting AI applications to external systems**. Using MCP, AI applications like Claude or ChatGPT can connect to data sources (e.g. local files, databases), tools (e.g. search engines, calculators) and workflows (e.g. specialized prompts) — enabling them to access key information and perform tasks. The official documentation describes it as being "like a USB-C port for AI applications": just as USB-C provides a standardized way to connect electronic devices, MCP provides a standardized way to connect AI applications to external systems.

Why it matters: developers get reduced development time and complexity when building or integrating with AI applications or agents; AI applications and agents gain access to an ecosystem of data sources, tools and apps that enhances their capabilities; and end users get more capable applications that can access user data and take actions on the user's behalf. MCP is an open protocol with broad ecosystem support — Claude, ChatGPT, VS Code, Cursor, MCPJam and many others all support it, making it easy to build once and integrate everywhere.

Source: https://modelcontextprotocol.io/docs/getting-started/intro

## 8. Notifications check

An attempt was made to skim the requesting user's GitHub notifications, but the GitHub API returned `403 Resource not accessible by personal access token` for the notifications endpoint. This is a token-scope limitation of the credentials used for this research, not an empty notification list — the notifications should be checked manually in the GitHub web UI, or the token should be granted the notifications scope.

---

## Summary assessment for the team

- **Maturity/licensing**: MIT licensed, official GitHub project, releasing regularly (v2.0.2 on 2026-10-08).
- **Deployment choice**: remote hosted endpoint is the fastest path (no infra, OAuth or PAT); local Docker image is available for cases where data must not leave our perimeter or when GitHub Enterprise Server is in play.
- **Control**: toolsets give a real, meaningful blast-radius control — start from the `default` toolset and consider `--read-only` mode for trials.
- **Activity**: active development, responsive human review on PRs, automated review as a first pass.
