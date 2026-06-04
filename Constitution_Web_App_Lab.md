# Lab Activity Solutions: Constitutional Governance for a Web Application
**Role:** Observer | **Duration:** 35 Minutes | **Objective:** Apply architectural laws to web development.

---

## PART 1: Law Summaries & Core Problems
*What problem does each law prevent in a standard web application?*

### 1. ENG-4.1 — Atomic TDD (Non-Negotiable)
* **Prevents:** **Broken user flows and production regressions.** It prevents a developer from shipping a new feature (like a checkout button) that silently breaks an existing feature (like user login) because they skipped writing unit tests under deadline pressure.

### 2. ENG-4.11 — Runtime Process Enforcement (Non-Negotiable)
* **Prevents:** **Silent configuration drift between environments.** It prevents a web app from working perfectly on a developer's local machine but crashing in production because API environment variables or CORS policies were manually altered without code-level enforcement.

### 3. ENG-6.1 — Security by Design (Non-Negotiable)
* **Prevents:** **Data breaches and account takeovers.** It prevents engineers from building a user profile page and forgetting to add authorization checks, which would allow a malicious user to modify another user's data by simply guessing their ID in the URL.

### 4. PRD-1.2 — Problem-First (Non-Negotiable)
* **Prevents:** **Wasted engineering hours on useless features.** It stops the team from spending weeks building a highly complex, real-time comment tracking system before validating whether web users actually want to leave comments in the first place.

### 5. SLS-1.1 — Human-in-the-Loop (Non-Negotiable)
* **Prevents:** **AI-generated security flaws or hallucinated libraries.** It stops a developer from using an AI coding assistant to generate a complex authentication script and pushing it live without a human verifying if the script leaves database connections wide open.

### 6. Performance Optimization Targets (Negotiable)
* **Prevents:** **Over-engineering simple pages.** It prevents an engineer from spending 3 days writing a complex custom database caching layer for a static "About Us" page that only gets 10 visits a day, keeping focus on readability first.

### 7. Documentation Format Preferences (Negotiable)
* **Prevents:** **Review blocks over cosmetics.** It prevents Pull Requests from getting stuck because team members are arguing over whether API routes should be documented in a Markdown file or directly inside Swagger/Postman annotations.

### 8. Naming Conventions (Negotiable)
* **Prevents:** **Pedantic debates in code reviews.** It prevents senior developers from blocking a feature deployment just because a database column was named `user_status` instead of `userStatus`, as long as the code remains clear.

### 9. Test Coverage Thresholds Above Min (Negotiable)
* **Prevents:** **Writing useless tests for vanity metrics.** It stops developers from writing superficial tests for auto-generated boilerplate code just to hit a strict 95% metric, allowing them to focus test coverage on critical business logic like payment processing.

### 10. Tool and Library Choices (Negotiable)
* **Prevents:** **Tech stack stagnation.** It prevents a web application from getting permanently stuck on an outdated framework, allowing the team to adopt a modern utility (like moving from raw CSS to Tailwind) if they present a solid justification to the team.

---

##  PART 2: Ranking Web App Engineering Decisions
*Using ENG-1.1: Security → Correctness → Reliability → Maintainability → Performance → DX.*

1. **Priority 1: Fixing a flaw allowing users to see other users' payment details (`ENG-6.x` / Security)**
   * *Defense:* Security is always #1. A fast, beautiful web application is completely worthless if user data is stolen or compromised.
2. **Priority 2: Fixing a bug where the shopping cart calculates a total of $0.00 (`ENG-4.x` / Correctness)**
   * *Defense:* Correctness is #2. A web app that processes transactions quickly but calculates the wrong math destroys business trust permanently.
3. **Priority 3: Fixing a memory leak causing the server to throw 502 Bad Gateway errors under load (`ENG-7.x` / Reliability)**
   * *Defense:* Reliability is #3. The app must remain online; clean code doesn't matter if customers only see an error page when they visit the site.
4. **Priority 4: Refactoring a 1,500-line routing file into clean, modular controllers (`ENG-3.x` / Maintainability)**
   * *Defense:* Maintainability is #4. If the web app's codebase becomes an unreadable "spaghetti" mess, adding new features in the future becomes impossible or highly prone to bugs.
5. **Priority 5: Updating local Webpack/Vite configurations to speed up hot-reloading for developers (`ENG-1.x` / DX)**
   * *Defense:* Developer Experience is #6 (last). While fast local build times make engineers happy, it must never take precedence over user-facing security, correctness, or app stability.

---

##  PART 3: Conflict Scenarios & Constitutional Verdicts
*Settling standard web development arguments via the law.*

### Scenario 1: The Form Bypass
* **Conflict:** *"We have a promotional landing page launch tomorrow. The frontend lead wants to skip setting up backend API input validation and just rely on frontend HTML validation to save time."*
* **Constitutional Verdict:** **BLOCKED.**
* **Reasoning:** This is a conflict between Launch Speed/DX (#5, #6) and Security (#1). Frontend validation can be easily bypassed by anyone using an API client like Postman or a browser console. Under the Constitution, deadline pressure never overrides security law (`ENG-6.1`).

### Scenario 2: The Shared Test Database
* **Conflict:** *"The web app's test pipeline takes 15 minutes because it creates a clean mock database for every test. A developer wants all tests to share the same staging database instance to cut pipeline times down to 1 minute."*
* **Constitutional Verdict:** **BLOCKED.**
* **Reasoning:** This pits Pipeline Performance/DX (#5, #6) against Test Correctness (#2). Shared database states cause tests to pollute one another, creating false positives or passing tests that actually fail in production. Accurate and correct tests outrank fast tests every single time.

### Scenario 3: The Unreadable SQL Query
* **Conflict:** *"An engineer wrote a highly complex, raw SQL query with nested loops to fetch user dashboards. It runs 3x faster than the clean code written via the ORM, but no one else on the team can decipher how it works."*
* **Constitutional Verdict:** **NEGOTIABLE / CONDITIONALLY BLOCKED.**
* **Reasoning:** This is Performance (#5) vs. Maintainability (#4). Code written for speed that cannot be read will inevitably break during future database updates. The engineer cannot simply ship this raw query; they must explicitly measure the performance bottleneck, heavily document the query's behavior, and bring it to the engineering ensemble for formal approval.
