# scry-proto

Wire protocol for the Scry daemon / client split. Authoritative `.proto`
schema, consumed as a git submodule (or, for the Elixir `scry` daemon, via
a sibling-checkout `protoc` invocation) by every daemon and client in the
Scry family: `scry-cpp`/`scry-cpp-quarm` and `scry-qt` (C++/Qt),
`scry-web`/`scry-web-demo` (TypeScript), `scry` (Elixir), and
`iced-miseru` (Rust).

## Layout

```
seq/v1/           # v1 schema (stable surface)
  events.proto    # Envelope + all server->client events
  client.proto    # client->server messages (Subscribe, etc.)
```

## Versioning

Field numbers are immutable. Removed fields go through `reserved`. Breaking
changes require a new package version (`seq.v2`).

CI enforces `buf breaking --against` the previous tag.

## License

MIT. See [LICENSE](LICENSE). The permissive license is deliberate: any
client — open-source, proprietary, personal — can consume the schema without
GPL obligations, even though the reference daemon (`scry-cpp`) is GPL-2.0.
