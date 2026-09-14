# ELSTARU Coding Guidelines

## Dependency Declaration Policy

ELSTARU will **not** use hard-coded dependency coordinates directly inside Gradle build files when a dependency can be managed through the project version catalog.

Avoid this style:

```kotlin
implementation("androidx.compose.material3.adaptive:adaptive:1.3.0")
```

Instead, dependencies should be declared through the Gradle Version Catalog and referenced from Kotlin DSL using the generated, type-safe accessor notation.

Preferred usage:

```kotlin
implementation(libs.androidx.material3.adaptive)
```

This keeps dependency versions centralized, reduces string duplication, makes upgrades easier to review, and gives the project a cleaner and more maintainable Gradle configuration.

> Note: although this style feels more object-oriented than a raw quoted dependency string, the precise Gradle term is **type-safe version catalog accessors**.

---

## Version Catalog Requirement

Adding a dependency therefore normally requires **two coordinated changes**:

1. Register the version and library alias in:

```text
gradle/libs.versions.toml
```

2. Reference the generated accessor from the appropriate module build file, usually:

```text
app/build.gradle.kts
```

For example, the Material 3 Adaptive dependency should be represented in the TOML catalog along these lines:

```toml
[versions]
material3Adaptive = "1.3.0"

[libraries]
androidx-material3-adaptive = { module = "androidx.compose.material3.adaptive:adaptive", version.ref = "material3Adaptive" }
```

Then `app/build.gradle.kts` should use:

```kotlin
implementation(libs.androidx.material3.adaptive)
```

The exact alias structure may evolve as the catalog grows, but the architectural rule stays the same:

**dependency coordinates and versions belong in the version catalog; module build files consume type-safe aliases.**

---

## Why We Use This Convention

This convention is intended to keep ELSTARU maintainable as the project grows into a foldable-first Android application with Compose, Material 3 Adaptive, WindowManager, DUUMA, BILDIGO, and quantum-computing integrations.

The goals are:

- one source of truth for dependency versions;
- fewer hard-coded strings in Gradle files;
- easier upgrades and dependency audits;
- cleaner diffs during code review;
- type-safe access from Kotlin DSL;
- consistent dependency naming across modules.

---

## General Rule

When adding a new library, do **not** begin by pasting a Maven coordinate into `build.gradle.kts`.

Begin by asking:

> Does this dependency belong in `gradle/libs.versions.toml`?

For ordinary project dependencies, the answer should normally be **yes**.

Then expose it through a clear alias and consume that alias from the module.

---

## Example: Material 3 Adaptive

Instead of:

```kotlin
dependencies {
    implementation("androidx.compose.material3.adaptive:adaptive:1.3.0")
}
```

Use:

```toml
# gradle/libs.versions.toml

[versions]
material3Adaptive = "1.3.0"

[libraries]
androidx-material3-adaptive = { module = "androidx.compose.material3.adaptive:adaptive", version.ref = "material3Adaptive" }
```

and:

```kotlin
// app/build.gradle.kts

dependencies {
    implementation(libs.androidx.material3.adaptive)
}
```

This is the preferred ELSTARU dependency-management style going forward.
