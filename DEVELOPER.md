# Online Food Delivery Management System (OFDMS)
## 🛠️ Developer Onboarding & Technical Documentation

Welcome to the **Online Food Delivery Management System (OFDMS)** developer guide! This document is designed to help you set up your local development environment, understand the system architecture, and follow the automated dependency management and CI/CD pipelines.

---

## 📌 Project Overview
OFDMS is a JSP/Servlet-based web application built using **Java 20** and **Apache Maven**. It features:
*   **User & Admin Management**: Role-based authentication and profile control.
*   **Food Item Management**: Custom data sorting using **Quick Sort** algorithm.
*   **Order Processing**: Queue-based sequential management.
*   **File-Based Storage**: Lightweight CRUD operations powered by plain text database files instead of heavy SQL servers.

---

## 💻 Local Development Setup

### 1. Prerequisites
Ensure you have the following installed on your machine:
*   **Java Development Kit (JDK 20)**: Download from [Adoptium Temurin](https://adoptium.net/).
*   **Apache Maven**: For dependency management and builds.
*   **Servlet Container**: [Apache Tomcat 10.1+](https://tomcat.apache.org/) (compatible with Jakarta EE 10 / Servlet 6.0).
*   **IDE**: [IntelliJ IDEA Ultimate](https://www.jetbrains.com/idea/) is highly recommended for JSP and Servlet support.

### 2. Environment Configuration
Clone the repository and switch to the development branch:
```bash
git clone https://github.com/SDil1/OFDMS.git
cd OFDMS
git checkout dev
```

### 3. Build & Package
To clean the project and compile it into a deployable Web Archive (`.war`) file, run:
```bash
# Windows
mvnw.cmd clean package

# Linux/macOS
./mvnw clean package
```
This will generate the packaged application in the `target/` directory:
`target/onlinefoodDelivery-1.0-SNAPSHOT.war`

---

## 💾 File-Based Database Architecture
To simplify hosting and setup, OFDMS stores all model state in flat-text files. The active text files are initialized inside the respective controllers/services and servlet packages:

| Data Type | Relative File Path | Purpose |
| :--- | :--- | :--- |
| **Users** | `src/main/java/com/example/onlinefooddeliverysystem/controllers/user/user.txt` | Credentials, registration date, and role info |
| **Food Items** | `src/main/java/com/example/onlinefooddeliverysystem/controllers/Fooditem/food.txt` | Available food items, pricing, and descriptions |
| **Orders** | `src/main/java/com/example/onlinefooddeliverysystem/controllers/order/order.txt` | Placed orders and their fulfillment status |
| **Reviews** | `src/main/java/com/example/onlinefooddeliverysystem/controllers/review/review.txt` | Customer reviews, ratings, and comments |
| **Drivers** | `src/main/java/com/example/onlinefooddeliverysystem/controllers/driver/driverRegister.txt` | Registered delivery agents |

> [!NOTE]
> Ensure that the user account running the servlet container has **read/write permissions** for these directories, as runtime updates directly modify these `.txt` files.

---

## 🤖 Automated Dependency & CI/CD Pipelines

To keep the application highly secure and robust, we have configured two automated workflows under the `.github/` folder:

### 1. Dependabot Version Updates (`.github/dependabot.yml`)
Dependabot automatically scans project files weekly for outdated dependencies and creates Pull Requests.
*   **Scan Frequency**: Every Monday at 08:00 UTC.
*   **Target Ecosystems**:
    *   `maven`: Upgrades compile-time dependencies like Junit, Jakarta Servlet API, and plugins in `pom.xml`.
    *   `github-actions`: Upgrades checkout and setup-java actions in workflows.
*   **Open PR Limit**: Dependabot will open a maximum of 10 Maven PRs and 5 GitHub Actions PRs concurrently to prevent repository noise.

### 2. GitHub Actions CI/CD (`.github/workflows/ci.yml`)
Every commit pushed or pull request opened against the `dev` or `master` branches automatically triggers the CI runner to build and test the codebase.
*   Installs JDK 20 (Temurin distribution).
*   Configures automated Maven dependency caching to speed up successive builds.
*   Runs `mvn clean package` to ensure there are no syntax, compiler, or build regressions.

---

## 🚀 Branching & Commit Guidelines

To maintain a clean and reliable release history, developers must adhere to the following workflow:

### 1. The Development Workflow
1.  **Work on `dev`**: All active feature development and bug fixes must be committed directly to the local `dev` branch.
2.  **Pull Requests**: Merge pull requests from custom feature branches into the `dev` branch.
3.  **CI Validation**: Confirm that the GitHub Actions build successfully passes on the `dev` branch.
4.  **Promoting to `master`**: Once a milestone is reached and fully validated, the `dev` branch is merged into the `master` branch for final deployment.

### 2. Committing Changes
Use descriptive, standard prefix tags for commits to allow clean changelogs:

*   `feat: <description>` — For new features (e.g. `feat: add order checkout queue`).
*   `fix: <description>` — For bug fixes (e.g. `fix: prevent out of bounds sorting`).
*   `chore: <description>` — For auxiliary tasks, setup, or dependency files (e.g. `chore: configure dependabot weekly updates`).
*   `docs: <description>` — For documentation edits (e.g. `docs: add developer guide`).

---
