# AGENTS.md

This file provides guidance to agents when working with code in this repository.

## What this is

`bos-mcp-engine-demo` is a starter template and public demonstration repository for the
[Blackout Secure MCP Registry Engine](https://github.com/blackoutsecure/bos-mcp-registry-engine).
It shows how to describe MCP services as split JSON manifests (`servers/<name>/server.json`
plus `servers/<name>/versions/<version>.json`), how to invoke the engine as a GitHub Action
to turn that tree into a static registry, and how to publish the generated output to two
different static hosts.

There is no engine code and no application code here. The repository holds no runtime, no
build system, no `package.json`, no tests, and no backend. Every generated artefact in this
repository is produced by `blackoutsecure/bos-mcp-registry-engine`, which lives in its own
repository and is the only place engine behaviour may be changed. The default branch is a
seven-file skeleton: `.github/copilot-instructions.md`, the two deployment workflows,
`.gitignore`, `CNAME`, `LICENSE`, and `README.md`.

`.github/copilot-instructions.md` is the authoritative constraints document for this
repository and is binding. Its purpose, scope constraints, content goals, preferred links,
and preferred patterns are restated throughout this file; where the two ever disagree, that
file wins.

## Branch layout

Branch choice is the single most important thing to get right here.

- The default branch (`dev`, with `main` as the promoted counterpart) is deliberately kept
  clean as a starter template. It carries workflow configuration and root files only, and no
  `servers/` directory.
- `registry-demo-cf` holds the working Cloudflare Pages example: `servers/echo-demo/` and
  `servers/filesystem-demo/`, the Cloudflare workflow, and committed generated output under
  `dist/public/` including `_headers` and `_redirects`.
- `registry-demo-gh` holds the working GitHub Pages example: the same two `servers/` entries,
  the GitHub Pages workflow, a `CNAME`, and committed generated output under `dist/public/`
  including `.nojekyll` and extensionless aliases alongside the `.json` files.

Add example servers and deployment configuration to the demo branches, not the default
branch. Keep the default branch a clean starter template.

## Commands

There is no local build system in this repository, so exercising it means driving the sibling
engine or inspecting JSON directly.

```bash
# Confirm the demo branches and switch to one before adding examples
git -C /Volumes/devbox/repos/blackoutsecure/bos-mcp-engine-demo branch -a
git -C /Volumes/devbox/repos/blackoutsecure/bos-mcp-engine-demo checkout registry-demo-cf

# Run the engine locally against a servers/ tree, from the sibling engine checkout
cd /Volumes/devbox/repos/blackoutsecure/bos-mcp-registry-engine
npm install
npm run validate    # node src/index.js --action-type validate_registry
npm run generate    # node src/index.js --action-type generate_registry

# Point the engine at this repository's servers/ tree and choose a hosting profile
node src/index.js --action-type validate_registry \
  --source /Volumes/devbox/repos/blackoutsecure/bos-mcp-engine-demo/servers
node src/index.js --action-type generate_registry \
  --source /Volumes/devbox/repos/blackoutsecure/bos-mcp-engine-demo/servers \
  --output dist --public-directory public --deployment-environment cloudflare

# Validate a manifest without the engine
python3 -m json.tool servers/echo-demo/server.json
jq . servers/echo-demo/versions/1.0.0.json
```

## Architecture

The manifest convention this demo teaches, on the demo branches:

```text
servers/
  <name>/
    server.json                 identity: name, title, description, author,
                                websiteUrl, repository{url,source}, license,
                                categories, tags
    versions/
      <version>.json            version, releaseDate, packages[], capabilities,
                                tools[], resources[], configuration
```

Server names are namespaced, for example `io.blackoutsecure/echo-demo`, and the version file
is named after the semantic version it declares (`versions/1.0.0.json`).

At the root of the default branch: `README.md` (the public explanation and links), `CNAME`
(`demo-gh.mcp.registry.blackoutsecure.dev`, the GitHub Pages custom domain), `LICENSE`
(Apache-2.0), `.gitignore`, and `.github/`.

The workflows write generated artefacts to `dist/public/` — a versioned `v0.1/` tree with
`servers.json`, `servers/index.json`, per-server `versions.json`, `versions/<version>.json`,
`versions/latest.json`, plus `health.json`, `ping.json`, `version.json` and an `index.html`.
Note that `.gitignore` lists `dist/`, yet the demo branches deliberately commit that output
because the workflows run with `commit_generated_artifacts: true`.

## Deployment workflows

Both workflows call `blackoutsecure/bos-mcp-registry-engine` with
`action_type: generate_registry`, `source: ./servers`, `output_directory: dist`,
`output: public`, `commit_generated_artifacts: true`, and `log_level: debug`. Neither passes
`cloudflare_lean_output`, so the engine default (`true`) applies; it only takes effect when
`deployment_environment` is `cloudflare`.

### `deploy-registry-demo-cf.yml`

Triggers on `push` to `registry-demo-cf` limited to `servers/**`, and on `workflow_dispatch`.
Permissions are `contents: write`. It checks out, runs the engine with
`deployment_environment: cloudflare` (emitting `_headers` and `_redirects`), then deploys
`dist/public` with `cloudflare/wrangler-action` to the Cloudflare Pages project
`bos-mcp-registry-demo`, using `CLOUDFLARE_API_TOKEN` and `CLOUDFLARE_ACCOUNT_ID` secrets.
Published at <https://demo.mcp.registry.blackoutsecure.dev/>.

### `deploy-registry-demo-gh.yml`

Triggers on `push` to `registry-demo-gh` limited to `servers/**`, and on `workflow_dispatch`.
Permissions are `contents: write`, `pages: write`, `id-token: write`, and the job targets the
`github-pages` environment. It checks out, runs the engine with
`deployment_environment: github` (emitting `.nojekyll`), uploads `dist/public` with
`actions/upload-pages-artifact`, then publishes with `actions/deploy-pages`.
Published at <https://demo-gh.mcp.registry.blackoutsecure.dev/>.

## Conventions

- Follow MCP registry schema conventions; the engine validates every manifest against its own
  `server.schema.json` and `version.schema.json` and a malformed file fails the run.
- Keep examples small, valid, and easy to understand. Two servers with one version each is the
  established size of this demo.
- Use only public MCP server references that anyone can access. No private endpoints, no
  internal hostnames.
- Prefer patterns and examples from the upstream `modelcontextprotocol/registry` repository
  over inventing new shapes.
- Keep the folder structure predictable: `servers/<name>/server.json` and
  `servers/<name>/versions/<version>.json`, nothing else under `servers/`.
- Keep workflows minimal and focused on demonstrating how to call the engine action.
- Prefer clarity and readability over abstraction; keep changes surgical and aligned with this
  repository's demo purpose.

## Preferred links

- Engine Marketplace: <https://github.com/marketplace/actions/blackout-secure-mcp-registry-engine>
- Engine Repository: <https://github.com/blackoutsecure/bos-mcp-registry-engine>
- MCP Registry Standard: <https://github.com/modelcontextprotocol/registry>
- Live demo (Cloudflare): <https://demo.mcp.registry.blackoutsecure.dev/>
- Live demo (GitHub Pages): <https://demo-gh.mcp.registry.blackoutsecure.dev/>
- Marketplace action reference: `blackoutsecure/bos-mcp-registry-engine@latest`

## Blackout Secure conventions

These apply to every repository in the `blackoutsecure` organization.

### Branch model

- The default branch is where all work lands; `main` is the promoted stable runtime that
  consumers reference through `@main`.
- Version tags (`vX.Y.Z` and a floating `vX`) point at promoted runtime commits.
- Promotion is driven from `bos-automation-hub`. Do not push directly to `main` and do not
  move tags by hand.

### Centrally managed files - do not hand-edit here

`blackoutsecure/bos-automation-hub` distributes these through
`bos-managed-file-sync-action`. Change the source under the hub's `sync-files/`, never the
copy in this repository:

- `LICENSE`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`
- `.github/FUNDING.yml`, `.github/PULL_REQUEST_TEMPLATE.md`, `.github/ISSUE_TEMPLATE/`
- any managed kicker workflow under `.github/workflows/`
- the managed-file-sync delimited blocks inside `.editorconfig`, `.markdownlint.yaml`,
  `.shellcheckrc`, `.yamllint.yml`, `.gitignore`, and `README.md`

### CI gate

Pushes and pull requests run the hub's reusable `bos-universal-security.yml`, reported as a
single required check. It runs markdownlint, yamllint, shellcheck, and actionlint;
`bos-code-scanning-kit` (secret scan, SAST, GHAS posture) and CodeQL; dependency review;
and compliance checks for the canonical README header and a conventional-commit PR title
(`feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert: subject`).

Every `uses:` reference in a workflow must be a commit SHA with a trailing version comment,
for example `actions/checkout@<sha> # v4.2.2`.

## Boundaries

### Always

- Read `.github/copilot-instructions.md` first and treat it as binding.
- Add example servers and deployment configuration to `registry-demo-cf` or
  `registry-demo-gh`, and keep the default branch a clean starter template.
- Keep manifests valid against the engine schemas; validate with
  `node src/index.js --action-type validate_registry` from the sibling engine checkout, or at
  minimum with `python3 -m json.tool` or `jq`, before committing.
- Keep both demo branches structurally aligned so the only meaningful difference is the
  hosting profile and its deployment workflow.
- Keep the two workflows in step with the engine contract in
  `bos-mcp-registry-engine/action.yml`; `deployment_environment` accepts only `github`,
  `cloudflare`, or `none`.

### Ask first

- Adding a third demo branch or a third hosting profile.
- Changing `CNAME` or any published demo URL; both are live and referenced from `README.md`
  and the engine documentation.
- Changing the engine inputs a workflow passes, or the `dist/public` output layout that the
  live sites already serve.
- Adding a new example server, or growing an existing one beyond the small, illustrative scope
  the demo is meant to keep.
- Adding any repository-local tooling — linters, `package.json`, tests, config files — to a
  repository that deliberately has none.

### Never

- Never reimplement the registry engine here; generation logic belongs only in
  `blackoutsecure/bos-mcp-registry-engine`.
- Never add backend servers, dynamic hosting logic, or anything requiring server compute; the
  output is static files served by a static host.
- Never modify the engine action itself from this repository; only reference and use it.
- Never add production or internal registry content, private MCP server references, internal
  endpoints, or anything a member of the public could not reach.
- Never add example servers to the default branch.
- Never commit secrets, tokens, or credentials; `CLOUDFLARE_API_TOKEN` and
  `CLOUDFLARE_ACCOUNT_ID` are repository secrets and stay that way.
- Never use an unpinned `uses:` ref; every reference must be a 40-character commit SHA with a
  trailing version comment.
