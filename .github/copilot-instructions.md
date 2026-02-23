# Copilot Instructions for `bos-mcp-registry-demo`

## Branch Focus (`registry-demo-cf`)
- This branch is specifically for Cloudflare-focused registry demo work.
- Prioritize Cloudflare deployment paths for static registry output.
- Do not introduce Azure, AWS, GCP, or other cloud deployment guidance in this branch.
- Keep all deployment-related examples and workflow changes aligned to Cloudflare targets.

## Purpose
This repository is a demo MCP registry that shows:

- How to define MCP services under `servers/`
- How to version MCP metadata under `servers/<name>/versions/`
- How to generate the registry with the marketplace action:
  - `blackoutsecure/bos-mcp-registry-engine@latest`
- How to commit and deploy static registry output under `registry/v0.1/`

## Scope and Constraints
- Do not reimplement the registry engine in this repo.
- Do not add backend servers or dynamic hosting logic.
- Do not modify the engine action itself; only reference and use it.
- Do not add production or internal registry content.
- For this branch, keep deployment guidance focused on Cloudflare only.
- Do not add or suggest non-Cloudflare deployment platforms or workflows.

## Content Goals
- Keep examples simple, valid, and easy to understand.
- Ensure JSON follows MCP schema conventions.
- Keep folder structure clean and predictable.
- Keep workflows minimal and focused on demonstrating action usage.
- Use only public MCP server references that anyone can access.
- Prefer patterns and examples from `https://github.com/modelcontextprotocol/registry`.
- For this demo's metadata links, prefer homepage `https://github.com/marketplace/actions/blackout-secure-mcp-registry-engine`.
- For this demo's metadata links, prefer repository `https://github.com/blackoutsecure/bos-mcp-registry-engine`.
- For engine behavior, configuration, and usage patterns, prioritize `https://github.com/blackoutsecure/bos-mcp-registry-engine` as the primary reference.

## Preferred Patterns
- Favor small, illustrative MCP service examples.
- Prefer clarity and readability over abstraction.
- Generate static output suitable for static hosting platforms.
- Keep changes surgical and aligned with this repository’s demo purpose.
- For this branch, keep deployment examples and workflow intent Cloudflare-specific.
