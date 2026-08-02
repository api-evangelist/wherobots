---
name: Explore the spatial data catalog
description: Browse Wherobots catalogs, namespaces, and tables to discover spatial datasets before querying.
api: openapi/wherobots-cloud-openapi-original.json
operations: [listCatalogs, listNamespaces, listTables, getTable, getCatalogHierarchy]
---

# Explore the Wherobots spatial data catalog

Discover available spatial datasets (including `wherobots_open_data`, Overture Maps,
and your own connected catalogs) before writing Spatial SQL.

## Auth
- `X-API-Key` header (or `WHEROBOTS_API_KEY`).

## Steps
1. `listCatalogs` — list catalogs available to your organization.
2. `getCatalogHierarchy` — get the full catalog/namespace/table tree in one call.
3. `listNamespaces` — list namespaces (schemas) within a catalog.
4. `listTables` — list tables within a namespace.
5. `getTable` — inspect a table's schema (geometry columns, fields) before querying.

## Conventions & gotchas
- List endpoints are cursor-paginated (`cursor`/`limit`/`offset`).
- External catalogs (AWS Glue, Databricks Unity Catalog, S3 Tables) are connected
  via Cloud Connections and queried in place (data federation).
- Once you have identified a table, run queries with the `wherobots-run-spatial-sql`
  skill. The hosted MCP server can also do this catalog exploration conversationally.
