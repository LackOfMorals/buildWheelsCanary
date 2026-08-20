<!-- mcp-name: io.github.neo4j-labs/neo4j-mcp-canary -->

# Neo4j MCP Canary — _The canary goes first so the rest of us know what's coming_

Neo4j MCP Canary is a fast-moving, experimental release of the Neo4j MCP server for customers who want to explore emerging capabilities before they are considered for the official server.

Based on the source of the official Model Context Protocol (MCP) server for Neo4j, this variant is here for exploring potential new capabilities with experimentation.

As it is a labs project, be aware that:

- It is not formally supported.
- It may contain breaking changes between its own releases and with the official Neo4j MCP server.
- It should be tested before using.

You are welcome to contribute — we are always open to new ideas, especially in this canary channel.

> Do not assume the canary will work for your situation. Test first.

## Prerequisites

- A running Neo4j database instance; options include [Aura](https://neo4j.com/product/auradb/), [Neo4j Desktop](https://neo4j.com/download/), or [self-managed](https://neo4j.com/deployment-center/#gdb-tab).
- APOC plugin installed in the Neo4j instance (required — `get-schema` uses `apoc.meta.schema`).
- Any MCP-compatible client (e.g. [VSCode](https://code.visualstudio.com/) with [MCP support](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)).
- To use Query API that provides HTTP(S) communication between the MCP Server and Neo4j server,  the Neo4j Server must be 2026.07 / 5.27 or later. 

> **⚠️ Known Issue**: Neo4j **5.26.18** has a bug in APOC that causes the `get-schema` tool to fail. This is fixed in **5.26.19** and above. If you're on 5.26.18, please upgrade. See [#136](https://github.com/neo4j-labs/neo4j-mcp-canary/issues/136) for details.



## Installation

There are several ways to install this package. 

```bash
# pip
pip install neo4j-mcp-canary

# pipx (isolated install, binary on PATH)
pipx install neo4j-mcp-canary


# uv (recommended)
uv tool install neo4j-mcp-canary

# uvx — run without installing
uvx neo4j-mcp-canary
```


You verify the installation with:

```bash
neo4j-mcp-canary -v
```

## Configuration

The MCP has a rich set of configuration paramneters. These can be supplied as environmental variables or command line.  

| Command line will always override anything else. 


**Command line**

| Command  | Description |
| ------------------------------------------------------------ | ------------------------------------------------------------|
| -h, --help  |  Show this help message |
| -v, --version |  Show version information |
| --neo4j-uri <URI> |  Neo4j connection URI (overrides environment variable NEO4J_URI) |
| --neo4j-username <USERNAME> | Database username (overrides environment variable NEO4J_USERNAME) |
| --neo4j-password <PASSWORD> | Database password (overrides environment variable NEO4J_PASSWORD) |
| --neo4j-database <DATABASE> | Database name (overrides environment variable NEO4J_DATABASE) |
| --neo4j-read-only <BOOLEAN> | Enable read-only mode: true or false (overrides environment variable NEO4J_READ_ONLY) |
| --neo4j-telemetry <BOOLEAN>  |  Enable telemetry: true or false (overrides environment variable NEO4J_TELEMETRY) |
| --neo4j-schema-sample-size <INT> | Number of nodes per label APOC samples when inferring schema (overrides environment variable NEO4J_SCHEMA_SAMPLE_SIZE) |
| --neo4j-cypher-max-rows <INT>  |  Per-call row cap for read-cypher and write-cypher; 0 disables (overrides environment variable NEO4J_CYPHER_MAX_ROWS) |
| --neo4j-cypher-max-bytes <INT>  |  Per-call byte cap for read-cypher and write-cypher; 0 disables (overrides environment variable NEO4J_CYPHER_MAX_BYTES) |
| --neo4j-cypher-timeout <INT>     |    Context timeout in seconds for read-cypher and write-cypher execution; 0 disables (overrides environment variable NEO4J_CYPHER_TIMEOUT) | 
| --neo4j-cypher-max-estimated-rows <INT> |  EXPLAIN-time estimate threshold above which read-cypher refuses the query; 0 disables (overrides environment variable NEO4J_CYPHER_MAX_ESTIMATED_ROWS) | 
| --neo4j-transport-mode <MODE>  |  MCP Transport mode (e.g., 'stdio', 'http') (overrides environment variable NEO4J_TRANSPORT_MODE & NEO4J_MCP_TRANSPORT(deprecated)) | 
| --neo4j-http-port <PORT> |  HTTP server port (overrides environment variable NEO4J_MCP_HTTP_PORT) | 
| --neo4j-http-host <HOST> | HTTP server host (overrides environment variable NEO4J_MCP_HTTP_HOST) | 
|  --neo4j-http-allowed-origins <ORIGINS> |  Comma-separated list of allowed CORS origins (overrides environment variable NEO4J_MCP_HTTP_ALLOWED_ORIGINS) | 
| --neo4j-http-tls-enabled <BOOLEAN> |  Enable TLS/HTTPS for HTTP server: true or false (overrides environment variable NEO4J_MCP_HTTP_TLS_ENABLED) | 
| --neo4j-http-tls-cert-file <PATH> |   Path to TLS certificate file (overrides environment variable NEO4J_MCP_HTTP_TLS_CERT_FILE) | 
| --neo4j-http-tls-key-file <PATH>   |  Path to TLS private key file (overrides environment variable NEO4J_MCP_HTTP_TLS_KEY_FILE)
| --neo4j-http-auth-header-name <HEADER> |  Name of the HTTP header to read auth credentials from (overrides NEO4J_HTTP_AUTH_HEADER_NAME) | 
| --neo4j-http-allow-unauthenticated-ping  <BOOLEAN> |  Allow unauthenticated ping health checks: true or false (overrides NEO4J_HTTP_ALLOW_UNAUTHENTICATED_PING) | 
| --neo4j-http-allow-unauthenticated-tools-list  <BOOLEAN> |  Allow unauthenticated tools list: true or false (overrides NEO4J_HTTP_ALLOW_UNAUTHENTICATED_TOOLS_LIST) | 
| --neo4j-http-allow-unauthenticated-initialize <BOOLEAN> |  Allow unauthenticated tools list: true or false (overrides NEO4J_HTTP_ALLOW_UNAUTHENTICATED_INITIALIZE) | 
| --neo4j-http-allow-unauthenticated-notifications-initialize  <BOOLEAN> |  Allow unauthenticated tools list: true or false (overrides NEO4J_HTTP_ALLOW_UNAUTHENTICATED_NOTIFICATIONS_INITIALIZE) |
| --config-file <PATH> | Path to an optional JSON or YAML config file, used as a lowest-priority configuration source (overrides environment variable NEO4J_CONFIG_FILE) |
| --neo4j-output-format <FORMAT> | Tool response format sent to the LLM client: json or toon (overrides environment variable NEO4J_OUTPUT_FORMAT) |


**Environment Variables**

| Variable  | Description |
| ------------------------------------------------------------ | ------------------------------------------------------------|
| NEO4J_URI   |    Neo4j database URI |
| NEO4J_USERNAME | Database username |
| NEO4J_PASSWORD | Database password |
| NEO4J_DATABASE | Database name (default: neo4j) |
| NEO4J_TELEMETRY | Enable/disable telemetry (default: true) |
| NEO4J_READ_ONLY | Enable read-only mode (default: false) |
| NEO4J_SCHEMA_SAMPLE_SIZE | Number of nodes per label APOC samples when inferring schema (default: 1000) |
| NEO4J_CYPHER_MAX_ROWS | Per-call row cap for read-cypher and write-cypher (default: 1000, 0 disables) |
| NEO4J_CYPHER_MAX_BYTES | Per-call byte cap for read-cypher and write-cypher (default: 900000, 0 disables) |
| NEO4J_CYPHER_TIMEOUT | Context timeout in seconds for read-cypher and write-cypher (default: 30, 0 disables) |
| NEO4J_CYPHER_MAX_ESTIMATED_ROWS | EXPLAIN-time estimate threshold for read-cypher refusal (default: 1000000, 0 disables) |
| NEO4J_TRANSPORT_MODE | MCP Transport mode (e.g., 'stdio', 'http') (default: stdio) |
| NEO4J_MCP_TRANSPORT | MCP Transport mode (e.g., 'stdio', 'http') (default: stdio) |
| NEO4J_MCP_HTTP_PORT | HTTP server port (default: 443 with TLS, 80 without TLS) |
| NEO4J_MCP_HTTP_HOST | HTTP server host (default: 127.0.0.1) |
| NEO4J_MCP_HTTP_ALLOWED_ORIGINS  | Comma-separated list of allowed CORS origins (optional) |
| NEO4J_MCP_HTTP_TLS_ENABLED | Enable TLS/HTTPS for HTTP server (default: false) |
| NEO4J_MCP_HTTP_TLS_CERT_FILE | Path to TLS certificate file (required when TLS is enabled) |
| NEO4J_MCP_HTTP_TLS_KEY_FILE | Path to TLS private key file (required when TLS is enabled) |
| NEO4J_HTTP_AUTH_HEADER_NAME | Name of the HTTP header to read auth credentials from (default: Authorization) |
| NEO4J_HTTP_ALLOW_UNAUTHENTICATED_PING | Allow unauthenticated ping health checks (default: true) |
| NEO4J_HTTP_ALLOW_UNAUTHENTICATED_TOOLS_LIST | Allow unauthenticated tool listing (default: true) |
| NEO4J_HTTP_ALLOW_UNAUTHENTICATED_INITIALIZE | Allow unauthenticated initialize (default: true) |
| NEO4J_HTTP_ALLOW_UNAUTHENTICATED_NOTIFICATIONS_INITIALIZE | Allow unauthenticated notification initialize (default: true) |
| NEO4J_CONFIG_FILE | Path to an optional JSON or YAML config file, used as a lowest-priority configuration source |
| NEO4J_OUTPUT_FORMAT |  Tool response format sent to the LLM client: json or toon (default: json) |


Run `neo4j-mcp-canary --help` to see the complete list with descriptions.

**Example**

```bash
neo4j-mcp-canary \
  --neo4j-uri "bolt://localhost:7687" \
  --neo4j-username "neo4j" \
  --neo4j-password "password" \
  --neo4j-database "neo4j" 
```


##  Connecting via the Query API instead of Bolt

`NEO4J_URI`'s scheme determines which wire protocol the server uses to talk
to Neo4j — no separate flag is needed:

- `bolt://`, `bolt+s://`, `neo4j://`, `neo4j+s://`, etc. → the Bolt driver (default, unchanged behaviour).
- `http://` or `https://` → the [Neo4j Query API](https://neo4j.com/docs/query-api/current/), Neo4j's HTTP-based query interface. Useful for deployments that only expose HTTP or otherwise prefer not to use Bolt.

| Query API mode requires Neo4j **2026.07** or newer (calendar-versioned releases) or **5.27** or newer 

`NEO4J_USERNAME`/`NEO4J_PASSWORD` and per-request Basic/Bearer credentials work the same way in Query API mode as they do for Bolt — see [Transport Modes](#transport-modes) and [Authentication Methods (HTTP Mode)](#authentication-methods-http-mode).


## MCP Tools & Usage

Provided tools:

| Tool                  | ReadOnly | Purpose                                              | Notes                                                                                                                          |
| --------------------- | -------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `get-schema`          | `true`   | Introspect labels, relationship types, property keys | Uses `apoc.meta.schema`. Sampling controlled by `NEO4J_SCHEMA_SAMPLE_SIZE`.                                                    |
| `read-cypher`         | `true`   | Execute arbitrary read-only Cypher                   | Rejects writes, schema/admin DDL, `EXPLAIN`, and `PROFILE`. See [Cypher Execution Safeguards](#cypher-execution-safeguards).   |
| `write-cypher`        | `false`  | Execute arbitrary Cypher (write mode)                | **Caution:** LLM-generated queries can cause harm. Use only in development environments. Not registered when `NEO4J_READ_ONLY=true`. |
| `list-gds-procedures` | `true`   | List GDS procedures available in the Neo4j instance  | Disabled automatically if GDS is not installed.                                                                                |
| `give-feedback`       | `true`   | Submit free-text feedback about the MCP server itself | For feedback on the server (tools, behaviour, docs), not on Cypher/database issues. Limited to 300 characters. See [Feedback](#feedback). |


## Feedback

`give-feedback` lets an agent submit free-text feedback about the MCP server itself — positive or negative — as a single `feedback` string argument, capped at 300 characters (enforced both in the advertised tool schema and by the handler, in case a client doesn't validate the schema before sending). It's for feedback on the server's tools, behaviour, or documentation, not for reporting Cypher/database errors.

Feedback is sent as a Mixpanel event alongside the server's other telemetry, so it is only recorded when telemetry is enabled (see [Telemetry](#telemetry)) — the tool call itself always succeeds either way.

## Usage Guidance

Lessons from canary testing that help an LLM (or a human) get the most out of `read-cypher`:

1. **Aggregate in the database.** `count`, `sum`, `avg`, `collect`, `reduce`, `percentileCont`, `stDev`, and similar reductions collapse to one row and are unaffected by the row cap. A query like `UNWIND range(1, 50000) AS i RETURN sum(i)` runs cleanly; the same range streamed row-by-row is truncated at the row cap.
2. **Always use `LIMIT` for exploratory queries.** The row cap will truncate bare `MATCH` returns; the truncation envelope's `hint` field will tell the caller to add a `LIMIT`. Prefer a `LIMIT` you picked over one the server imposed.
3. **Narrow the `RETURN` projection for wide nodes.** When a record carries many properties (e.g. a full Company node with 19 fields), the byte cap fires before the row cap. Return only the fields you need (`RETURN c.name, c.companyNumber`) rather than the whole node.
4. **Use parameters, including nested maps.** Parameter placeholders (`$name`) are bound from the `params` object; nested access works (`$config.thresholds.pr`). Missing required parameters produce a clear `ParameterMissing` error; extra parameters are silently ignored.
5. **Be explicit about types in comparisons.** Cross-type comparisons like `t.amount > "foo"` evaluate to null and silently filter everything out — no error, just an empty result set. Validate incoming parameter types on the caller side when the result shape surprises you.
6. **`SHOW INDEXES` / `SHOW CONSTRAINTS` are allowed.** Useful before writing a query that depends on an index, or for debugging why a match is slow.
7. **`EXPLAIN` and `PROFILE` are not exposed on `read-cypher`.** Runaway-query protection is already handled by the planner-estimate guard and execution timeout. If you need a profiled plan with runtime stats, use `write-cypher` with `PROFILE`.
8. **Watch for duplicated payloads when returning paths.** `RETURN p, nodes(p), relationships(p)` triples the serialised payload. Return the path or its components, not both.
9. **Long-running queries return a classified error.** When `NEO4J_CYPHER_TIMEOUT` fires, the error names the timeout value and suggests remediation (bound variable-length patterns, add `WHERE` filters, use `LIMIT`) instead of a raw `context deadline exceeded` from the driver.
10. **`OPTIONAL MATCH` for missing data.** When looking up by ID where some IDs may not exist, `OPTIONAL MATCH` returns nulls for misses instead of dropping rows — better for batch lookups.
11. **Defaults are calibrated, not arbitrary.** `1000` rows / `~900 KB` / `30s` / `1M` planner estimate cover the overwhelming majority of exploratory and production queries. Increase them for bulk export workloads; reduce them when serving high-traffic agent deployments.

## Example Natural Language Prompts

Prompts to try in Copilot or any other MCP client:

- "What does my Neo4j instance contain? List all node labels, relationship types, and property keys."
- "Find all Person nodes and show their top relationships, limited to 50 results."
- "What indexes and constraints exist on my database?"
- "Summarise the transaction graph: total count, average amount, and the top 5 customers by PageRank."

## Security tips

- Use a restricted Neo4j user for exploration.
- Review LLM-generated Cypher before executing it in production databases.
- Keep `NEO4J_READ_ONLY=true` for any deployment that shouldn't mutate the graph.
- Leave the Cypher safeguards at their defaults unless you have a specific reason to change them.

## Logging

The server uses structured logging with support for multiple log levels and output formats.

### Configuration

**Log Level** (`NEO4J_LOG_LEVEL`, default: `info`)

Controls verbosity. Supports all [MCP log levels](https://modelcontextprotocol.io/specification/2025-03-26/server/utilities/logging#log-levels): `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert`, `emergency`.

**Log Format** (`NEO4J_LOG_FORMAT`, default: `text`)

- `text` — human-readable (default)
- `json` — structured JSON (useful for log aggregation)

## Telemetry

By default, `neo4j-mcp-canary` collects anonymous usage data to help improve the product. This includes information such as the tools being used, the operating system, and CPU architecture. No personal or sensitive information is collected.

To disable telemetry, set `NEO4J_TELEMETRY=false` (accepted: `true` / `false`; default: `true`). You can also use the `--neo4j-telemetry` CLI flag.

## Reporting feedback

Issues / feedback: open a [GitHub issue](https://github.com/neo4j-labs/neo4j-mcp-canary/issues) with reproduction details (omit sensitive data).
