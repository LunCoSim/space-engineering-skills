---
name: communications-assessment
description: Perform RF link budget analysis, data volume assessments, and communication architecture sizing. Use this skill to calculate SNR, link margins, required antenna gains, data throughput, and ground station requirements. Trigger this for "link budget," "downlink rate," "RF analysis," "antenna sizing," "ground station," or "data budget."
---

# Communications Assessment Skill

> Read `CONVENTIONS.md` at the repo root before proceeding.

This skill sizes the telecommunications subsystem to ensure command and telemetry links meet required bit error rates (BER) and data throughput objectives.

## Before You Begin

Ask the user (if not already known):
1. What orbit/distance? (determines path loss — LEO, GEO, Lunar, Deep Space)
2. What ground network? (Own ground station, NASA DSN, KSAT, SSC, ATLAS, etc.)
3. What frequency bands are allocated or desired? (S-band, X-band, Ka-band, UHF, optical)
4. What data volume per day must be downlinked? (drives data rate and pass duration needs)
5. Are there regulatory / licensing constraints? (ITU coordination, FCC license)
6. What standards framework?

## Applicable Phases
- **Primary**: Phase A (architecture selection), Phase B (link budget closure)
- **Supporting**: Phase C/D (ground station commissioning, frequency coordination)

## Analysis Workflows

### 1. RF Link Budget
- **Equation**: $P_{rx} = P_{tx} + G_{tx} + G_{rx} - L_{path} - L_{other}$
- **Path Loss**: $L_{path} = 20 \log_{10}(4\pi d / \lambda)$ — scales with distance and frequency.
- **Noise**: Calculate System Noise Temperature ($T_{sys}$) and $G/T$ figure of merit.
- **Margin policy** (ask user to confirm):
  - LEO: ≥ 3 dB
  - GEO / Lunar: ≥ 6 dB
  - Deep Space: ≥ 3 dB (assuming DSN with high-gain antenna)
- **Modulation & Coding**: Note assumed modulation scheme (BPSK, QPSK, 8PSK) and coding gain (LDPC, turbo codes). At Phase A, typical coding gain is 8-10 dB.

### 2. Data Volume & Throughput
- **Daily data budget**: Total Mbits/day = (science data + housekeeping + overhead) / compression ratio.
- **Pass capacity**: Data per pass = data rate × pass duration × protocol efficiency (~90%).
- **Storage**: Size onboard Solid State Recorder (SSR) to handle accumulation between ground passes.

### 3. Hardware Architecture
- **Frequency selection**: S-band (TT&C, lower data rate), X-band (payload data), Ka-band (high-rate), optical (emerging, very high rate).
- **Antenna trade**: Omni (low gain, wide beam, simple ops) vs. HGA (high gain, narrow beam, requires pointing).
- **Transponder**: COTS vs. custom, power consumption, mass.

### 4. Ground Segment Awareness
- **Ground station network**: Number of stations, geographic distribution, antenna diameter.
- **Coverage**: Total contact time per day (depends on orbit and station locations — request access analysis from `mission-analysis-specialist`).
- **Latency**: For deep space, note one-way light time for command/response planning.

## Output Format
1. **Link Budget Table (`link_budget.md`)**: Tabular breakdown of uplink and downlink parameters with margin status.
2. **Data Pass Summary (`data_summary.md`)**: Daily data throughput estimates and storage margins.
3. **Comm Architecture Summary**: Selected frequency, antenna type, transponder, and ground network.

## Interface
- **Reads from**: `/requirements/`, `/analysis/mission-analysis-specialist/` (access windows, distance), `/analysis/power-assessment/` (available power for transmitter)
- **Writes to**: `/analysis/communications-assessment/`
- **Consumed by**: `systems-engineering-assessment` (link budget summary), `mission-operations-manager` (pass planning)
