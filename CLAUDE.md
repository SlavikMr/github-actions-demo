# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install       # Install dependencies (no external deps; devDependencies is empty)
npm test          # Run tests via Node.js built-in test runner (node --test)
npm run build     # Copy src/ to dist/
npm start         # Start the HTTP server on PORT (default 3000)
npm run lint      # No-op placeholder (exits 0)
```

To run a single test file directly:
```bash
node --test src/index.test.js
```

## Architecture

This is a **tutorial/demo project** for teaching GitHub Actions — the application code is intentionally minimal.

**`src/index.js`** exports four pure calculator functions (`add`, `subtract`, `multiply`, `divide`) and an HTTP server that exposes them as query-param endpoints (`/add?a=5&b=3`). The server only starts when the file is run directly (`require.main === module`), so tests import the functions without side effects.

**`src/index.test.js`** uses Node's built-in `node:test` and `node:assert` modules — no Jest or Mocha.

**`.github/workflows/`** contains six workflow files that serve as learning examples, not production infrastructure:

| File | Purpose |
|------|---------|
| `ci.yml` | Basic CI: lint → test → build, triggers on push/PR to main |
| `ci-with-cache.yml` | Same pipeline with `actions/cache` for `~/.npm` |
| `matrix.yml` | Matrix strategy across Node 18/20/22 × ubuntu/windows |
| `deploy.yml` | Four-stage pipeline: test → build → staging → production, with `concurrency` guard and artifact passing via `actions/upload-artifact` |
| `scheduled.yml` | Cron-triggered workflow example |
| `secrets-demo.yml` | Demonstrates `${{ secrets.* }}` and environment variables |

The `deploy.yml` workflow uses GitHub Environments (`staging`, `production`) which can be configured in repo Settings to require manual approval before the production job runs.
