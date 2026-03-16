# OpenClaw Browser Configuration Guide

Complete configuration reference for OpenClaw browser automation.

## Table of Contents

1. [Configuration File](#configuration-file)
2. [Platform Configurations](#platform-configurations)
3. [Browser Paths](#browser-paths)
4. [Configuration Options](#configuration-options)
5. [CLI Commands](#cli-commands)
6. [Troubleshooting](#troubleshooting)

---

## Configuration File

Location: `~/.openclaw/openclaw.json` (Linux/macOS) or `%USERPROFILE%\.openclaw\openclaw.json` (Windows)

### Basic Structure

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "headless": false,
    "noSandbox": false,
    "executablePath": "/path/to/chrome",
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

---

## Platform Configurations

### Linux (Ubuntu/Debian)

#### Option A: Playwright Chromium (Recommended)

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "headless": true,
    "noSandbox": true,
    "executablePath": "/root/.cache/ms-playwright/chromium-1208/chrome-linux64/chrome",
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

**Install dependencies:**
```bash
apt-get update
apt-get install -y libatk1.0-0 libatk-bridge2.0-0 libcups2 libdrm2 \
  libxcomposite1 libxdamage1 libxfixes3 libxrandr2 libgbm1 \
  libpango-1.0-0 libcairo2 libasound2t64 libnss3 libxss1 libgtk-3-0
```

#### Option B: System Chromium

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "headless": true,
    "noSandbox": true,
    "executablePath": "/usr/bin/chromium-browser",
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

> ⚠️ Note: Snap Chromium often has permission issues. Use Playwright Chromium instead.

---

### Windows

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "headless": false,
    "noSandbox": false,
    "executablePath": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe",
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

**Alternative Browsers:**

**Microsoft Edge:**
```json
"executablePath": "C:\\Program Files (x86)\\Microsoft\\Edge\\Application\\msedge.exe"
```

**Brave:**
```json
"executablePath": "C:\\Program Files\\BraveSoftware\\Brave-Browser\\Application\\brave.exe"
```

---

### macOS

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "headless": false,
    "noSandbox": false,
    "executablePath": "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

**Brave on macOS:**
```json
"executablePath": "/Applications/Brave Browser.app/Contents/MacOS/Brave Browser"
```

---

## Browser Paths

### Playwright Chromium Paths

| Platform | Default Path |
|----------|--------------|
| Linux | `~/.cache/ms-playwright/chromium-*/chrome-linux64/chrome` |
| Windows | `%LOCALAPPDATA%\ms-playwright\chromium-*\chrome-win\chrome.exe` |
| macOS | `~/Library/Caches/ms-playwright/chromium-*/chrome-mac/Chromium.app/Contents/MacOS/Chromium` |

### System Browser Paths

| Browser | Linux | Windows | macOS |
|---------|-------|---------|-------|
| Chrome | `/usr/bin/google-chrome` | `C:\Program Files\Google\Chrome\Application\chrome.exe` | `/Applications/Google Chrome.app/...` |
| Edge | - | `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe` | `/Applications/Microsoft Edge.app/...` |
| Brave | `/usr/bin/brave-browser` | `C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe` | `/Applications/Brave Browser.app/...` |
| Chromium | `/usr/bin/chromium-browser` | - | - |

---

## Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `enabled` | boolean | true | Enable browser automation |
| `defaultProfile` | string | "chrome" | Default profile to use |
| `headless` | boolean | false | Run without GUI |
| `noSandbox` | boolean | false | Disable Chrome sandbox |
| `executablePath` | string | auto-detected | Path to Chrome/Chromium |
| `attachOnly` | boolean | false | Only attach, never start browser |

### Profile Options

| Option | Type | Description |
|--------|------|-------------|
| `cdpPort` | number | Chrome DevTools Protocol port |
| `cdpUrl` | string | Remote CDP URL (for remote browsers) |
| `color` | string | UI color for this profile |

### Multiple Profiles Example

```json
{
  "browser": {
    "enabled": true,
    "defaultProfile": "openclaw",
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      },
      "work": {
        "cdpPort": 18801,
        "color": "#0066CC"
      },
      "remote": {
        "cdpUrl": "http://10.0.0.42:9222",
        "color": "#00AA00"
      }
    }
  }
}
```

---

## CLI Commands

### Basic Operations

```bash
# Status
openclaw browser status

# Start/Stop
openclaw browser start
openclaw browser stop

# Tabs
openclaw browser tabs
openclaw browser open <url>
openclaw browser focus <targetId>
openclaw browser close <targetId>
```

### Page Interaction

```bash
# Navigation
openclaw browser navigate <url>

# Snapshot (get interactive elements)
openclaw browser snapshot --interactive

# Screenshot
openclaw browser screenshot
openclaw browser screenshot --full-page
openclaw browser screenshot --ref <ref>

# Actions (ref from snapshot)
openclaw browser click <ref>
openclaw browser type <ref> "text" --submit
openclaw browser press Enter
openclaw browser hover <ref>
```

### Using Specific Profile

```bash
openclaw browser --browser-profile work start
openclaw browser --browser-profile work open https://example.com
```

---

## Troubleshooting

### Issue: `libatk-1.0.so.0: not found`

**Cause:** Missing system libraries

**Solution:**
```bash
# Ubuntu/Debian
apt-get install -y libatk1.0-0 libatk-bridge2.0-0 libcups2 libdrm2 \
  libxcomposite1 libxdamage1 libxfixes3 libxrandr2 libgbm1 \
  libpango-1.0-0 libcairo2 libasound2t64 libnss3 libxss1 libgtk-3-0

# CentOS/RHEL/Fedora
yum install -y atk at-spi2-atk cups-libs libdrm libXcomposite \
  libXdamage libXfixes libXrandr mesa-libgbm pango cairo alsa-lib \
  nss libXScrnSaver gtk3
```

---

### Issue: `Failed to start Chrome CDP on port 18800`

**Cause:** Port in use or Chrome failed to start

**Solution:**
```bash
# Check if port is in use
lsof -i :18800

# Kill existing process or change port in config
# Then restart gateway
openclaw gateway restart
```

---

### Issue: Snap Chromium Permission Errors

**Cause:** Snap sandbox restrictions

**Solution:** Use Playwright Chromium instead:
```bash
npm install -g playwright
npx playwright install chromium
# Then use Playwright path in config
```

---

### Issue: `Browser disabled`

**Cause:** Browser not enabled in config

**Solution:**
```json
{
  "browser": {
    "enabled": true
  }
}
```

---

### Issue: Windows Path Errors

**Cause:** Single backslashes or spaces not escaped

**Solution:** Use double backslashes:
```json
"executablePath": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"
```

---

## Testing Checklist

After configuration, run this test:

```bash
# 1. Check status
openclaw browser status

# 2. Start browser
openclaw browser start

# 3. Open test page
openclaw browser open https://openclaw.ai

# 4. Get snapshot
openclaw browser snapshot --interactive

# 5. Take screenshot
openclaw browser screenshot --full-page

# 6. Stop browser
openclaw browser stop
```

All steps should complete without errors.

---

## References

- [OpenClaw Browser Documentation](https://docs.openclaw.ai/zh-CN/tools/browser)
- [Playwright Documentation](https://playwright.dev/)
