# Task 2 — Telemetry config fields for analog moisture

## Goal
Add Telemetry module configuration fields that capture the analog moisture sensor’s ADC pin and its two-point calibration (dry + wet readings). These belong in the **Telemetry config protobuf**, which is the canonical home for telemetry module settings (see the generated `meshtastic_ModuleConfig_TelemetryConfig` struct).【F:src/mesh/generated/meshtastic/module_config.pb.h†L328-L367】

## Proposed fields
Add these fields to `meshtastic.ModuleConfig.TelemetryConfig` in `module_config.proto` (and thus `meshtastic_ModuleConfig_TelemetryConfig` in the generated header).【F:src/mesh/generated/meshtastic/module_config.pb.h†L328-L367】

| Field name | Type | Default | Purpose |
| --- | --- | --- | --- |
| `moisture_analog_pin` | `uint32` | `0` (unset) | ADC pin number used for the analog moisture sensor. Matches Arduino GPIO numbering. |
| `moisture_raw_dry` | `uint32` | `0` (unset) | Raw ADC value captured during dry calibration. |
| `moisture_raw_wet` | `uint32` | `0` (unset) | Raw ADC value captured during wet calibration. |

### Notes on defaults
- Protobuf scalar defaults are `0` unless otherwise specified, so the “unset” state is represented by `0` for all three fields. This keeps parity with other Telemetry config scalars unless explicit defaults are added later in the proto definition.【F:src/mesh/generated/meshtastic/module_config.pb.h†L328-L367】

## Storage location (Telemetry config protobuf)
The fields should live in the **Telemetry module config** message (`meshtastic.ModuleConfig.TelemetryConfig`), which is generated into the `meshtastic_ModuleConfig_TelemetryConfig` struct in `src/mesh/generated/meshtastic/module_config.pb.h`.【F:src/mesh/generated/meshtastic/module_config.pb.h†L328-L367】
