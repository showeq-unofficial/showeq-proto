# scry-proto

Authoritative `seq.v1` protobuf schema — the wire contract every Scry
daemon and client builds against. See [`README.md`](README.md) for the
consumer list and the field-immutability/versioning rules.

## Stack

- `buf` (v2 config) for lint + breaking-change checks + codegen.
- Generated via remote plugins: `buf.build/bufbuild/es` (TypeScript) and
  `buf.build/protocolbuffers/cpp` (C++). Consumers not covered by
  `buf.gen.yaml` (the Elixir `scry`, Rust `iced-miseru`) run their own
  `protoc`/`prost-build` step against this schema directly.

## Structure

- `seq/v1/events.proto` — `Envelope` + all server→client events
- `seq/v1/client.proto` — client→server messages (`Subscribe`, etc.)

## Commands

- `buf lint`
- `buf breaking --against '.git#tag=<last-tag>'`
- `buf generate` — regenerate the TS/C++ bindings this repo produces
  directly (each consumer repo also has its own regen step for its own
  language — see that repo's own docs)

## Conventions

- **Field numbers are immutable.** Removed fields go through `reserved`.
  Breaking changes require a new package version (`seq.v2`).
- Two `buf lint` rules are deliberately excepted in `buf.yaml`, and both
  are load-bearing, not style slips:
  - `ENUM_VALUE_PREFIX` — `Topic`/`SpawnType` enum values are referenced
    unprefixed across the daemon + web sources; renaming is wire-safe but
    high-churn, so it's left alone.
  - `FIELD_LOWER_SNAKE_CASE` — `class_`/`int_` intentionally shadow
    reserved words in C++/TS; the trailing underscore is required, not a
    naming mistake to "fix".

## Before Committing

- `buf lint`
- `buf breaking --against` the previous tag (CI runs this on every PR).

## Documentation

- [`README.md`](README.md) — consumer list, versioning rules, license
  rationale (MIT here vs. GPL-2.0 in the reference daemon).
