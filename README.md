# Static Timing Analysis — s9234 Benchmark Circuit

![Tool](https://img.shields.io/badge/Tool-Synopsys%20PrimeTime-blue)
![Technology](https://img.shields.io/badge/Technology-SAED%2090nm-orange)
![Setup](https://img.shields.io/badge/Setup%20Timing-MET-brightgreen)
![Hold](https://img.shields.io/badge/Hold%20Timing-MET-brightgreen)
![TNS](https://img.shields.io/badge/TNS-0-brightgreen)
![Violations](https://img.shields.io/badge/Violations-0-brightgreen)

---

## Overview

Complete Static Timing Analysis of the **s9234 ISCAS'89 benchmark circuit** using Synopsys Design Compiler (DC) for synthesis and Synopsys PrimeTime (PT) for timing verification. The study evaluates setup and hold timing, identifies the critical path, and determines the maximum safe operating frequency through a clock period sweep.

---

## Design Specifications

| Parameter | Value |
|-----------|-------|
| Benchmark Circuit | s9234 (ISCAS'89) |
| Technology | SAED 90nm standard cell library |
| Total Cells | 497 |
| Sequential Cells | 125 flip-flops |
| Combinational Cells | 371 |
| Total Area | 6562.38 units |
| Number of Ports | 76 |
| Number of Nets | 569 |

---

## Flow

```
s9234 Verilog RTL
       │
       ▼
 Synthesis (Synopsys DC)
 saed90nm_typ.db + compile_ultra
       │
       ▼  s9234_synth.v + s9234.sdc
       │
       ▼
 Static Timing Analysis (Synopsys PrimeTime)
 update_timing → setup / hold / QoR / coverage reports
```

---

## Constraints

| Constraint | Value |
|-----------|-------|
| Clock (CK) period | 3 ns (primary) |
| Input delay | 0.1 ns |
| Output delay | 0.1 ns |
| Input transition | 0.05 ns |
| Output load | 0.01 fF |
| Clock uncertainty | 0.02 ns |

---

## Results

### Critical Path

| Parameter | Value |
|-----------|-------|
| Startpoint | DFF_140/Q_reg |
| Endpoint | DFF_88/Q_reg |
| Logic levels | 17 |
| Critical path delay | 2.42 ns |
| Data required time | 4.07 ns |
| **Setup slack** | **+1.65 ns ✅** |
| Hold violations | 0 ✅ |

### Clock Period Sweep

| Clock Period | Frequency | Slack | Timing Status |
|-------------|-----------|-------|---------------|
| 5 ns | 200 MHz | Positive | ✅ No violation |
| 3 ns | 333 MHz | +1.65 ns | ✅ No violation |
| 2 ns | 500 MHz | Small positive | ✅ No violation |
| < 2 ns | > 500 MHz | Negative | ❌ Violation |

> **Maximum safe operating frequency: 500 MHz (2 ns)**  
> Failure below 2 ns is caused by the combinational logic depth (17 levels, 2.42 ns critical path) exceeding the available timing budget — an inherent limit of the design's logic structure.

### QoR Summary

| Metric | Setup (max) | Hold (min) |
|--------|-------------|------------|
| Critical Path Length | 2.27 ns | 0.29 ns |
| Critical Path Slack | 0.80 ns | 0.14 ns |
| Total Negative Slack (TNS) | 0.00 | 0.00 |
| Violating Paths | 0 | 0 |

### Timing Coverage

| Check Type | Total | Met | Violated | Untested |
|-----------|-------|-----|----------|---------|
| setup | 131 | 128 (98%) | 0 | 3 (2%) |
| hold | 131 | 128 (98%) | 0 | 3 (2%) |
| min_pulse_width | 250 | 250 (100%) | 0 | 0 |
| out_setup | 39 | 37 (95%) | 0 | 2 (5%) |
| out_hold | 39 | 37 (95%) | 0 | 2 (5%) |
| **All Checks** | **590** | **580 (98%)** | **0** | **10 (2%)** |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Synopsys Design Compiler | Logic synthesis, compile_ultra |
| Synopsys PrimeTime | STA — setup, hold, QoR, coverage |
| SAED 90nm standard cell library | Target technology |
| SDC | Timing constraint specification |

---

## RTL Source

The s9234 circuit is from the **ISCAS'89 sequential benchmark suite**, a standard public benchmark widely used in academic EDA research.

---

## References

1. https://sportlab.usc.edu/~msabrishami/benchmarks.html
2. https://github.com/santoshsmalagi/Benchmarks
3. Synopsys PrimeTime — https://www.synopsys.com/implementation-and-signoff/signoff/primetime.html
4. Synopsys Design Compiler — https://www.synopsys.com/implementation-and-signoff/rtl-synthesis-test/design-compiler-graphical.html
