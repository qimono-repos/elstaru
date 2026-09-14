# Functional Specifications - Elstaru

This document tracks the specific features and behaviors required for the Elstaru foldable application.

## [SPEC-001] Tabletop Mode Reports

### Description
The application must transition to a specialized "Report Dashboard" when the device is placed in a tabletop posture.

### Triggers
- Device posture changes to **Half-Opened** (Tabletop).
- Hinge angle is typically between 60° and 120°.

### Behavior
- **Automatic Layout Shift:** The UI must detect the hinge position and split the interface.
- **Top Half (Vertical):** Displays data visualizations, charts, and primary report metrics. This section should be optimized for viewing without hands.
- **Bottom Half (Base/Horizontal):** Displays interactive controls, time-range filters, and secondary details. This section acts as a "control surface" on the flat part of the device.
- **Continuity:** If the device is fully unfolded (Flat), the app should merge these views into a dual-column or expanded dashboard.
- **Folded (Closed):** Show a summarized "Report Preview" or notification on the cover screen.

### Technical Requirements
- Use `androidx.window.layout.WindowInfoTracker` to detect `FoldingFeature`.
- Handle `FoldingFeature.State.HALF_OPENED`.
- Use `androidx.compose.material3.adaptive` layouts to manage the split-pane transition.
