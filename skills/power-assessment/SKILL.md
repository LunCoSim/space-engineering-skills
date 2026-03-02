---
name: power-assessment
description: Perform detailed Electrical Power System (EPS) sizing and analysis. Use this skill to model solar array degradation, battery depth-of-discharge (DoD), and power bus regulation. Trigger this for "solar array sizing," "battery life," or "EPS architecture."
---

# Power Assessment Skill (EPS)

This skill provides high-fidelity sizing for the Electrical Power System, moving beyond simple orbital averages to detailed energy balance.

## Analysis Domains

### 1. Solar Array Sizing
- **Inputs**: Solar constant ($1361 W/m^2$), Cell Efficiency, Packing Factor.
- **Degradation**: Apply End-of-Life (EOL) factors for radiation, thermal cycling, and UV browning.
- **Cosine Loss**: Calculate effective power based on sun incidence angle and spacecraft attitude.

### 2. Battery & Energy Storage
- **Sizing**: Calculate required Capacity (Wh) = $(P_{eclipse} \cdot t_{eclipse}) / (DoD \cdot n_{discharge})$.
- **Life Cycle**: Estimate battery health based on Depth of Discharge (DoD) per orbit over the mission duration.
- **Thermal Interdependency**: Check battery temperature limits (typically 0°C to 30°C for charge).

### 3. Power Distribution (PDU)
- **Architecture**: Trade-off between Regulated (e.g., 28V) vs. Unregulated (Battery Voltage) buses.
- **Peak Power**: Model Peak Power Tracking (MPPT) efficiency vs. Direct Energy Transfer (DET).

## Output Format
1. **Power Analysis Report (`power_analysis.md`)**: Detailed BOL vs. EOL power generation and battery state-of-charge (SoC) profiles.
2. **EPS Configuration (`eps_config.csv`)**: Summary of solar area, battery Wh, and bus voltage.
