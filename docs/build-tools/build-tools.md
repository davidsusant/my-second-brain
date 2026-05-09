# Build Tools

## What is the difference between `version '1.0-SNAPSHOT'` and `version '1.0.0'`?

These are two differences: syntax an version string.

**Syntax difference:**

`version '1.0-SNAPSHOT'` uses Groovy's method-call syntax -- it's calling `version()` as a method with the string as an argument. `version '1.0.0'` uses property assignment syntax -- it's directly setting the `version` property. In Gradle Groovy DSL, both are functionally equivalent and set the `project.version` property.

**Version string difference:**

`1.0-SNAPSHOT` follows Maven/Gradle convention where `-SNAPSHOT` indicates this is a **development/in-progress build** -- not yet released, potentially unstable, and subject to change. Dependency caches treat snapshots differently (they re-check for updates).

`1.0.0` follows semantic versioning and represents a **stable, fixed release**. Once published, this version is considered immutable.
