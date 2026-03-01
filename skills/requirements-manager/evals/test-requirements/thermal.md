# Thermal Subsystem Requirements

### [REQ-THERMAL-001] Lunar Night Survival
**Status**: DRAFT
**Parent**: [REQ-SYS-010]
**Verification Method**: Analysis

The thermal subsystem shall maintain all critical spacecraft components within their non-operational temperature limits during the lunar night environment (-170°C).

**Rationale**: Lunar night lasts for approximately 14 Earth days, and components will freeze and incur permanent damage without thermal management.

### [REQ-THERMAL-002] Lunar Day Operations
**Status**: DRAFT
**Parent**: [REQ-SYS-011]
**Verification Method**: Test

The thermal subsystem shall maintain all operating spacecraft components within their operational temperature limits during lunar day operations (up to 120°C ambient).

**Rationale**: The lunar surface becomes extremely hot under direct solar illumination, requiring active or passive heat rejection to prevent electronics failures.

### [REQ-THERMAL-003] Heat Rejection
**Status**: DRAFT
**Parent**: [REQ-THERMAL-002]
**Verification Method**: Inspection

The thermal subsystem shall include a radiator to reject excess heat during lunar day operations.

**Rationale**: Necessary to satisfy REQ-THERMAL-002 and dump internal heat loads.

### [REQ-THERMAL-004] Survival Heating
**Status**: DRAFT
**Parent**: [REQ-THERMAL-001]
**Verification Method**: Inspection

The thermal subsystem shall include survival heaters to keep critical components above their minimum survival temperatures.

**Rationale**: Necessary to satisfy REQ-THERMAL-001. Active heating is required when passive insulation is insufficient.

### [REQ-THERMAL-005] Survival Heater Power Limit
**Status**: DRAFT
**Parent**: [REQ-THERMAL-004]
**Verification Method**: Analysis

The survival heaters shall draw no more than 20W of continuous electrical power during lunar night operations.

**Rationale**: The power budget during eclipse/night is severely constrained, but the allocation was increased from 15W to 20W following the thermal design review.
**History**: Increased from 15W to 20W based on updated thermal analysis of minimum survival limits.
