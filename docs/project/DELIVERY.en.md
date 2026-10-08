# BundleMark delivery record — 8 October 2026

[Project overview](../../README.md) · [Simplified Chinese](DELIVERY.zh-CN.md) · [Verification receipt](VALIDATION_20261008.json)

BundleMark is independently hosted at [the project repository](https://github.com/greywolf8888/BundleMark-Arc-Transaction-Report-Verifier). The main README is entirely English. A separate Simplified Chinese README and operation guide provide explicit language boundaries. The supplied second image is the project icon; both original images retain their exact bytes and [recorded digests](ORIGINAL_ASSETS.json).

## Versions and history

The 18 historical independent Arc commits were pushed one at a time in chronological order, with each remote SHA verified. Their original commit IDs were preserved. See [history](HISTORY.json), [push receipts](HISTORY_PUSH_RECEIPTS.jsonl), and the [bounded publication audit](HISTORY_AUDIT.json).

The deployed application commit is `56218638e8edf6a3494cee22671b301f3a36ee79`, with interface `atl-ui-v1.4.3`, release `2.0.0`, and schema migration `7`. Later commits update documentation and delivery evidence only. They do not change executable application files, financial rules, or historical evidence. The release archive identifies its exact documentation commit in its attached receipt; the included manifest hashes Git blob bytes, excluding the manifest itself.

## Actual verification this run

- The independent repository passed formatting, lint, 118 Arc unit tests, client type checking, and permissive dependency license checks. Production dependency audit reported zero vulnerabilities at the recorded time.
- A freshly extracted source archive was independently installed and built. Client type checking and offline replay of the supplied recorded mainnet bundle exited successfully. Compiled web assets matched the deployed asset hashes. These are engineering and recorded-bundle checks, not a fresh mainnet settlement conclusion.
- Scoped application validation passed 43 PostgreSQL integration checks and 40 desktop/mobile browser checks. The isolated test database was separate from the public database.
- Public post-deployment reads confirmed both services completed, the expected source commit and interface version, exact original icon/wordmark hashes, unchanged historical public bundles, unchanged task #18 result/evidence/snapshot, and compatibility with an existing pagination cursor.
- A fresh GitHub browser observation confirmed no Chinese text or unrelated project name in the English README and successful image loading. The live website showed the supplied icon and BundleMark title; explicit Chinese selection survived reload and the interface was restored to English.

Timestamps, commands, log digests, and public asset hashes are in the [verification receipt](VALIDATION_20261008.json). Original local logs are retained separately; older validation reports are not reclassified as current PASS results.

## Public deployment

[Open BundleMark](https://web--atl-web--xtd599t97njk.code.run/).

The existing API and web services now use this independent repository's `main` branch for future builds. Their current deployed execution commit remains the verified `5621863` release. Both retained one instance and the existing compute plan. The five-resource inventory, database, suspended migration job, and pinned job images were preserved. No new resource, database reset, migration dispatch, chain write, or paid procurement was introduced. The recorded prior-24-hour usage was USD 0; this is a time-bound observation, not a guarantee about future billing.

## Validation boundaries

This branding/documentation delivery did not rerun the full new-transaction mainnet acceptance matrix. Historical chain receipts remain dated observations. Full forensic acceptance remains closed where durable artifacts, replay, source verification, or coverage are insufficient. Offline replay does not authenticate chain provenance. Private management depends on the original expiring browser session; public immutable versions cannot be withdrawn. See the README's result interpretation and operation guide.

No grant application was submitted, no human approval or adoption was asserted, and no funding outcome was fabricated. The delivery is ready as an independently versioned source package and public UI presentation; broader product acceptance retains its existing evidence boundaries.
