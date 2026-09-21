# Hyperium Fleet Dashboard

A static page showing live Hyperium/Alexandria worker-fleet activity: open PRs, CI status, the
Control Plane queue, and active peer sessions — rendered as a small pixel-art "studio floor" scene.

## How it works

`index.html` is a plain static page with no build step. It fetches `data.json` on load and every
30 seconds after, and re-renders. It never talks to any private infrastructure directly.

`data.json` is regenerated every few minutes by a scheduled job in
[`ExistenceDenied/Alexandria`](https://github.com/ExistenceDenied/Alexandria)
(`scripts/generate_fleet_dashboard_snapshot.py`, run by
`.github/workflows/publish-hyperium-fleet-dashboard.yml`), which reads real GitHub PR/CI state and
the real Control Plane queue/peer-presence state, then pushes the resulting `data.json` here. This
repo never holds credentials for that private infrastructure — the snapshot job runs entirely on
Alexandria's own trusted, already-authenticated host.

## Design

See `ExistenceDenied/Alexandria`'s
`docs/superpowers/specs/2026-09-21-hyperium-fleet-dashboard-standalone-design.md` for the full
design record, including what was deliberately deferred (real-time updates, authentication).

## Deploying

Any static host works (Vercel, Cloudflare Pages, GitHub Pages) — connect it to this repo and it
will redeploy automatically whenever `data.json` (or `index.html`) is pushed.
