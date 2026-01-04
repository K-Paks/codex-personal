---
name: docker-playwright
description: Run Playwright checks inside an already-running Playwright Docker container; use when you need to install or align the Playwright npm package via pnpm, start a local dev server for any project, and verify UI on localhost:PORT.
---

# Docker Playwright

## Overview

Assume you are already inside the container and Codex is running there. Use this workflow to verify any project by running Playwright against a local server inside the container.

## Inputs

- `APP_DIR`: project root (default: current directory)
- `PORT`: localhost server port (required)

## Workflow

### 1) Set variables

```bash
APP_DIR="${APP_DIR:-$PWD}"
PORT="${PORT:?set PORT to the local server port}"
```

### 2) Ensure Playwright is installed via pnpm

```bash
cd "$APP_DIR"

if ! command -v pnpm >/dev/null 2>&1; then
  echo "pnpm not found; install it before proceeding." >&2
  exit 1
fi

node -e "require('playwright/package.json').version" \
  || pnpm add -D playwright@<version>
```

Notes:
- Replace `<version>` with the Playwright image version (e.g., `1.50.0`).
- If you see a mismatch error on launch, reinstall using the version shown in the error message.
- This updates `package.json` and `pnpm-lock.yaml` in the project.

### 3) Start the dev server

Run your project’s server command. Examples:

```bash
# Example only; use your project’s command
pnpm exec next dev -p "$PORT"
```

In another shell (or after backgrounding the server), wait for readiness:

```bash
until curl -fsS "http://localhost:$PORT" >/dev/null; do sleep 1; done
```

### 4) Run a Playwright check

Replace the selector/text with what you need to verify.

```bash
node -e "const { chromium } = require('playwright');(async()=>{const browser=await chromium.launch();const page=await browser.newPage();await page.goto('http://localhost:' + process.env.PORT,{waitUntil:'domcontentloaded'});await page.waitForSelector('text=YAFOO',{timeout:5000});console.log('title='+await page.title());await browser.close();})().catch(e=>{console.error(e);process.exit(1);});"
```

### 5) Cleanup

Stop the dev server (Ctrl-C) and revert Playwright install if you don’t want to keep it.
