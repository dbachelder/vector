---
what: "Boolean shorthand for OpenTelemetry `use_otlp_decoding`"
deprecated_since: "0.60.0"
---

### OpenTelemetry: `use_otlp_decoding`

The boolean form of `use_otlp_decoding` is deprecated, but existing boolean values continue to work.
**Users will have at least two minor release cycles to migrate before this shorthand is removed.
No removal release has been scheduled.**

This option controls which signals preserve their OTLP representation instead of being converted to
Vector's native representation. Its boolean shorthand sets **all three signal options**.

#### Planned array form

The planned interface is an array of signal names. **Array support is not implemented yet; do not use
these array values until support is released.** The intended migration is:

| Existing configuration | Planned array form |
| --- | --- |
| `use_otlp_decoding: false` | `use_otlp_decoding: []` |
| `use_otlp_decoding: true` | `use_otlp_decoding: ["logs", "metrics", "traces"]` |
| `use_otlp_decoding: {logs: true, metrics: false, traces: true}` | `use_otlp_decoding: ["logs", "traces"]` |
| Option omitted | Leave it omitted; all signals continue to use native conversion |

An explicit array enables OTLP decoding only for the listed signals. An empty array and an omitted
option both mean no signals use OTLP decoding, preserving the current default. Array order has no
meaning, and unknown signal names will be rejected.

This setting is separate from the OTLP codec's `decoding.signal_types` array, which controls the
signal formats the codec tries to parse and their priority.

#### Migration available today

Until array support is released, use the supported per-signal object form. Replace:

```yaml
use_otlp_decoding: true
```

with:

```yaml
use_otlp_decoding:
  logs: true
  metrics: true
  traces: true
```

When migrating `use_otlp_decoding: false`, set all three options to `false` instead.
