---
name: Submit and monitor a job run
description: Upload a script, submit it as a Wherobots job run, then stream logs and read metrics to completion.
api: openapi/wherobots-cloud-openapi-original.json
operations: [createUploadUrl, createJobRun, getJobRun, getJobRunLogs, getJobRunMetrics, cancelJobRun]
---

# Submit and monitor a Wherobots job run

Run Python spatial workloads as managed Job Runs on Wherobots Cloud. Requires a
paid organization edition.

## Auth
- `X-API-Key` header (or `WHEROBOTS_API_KEY`). See `authentication/wherobots-authentication.yml`.

## Steps
1. `createUploadUrl` — get a presigned URL and upload your job script/artifacts to
   managed storage (the CLI does this automatically for local files).
2. `createJobRun` — submit the run, referencing the uploaded script plus runtime
   and region.
3. `getJobRun` — poll run status.
4. `getJobRunLogs` — stream logs in real time; `getJobRunMetrics` — read resource
   metrics.
5. `cancelJobRun` — cancel if needed.

## Conventions & gotchas
- Job runs require a Community-plus **paid** subscription; a Community org can
  authenticate but cannot run jobs.
- The `wherobots` CLI (`wherobots` binary) and the Airflow `WherobotsRunOperator`
  wrap these same operations for terminal/orchestration use.
- Errors use the FastAPI `{ "detail": ... }` envelope. See
  `errors/wherobots-problem-types.yml`.
