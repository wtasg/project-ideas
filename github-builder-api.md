# GitHub Builder API

## Project Description

GitHub Builder API is a Spring Boot–based DevOps automation backend service that allows users to provide a GitHub repository URL and automatically:

* Clone the repository
* Detect and build Java projects using Maven or Gradle
* Generate build artifacts such as JAR or WAR files
*  Store build output in two locations:
   * Internal server storage
   * User-defined destination path

The API is designed to function as a lightweight CI/CD build service. It operates without any user interface and can be fully tested using tools such as curl or Postman.

---

## What This Project Does 

* Accepts a GitHub repository link
* Downloads the source code
* Builds the Java project
* Copies the generated build output to a location specified by the user

---

## Key Features

1 GitHub repository cloning
* Automated Java build execution
* Artifact generation and management (JAR/WAR)
* Dual output distribution
* Modular Spring Boot architecture
* API-only backend service
* Basic path and process safety handling

---

## High-Level Workflow

```
Client (curl/Postman)
        |
        | POST /api/github/build
        | (GitHub URL + Target Path)
        ↓
GitHub Builder API
        |
        ├── Clone GitHub Repository
        ├── Detect Build Tool (Maven/Gradle)
        ├── Execute Build Process
        ├── Generate Artifacts
        ├── Store Internally
        └── Copy to User Location
```

---

## Workflow (Step-by-Step)

* The user sends a GitHub repository URL
* The API clones the repository using Git operations
* The build configuration is detected (Maven or Gradle)
* The build process is executed
* Build artifacts are generated
* Artifacts are copied:
   * To internal storage for system use
   * To a user-defined destination path

---

## Tech Stack

* Backend: Spring Boot (Java)
* Git Integration: JGit
* Build Tools: Maven, Gradle
* File Handling: Java NIO
* Testing: curl, Postman

---

## Difficulty Level

* Level: Medium to Hard
* Category: DevOps Automation / Build Tooling API

---

## Future Enhancements

* Asynchronous build execution
* Build log streaming
* Docker-based sandboxed builds
* Authentication and rate limiting
* Build status tracking
* Artifact download endpoints

---

