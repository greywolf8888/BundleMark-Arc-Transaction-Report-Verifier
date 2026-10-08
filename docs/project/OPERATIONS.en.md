# Running BundleMark

[简体中文](OPERATIONS.zh-CN.md) · [Home](../../README.md)

## Offline first

Node.js 24 and npm 11 are required. Install using `npm ci --ignore-scripts --no-audit --no-fund`, then run `npm run arc:build`. Replay a bundle with `npm run arc:replay -- bundle.json`. The local replay does not need a database or network and returns integrity/recomputation results separately from mainnet authenticity.

For local reconciliation: `npm run arc:reconcile -- bundle.json accounting.sqlite namespace business_reference`. The consumer SQLite is local accounting, not the settlement authority. Only MATCHED movements can be allocated; duplicate movements and conflicting references are rejected. No fund movement or automatic delivery occurs.

## API and frontend

Use dedicated PostgreSQL 16. Back up and restore-check existing data first. Apply `npm run arc:migrate` using the schema owner's connection; migrations are additive, currently through version 7. The migration connection is not the runtime API connection.

Configure environment variables through your shell or secret manager; the application does not automatically load an `.env` file:

- `ARC_DATABASE_URL`: runtime read-only database role, with schema usage and SELECT privileges.
- `ARC_REQUEST_DATABASE_URL`: restricted request/report append role. Its access must match the queries in `verifier-storage.ts`, `verifier-session.ts`, and request storage; do not give it schema-owner or superuser privileges. Absence disables request writes.
- `ARC_CURSOR_SECRET`: stable secret, retained across releases to preserve cursors. Do not embed it in frontend assets or bundles.
- `ARC_PUBLIC_ORIGIN`: the actual frontend origin, such as `http://127.0.0.1:5178` for development. Private write requests are origin- and CSRF-bound.
- `ARC_API_PORT`: default 8087; `ARC_API_HOST`: default loopback.
- Optional registered `ARC_RPC_URL` / `ARC_PROVIDER_ALIAS`: use versioned registered sources; do not invent endpoints or purchase provider access.

Start `npm run arc:start`. In a second terminal run `npm run dev -w @zerotrace/arc-task-ledger-web -- --host 127.0.0.1 --port 5178`. The development frontend proxies `/api` to port 8087; set `ARC_API_PROXY` if your API port differs. Real transaction acquisition is bounded and read-only. Legacy task collection uses a separate `npm run arc:worker` process and explicit scan budgets; it is unnecessary for offline replay.

For a production frontend, use the supplied Dockerfile/web target and same-origin reverse proxy. The inherited Compose template refers to an old export directory and does not provision the new request-write roles; do not treat it as a turnkey verifier deployment. Current hosted release receipts are the source of actual cloud configuration facts. Keep database volumes, secrets, and permissions stable.

## Verification

`npm run arc:test:unit`, `npm run arc:client:typecheck`, `npm run lint`, `npm run arc:build`, and `npm run license:check` are standalone checks. Set `ARC_TEST_DATABASE_URL` to a dedicated disposable database for `npm run arc:test:integration` and `npm run arc:test:e2e`; never use production. E2E runs separate desktop/mobile specimens to stay within actual API quotas. Historical receipts are not current test runs.

Private ownership uses seven-day sessions. Public report versions cannot be withdrawn. Export before losing session access; review the full publication preview. Preserve Unknown and provider-down states in consumers. Full protocol and deployment limitations remain documented in [KNOWN_LIMITATIONS.md](../arc-task-ledger/settlement-verifier/KNOWN_LIMITATIONS.md).
