# Teltonika FMC003 — Data Capabilities & Swedish Context

## Overview

The FMC003 is a compact OBD-II plug-and-play tracker with 4G LTE Cat-1 / 2G fallback, Bluetooth 4.0 BLE, and an internal 3-axis accelerometer. It reads both GPS position and live vehicle diagnostics directly from the OBD-II port.

---

## Data the Device Can Track

### GPS / GNSS Position
| Parameter | Notes |
|---|---|
| Latitude / Longitude | ~3 m nominal accuracy under open sky |
| Altitude | Less reliable than horizontal position |
| Speed (GNSS-derived) | Calculated from position delta, not wheel sensors |
| Heading / Course | Derived from movement direction |
| GNSS quality (PDOP/HDOP) | Indicates fix quality; high values = poor fix |
| Odometer (GNSS trip/total) | Accumulates driven distance from GPS track |

### OBD-II Vehicle Diagnostics (up to 32 configurable parameters)
| Parameter | Notes |
|---|---|
| Engine RPM | Can lag 4–5 min after cold start |
| Vehicle speed (ECU) | More accurate than GNSS speed in urban stop-go |
| Engine load % | |
| Coolant temperature | |
| Fuel level | OEM sensor; accuracy depends on vehicle calibration |
| Fuel rate / consumption | Calculated using default speed-tiers (see limitations) |
| Real odometer (ECU) | OEM odometer value from ECU — generally reliable |
| Throttle position | |
| Intake air temperature | |
| Battery/system voltage | |
| Active DTCs / fault codes | Diagnostic Trouble Codes |

> **Note:** OBD-II parameter support varies by vehicle make, model, and year. Older vehicles, some commercial vans, and non-EU spec vehicles may expose fewer parameters or use non-standard PIDs.

### Accelerometer & Motion Events
| Event | Description |
|---|---|
| Harsh braking | Deceleration above configurable threshold |
| Harsh acceleration | |
| Sharp cornering | |
| Crash detection | High-G event; triggers panic-priority record |
| Towing detection | Movement without ignition |
| Excessive idling | Engine on, no movement over time threshold |

### Network & System Telemetry
- GSM cell ID + area code (coarse location fallback, ~100–1000 m accuracy)
- Active operator / roaming status
- Signal strength
- Device battery voltage and charge current
- Sleep mode state

### Geofencing
- Configurable zones; entry/exit events transmitted as high/panic priority records

### Bluetooth (BLE) Sensors
- Supports Teltonika-compatible BLE peripherals: temperature/humidity sensors, driver ID tags (iBeacon/ELA)

---

## Transmission & Storage

- **Default report interval:** 120 seconds (configurable per movement state / ignition state)
- **Flash memory:** 128 MB (~422,000 records) — stores records when offline, flushes on reconnect
- **Protocols:** Teltonika Codec 8 over TCP or UDP
- **Typical data usage:** 5–15 MB/month per vehicle

---

## Known Limitations & Data Quality Issues

### GPS Accuracy
- Nominal **±3 m** under clear sky — degrades significantly in:
  - Urban canyons (Stockholm inner city, dense blocks)
  - Underground parking and road tunnels (Södra länken, Norra länken, Slottsskogen tunnel)
  - Dense forest terrain (common in rural Sweden)
  - Multi-storey car parks
- During signal loss the device dead-reckons briefly then stops recording position — **gaps in track are expected in tunnels**
- GNSS altitude is unreliable; do not use for elevation profiling

### Speed Data
- GNSS-derived speed has ~1–2 second latency and is noisier at low speeds (<15 km/h)
- ECU speed (OBD) is more accurate but requires a stable OBD connection
- At very low crawl speeds (parking manoeuvres) both sources can report 0 when the vehicle is moving

### OBD Communication Delays
- OBD protocol negotiation takes time on startup — **RPM and some parameters may not appear for the first 4–5 minutes** of a trip
- Some vehicles disconnect the OBD bus when the engine is off; the device loses diagnostic data until next ignition cycle

### Fuel Consumption Calculation
- The device uses **simplified default speed tiers** (city=30 km/h, average=60 km/h, highway=90 km/h) with a +20% multiplier per additional 50 km/h
- This is a rough estimate — **not suitable for precise fuel cost allocation or invoicing** without custom calibration per vehicle model
- Real fuel level from OBD sensor is the fuel tank float sensor, which can be inaccurate ±5–10% depending on vehicle

### Harsh Driving Event Thresholds
- Thresholds are configurable but defaults may not fit all vehicle types (a loaded truck vs. a light van have very different normal deceleration profiles)
- Cornering events on Swedish roundabouts (which are very common) may generate false positives if thresholds are not tuned

