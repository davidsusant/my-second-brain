# Maven

## Install

- Download from: https://maven.apache.org
- Extract
- Set environment variables:

  ```bash
  export M2_HOME=/path/to/apache-maven-3.9.x
  export PATH=$M2_HOME/bin:$PATH
  ```

  Those two lines configure Maven on your shell's `PATH` so you can run `mvn` from any directory.

  `M2_HOME` -- points to the Maven installation root.
  `PATH=$M2_HOME/bin:$PATH` -- preprends Maven's `bin/` directory to your `PATH`, which is what makes the `mvn` command resolvable in any terminal.
  Without these, you'd have to invoke Maven by its full path every time, e.g., `/path/to/apache-maven-3.9.x/bin/mvn -v`.

  On macOs/Linux, put those `export` lines in `~/.zshrc`, `~/.bashrc`, or `~/.bash_profile` so they persist across terminal sessions.

  `M2_HOME` is technically optional on modern Maven -- only `PATH` is strictly required for the `mvn` command. But setting both is the conventional, safer approach.

  If you installed Maven via a package manager (Homebrew, SDKMAN, apt, choco), `PATH` is usually configured automatically -- verify with `mvn -v` before adding anything manually.

- Verify: `mvn -v`

## Generate a Project

```bash
mvn archetype:generate \
    -DgroupId=io.davidsusanto \
    -DartifactId=api-test \
    -DarchetypeArtifactId=maven-archetype-quickstart \
    -DarchetypeVersion=1.4 \
    -DinteractiveMode=false
```

- `archetype:generate` is one of the goal of Maven Archetype Plugin -- other plugin are:

  | Goal                             | Purpose                                                                                                                      |
  | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
  | `archetype:generate`             | Create a new project from an archetype (interactive or batch mode)                                                           |
  | `archetype:create-from-project`  | Turn an existing project into a reusable archetype -- useful if you want to template your API test framework for other teams |
  | `archetype:crawl`                | Crawl a Maven repo and build a catalog of available archetypes                                                               |
  | `archetype:update-local-catalog` | Update your local archetype catalog (`~/.m2/archetype-catalog.xml`)                                                          |
  | `archetype:integration-test`     | Run integration tests on an archetype you're developing                                                                      |
  | `archetype:help`                 | Show plugin help; `mvn archetype:help -Dgoal=generate -Ddetail=true` lists all parameters for a specific goal                |

- `mvn archetype:generate` without `-DarchetypeArtifactId=...` drops you into interactive mode with a numbered list of archetype to pick from.
- Interactive mode is Maven's prompt-driven flow where the plugin asks you questions instead of requiring all parameters on the command line.
- `-DarchetypeArtifactId` is the `artifactId` of the archetype (template) you want to generate your project from.
- A full archetype is identified by three coordinates:

  ```bash
  -DarchetypeGroupId=org.apache.maven.archetypes
  -DarchetypeArtifactId=maven-archetype-quickstart
  -DarchetypeVersion=1.4
  ```

- Common official Apache archetypes:

  | Archetype                     | Generates                                                                            |
  | ----------------------------- | ------------------------------------------------------------------------------------ |
  | `maven-archetype-quickstart`  | Minimal Java app with a `Main` class and one JUnit test (most common starting point) |
  | `maven-archetype-simple`      | Even barer skeleton, no sample code                                                  |
  | `maven-archetype-webapp`      | Basic Java web app with `web.xml` and WAR packaging                                  |
  | `maven-archetype-j2ee-simple` | Multi-module J2EE skeleton (EJB, web, ear)                                           |
  | `maven-archetype-archetype`   | Template for creating your own archetype                                             |
  | `maven-archetype-plugin`      | Skeleton for building a Maven plugin                                                 |
  | `maven-archetype-site`        | Project with Maven site documentation setup                                          |
  | `maven-archetype-profiles`    | Starter demonstrating build profiles                                                 |
