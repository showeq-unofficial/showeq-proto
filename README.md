# showeq-proto

Wire protocol for the ShowEQ daemon / client split. Authoritative `.proto`
schema consumed by the C++ daemon, the TypeScript web client, and the
forthcoming Rust decoder.

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
GPL obligations, even though the reference daemon (`showeq-daemon`) is GPL-2.0.
