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
    -DarchetypeVersion=1.5 \
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
  -DarchetypeVersion=1.5
  ```

- `maven-archetype-quickstart` release history:

  | Version      | Notable changes                                                                                                                               |
  | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
  | `1.0`, `1.1` | Original, very old releases (JUnit 3.8.x)                                                                                                     |
  | `1.3`        | JUnit 4                                                                                                                                       |
  | `1.4`        | Junit 4.11, modernized POM                                                                                                                    |
  | `1.5`        | Latest, published 2024-08-20 -- defaults to Java 17 and JUnit 5.11.0, also generates a `.mvn/` directory with `jvm.config` and `maven.config` |

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

## Standard Directory Structure

```plain
api-test/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   └── resources/
    └── test/
        ├── java/
        └── resources/
```

## Minimal `pom.xml` (Java 17, JUnit 5)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>io.davidsusanto</groupId>
    <artifactId>api-test</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <junit.version>5.11.4</junit.version>
        <surefire.version>3.5.2</surefire.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven.surefire.plugin</artifactId>
                <version>{$surefire.version}</version>
            </plugin>
        </plugins>
    </build>
</project>
```

- `junit-jupier` -- your test execution engine. Without it, `src/test/java` has no framework to run tests against. `junit-jupiter` is an aggregator artifact that pulls in three pieces:

  | Pulled-in artifact     | Purpose                                                                            |
  | ---------------------- | ---------------------------------------------------------------------------------- |
  | `junit-jupiter-api`    | The annotations and assertions you write tests with (`@Test`, `assertEquals`, etc) |
  | `junit-jupiter-params` | Parameterized test support (`@ParameterizedTest`)                                  |
  | `junit-jupiter-engine` | The runtime that actually discovers and executes Jupiter tests                     |

- `maven-surefire-plugin` -- the plugin Maven uses during the `test` phase to discover and run tests.
- The `maven-compiler-plugin` is there for the same reason: pin the version so builds are reproducible.
- It's the smallest set that lets you do `mvn test` reliably and get predictable behavior. You need:
  - A test framework (Jupiter)
  - A way to run it (Surefire)
  - A way to compile sources to your target Java version (Compiler plugin)

## Core Lifecycle Commands

| Command               | Purpose                                                                                   |
| --------------------- | ----------------------------------------------------------------------------------------- |
| `mvn clean`           | Remove `target/`                                                                          |
| `mvn compile`         | Compile `src/main/java`                                                                   |
| `mvn test`            | Run unit tests                                                                            |
| `mvn package`         | Build JAR                                                                                 |
| `mvn install`         | Install to local repo (`~/.m2/repository`)                                                |
| `mvn clean install`   | Full clean build (your standard verification)                                             |
| `mvn dependency:tree` | Inspect transitive dependencies -- useful for the version conflicts you've been resolving |
