# Playwright MCP Setup Guide

This guide walks you through installing and configuring the **Playwright MCP server**
for Claude Code — a required dependency of the PTA Solver skill.

## What is Playwright MCP?

Playwright MCP is a [Model Context Protocol](https://modelcontextprotocol.io/) server
that grants Claude Code full browser automation capabilities. It enables:

- Page navigation (`mcp__playwright__browser_navigate`)
- Element clicking and typing (`mcp__playwright__browser_click`, `browser_type`)
- JavaScript execution in the browser (`mcp__playwright__browser_evaluate`)
- Clipboard manipulation (`navigator.clipboard.writeText()`)
- Screenshot capture (`mcp__playwright__browser_take_screenshot`)

The PTA Solver skill relies on these tools to interact with PTA's CodeMirror 6
editors and submission system.

---

## Step 1: Install the Playwright MCP Package

```bash
npm install -g @playwright/mcp
```

This installs the MCP server globally. Verify the installation:

```bash
npx @playwright/mcp --version
```

> **Note:** If you're behind a proxy or firewall, you may need to configure
> npm proxy settings first: `npm config set proxy http://proxy.example.com:8080`

---

## Step 2: Install Playwright Browsers

The MCP server requires Playwright browsers to be installed:

```bash
npx playwright install chromium
```

This downloads a Chromium binary that the MCP server will use for automation.
If you prefer Google Chrome or Microsoft Edge:

```bash
npx playwright install chrome
```

---

## Step 3: Configure Claude Code

Add the Playwright MCP server to your Claude Code configuration. You have two options:

### Option A: Global Configuration (Recommended)

Edit `~/.claude/settings.json` and add an `mcpServers` key:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp", "--headless"],
      "env": {}
    }
  }
}
```

### Option B: Project-Level Configuration

Create or edit `.claude/settings.json` in your project root with the same
`mcpServers` configuration. This keeps the Playwright dependency scoped to your
project.

### Option C: Separate MCP Config File

Create `.claude/mcp.json` in your home directory or project root:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp", "--headless"],
      "env": {}
    }
  }
}
```

### Configuration Options

| Flag | Description |
|------|-------------|
| `--headless` | Run browser in headless mode (no visible window) |
| `--browser <name>` | Browser to use: `chromium`, `chrome`, `firefox`, `webkit` |
| `--viewport-size <WxH>` | Browser viewport size, e.g., `1920x1080` |
| `--isolated` | Use isolated browser context (no persistent state) |

For PTA automation, we recommend:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp",
        "--headless",
        "--browser", "chromium",
        "--viewport-size", "1920x1080",
        "--isolated"
      ]
    }
  }
}
```

---

## Step 4: Restart Claude Code

After updating the configuration, restart Claude Code. On startup, you should see
Playwright MCP tools in the tool list. Verify by asking Claude:

> "What Playwright MCP tools do you have available?"

You should see tools prefixed with `mcp__playwright__`.

---

## Step 5: Smoke Test

Test that everything works with a simple browser navigation:

> "Navigate to https://example.com and tell me the page title."

If Claude successfully navigates and reports the title, your setup is working.

---

## Troubleshooting

### "mcp__playwright__* tools not available"

1. Verify the MCP server is listed in your configuration
2. Check Claude Code startup logs for MCP connection errors
3. Ensure `@playwright/mcp` is installed globally: `npm ls -g @playwright/mcp`
4. Try running the MCP server manually to see errors:
   ```bash
   npx @playwright/mcp --headless
   ```

### "Browser not found" or Playwright errors

```bash
npx playwright install chromium
npx playwright install-deps chromium
```

On Linux, `install-deps` installs system libraries required by Chromium (libgbm,
libnss3, etc.).

### "Headless mode" issues on Linux

If you get blank pages or rendering problems in headless mode on Linux, add these
flags:

```json
{
  "args": [
    "@playwright/mcp",
    "--headless",
    "--browser", "chromium"
  ],
  "env": {
    "PLAYWRIGHT_CHROMIUM_ARGS": "--no-sandbox --disable-setuid-sandbox --disable-gpu"
  }
}
```

### Permission prompts for browser actions

Claude Code will prompt for permission before each browser action. To reduce prompts,
add to your `.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "mcp__playwright__browser_navigate",
      "mcp__playwright__browser_snapshot",
      "mcp__playwright__browser_evaluate",
      "mcp__playwright__browser_click",
      "mcp__playwright__browser_press_key",
      "mcp__playwright__browser_run_code_unsafe",
      "mcp__playwright__browser_type",
      "mcp__playwright__browser_wait_for",
      "mcp__playwright__browser_handle_dialog",
      "mcp__playwright__browser_fill_form"
    ]
  }
}
```

> **Security Note:** Only allow these permissions in projects where you trust the
> automation target. `browser_run_code_unsafe` executes arbitrary JavaScript in the
> Playwright server process.

---

## Uninstall

```bash
npm uninstall -g @playwright/mcp
```

Also remove the `mcpServers.playwright` block from your Claude Code configuration.

---

## References

- [Playwright MCP on npm](https://www.npmjs.com/package/@playwright/mcp)
- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [Claude Code MCP Integration Guide](https://docs.claude.codes/mcp)
