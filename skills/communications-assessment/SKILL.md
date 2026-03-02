---
name: communications-assessment
description: Perform RF link budget analysis and data volume assessments. Use this skill to calculate SNR, link margins, and required antenna gains. Trigger this for "link budget," "downlink rate," or "RF analysis."
---

# Communications Assessment Skill

This skill sizes the telecommunications subsystem to ensure command and telemetry links meet the required bit error rates (BER).

## Analysis Workflows

### 1. RF Link Budget
- **Equation**: $P_{rx} = P_{tx} + G_{tx} + G_{rx} - L_{path} - L_{other}$
- **Noise**: Calculate System Noise Temperature and $G/T$.
- **Margin**: Ensure a minimum 3 dB margin for LEO and 6 dB for Deep Space/Lunar under worst-case conditions.

### 2. Data Volume & Throughput
- **Capacity**: Calculate total Mbits per day based on pass durations, data rates, and protocol overhead.
- **Storage**: Size onboard Solid State Recorder (SSR) to handle data accumulation between ground station passes.

### 3. Hardware Requirements
- **Frequency**: Identify S-band (TT&C), X-band (Payload), or Ka-band (High-rate) constraints.
- **Antenna**: Trade-off between Omni-directional (low gain, wide beam) vs. Parabolic/HGA (high gain, narrow beam).

## Output Format
1. **Link Budget Spreadsheet (`link_budget.md`)**: A tabular breakdown of the uplink and downlink parameters.
2. **Data Pass Summary (`data_summary.md`)**: Estimates of daily data throughput and storage margins.
