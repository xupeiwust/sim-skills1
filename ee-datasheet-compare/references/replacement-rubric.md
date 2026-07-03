# Replacement Rubric

Use this rubric for electronic component datasheet comparisons and BOM
alternate checks. Apply only the rows that match the component type and circuit
context.

## Severity

| Severity | Meaning |
|---|---|
| `blocker` | Strong evidence the candidate cannot be used without redesign, firmware change, safety violation, or unacceptable operating risk. |
| `high` | Likely to break or degrade the design unless an engineer verifies mitigation. |
| `medium` | May matter depending on margins, environment, or product requirements. |
| `low` | Difference exists but is unlikely to affect the stated use. |
| `unknown` | Required evidence is missing, ambiguous, unreadable, or condition-dependent. |

## Required First-Pass Checks

| Area | What To Compare | Escalate When |
|---|---|---|
| Exact part variant | ordering code, suffix, grade, package, reel/tape option, revision | candidate suffix implies different package, temperature grade, pinout, threshold, tolerance, memory size, or qualification |
| Package and footprint | package family, dimensions, pitch, exposed pad, height, land pattern | footprint, height, pad, thermal pad, pin count, or orientation differs |
| Pinout and pin functions | pin names, no-connect rules, exposed pad, bootstrap/config pins, alternate functions | any signal, power, ground, enable, sense, feedback, address, or strap pin changes meaning or required connection |
| Operating range | recommended supply, input/output ranges, common-mode range, ambient/junction temperature | candidate min/max does not cover the actual circuit operating envelope |
| Absolute maximum | voltage, current, ESD, surge, reverse, transient, junction temperature | candidate has lower stress tolerance close to known board conditions |
| Electrical performance | resistance, offset, gain, accuracy, leakage, threshold, quiescent current, dropout, on-resistance, capacitance | candidate worsens the design's limiting parameter or changes min/typ/max interpretation |
| Timing and interface | setup/hold, propagation delay, clock rate, bus mode, protocol level, startup time | timing margin, firmware assumptions, or interface voltage compatibility changes |
| Power and thermal | power dissipation, thermal resistance, derating, SOA, efficiency, heat path | thermal margin is unknown or candidate package changes heat flow |
| External circuit | required passives, compensation network, oscillator/crystal, bootstrap, feedback divider, layout rules | candidate requires new values, different topology, or layout-sensitive guidance |
| Quality and compliance | automotive/industrial grade, AEC-Q, isolation, safety, RoHS/REACH, moisture sensitivity | required qualification or compliance is absent or a lower grade |
| Lifecycle and sourcing | status, PCN/EOL, manufacturer support, authorized stock | only check with current web sources when asked or needed for the decision |

## Component-Specific Focus

| Component Type | Extra Checks |
|---|---|
| MCU/SoC/FPGA | memory size, boot mode, package pin mux, oscillator, reset, programming interface, peripheral count, voltage domains, errata, firmware impact |
| Power regulator | topology, input/output range, dropout, current limit, compensation, enable logic, soft-start, switching frequency, inductor/capacitor requirements, thermal pad |
| MOSFET/IGBT | Vds/Vce, Id, Rds(on), Vgs threshold, gate charge, SOA, body diode, avalanche, package thermal limits |
| Op-amp/comparator/ADC/DAC | input/output range, offset, bias, noise, bandwidth, slew, stability with load, supply current, reference requirements |
| Sensor | range, accuracy, calibration, interface, sampling rate, package orientation, mechanical opening, environmental limits |
| Connector | pin count, pitch, mating part, plating, height, current rating, keying, retention, orientation |
| Passive | value, tolerance, voltage/current/power, dielectric/material, ESR/ESL, temperature coefficient, package, derating |

## Verdict Rules

- Use `not drop-in` when there is at least one unresolved blocker.
- Use `likely not drop-in` when high risks affect package, pinout, operating
  range, timing, external circuit, or firmware assumptions.
- Use `possible with review` when no blockers are visible but high or medium
  risks remain.
- Use `likely compatible` only when package, pinout, operating range, and
  relevant performance checks are source-backed and remaining differences are
  low risk for the stated circuit.
- Use `insufficient evidence` when critical datasheet sections, package
  variants, pin diagrams, or operating conditions are missing.
