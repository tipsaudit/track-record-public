# TipsAudit — Public Track Record

Periodic, immutable snapshots of the TipsAudit prediction track record, published whenever the
ledger changes (at least daily).

Each `data/track-<UTC-timestamp>.json` is the exact public export from https://tipsaudit.com at
that moment — snapshots are never overwritten, so every version stays separately auditable.
`manifest.csv` lists the SHA-256 of every snapshot, and each file is anchored in the Bitcoin
blockchain via [OpenTimestamps](https://opentimestamps.org) (`.ots`).

External independent verification has been active since 20 June 2026. Earlier predictions are
preserved in the internal ledger but were not externally timestamped before kick-off.

## Verify a snapshot

```bash
sha256sum data/track-2026-06-20T07-40-00Z.json      # must match manifest.csv
ots verify data/track-2026-06-20T07-40-00Z.json.ots # confirms it existed at that time
```

Because the Git history and the OpenTimestamps proofs are external and append-only, no one —
including TipsAudit — can rewrite a past record without it being detectable.
