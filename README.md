# tinify.ai Plugin

AI-powered image optimization for your coding workflow. Compress, resize, upscale, convert formats, and generate SEO metadata — all from your AI assistant.

Works with **Claude Code**, **Cursor**, **GitHub Copilot**, and **ChatGPT**.

## Installation

### Claude Code

```bash
claude /install-plugin https://github.com/onepunchtechnology/tinify-ai-plugin
```

### Cursor

Install from the [Cursor Marketplace](https://cursor.com/marketplace) — search for "tinify.ai".

Or add manually: clone this repo and load as a project plugin.

### GitHub Copilot

Install using the Open Plugins standard:

```bash
# Add to your project
git submodule add https://github.com/onepunchtechnology/tinify-ai-plugin .plugins/tinify-ai
```

### ChatGPT

ChatGPT connects to the remote MCP server directly (no plugin needed):

1. Open ChatGPT → Settings → Developer Mode (beta)
2. Add MCP Server → URL: `https://app.tinify.ai/mcp`
3. Start chatting — the `optimize_image` tool is now available

> Requires ChatGPT Pro, Team, Enterprise, or Edu tier.

## Requirements

- **Node.js 18+** (for Claude Code, Cursor, GitHub Copilot installations)
- No signup required — 20 free credits/day as guest

## Skills

### optimize-images

Batch-optimize images in your project. Audits file sizes, selects optimal formats, and compresses everything.

```
"Optimize all images in src/assets/"
"Compress the images in this project for production"
"Convert all PNGs to WebP"
```

### image-seo

Scan your codebase for images missing alt text and generate AI-powered SEO metadata.

```
"Add alt text to all images missing it"
"Audit image SEO in this project"
"Generate alt text for the product images"
```

### web-performance-images

Audit and fix image-related Core Web Vitals issues (LCP, CLS, lazy loading).

```
"Fix image performance issues in this project"
"Audit Core Web Vitals for images"
"Which images are slowing down page load?"
```

## MCP Tools

The plugin provides these tools via the tinify-ai MCP server:

| Tool | Description |
|------|-------------|
| `optimize_image` | Compress, resize, upscale, convert format, and generate SEO tags for an image |
| `status` | Check account tier, credits remaining, and reset time |
| `login` | Authenticate via browser for more credits |
| `logout` | Sign out and clear local session |
| `upgrade` | Open pricing page for higher credit limits |

### `optimize_image` Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `input` | string (required) | Absolute file path or URL |
| `output_path` | string | Where to save (file path or directory ending in `/`) |
| `output_format` | enum | `jpg`, `png`, `webp`, `avif`, `gif`, `svg`, `ico`, `original` |
| `output_width_px` | int | Target width in pixels |
| `output_height_px` | int | Target height in pixels |
| `output_upscale_factor` | 2 \| 4 | AI upscale factor (2x or 4x) |
| `output_resize_behavior` | enum | `pad` or `crop` (when both dimensions set) |
| `output_seo_tag_gen` | boolean | Generate alt text, keywords, and SEO filename (default: true) |
| `output_file_size_limit` | int | Target max output file size in bytes |
| `confirm_gif_cost` | boolean | Required to proceed with animated GIF processing after cost warning |
| `gif_frame_limit` | int (1-100) | Max frames to process for animated GIFs (default: 100) |

**Supported formats:** JPG, PNG, WebP, AVIF, GIF (animated), HEIC, TIFF, BMP, SVG, ICO (max 50 MB)

## Pricing

| Tier | Credits | Cost |
|------|---------|------|
| Guest | 20/day | Free, no signup |
| Free | 50/day | Free with account |
| Starter | 1,000/month | $10/month |
| Pro | 3,000/month | $29/month |
| Max | 10,000/month | $89/month |
| Enterprise | Custom | [Contact us](mailto:hello@tinify.ai) |

Use the `status` tool to check your remaining credits. Use `login` to authenticate for more credits.

## Links

- [tinify.ai](https://tinify.ai) — Web app
- [npm package](https://www.npmjs.com/package/@tinify-ai/mcp-server) — Standalone MCP server
- [GitHub](https://github.com/onepunchtechnology/tinify-ai-plugin) — This plugin

## License

MIT
