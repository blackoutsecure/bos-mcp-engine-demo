# Copilot Instructions for `bos-mcp-engine-demo`

## Purpose
This repository is a **starter template and demo** showing how to:

- Define MCP services under `servers/<name>/server.json`
- Version MCP metadata under `servers/<name>/versions/<version>.json`
- Generate static registries using the marketplace GitHub Action
- Deploy to different platforms (Cloudflare Pages, GitHub Pages)

**See the demo branches for working examples:**
- `registry-demo-cf` - Cloudflare Pages deployment
- `registry-demo-gh` - GitHub Pages deployment

## Scope and Constraints
- Do not reimplement the registry engine in this repo.
- Do not add backend servers or dynamic hosting logic.
- Do not modify the engine action itself; only reference and use it.
- Keep the `main` branch clean as a starter template.
- Add actual server examples to demo branches, not main.
- Do not add production or internal registry content.

## Content Goals
- Keep examples simple, valid, and easy to understand.
- Ensure JSON follows MCP schema conventions.
- Keep folder structure clean and predictable.
- Keep workflows minimal and focused on demonstrating action usage.
- Use only public MCP server references that anyone can access.
- Prefer patterns and examples from `https://github.com/modelcontextprotocol/registry`.
- Reference the marketplace action: `blackoutsecure/bos-mcp-registry-engine@latest`

## Preferred Links
- **Engine Marketplace**: https://github.com/marketplace/actions/blackout-secure-mcp-registry-engine
- **Engine Repository**: https://github.com/blackoutsecure/bos-mcp-registry-engine
- **MCP Registry Standard**: https://github.com/modelcontextprotocol/registry
- **Live Demos**:
  - Cloudflare: https://demo.mcp.registry.blackoutsecure.dev/
  - GitHub Pages: https://demo-gh.mcp.registry.blackoutsecure.dev/

## Preferred Patterns
- Favor small, illustrative MCP service examples.
- Prefer clarity and readability over abstraction.
- Generate static output suitable for static hosting platforms.
- Keep changes surgical and aligned with this repository's demo purpose.
- Document deployment configurations clearly in demo branches.
