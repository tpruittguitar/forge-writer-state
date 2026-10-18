# Forge Writer state

Receipt-index destination approved by Tim on 2026-10-17 (REV-010). The canonical job master and Writer queue remain on Drive.

## Seed status

The initial commit preserves the complete validated Drive receipt-index snapshot. The next commit compacts older COMPLETE row detail while retaining every request ID, completion state, write ID, timestamp and correction. Full detail remains in Git history and the unchanged Drive source. The newest 100 COMPLETE entries retain row detail in the working file.

**This is a review seed, not the active authority.** Production continues reading Drive until the new Writer code is deployed and its locked, frozen migration succeeds. Migration re-reads the live Drive index, checks the operator's source hash and expected destination blob SHA, verifies the GitHub copy, then sets `RECEIPT_INDEX_SOURCE=GITHUB`.

## Ownership and safety

After cutover, the Apps Script Writer alone updates `PIPELINE_RECEIPT_INDEX.json` using GitHub Contents API writes conditional on the exact blob SHA read. Conflicts, missing files and unreadable state fail closed. Never edit, delete, reset or hand-replace the active index. Never remove historical request IDs: they prevent replay even after receipt-log rotation.

The Writer needs a dedicated Script Property `GITHUB_WRITER_STATE_TOKEN` with Contents read/write permission on this repo. Never commit tokens or keys. Deployment and cutover instructions live in `tpruittguitar/foreman-cockpit/docs/RECEIPT_INDEX_GITHUB_CUTOVER.md`.

`SEED_MANIFEST.json` describes the initial review snapshot only. It does not track subsequent live Writer checkpoints.
