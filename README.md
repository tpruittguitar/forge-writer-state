# Forge Writer state

Receipt-index destination approved by Tim on 2026-10-17 (REV-010). The canonical job master and Writer queue remain on Drive.

## Live status

**Active on GitHub since 2026-10-17 23:29 ET.** Writer version 68, merged build `26d931a` (PR #118), copied the current Drive index under a maintenance freeze and lock, verified the GitHub file and historical request states, switched its source, and was unfrozen. At cutover: 5,257 requests (5,252 COMPLETE, 5 FALSE_COMPLETE), zero pending; compacted file 946,968 bytes. See `CUTOVER_RECEIPT_20261017.json` for the immutable cutover facts. The live blob SHA and counts will advance with ordinary Writer activity.

## Initial seed history

The initial commit preserves the complete validated Drive receipt-index snapshot. The next commit compacts older COMPLETE row detail while retaining every request ID, completion state, write ID, timestamp and correction. Full detail remains in Git history and the unchanged Drive source. The newest 100 COMPLETE entries retain row detail in the working file.

The initial review seed was inactive and was replaced at cutover by a new locked Drive snapshot. The migration checked the source hash and expected destination blob SHA, verified the GitHub copy, then set `RECEIPT_INDEX_SOURCE=GITHUB`. The old Drive index is now superseded evidence; never treat it as live state or silently fall back to it.

## Ownership and safety

After cutover, the Apps Script Writer alone updates `PIPELINE_RECEIPT_INDEX.json` using GitHub Contents API writes conditional on the exact blob SHA read. Conflicts, missing files and unreadable state fail closed. Never edit, delete, reset or hand-replace the active index. Never remove historical request IDs: they prevent replay even after receipt-log rotation.

The Writer uses Script Property `GITHUB_WRITER_STATE_TOKEN` with Contents read/write permission on this repo. At cutover, the existing authorized Writer GitHub credential was reused after a successful Contents write test; no credential value was exposed. Never commit tokens or keys. Deployment and cutover instructions live in `tpruittguitar/foreman-cockpit/docs/RECEIPT_INDEX_GITHUB_CUTOVER.md`.

`SEED_MANIFEST.json` describes the initial review snapshot only. It does not track subsequent live Writer checkpoints.
