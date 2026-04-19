# Copilot CLI Plugin Publishing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish the tinify-ai plugin to GitHub Copilot CLI via direct install and marketplace, without touching any existing Claude Code or Cursor plugin files.

**Architecture:** Add a root-level `plugin.json` that Copilot CLI reads for direct install, and a `.github/plugin/marketplace.json` that enables marketplace discovery. Both files reference existing shared assets (`skills/`, `.mcp.json`) — no shared files are modified.

**Tech Stack:** JSON, GitHub Copilot CLI (`copilot` CLI tool)

---

### Task 1: Create root `plugin.json` for Copilot CLI

**Files:**
- Create: `plugin.json`

- [ ] **Step 1: Verify nothing at root will be overwritten**

```bash
ls plugin.json 2>/dev/null && echo "EXISTS" || echo "SAFE"
```

Expected: `SAFE`

- [ ] **Step 2: Create `plugin.json`**

Create `/plugin.json` with this exact content:

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

- [ ] **Step 3: Verify existing plugin manifests are untouched**

```bash
cat .claude-plugin/plugin.json | grep '"name"'
cat .cursor-plugin/plugin.json | grep '"name"'
```

Expected: both print `"name": "tinify-ai"` — confirming they were not modified.

- [ ] **Step 4: Commit**

```bash
git add plugin.json
git commit -m "feat: add Copilot CLI plugin manifest"
```

---

### Task 2: Create `.github/plugin/marketplace.json`

**Files:**
- Create: `.github/plugin/marketplace.json`

- [ ] **Step 1: Create the directory**

```bash
mkdir -p .github/plugin
```

- [ ] **Step 2: Create `marketplace.json`**

Create `.github/plugin/marketplace.json` with this exact content:

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

- [ ] **Step 3: Commit**

```bash
git add .github/plugin/marketplace.json
git commit -m "feat: add Copilot CLI marketplace manifest"
```

---

### Task 3: Verify locally with Copilot CLI

**Files:** None (verification only)

- [ ] **Step 1: Install the plugin locally from the working directory**

```bash
copilot plugin install .
```

Expected: success message, no errors.

- [ ] **Step 2: Confirm the plugin is listed**

```bash
copilot plugin list
```

Expected: `tinify-ai` appears in the list.

- [ ] **Step 3: Confirm skills loaded (in a Copilot interactive session)**

Start a Copilot session and run:

```
/skills list
```

Expected: `optimize-images`, `image-seo`, `web-performance-images` all appear.

- [ ] **Step 4: Uninstall local test version**

```bash
copilot plugin uninstall tinify-ai
```

---

### Task 4: Push and verify remote install

**Files:** None (push + remote verification)

- [ ] **Step 1: Push to GitHub**

```bash
git push
```

- [ ] **Step 2: Verify direct install from GitHub**

```bash
copilot plugin install onepunchtechnology/tinify-ai-plugin
```

Expected: success message.

- [ ] **Step 3: Confirm the plugin is listed**

```bash
copilot plugin list
```

Expected: `tinify-ai` appears.

- [ ] **Step 4: Verify marketplace install flow**

```bash
copilot plugin uninstall tinify-ai
copilot plugin marketplace add onepunchtechnology/tinify-ai-plugin
copilot plugin install tinify-ai
```

Expected: all three commands succeed. `tinify-ai` appears in `copilot plugin list` after the final install.
