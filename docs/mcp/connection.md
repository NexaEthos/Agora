# The connection contract

The technical rules an assistant connection follows. The [guide](README.md) is the step-by-step version.

- **Transport:** remote Streamable HTTP. Copy the exact server URL from the web app's
  **Settings › Your external agent**, including its path and trailing slash.
- **Authentication:** the connection's own MCP key, or the public OAuth/PKCE client that the web app offers
  (Sign in with Nexa). Never infer a client ID or a URL.
- **Authorization:** your access and the connection's activities decide which operations are available. The live
  tool catalog and its schemas are authoritative.
- **Discovery:** use the assistant's MCP tool discovery. Do not rely on a fixed tool count, and do not assume
  complete parity with the web app.
- **Results:** keep the identifiers and links returned. A draft in the assistant's chat is not a saved Agora object;
  confirm a write from its result and a read-back.
- **Failures:** authorization, validation, missing-tool and connection errors are reported as such. An uncertain
  write is checked before it is retried.

This repository defines no API, event schema, metric or capability flag of its own.
