# OpenClaw Northflank Deployment

Auto-updating OpenClaw deployment wrapper for [Northflank](https://northflank.com).

## What This Does

OpenClaw's gateway binds to `127.0.0.1` by default, which is unreachable behind Northflank's load balancer. This wrapper:

1. Builds OpenClaw from source (public upstream)
2. Runs the gateway on internal loopback (`:18789`)
3. Reverse-proxies all traffic from the public port (`:8080`)
4. Provides a browser-based setup wizard at `/setup`
5. Auto-updates via GitHub Actions when new OpenClaw releases are published

## Deploy

Create a Northflank service pointing to this repo, set `SETUP_PASSWORD`, build and deploy. Visit `/setup` to configure.

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `SETUP_PASSWORD` | Yes | Protects the `/setup` wizard |
| `OPENCLAW_STATE_DIR` | No | Config storage (default: `~/.openclaw`) |
| `OPENCLAW_WORKSPACE_DIR` | No | Workspace storage (default: `$STATE_DIR/workspace`) |
| `OPENCLAW_GATEWAY_TOKEN` | No | Auto-generated if not set |

## Auto-Updates

The GitHub Actions workflow checks for new upstream OpenClaw releases daily at 06:00 UTC, records the latest version in `.openclaw-version`, and triggers a Northflank rebuild via webhook.

Set `NORTHFLANK_WEBHOOK_URL` in repo secrets to enable auto-deploy.

## License

MIT
