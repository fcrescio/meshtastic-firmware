# Task 1 — Heltec V4 pin plan (moisture sensor)

## Analog ADC pin selection
- **GPIO5 (A4) on ADC1, channel 4** for the analog moisture output.
  - Rationale: Heltec V4 already uses **GPIO1 / ADC1** for battery measurement, so the sensor should avoid GPIO1 to prevent conflicts with the onboard divider. GPIO5 is listed as an available analog pin (`A4`) in the board’s Arduino pin mapping, while GPIO1 is consumed by the battery ADC in the variant definition.【F:variants/esp32s3/heltec_v4/pins_arduino.h†L19-L39】【F:variants/esp32s3/heltec_v4/variant.h†L5-L9】
  - **ADC range:** use **ADC attenuation 11 dB** for a 0–3.3 V input range (safe for typical capacitive probes that swing near 0–3 V). This keeps full-scale headroom on ESP32-S3’s ADC1.

## Calibration plan
- Capture two calibration points (per probe):
  1. **Dry reading**: probe in air (or dry substrate).
  2. **Wet reading**: probe submerged in water (or saturated substrate).
- Store these as `raw_dry` and `raw_wet`, then scale in firmware:
  - `moisture_percent = clamp((raw - raw_dry) / (raw_wet - raw_dry), 0.0, 1.0) * 100`.
- Recalibrate if the probe chemistry drifts or if the soil type changes.

## Digital alarm pin selection
- **GPIO45** for the digital alarm (comparator) output from the probe.
  - **Polarity:** treat as **active-low** (alarm asserted = LOW).
  - **Pull-up:** **enable `INPUT_PULLUP`**, assuming the probe’s digital output is open-collector/open-drain or otherwise can safely sink to ground.

## Detection Sensor module wiring hints (for task 3)
- `monitor_pin`: **GPIO45**
- `trigger_type`: **active-low / falling** (trigger on LOW)
- `pullup`: **enabled** (`INPUT_PULLUP`)

> Notes: GPIO1 is already assigned to the battery ADC on Heltec V4, so the analog probe should not share that pin; GPIO5 is an available analog pin from the board’s Arduino pin map.【F:variants/esp32s3/heltec_v4/variant.h†L5-L9】【F:variants/esp32s3/heltec_v4/pins_arduino.h†L19-L39】
