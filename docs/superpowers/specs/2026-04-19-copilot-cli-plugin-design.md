# GitHub Copilot CLI Plugin — Design Spec

**Date:** 2026-04-19
**Status:** Approved

## Goal

Publish the tinify-ai plugin to GitHub Copilot CLI, enabling both direct install and marketplace discovery, without affecting the existing Claude Code or Cursor plugin distributions.

## Constraints

- No changes to `.claude-plugin/` or `.cursor-plugin/` directories
- No changes to existing `skills/`, `.mcp.json`, or any other shared assets
- Two new files only

## Files to Create

```
tinify-ai-plugin/
├── plugin.json                     ← NEW: Copilot root manifest
├── .github/
│   └── plugin/
│       └── marketplace.json        ← NEW: marketplace entry
│
├── .claude-plugin/plugin.json      (unchanged)
├── .cursor-plugin/plugin.json      (unchanged)
├── skills/                         (unchanged, already Copilot-compatible)
│   ├── optimize-images/SKILL.md
│   ├── image-seo/SKILL.md
│   └── web-performance-images/SKILL.md
└── .mcp.json                       (unchanged)
```

## File Contents

### `/plugin.json`

```json
{
  "name": "tinify-ai",
  "description": "AI-powered image optimization — compress, resize, upscale, convert formats, and generate SEO metadata. Includes skills for project-wide image auditing, SEO, and web performance.",
  "version": "1.0.0",
  "author": {
    "name": "tinify.ai",
    "email": "hello@tinify.ai",
    "url": "https://tinify.ai"
  },
  "homepage": "https://tinify.ai",
  "repository": "https://github.com/onepunchtechnology/tinify-ai-plugin",
  "license": "MIT",
  "keywords": ["image", "optimization", "compression", "seo", "web-performance", "webp", "avif"],
  "skills": "skills/",
  "mcpServers": ".mcp.json"
}
```

### `/.github/plugin/marketplace.json`

```json
{
  "name": "tinify-ai-marketplace",
  "owner": {
    "name": "tinify.ai",
    "email": "hello@tinify.ai"
  },
  "metadata": {
    "description": "AI-powered image optimization for developers",
    "version": "1.0.0"
  },
  "plugins": [
    {
      "name": "tinify-ai",
      "description": "AI-powered image optimization — compress, resize, upscale, convert formats, and generate SEO metadata.",
      "version": "1.0.0",
      "source": "."
    }
  ]
}
```

## Install Instructions for Users

```shell
# Direct install
copilot plugin install onepunchtechnology/tinify-ai-plugin

# Via marketplace
copilot plugin marketplace add onepunchtechnology/tinify-ai-plugin
copilot plugin install tinify-ai
```

## Out of Scope

- Agents (no `.agent.md` files needed — skills cover all use cases)
- Hooks
- Changes to Claude Code or Cursor plugin manifests
- Submitting to third-party community marketplaces (e.g. `github/awesome-copilot`) — that's a follow-up step after this ships
