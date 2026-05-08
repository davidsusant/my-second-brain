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
