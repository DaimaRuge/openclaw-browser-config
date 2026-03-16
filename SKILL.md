---
name: openclaw-browser-config
description: Configure and troubleshoot OpenClaw browser automation (OpenClaw-managed browser). Use when setting up browser automation on Windows/Linux/macOS, fixing Chrome/Chromium startup issues, configuring headless mode, or setting up Playwright browser for OpenClaw.
---

# OpenClaw Browser Configuration

Guide for configuring OpenClaw's managed browser automation across different platforms.

## Quick Setup

### 1. Install Playwright Chromium (Recommended)

```bash
npm install -g playwright
npx playwright install chromium
```

### 2. Find Browser Path

```bash
# Linux
find ~/.cache/ms-playwright -name "chrome" 2>/dev/null

# Windows (PowerShell)
Get-ChildItem -Path "$env:LOCALAPPDATA\ms-playwright" -Recurse -Filter "chrome.exe" -ErrorAction SilentlyContinue

# macOS
find ~/Library/Caches/ms-playwright -name "Chrome" 2>/dev/null
```

### 3. Configure openclaw.json

See [references/openclaw-browser-config.md](references/openclaw-browser-config.md) for platform-specific configurations.

## Platform-Specific Notes

### Linux
- May need additional dependencies: `libatk1.0-0`, `libatk-bridge2.0-0`, `libcups2`, `libdrm2`, etc.
- Use `headless: true` for servers without display
- Use `noSandbox: true` if running as root

### Windows
- Use double backslashes in paths: `C:\\Program Files\\...`
- Can use `headless: false` for GUI visibility
- Chrome usually at: `C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe`

### macOS
- Chrome usually at: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- Brave: `/Applications/Brave Browser.app/Contents/MacOS/Brave Browser`

## Common Issues

| Issue | Solution |
|-------|----------|
| `libatk-1.0.so.0: not found` | Install system dependencies (see Linux section) |
| `Failed to start Chrome CDP` | Check port 18800 availability; restart gateway |
| `Browser disabled` | Set `browser.enabled: true` in config |
| Snap Chromium issues | Use Playwright Chromium instead |

## Testing

After configuration, test with:

```bash
openclaw browser status
openclaw browser start
openclaw browser open https://openclaw.ai
openclaw browser snapshot --interactive
openclaw browser screenshot --full-page
openclaw browser stop
```
