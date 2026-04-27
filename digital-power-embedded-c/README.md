# digital-power-embedded-c

> An AI pair-programming skill for STM32G4xx digital power control firmware.  
> Provides deep domain knowledge for PFC, DC-DC converters, LLC/PSFB, and grid-tied/off-grid inverters — with production-grade C code, scheduling patterns, and hardware-verified control algorithms.

---

## What This Skill Does

When an AI coding agent (such as Antigravity / Codex) is working on digital power firmware, it loads this skill to replace generic code generation with domain-specific, physically executable engineering.

The skill enforces:
- **Physics-first constraints** — ADC sampling windows, PWM-ADC sync, ringing margins, HRTIM dead-time
- **Hard real-time correctness** — fast-loop / slow-loop / 1 ms task layering with explicit scheduling ownership
- **MISRA-oriented design** — no dynamic memory, no recursion, explicit types, full interface prototypes
- **Protection priority** — OVP/OCP/state machine decoupling, recoverable fault chains, auto-recovery sub-states
- **Hardware acceptance criteria** — every code answer includes a scope-based verification plan

---

## Trigger Scope

The agent activates this skill when the task involves:

| Topic | Examples |
|---|---|
| **Target MCU** | STM32G4xx, STM32G474, STM32G431 with FPU, CORDIC, HRTIM |
| **Power Topologies** | PFC boost, Vienna rectifier, buck, boost, buck-boost, LLC, PSFB, grid-tied inverter, off-grid inverter |
| **Control Algorithms** | PI, PR, LPF, NOTCH, IIR 2P2Z, SRF-PLL, DSOGI-PLL, dq/Clarke/Park transforms |
| **Hardware Integration** | ADC-PWM synchronization, injected/regular ADC groups, dual-ADC simultaneous sampling, CORDIC trig |
| **Code Quality** | MISRA C:2012, static analysis, unit testing, CI/CD for embedded power |
| **Scheduling** | ISR hot-path optimization, precomputed coefficients, fast/slow loop layering |
| **Protection** | OVP, OCP, recoverable state machines, hardware analog watchdog, PWM shutdown path |

---

## Reference Documents

The skill uses **task-based routing** — only the relevant reference is loaded per task, not all 20 at once.

| Reference File | Contents |
|---|---|
| `pi-code-variants.md` | Standard, fast-path, and extreme PI implementations; anti-windup; CMSIS DSP reuse |
| `pi-fast-path.md` | Hot-path PI for high-frequency ISRs; branch elimination; cycle optimization |
| `filter-and-resonant-variants.md` | PR, LPF, NOTCH, IIR 2P2Z discrete design and optimization |
| `lpf-and-rms-variants.md` | First-order LPF, cascaded RMS/LPF, coefficient precomputation |
| `notch-and-2p2z.md` | NOTCH and IIR 2P2Z structures; cascaded biquad vs direct IIR |
| `pll-extreme-variants.md` | SRF-PLL, DSOGI-PLL, DDSRF, EPLL; lock-state detection; CORDIC acceleration |
| `dq-transform-optimization.md` | Clarke/Park/Inverse Park; CORDIC acceleration; multi-rate angle compensation |
| `state-machine-recovery-templates.md` | Fault detect/confirm/shutdown/recovery chain; recoverable sub-states |
| `scheduling-patterns.md` | ISR load budgeting; fast/slow loop split; ADC-PWM-CAN timing guidelines |
| `protection-integration.md` | OVP/OCP/PG/Fault integration; hardware signal + software lock; anti-chattering |
| `architecture-design-patterns.md` | Event-driven control loops; data flow pipelines; configuration-driven design |
| `performance-analysis-optimization.md` | Cycle budget analysis; CORDIC/DMA acceleration; compiler techniques |
| `code-quality-testing.md` | MISRA compliance strategy; static analysis; unit/integration testing |
| `engineering-practices-devops.md` | Build system; CI/CD pipeline; documentation generation; release management |
| `adc-calibration-sampling.md` | STM32G4 ADC self-calibration; OFR register; dual-ADC DMA; hardware oversampling; AWD |
| `topology-pfc-vienna.md` | Single-phase boost PFC; Vienna rectifier dq control; THD optimization |
| `topology-dcdc-converters.md` | Buck/boost voltage-mode/PCMC/ACMC; CCM/DCM; synchronous rectification |
| `topology-llc-psfb.md` | LLC frequency control; PSFB phase-shift; ZVS conditions; burst mode; soft-start |
| `topology-inverter-grid.md` | Grid-tied dq current control; off-grid voltage control; SVM; anti-islanding; MPPT |
| `stm32g4-capability-matrix.md` | STM32G4 family feature availability; CORDIC/HRTIM/COMP/OPAMP verification checklist |

---

## Design Baseline

| Parameter | Default |
|---|---|
| **MCU** | STM32G4xx (e.g., STM32G474, STM32G431) |
| **Language** | C99 / C11 |
| **Numeric type** | `float32_t` (single-precision FPU) |
| **Fast loop** | 40 kHz (conservative default) |
| **Slow loop** | 10 kHz (fast loop ÷ 4 to ÷ 20) |
| **Background task** | 1 ms |
| **PWM update** | TIM1/TIM8 update event at counter zero |
| **ADC trigger** | TIM1/TIM8 PWM-synchronized to switching period |
| **Code style** | MISRA-oriented; no dynamic memory; no recursion |

---

## Output Structure (Per Task)

Every code response from this skill includes:

1. **Assumptions** — explicitly stated boundary conditions
2. **Interface / data structures** — complete prototypes and struct definitions
3. **Full implementation** — scheduling-aware, ISR-safe C code
4. **Integration notes** — call order, precomputation location, loop ownership
5. **Hardware acceptance criteria** — what to probe on the oscilloscope, pass/fail limits, failure clues

---

## Skill File

The AI agent reads [`SKILL.md`](./SKILL.md) for all behavioral rules, routing tables, and algorithm preferences.  
`README.md` (this file) is for human readers only and is not loaded by the agent at runtime.

---

## License

MIT — see [LICENSE](../LICENSE)
