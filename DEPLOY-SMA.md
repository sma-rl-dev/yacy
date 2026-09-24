# YaCy tester environment

Baseline source: upstream `Release_1.941` at `f0464e7fbcfcb69127f0325910f92f113ce23677`.

## Deploy, reset, and seed

```bash
./tester-env deploy
./tester-env seed
./tester-env verify
./tester-env reset
```

`deploy` builds the editable checkout with `Dockerfile.tester-env`, starts YaCy on `http://localhost:18090/`, and uses HTTP Digest credentials `admin` / `yacy`. `IMAGE_TAG` controls the image name (default `tester-env-yacy:dev`); `RUN_ID` scopes only the Compose project, container, and volume; `YACY_PORT` defaults to `18090`.

`reset` runs Compose `down -v`, removing only this environment's container, volume, and network while retaining its image. The Docker bridge has outbound masquerading disabled. `seed` posts four fixed local catalog documents directly to YaCy's local push API; it does not crawl or require Internet access.

## Verification evidence

Two `reset && deploy && seed && verify` cycles passed. On 2026-09-08, browser verification at `http://host.docker.internal:18090/` searched for `harbor` and visibly displayed `1-3 of 3`: **Harbor Procurement Handbook**, **Warehouse Safety Checklist**, and **Quarterly Supply Review**.

On 2026-09-24 the seed catalog grew to four fixed Harbor Operations documents with the addition of `tester-env-seed/harbor-dock-scheduling-bulletin.html` (script-bearing snippet doc for scenario 005; its brace-delimited fragment is mirrored in a noscript-visible paragraph) plus a handbook meta description; `seed` posts all four to the local push API and `verify` asserts all four titles are visible for query `harbor` with `resource=local`. `./tester-env verify` passed against the running container on 2026-09-24, and seed-compliance PASS `20260924-193054Z` plus rl-env prepare-only pass+fail runs confirm scenarios 001 and 005 execute against the 4-doc catalog.
