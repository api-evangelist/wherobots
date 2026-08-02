---
name: Run a Spatial SQL query
description: Authenticate, open a WherobotsDB SQL session, and run a Spatial SQL query against Wherobots Cloud.
api: openapi/wherobots-cloud-openapi-original.json
operations: [createApiKey, createSqlSession, listSqlSessions, getSqlSession, deleteSqlSession]
---

# Run a Spatial SQL query on Wherobots Cloud

Use the Wherobots Cloud API (base `https://api.cloud.wherobots.com`) to execute
Spatial SQL powered by WherobotsDB / Apache Sedona.

## Auth
- Authenticate with a Wherobots API key in the `X-API-Key` header (create/list with
  `createApiKey` / `getApiKeys`, or in Settings > API Keys). SDKs/CLI read it from
  `WHEROBOTS_API_KEY`.

## Steps
1. `createSqlSession` — start a SQL Session on a chosen runtime (e.g. TINY) and
   region (default `us-west-2`). This provisions managed compute.
2. Poll `getSqlSession` (or `listSqlSessions`) until the session is ready.
3. Execute Spatial SQL through the session (via the Python `wherobots-python-dbapi`,
   the JDBC driver, or the `wherobots-sql-driver` TypeScript SDK — all connect to
   the session started above).
4. `deleteSqlSession` when finished to release compute.

## Conventions & gotchas
- The SQL API caps returned results at **1000 rows** on the wire (cluster-side
  computation is unaffected). Aggregate or `LIMIT` accordingly.
- Results default to Apache Arrow encoding, Brotli compression, EWKT geometry.
- Pagination on list endpoints is cursor-based (`cursor`/`limit`/`offset`).
- Errors use the FastAPI `{ "detail": ... }` envelope; 401 = bad/expired key,
  403 = role/edition restriction, 422 = validation error. See
  `errors/wherobots-problem-types.yml`.