### Connectivity Gaps
- 2G fallback is used when 4G is unavailable — Sweden's 2G network (GSM 900/1800) is being progressively decommissioned; **Tele2 ended 2G in 2024, Telia in 2025**. Check that your SIM operator still maintains 2G coverage in areas of expected travel or that the device operates reliably on 4G only
- Records are buffered in flash and uploaded on reconnect — timestamps are accurate but there will be a delivery delay

---

## Swedish Legal & Regulatory Considerations

### GDPR / IMY (Integritetsskyddsmyndigheten)

Vehicle tracking that identifies a natural person (employee, sole trader, private individual) is **processing of personal data** under GDPR (Regulation 2016/679), which applies directly in Sweden.

**Key obligations:**

| Requirement | What it means in practice |
|---|---|
| **Legal basis** | You must have a documented lawful basis — typically *legitimate interest* (Art. 6.1.f) for fleet management, or *contract* for logistics. Consent is rarely appropriate for employee tracking as it's not freely given in an employment context. |
| **Purpose limitation** | Data collected for route optimisation cannot be repurposed for covert investigation of an individual employee |
| **Data minimisation** | If 10-minute intervals serve your purpose, collecting every 30 seconds is not justified |
| **Retention limits** | Define and enforce a retention period. IMY has fined companies for keeping tracking data indefinitely |
| **Transparency** | Employees must be informed clearly — what is tracked, why, how long data is kept, and their right to access it |
| **Data subject rights** | Employees can request access, rectification, and erasure (subject to legitimate interest balancing) |

**IMY guidance:** IMY has published specific guidance on employee monitoring (*Kontroll av anställda*, updated 2022). Fleet tracking for operational purposes is generally permissible under legitimate interest, but covert tracking of private-use vehicle time is not.

### Swedish Co-determination Act (MBL) — Union Consultation
If the company has collective agreements or union representation, introducing a tracking system typically requires **primary negotiation (primär förhandling) under MBL §11** before deployment. Skipping this is a common mistake and can result in labour law disputes.

### Private Use vs. Business Use
- If company vehicles are used for private travel, **tracking during private-use hours requires stronger justification or explicit consent**
- A common compliant approach: geo-fence the depot/office; tracking is active only during working hours or ignition-on during defined business hours
- If the vehicle is strictly a work tool with no private use permitted (e.g. van goes home overnight for on-call), the legitimate interest basis is stronger

### Speed Data and Legal Proceedings
- GNSS speed from a tracker is **not admissible as evidence of speeding** in Swedish courts — only approved speed measurement devices (Trafikverket type-approved) can be used for prosecution
- However, speed data can be used internally for HR purposes and insurance assessments
- At ±3 m GPS accuracy and 1–2 s latency, **do not use tracker speed to determine whether a vehicle exceeded 90 km/h on a specific road** — the margin of error is too large for that level of precision

### Insurance Implications
- Some Swedish insurers (Länsförsäkringar, If, Trygg-Hansa) offer telematics-discounted premiums; the FMC003 data may qualify if the insurer accepts the data format
- Crash detection events (high-G records) can be useful for post-accident reconstruction, but treat them as indicative, not definitive — airbag ECU data is more authoritative

### Stolen Vehicle Recovery
- Tracking data is usable by Swedish police for stolen vehicle recovery under **RB 27 kap.** (search/seizure law) — ensure your data retention covers at least 30 days to support this use case

### Heavy Vehicles / Tachograph
- If tracking commercial vehicles >3.5 t GVW, the FMC003 **does not replace the legal tachograph requirement** under EU Regulation 165/2014 (implemented in Sweden via Transportstyrelsens regulations). The tracker is a supplement, not a substitute.

---

## Summary: What the Data Is Good For vs. Where to Be Careful

| Use case | Reliability | Notes |
|---|---|---|
| Route history / proof of visit | Good | Gaps in tunnels; log timestamps |
| Trip start/end times | Good | Ignition event + GNSS |
| Mileage (ECU odometer) | Good | Prefer OBD odometer over GNSS odometer |
| Driver behaviour scoring | Moderate | Tune thresholds per vehicle type |
| Precise speeding evidence | Poor | Not suitable for enforcement |
| Fuel cost allocation | Moderate | Use ECU fuel level delta, not calculated rate |
| Crash reconstruction | Indicative | High-G event gives timing, not full physics |
| Real-time location | Good | 2-min default interval; 2G sunset risk |
| Altitude / 3D routing | Poor | GNSS altitude unreliable |
