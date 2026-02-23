# Blackout Secure MCP Engine Demo

A **starter repository** demonstrating how to define MCP server metadata and generate a static registry using the [Blackout Secure MCP Registry Engine](https://github.com/blackoutsecure/bos-mcp-registry-engine).

## Quick Start

See live demos deployed on different platforms:

- **Cloudflare Pages**: [`registry-demo-cf` branch](https://github.com/blackoutsecure/bos-mcp-engine-demo/tree/registry-demo-cf) → https://demo.mcp.registry.blackoutsecure.dev/
- **GitHub Pages**: [`registry-demo-gh` branch](https://github.com/blackoutsecure/bos-mcp-engine-demo/tree/registry-demo-gh) → https://demo-gh.mcp.registry.blackoutsecure.dev/

## What This Shows

- Defining MCP servers under `servers/<name>/server.json`
- Versioned metadata under `servers/<name>/versions/<version>.json`
- Generating static registry output for static hosting platforms
- Deploying registries to Cloudflare Pages or GitHub Pages
- Keeping examples simple, public, and easy to understand

## Repository Structure

**`main` branch** (this branch):
- Clean starter template
- Workflow configurations
- Configuration files (CNAME, LICENSE, etc.)
- Server metadata added to demo branches

**`registry-demo-cf` branch**:
```
servers/
  echo-demo/
    server.json
    versions/
      1.0.0.json
  filesystem-demo/
    server.json
    versions/
      1.0.0.json
registry/v0.1/       (generated)
.github/workflows/   (Cloudflare deployment)
```

**`registry-demo-gh` branch**:
```
servers/
  (same structure)
registry/v0.1/       (generated)
.github/workflows/   (GitHub Pages deployment)
```

## How It Works

1. **Define** MCP servers in `servers/<name>/server.json` and versions
2. **Generate** static registry using the GitHub Action on each branch
3. **Deploy** generated output to static hosting (Cloudflare Pages or GitHub Pages)
4. **Access** through the live sites above

### The Engine

Registry generation is automated by the **Blackout Secure MCP Registry Engine** GitHub Action:

- **Marketplace**: https://github.com/marketplace/actions/blackout-secure-mcp-registry-engine
- **Source Code**: https://github.com/blackoutsecure/bos-mcp-registry-engine
- **Latest Action**: `blackoutsecure/bos-mcp-registry-engine@latest`

## Using This as a Template

1. Fork or clone this repository
2. Choose a demo branch (`registry-demo-cf` or `registry-demo-gh`) to start from
3. Add your MCP servers under `servers/<name>/`
4. Push to trigger the GitHub Action
5. Deploy to your chosen platform

For detailed examples, see the demo branches.

## Important Notes

- **Purpose**: This is a public demo repository. Use it as a template or reference.
- **Production**: Do not treat generated examples as production content.
- **Engine Implementation**: The registry engine lives in a separate [repository](https://github.com/blackoutsecure/bos-mcp-registry-engine).
- **Standards**: All JSON metadata follows [MCP registry schema](https://github.com/modelcontextprotocol/registry) conventions.

---

## Sponsored by Blackout Secure

This project is developed and maintained by **[Blackout Secure](https://blackoutsecure.app/)**.

[Blackout Secure](https://blackoutsecure.app/) provides open-source tools and infrastructure for secure model context protocol (MCP) implementations, enabling developers to build reliable and scalable AI integrations.
