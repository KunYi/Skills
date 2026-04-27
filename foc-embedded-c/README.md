# foc-embedded-c

> An AI pair-programming skill for STM32G4 Field Oriented Control (FOC) motor drive firmware.  
> Provides production-grade PMSM/BLDC control knowledge — SVPWM, Clarke/Park transforms, sensorless observers, current sensing topologies, emergency protection, and 35+ domain reference documents covering the full drive system lifecycle.

---

## What This Skill Does

When an AI coding agent (such as Antigravity / Codex) is working on motor drive firmware, it loads this skill to deliver physically verified, hard real-time FOC code — not generic textbook templates.

The skill enforces:
- **Hardware constraints first** — ADC sampling windows, dead-time current distortion, bootstrap refresh limits, gate-driver UVLO/desat, PCB Kelvin routing
- **Energy-flow awareness** — always identifies whether the DC bus can absorb regenerative power; defines brake chopper strategy; handles source sink-capability constraints
- **Inner loop determinism** — CORDIC/FMAC acceleration where justified; ISR cycle margin tracked via `DWT_CYCCNT`; worst-case verified across operating points
- **Safety boundaries** — distinguishes nominal control, diagnostic monitoring, and safety action paths; documents what is latched vs auto-recoverable
- **Abuse-case mindset** — sensor dropouts, bus faults, stale commands, bad unit assumptions, and fault injection are treated as first-class design inputs

---

## Trigger Scope

The agent activates this skill when the task involves:

| Topic | Examples |
|---|---|
| **Motor Types** | PMSM (sinusoidal FOC), BLDC (six-step commutation) |
| **Control Loops** | Current loop (Id/Iq PI), speed loop, position loop, MTPA, field weakening |
| **Modulation** | 7-segment SVPWM, 5-segment DPWM, overmodulation, minimum pulse width |
| **Transforms** | Clarke, Park, Inverse Park, CORDIC acceleration |
| **Current Sensing** | 1-shunt, 2-shunt, 3-shunt, inline; asymmetric PWM injection |
| **Position / Speed** | QEP encoder, Hall effect, SPI absolute encoder, sensorless SMO + PLL |
| **Protection** | COMP→TIM_BRK fast trip, High-Z vs Active Short Circuit, stall detection, catch-spin |
| **Hardware** | STM32G4 TIM1 center-aligned PWM, dual-ADC DMA, internal OPAMP PGA, CORDIC, FMAC |
| **System Integration** | Braking/regen, EMC/isolation, thermal derating, CAN/UART telemetry, production test |

---

## Reference Documents

The skill uses **task-based routing** — `problem-oriented-reading-guide.md` narrows the load per task.

### Core Control References

| Reference File | Contents |
|---|---|
| `problem-oriented-reading-guide.md` | First-stop navigation by task or symptom |
| `control-foc-loops.md` | Cascaded PI loops, feed-forward, anti-windup, MTPA LUT, field weakening, bandwidth rules |
| `algorithm-svpwm-variants.md` | 7-segment SVPWM, sector generation, DPWM constraints, overmodulation, min pulse width |
| `dq-transform-cordic.md` | Clarke/Park/Inverse Park; STM32G4 CORDIC CSR init; async CORDIC ISR pipeline |
| `fmac-filtering-and-compensation.md` | When to use FMAC vs plain FPU; IIR coefficient loading; trade-off justification |
| `auto-tuning-identification.md` | Rs, Ld/Lq, flux linkage identification; zero-pole cancellation PI tuning |
| `emergency-protection-halt.md` | High-Z vs ASC decision tree; HardFault handling; STM32G4 BDTR/OISx safe states |
| `motor-protection-state-machine.md` | FOC state machine; alignment; open-loop V/f ramp; observer convergence; catch-spin |

### Sensing & Hardware References

| Reference File | Contents |
|---|---|
| `current-sensing-topology.md` | 1/2/3-shunt timing; inline sensing; PCB Kelvin routing; RC anti-aliasing; topology guide |
| `adc-dma-buffering-and-timing.md` | Circular vs ping-pong DMA; ADC packet integrity; compile-time timing guards |
| `stm32g4-foc-hardware.md` | Dead-time distortion compensation; TIM1 init; COMP→BRK; dual-ADC DMA; OPAMP PGA |
| `sensorless-observers.md` | SMO with sigmoid boundary layer; BEMF extraction; observer PLL; HFI |
| `position-sensors.md` | QEP M/T speed estimation; Hall 60° interpolation; SPI absolute encoder delay compensation |
| `bldc-six-step.md` | Trapezoidal commutation table; Hall→sector→gate; TIM1 COM event; sensorless BEMF zero-crossing |

### System Integration References (27 Extensions)

