---
what: "OpenTelemetry `use_otlp_decoding` configuration option"
deprecated_since: "0.60.0"
---

### OpenTelemetry: `use_otlp_decoding`

We plan to replace the entire `use_otlp_decoding` option, including its boolean and per-signal object
forms, with the standard `decoding` configuration. This avoids introducing another representation for
this option or requiring users to migrate through an intermediate object form.

**The replacement is not implemented in the OpenTelemetry source yet. Keep your existing
`use_otlp_decoding` configuration until a supported migration is released.** Existing values continue
to work. Removal is not yet scheduled. Once the replacement is available and the deprecation is
announced, we will give users at least one month to migrate before removing this option.

#### Planned migration

For configurations that preserve the OTLP representation of all signals, replace:

```yaml
use_otlp_decoding: true
```

with the following configuration **once the OpenTelemetry source supports it**:

```yaml
decoding:
  codec: otlp
```

Omitting `decoding.signal_types` selects all three OTLP signals: logs, metrics, and traces. The existing
OTLP codec already has this default; accepting `decoding` on the OpenTelemetry source requires new
support.

For `use_otlp_decoding: false`, the intended migration is to remove the option and omit the entire
`decoding` block. Omitting the block will continue to use Vector's native conversion for all signals,
preserving the source's current default.

#### Mixed native and OTLP configurations

A configuration such as `use_otlp_decoding: {traces: true}` preserves OTLP traces while converting logs
and metrics to Vector's native representation. The existing OTLP codec's `signal_types` array selects
which signal formats to try parsing, in priority order; it does not provide native conversion for
signals omitted from that array. Setting `decoding.signal_types: ["traces"]` is therefore not an
equivalent migration.

We will define and release a supported migration for mixed configurations before deprecating the
whole option. Until then, keep the existing per-signal configuration.
