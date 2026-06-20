# TipsAudit — Public Track Record

Daily, immutable snapshots of the TipsAudit prediction track record.

Each `data/track-YYYY-MM-DD.json` is the exact public export from https://tipsaudit.com,
captured once per day. `manifest.csv` lists the SHA-256 of every snapshot, and each file is
anchored in the Bitcoin blockchain via [OpenTimestamps](https://opentimestamps.org) (`.ots`).

## Verify a snapshot

```bash
sha256sum data/track-2026-06-20.json      # must match manifest.csv
ots verify data/track-2026-06-20.json.ots # confirms it existed at that time
```

Because the Git history and the OpenTimestamps proofs are external and append-only, no one —
including TipsAudit — can rewrite a past record without it being detectable.
