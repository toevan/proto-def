# Toevan Evidence Ledger (tamper-evident twin)

This orphan branch is a git-native mirror of Toevan's append-only compliance
evidence ledger for this repository. Each commit adds exactly one evidence run:

    records/<head_sha>/<record_id>.json   the canonical record bytes, VERBATIM

To verify independently (Toevan not required):
  1. For each records file, sha256(file bytes) is the run's record_hash.
  2. Each record's prev_record_hash chains to the previous record (by chain_seq).
  3. The git commit DAG is strictly linear (fast-forward only) — a second,
     independent hash chain over the same bytes.

`index/chain-head` holds the latest "<chain_seq> <record_hash>".

    attestations/<head_sha>/<record_id>.dsse.json

holds the run's SIGNED attestation (in-toto SLSA-VSA statement in a DSSE
envelope). Verify with stock tooling and Toevan's published public key:

    cosign verify-blob-attestation --key toevan.pub \
        --type https://slsa.dev/verification_summary/v1 <file>

Do not force-push or delete this branch; configure branch protection to block it.
