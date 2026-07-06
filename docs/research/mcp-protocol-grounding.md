# Research: MCP Protocol & TypeScript SDK Grounding

Status: informational grounding for `docs/DESIGN.md`
Last verified: 2026-07-06 (via web)

This note records the external facts that constrain the mcplay design so implementation
agents do not have to re-derive them. Where a fact is time-sensitive it is dated.

## Protocol versions

| Revision | Status (2026-07-06) | mcplay use |
|---|---|---|
| `2025-11-25` | Stable release | **v1 target protocol version** |
| `2026-07-28` | Release candidate (final publishes 2026-07-28) | Forward-compat awareness only; NOT a v1 implementation target |

Sources:
- https://modelcontextprotocol.io/specification/2025-11-25
- https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/
- https://github.com/modelcontextprotocol/modelcontextprotocol/releases

Implication: mcplay must not hard-code a single protocol version as a compile-time
constant scattered across the codebase. The negotiated protocol version is a runtime
value returned by the server during `initialize` and MUST be stored in each scenario
file (`protocol.negotiatedVersion`). See `docs/DESIGN.md` §Data Model.

## Transport

- MCP defines `stdio` (local subprocess over stdin/stdout, newline-delimited JSON-RPC 2.0)
  and a Streamable HTTP transport.
- **v1 supports `stdio` only.** Streamable HTTP is a v2 item (adds a network trust boundary).

## JSON-RPC framing (stdio)

- JSON-RPC 2.0 messages, newline-delimited, UTF-8, over the child process stdio.
- Message kinds: request (has `id`), response (has `id` + `result` xor `error`),
  notification (no `id`).
- Requests may flow client→server (most) and server→client (sampling, elicitation, roots
  list changes as notifications, ping).

## Core methods relevant to v1

| Method | Direction | Capability | mcplay v1 (MVP) |
|---|---|---|---|
| `initialize` (+ `notifications/initialized`) | client→server | core | Yes |
| `ping` | both | core | Yes (pass-through) |
| `tools/list` | client→server | tools | Yes (MVP) |
| `tools/call` | client→server | tools | Yes (MVP) |
| `notifications/tools/list_changed` | server→client | tools | Capture as notification |
| `resources/list`, `resources/read`, `resources/templates/list` | client→server | resources | Planned issues (post-MVP) |
| `resources/subscribe`, `notifications/resources/updated` | both | resources | Planned issues (post-MVP) |
| `prompts/list`, `prompts/get` | client→server | prompts | Planned issues (post-MVP) |
| `sampling/createMessage` | server→client | sampling (client) | Planned issues (post-MVP) |
| `roots/list`, `notifications/roots/list_changed` | server→client / client→server | roots (client) | Planned issues (post-MVP) |
| `elicitation/create` | server→client | elicitation (client) | Planned issues (post-MVP) |
| `logging/setLevel`, `notifications/message` | both | logging | Capture as notifications |
| `completion/complete` | client→server | completions | Planned issues (post-MVP) |

## TypeScript SDK

- Package: `@modelcontextprotocol/sdk` (npm). Latest observed: `1.29.0` (2026-07-06).
- Client API: `Client`, `StdioClientTransport({ command, args, env, cwd })`,
  helpers `listTools()`, `callTool()`, `listResources()`, `readResource()`,
  `listPrompts()`, `getPrompt()`.
- Server API: `Server` / `McpServer`, `StdioServerTransport`, request handlers keyed by
  method schemas.
- Sources: https://github.com/modelcontextprotocol/typescript-sdk ,
  https://www.npmjs.com/package/@modelcontextprotocol/sdk

Implication: mcplay uses the SDK `Client` for the recorder harness and the SDK `Server`
for the replay/mock server, rather than hand-rolling JSON-RPC. The recorder additionally
needs raw message visibility (to capture exact bytes/notifications); see
`docs/DESIGN.md` §Recorder for the interception strategy (transport wrapper).

## Known unknowns (may spawn issues during implementation)

- Exact SDK surface for intercepting raw transport frames (vs. only high-level helpers).
  If the SDK does not expose a clean hook, the recorder wraps `StdioClientTransport`'s
  `onmessage`/`send`. Confirm during Issue implementation.
- Whether `McpServer` high-level API can serve fully arbitrary recorded tool schemas
  dynamically, or whether the low-level `Server` with manual request handlers is required
  for the mock. Design assumes the low-level `Server` for maximum fidelity.
