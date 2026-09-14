# Agent Guidelines for Elstaru

This document provides context and rules for AI agents working on the Elstaru project.

## Project Vision
Elstaru is not a generic phone app; it is a **foldable-first** application. All UI/UX decisions should account for dual-screen states, hinge postures, and screen continuity. Functional requirements are tracked in [SPECS.md](file:///C:/Users/qi/StudioProjects/elstaru/SPECS.md).

## Technical Standards

### 1. Dependency Management
- **MANDATORY:** Always use the Version Catalog located at `gradle/libs.versions.toml`.
- **PROHIBITED:** Do not use hardcoded string implementations in `build.gradle.kts` files.
- When adding new dependencies, update the TOML file first using the `libs.xxx` naming convention.

### 2. SDK & APIs
- **Target/Compile SDK:** Always maintain at **API 37** (Android 15) or higher.
- This is required for compatibility with the latest `androidx.compose.material3.adaptive` components.

### 3. Adaptive Layouts
- Prioritize using the `androidx.compose.material3.adaptive` suite.
- Focus on components like `ListDetailPaneScaffold` and `SupportingPaneScaffold`.
- Logic should handle `WindowSizeClass` changes dynamically.

### 4. Build Performance
- Gradle **Configuration Cache** is enabled (`org.gradle.configuration-cache=true`).
- Ensure any build script modifications do not break configuration caching.

## Context for Design
- **Folded State:** Treat as a standard "Compact" phone layout.
- **Unfolded State:** Treat as an "Expanded" tablet-style layout.
- **Hinge Posture:** Account for "Tabletop" mode where the hinge is partially folded.
