# oneshotfig (beta distribution)

Deterministic **Figma-to-code parity tooling** an agent drives: it renders your app,
measures it against a Figma frame, and hands a coding agent absolute targets to fix each
round until the app matches the design — coverage, then alignment, then paint.

> This repo only hosts the **built tarball** for beta testers. The source lives in a
> private repo and the package is **not published to npm** yet. Install straight from the
> release asset below.

## Quick start — the agent on-ramp

Point your coding agent (Claude Code, etc.) at the runbook. It prints the exact steps the
agent follows to wire the tool into your repo, ask you for the few params it needs, and
start the loop:

```bash
export ONESHOTFIG_NPX_SPEC="https://github.com/samwesto/oneshotfig/releases/download/v0.1.0-beta.1/oneshotfig-0.1.0.tgz"
npx "$ONESHOTFIG_NPX_SPEC" --use-one-shot-fig
```

Setting `ONESHOTFIG_NPX_SPEC` makes every command the runbook prints install from this
tarball (instead of the not-yet-published `oneshotfig` npm name), so they're copy-paste
runnable. Then just hand the printed runbook to your agent.

## Run it directly

```bash
# from your project root; the loop auto-provisions a React app if none exists
npx "$ONESHOTFIG_NPX_SPEC" --figma-url "<figma-frame-url>" --mcp-config <figma-mcp.json>
```

The only required input is `--figma-url`. You'll also need a `FIGMA_TOKEN` (Figma →
Settings → Security → personal access tokens) in your env or a gitignored `.env`, and a
**local** Figma MCP server config (the claude.ai connector can't be used by the headless
agent the loop spawns).

## Prerequisites

- **Node ≥ 18** and the **`claude`** Claude Code CLI on `PATH` (the loop spawns `claude -p`
  each round).
- **python3** (the diff engine self-manages its venv on first run).
- For web targets: **Playwright Chromium** — `npx playwright install chromium`.
- Check everything: `npx -p "$ONESHOTFIG_NPX_SPEC" figstate-web doctor`

## Feedback

Please send bugs / rough edges straight to Sam. This is an early beta — expect sharp edges.

## Versioning

Each beta build is a new release tag (`v0.1.0-beta.N`) with its own tarball URL. Update
`ONESHOTFIG_NPX_SPEC` to the latest tag; `npx` caches by URL, so a new tag also sidesteps
any stale cache.
