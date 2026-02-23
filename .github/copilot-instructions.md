# Copilot Instructions for `bos-mcp-registry-demo`

## Purpose
This repository is a demo MCP registry that shows:

- How to define MCP services under `servers/`
- How to version MCP metadata under `servers/<name>/versions/`
- How to generate the registry with the marketplace action:
  - `blackoutsecure/bos-mcp-registry-engine@latest`
- How to commit and deploy static registry output under `registry/v0.1/`

## Scope and Constraints
- This branch (`registry-demo-gh`) is **GitHub Pages focused** for static hosting only.
- Do not implement deployments for other cloud platforms (Cloudflare, AWS, Azure, etc) on this branch.
- Do not reimplement the registry engine in this repo.
- Do not add backend servers or dynamic hosting logic.
- Do not modify the engine action itself; only reference and use it.
- Do not add production or internal registry content.

## Content Goals
- Keep examples simple, valid, and easy to understand.
- Ensure JSON follows MCP schema conventions.
- Keep folder structure clean and predictable.
- Keep workflows minimal and focused on demonstrating action usage.
- Use only public MCP server references that anyone can access.
- Prefer patterns and examples from `https://github.com/modelcontextprotocol/registry`.
- For this demo's metadata links, prefer homepage `https://github.com/marketplace/actions/blackout-secure-mcp-registry-engine`.
- For this demo's metadata links, prefer repository `https://github.com/blackoutsecure/bos-mcp-registry-engine`.

## Deployment
- **Primary Platform**: GitHub Pages (via GitHub Actions)
- Registry is deployed automatically to GitHub Pages on push to `registry-demo-gh` branch
- Static artifacts are committed to the repository and served via GitHub Pages

## Preferred Patterns
- Favor small, illustrative MCP service examples.
- Prefer clarity and readability over abstraction.
- Generate static output suitable for GitHub Pages hosting.
- Keep changes surgical and aligned with this repository’s demo purpose.