| Reference File | Contents |
|---|---|
| `command-and-supervisory-interfaces.md` | UART/CAN/PWM command paths; mode management; timeout; target ramping |
| `can-uart-telemetry-and-diagnostics.md` | Command framing; telemetry grouping; fault reporting; CRC; bus-load limits |
| `braking-and-regeneration.md` | Normal vs emergency braking; regenerative vs dissipative energy; brake chopper |
| `power-entry-and-dc-link-management.md` | Precharge; inrush; bus-ready criteria; contactor sequencing; brown-in/out behavior |
| `gate-driver-and-power-stage-constraints.md` | UVLO/desat; bootstrap edge cases; MOSFET/SiC/GaN SOA; fault containment |
| `emi-emc-isolation-and-cabling.md` | Common-mode current; shielding; grounding; encoder isolation; dv/dt issues |
| `thermal-system-modeling-and-derating.md` | Winding/module/heatsink thermal paths; sensor placement; derating law |
| `safety-architecture-and-diagnostic-coverage.md` | Fault layering; watchdog policy; latched vs recoverable faults; diagnostic coverage |
| `mechanical-integration-and-servo-behavior.md` | Homing; endstops; brake release; gearbox/backlash; resonance-aware servo behavior |
| `production-test-calibration-and-service.md` | Self-test; calibration retention; end-of-line checks; service logging |
| `firmware-lifecycle-and-update-strategy.md` | Bootloader; rollback; protocol versioning; configuration migration |
| `fault-injection-and-abuse-testing.md` | Electrical/power-stage/sensor/host abuse cases; expected response categories |
| `unit-conventions-and-control-contracts.md` | Mechanical vs electrical frames; peak vs RMS; sign conventions; telemetry naming |
| `nvh-and-acoustic-optimization.md` | Audible-noise sources; PWM/dead-time trade-offs; resonance-aware tuning |
| `commissioning-and-bring-up-playbook.md` | Safe first-power sequence; staged loop enablement; common bring-up failure patterns |
| `common-symptom-to-debug-map.md` | Symptom-driven debug triage; domain narrowing; first measurements to make |
| `data-logging-replay-and-diagnostics-workflow.md` | Event-triggered snapshots; replay mindset; structured debug traces |
| `configuration-and-parameter-governance.md` | Parameter ownership; variant selection; calibration traceability |
| `resonance-identification-and-speed-avoidance.md` | Narrow-band vibration; forbidden-speed regions; notch filters; fast-crossing |
| `motor-and-load-characterization.md` | Inertia; friction; compliance; operating-envelope characterization |
| `compliance-and-qualification.md` | Qualification-style disturbance; pass/fail evidence; firmware compliance robustness |
| `sil-and-model-based-validation-boundaries.md` | What SIL proves vs what it cannot; model-fidelity levels; hardware correlation |
| `measurement-and-instrumentation-best-practices.md` | Probe selection; grounding pitfalls; DAC/telemetry observability limits |
| `compressor-and-refrigeration-drive-applications.md` | Pressure-dependent load; compressor pulsation; operating-map mitigation |
| `fan-and-blower-applications.md` | Airflow-related load; startup under restriction; acoustic priorities |
| `pump-applications.md` | Hydraulic load; startup under head; blocked-condition handling |
| `servo-actuator-applications.md` | Trajectory quality; reversal shock; hold stability; endstop/homing behavior |

---

## ISR Timing Baseline

| Check | Criterion |
|---|---|
| 20 kHz FOC ISR budget | Must not exceed **10–12 µs** (hardware + software overhead) |
| Timing measurement | `DWT_CYCCNT` capture on target hardware |
| Per-block tracking | ADC+Clarke, CORDIC/Trig, Current PI, InvPark+SVPWM, TIM update |
| Worst-case coverage | Verified across high/low speed, regen, and fault recovery |

---

## Core Bring-Up Path

Before writing any control code, the skill requires:

1. Confirm motor type, shunt config, PWM frequency, command source, control mode, DC bus energy path
2. If motor parameters unknown → run auto-tuning (Rs, Ld/Lq, flux linkage, pole pairs)
3. Read hardware constraints (ADC sync, dead-time, ringing margins, bootstrap limits)
4. Select control method (sinusoidal FOC for PMSM / six-step for true BLDC)
5. Select sensing topology and position sensor
6. Choose numeric/acceleration strategy (FPU vs CORDIC vs FMAC)
7. Implement control loops with memory placement and bandwidth separation
8. Verify fault states (High-Z vs Active Short Circuit)
9. Provide hardware acceptance criteria (scope probing plan, pass criteria, failure clues)

---

## Skill File

The AI agent reads [`SKILL.md`](./SKILL.md) for all behavioral rules, routing tables, and algorithm preferences.  
`README.md` (this file) is for human readers only and is not loaded by the agent at runtime.

---

## License

MIT — see [LICENSE](../LICENSE)
