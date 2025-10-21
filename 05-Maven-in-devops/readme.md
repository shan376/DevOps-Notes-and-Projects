Topic 5 – Maven (Build & Dependency Management)

### 📘 What is Maven?

Maven is a **build automation tool for Java projects**.
It helps developers **compile, test, package, and manage dependencies** automatically using a single file — `pom.xml`.

---

### ⚙️ Key Features

* Automates project build lifecycle
* Manages dependencies via Maven Central
* Follows standard directory structure
* Uses plugins for testing & deployment

---

### 🧪 Quick Setup

```bash
sudo apt install maven
mvn -version
```

Create and build a sample project:

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app \
-DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd my-app
mvn clean install
```

---

### 📄 pom.xml (Project Object Model)

Defines project details, dependencies, and plugins.
**Example:**

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13.1</version>
</dependency>
```

---

### 🚀 Build Lifecycle Diagram

```
Source Code → Compile → Test → Package (.jar/.war) → Install → Deploy
```

---

### ✅ Summary

Maven makes Java project builds **consistent, automated, and DevOps-ready** — saving time and ensuring dependency control.
