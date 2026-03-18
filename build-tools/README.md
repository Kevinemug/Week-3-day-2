# 🛠️ Build Tools

## 🎯 Learning Objectives

- Understand why build tools exist and what problems they solve
- Know the basic structure of a Maven `pom.xml` and a Gradle `build.gradle`
- Run common build lifecycle commands (compile, test, package)
- Understand dependency management and where dependencies come from

---

## 📌 Exercise 1: Why Build Tools?

### Question

Without a build tool, a developer must manually:
- Run `javac` with long classpaths to compile source files
- Download and manage third-party `.jar` files
- Run `java` with the correct classpath for tests
- Assemble a `.jar` or `.war` for deployment

### Task

List **three concrete problems** that arise from doing all of this manually, and explain how a build tool such as Maven or Gradle solves each one.

---

### ✅ Answer

| Problem | How a build tool solves it |
|---|---|
| **Classpath management** – manually listing dozens of `.jar` paths is error-prone | The build tool resolves dependencies automatically from a central repository (e.g. Maven Central) and computes the classpath for you |
| **Reproducibility** – different developers may have different versions of dependencies | A `pom.xml` or `build.gradle` file checked into source control pins exact versions, ensuring every developer and CI server uses the same artifacts |
| **Tedious multi-step process** – compile → test → package requires multiple commands | A single command (`mvn package` or `./gradlew build`) executes the full lifecycle in the correct order |

---

## 📌 Exercise 2: Maven – Reading a `pom.xml`

### Question

Given the following `pom.xml`:

```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0.0</version>

    <dependencies>
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>32.1.2-jre</version>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

### Task

Answer the following questions:

1. What is the **artifact ID** and **version** of this project?
2. What runtime dependency is declared?
3. What does `<scope>test</scope>` mean for the JUnit dependency?
4. What Maven command would you run to **compile** this project?
5. What Maven command would you run to **package** it into a JAR?

---

### ✅ Answer

1. **Artifact ID**: `my-app` | **Version**: `1.0.0`
2. **Runtime dependency**: `com.google.guava:guava:32.1.2-jre` – available on the compile and runtime classpath.
3. `<scope>test</scope>` – JUnit is only on the classpath during test compilation and execution; it is **not** included in the final packaged JAR.
4. `mvn compile`
5. `mvn package`  (also compiles and runs tests before packaging)

---

## 📌 Exercise 3: Gradle – Reading a `build.gradle`

### Question

Given the following `build.gradle` (Groovy DSL):

```groovy
plugins {
    id 'java'
}

group = 'com.example'
version = '1.0.0'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'com.google.guava:guava:32.1.2-jre'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.0'
}

test {
    useJUnitPlatform()
}
```

### Task

1. What repository is configured for downloading dependencies?
2. What is the difference between `implementation` and `testImplementation`?
3. What Gradle command compiles and runs all tests?
4. What Gradle command produces a runnable JAR in `build/libs/`?

---

### ✅ Answer

1. **Repository**: Maven Central (`mavenCentral()`) – the default public repository for JVM artifacts.
2. **`implementation`** – dependency is available at compile time and runtime in production code. **`testImplementation`** – dependency is available only when compiling and running tests; it is excluded from the production build.
3. `./gradlew test`
4. `./gradlew jar`  (or `./gradlew build` to compile + test + jar)

---

## 📌 Exercise 4: Common Lifecycle Commands

### Task

Fill in the blank – match each description to the correct Maven **and** Gradle command.

| Description | Maven | Gradle |
|---|---|---|
| Download dependencies and compile source code | `mvn compile` | `./gradlew compileJava` |
| Run unit tests | `mvn test` | `./gradlew test` |
| Package into a JAR (skip tests) | `mvn package -DskipTests` | `./gradlew jar -x test` |
| Remove all build output | `mvn clean` | `./gradlew clean` |
| Full build: clean + compile + test + package | `mvn clean package` | `./gradlew clean build` |
| Install artifact to local repository | `mvn install` | `./gradlew publishToMavenLocal` |

---

## 📌 Key Concepts Summary

| Concept | Description |
|---|---|
| **Build tool** | Automates compiling, testing, and packaging Java projects (Maven, Gradle, Ant) |
| **pom.xml** | Maven's project descriptor; defines dependencies, plugins, and build settings |
| **build.gradle** | Gradle's build script; supports Groovy or Kotlin DSL |
| **Dependency scope** | Controls which phase a dependency is available (`compile`, `test`, `runtime`, etc.) |
| **Maven Central** | The default public artifact repository; hosts virtually all open-source JVM libraries |
| **Build lifecycle** | The ordered sequence of phases Maven/Gradle executes: compile → test → package → install → deploy |
