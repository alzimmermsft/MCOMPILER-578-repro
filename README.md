# Reproducer for MCOMPILER-578

This project is a reproducer for [MCOMPILER-578](https://issues.apache.org/jira/browse/MCOMPILER-578).

## How to reproduce

Use a JDK version of 17 or 21.

Run the build using `mvn clean install`. The build should run compilation for each of the `execution`s for the
`maven-compiler-plugin` in the `pom.xml`, `default-compile`, `base-modules-compile`, and `base-compile`. 
In `/target/classes` the class level for the `module-info.class` should be `55.0`.

Re-run the build using `mvn install`. Again, the build should run compilation for each of the `execution`s, but this
time you'll see it states that there were no files to compile in the `base-modules-compile`. In `/target/classes` the
class level for the `module-info.class` should be the class level of the JDK running build.

If you change the version of `maven-compiler-plugin` to `3.10.1` you'll see that the `base-modules-compile` execution
is always ran, no matter if `mvn clean install` or `mvn install` is used.

## Example output

`mvn clean install`

```
D:\GitHub\MCOMPILER-578-repro [main +3 ~0 -0 | +3 ~2 -0 !]> mvn clean install
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.example:MCOMPILER-578-repro >-------------------
[INFO] Building MCOMPILER-578-repro 1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- clean:3.2.0:clean (default-clean) @ MCOMPILER-578-repro ---
[INFO] Deleting D:\GitHub\MCOMPILER-578-repro\target
[INFO]
[INFO] --- resources:3.3.1:resources (default-resources) @ MCOMPILER-578-repro ---
[INFO] Copying 0 resource from src\main\resources to target\classes
[INFO]
[INFO] --- compiler:3.12.1:compile (default-compile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of changed source code.
[INFO] Compiling 3 source files with javac [debug release 21] to target\classes
[INFO]
[INFO] --- compiler:3.12.1:compile (base-modules-compile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of added or removed source files.
[INFO] Compiling 1 source file with javac [debug release 11 module-path] to target\classes
[INFO]
[INFO] --- compiler:3.12.1:compile (base-compile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of changed source code.
[INFO] Compiling 2 source files with javac [debug release 8] to target\classes
[INFO]
[INFO] --- resources:3.3.1:testResources (default-testResources) @ MCOMPILER-578-repro ---
[INFO] skip non existing resourceDirectory D:\GitHub\MCOMPILER-578-repro\src\test\resources
[INFO]
[INFO] --- compiler:3.12.1:testCompile (default-testCompile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of changed dependency.
[INFO]
[INFO] --- surefire:3.2.2:test (default-test) @ MCOMPILER-578-repro ---
[INFO]
[INFO] --- jar:3.3.0:jar (default-jar) @ MCOMPILER-578-repro ---
[INFO] Building jar: D:\GitHub\MCOMPILER-578-repro\target\MCOMPILER-578-repro-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- install:3.1.1:install (default-install) @ MCOMPILER-578-repro ---
[INFO] Installing D:\GitHub\MCOMPILER-578-repro\pom.xml to D:\maven\org\example\MCOMPILER-578-repro\1.0-SNAPSHOT\MCOMPILER-578-repro-1.0-SNAPSHOT.pom
[INFO] Installing D:\GitHub\MCOMPILER-578-repro\target\MCOMPILER-578-repro-1.0-SNAPSHOT.jar to D:\maven\org\example\MCOMPILER-578-repro\1.0-SNAPSHOT\MCOMPILER-578-repro-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

`mvn install`

```
D:\GitHub\MCOMPILER-578-repro [main +4 ~0 -0 | +3 ~3 -0 !]> mvn install
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.example:MCOMPILER-578-repro >-------------------
[INFO] Building MCOMPILER-578-repro 1.0-SNAPSHOT
[INFO]   from pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- resources:3.3.1:resources (default-resources) @ MCOMPILER-578-repro ---
[INFO] Copying 0 resource from src\main\resources to target\classes
[INFO]
[INFO] --- compiler:3.12.1:compile (default-compile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of changed source code.
[INFO] Compiling 3 source files with javac [debug release 21 module-path] to target\classes
[INFO]
[INFO] --- compiler:3.12.1:compile (base-modules-compile) @ MCOMPILER-578-repro ---
[INFO] Nothing to compile - all classes are up to date.
[INFO]
[INFO] --- compiler:3.12.1:compile (base-compile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of changed source code.
[INFO] Compiling 2 source files with javac [debug release 8] to target\classes
[INFO]
[INFO] --- resources:3.3.1:testResources (default-testResources) @ MCOMPILER-578-repro ---
[INFO] skip non existing resourceDirectory D:\GitHub\MCOMPILER-578-repro\src\test\resources
[INFO]
[INFO] --- compiler:3.12.1:testCompile (default-testCompile) @ MCOMPILER-578-repro ---
[INFO] Recompiling the module because of changed dependency.
[INFO]
[INFO] --- surefire:3.2.2:test (default-test) @ MCOMPILER-578-repro ---
[INFO]
[INFO] --- jar:3.3.0:jar (default-jar) @ MCOMPILER-578-repro ---
[INFO] Building jar: D:\GitHub\MCOMPILER-578-repro\target\MCOMPILER-578-repro-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- install:3.1.1:install (default-install) @ MCOMPILER-578-repro ---
[INFO] Installing D:\GitHub\MCOMPILER-578-repro\pom.xml to D:\maven\org\example\MCOMPILER-578-repro\1.0-SNAPSHOT\MCOMPILER-578-repro-1.0-SNAPSHOT.pom
[INFO] Installing D:\GitHub\MCOMPILER-578-repro\target\MCOMPILER-578-repro-1.0-SNAPSHOT.jar to D:\maven\org\example\MCOMPILER-578-repro\1.0-SNAPSHOT\MCOMPILER-578-repro-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```
