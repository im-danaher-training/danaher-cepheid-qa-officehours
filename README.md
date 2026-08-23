# Danaher Cepheid — QA Copilot Office Hours

## 🎯 Purpose

This is a hands-on practice environment designed to build **expert-level GitHub Copilot skills** for QA engineers working with Selenium, TestNG, and enterprise test automation frameworks.

This is **NOT** intended to teach the complete application domain. The application is the practice playground.

The primary objective is to learn how to use Copilot across an enterprise-style QA lifecycle: understanding an application and its automation framework, planning test coverage, writing structured prompts, generating test artifacts, reviewing them critically, debugging failures, and iterating — rather than blindly accepting whatever Copilot produces.

---

## 🏢 Business Scenario

> This is a fictional training application created for GitHub Copilot hands-on practice. It does not contain or represent production Cepheid software, data, architecture, or processes.

**CepheidDx Instrument Care** is a fictional internal tool used by field service engineers and lab administrators to manage diagnostic instrument fleets deployed at customer sites (the same fictional scenario used by the companion developer office-hours repository, so QA and Dev participants can discuss a shared example).

Business context:
- CepheidDx sells GeneXpert-style diagnostic instruments to labs and hospitals (fictional names only, e.g., "Fictional Regional Lab").
- Each instrument requires periodic preventive maintenance (PM) on a fixed schedule.
- The team is building **CepheidDx Instrument Care**, a small internal web application to track instruments, their maintenance schedule, and maintenance history.
- QA is responsible for validating the **Preventive Instrument Maintenance Scheduling** feature (schedule, view, update, validate) through an automated Selenium/TestNG regression suite.

The business domain is intentionally simple so that **Copilot usage is the skill being practiced, not domain modeling or manual test execution**.

---

## 🏗️ Application Architecture

```
Angular Web Application (instrument-care-ui)
        ↓  HTTP/JSON (REST)
Spring Boot REST API (instrument-care-api)
        ↓  JPA/Hibernate
Local H2 Database (file-based, in-repo, no external DB server)

        +

Selenium WebDriver
TestNG
Page Object Model
```

**Application under test** (built out during the lab, mirroring the developer repository's feature):
- Angular UI with an instrument list, instrument detail view, and a maintenance-scheduling form.
- Spring Boot REST API backing the UI (`/api/instruments`, `/api/maintenance-records`).
- Local H2 database seeded with fictional instruments and maintenance records.

**Automation framework** (what QA participants will build/extend):
```
Test Layer (TestNG test classes)
    ↓
Page Objects (InstrumentListPage, MaintenanceFormPage, ...)
    ↓
Reusable Components (BaseTest, WaitHelper)
    ↓
WebDriver Management (DriverFactory)
    ↓
Configuration (browser, base URL, timeouts)
    ↓
Test Data (fictional instruments/maintenance records)
```

Do not implement the application or automation framework as part of this README — this document describes what participants will build later.

---

## 🛠️ Technology Stack

- **Java 17**
- **Maven** (build + dependency management for the automation project)
- **Angular** (application under test — same stack as the developer repository)
- **Spring Boot** (application under test's API layer)
- **H2 Database** (application under test's local database)
- **Selenium WebDriver 4.x** (Selenium Manager handles driver binaries automatically)
- **TestNG** (test execution, `@DataProvider`, parallel execution)
- **Page Object Model** (locators/actions isolated from test logic)
- **TestNG DataProvider** (data-driven test scenarios)
- **GitHub Actions** (CI: build app → start app → smoke tests → regression tests → report)
- **Optional: Cucumber** (BDD conversion — Expert Challenge only)
- **Optional: cloud-browser execution** (BrowserStack/Sauce Labs style — Expert Challenge only)

Do not force Cucumber, cloud-browser execution, or any other technology into the core exercise — they are optional stretch activities only.

---

## ⏱️ Duration

**90–120 minutes total**

### Core Lab (required, ~90 minutes)
Phases 1–6 (Understand, Test Plan, Test Cases, Test Data, Automate, Regression Debugging).

### Optional Challenges (~30+ minutes, only if time remains)
Phase 7 (Cross-Browser), Phase 8 (Parallel Execution), Phase 9 (CI/CD), Phase 10 (Advanced: Cloud Browsers / BDD), and the Expert Challenges section.

---

## 🎯 Learning Objectives

By the end of this session, participants should be able to:

- Understand an application and its existing automation framework using Copilot.
- Provide effective context to Copilot (open files, selections, workspace structure).
- Write structured prompts instead of vague, one-line requests.
- Reduce unnecessary re-prompting by front-loading context and constraints.
- Use repository instructions for persistent QA conventions.
- Use prompt files for reusable, task-specific prompts.
- Use custom agents for multi-step QA workflows.
- Use skills for reusable specialized QA capabilities.
- Critically review Copilot-generated test code rather than accepting it as-is.
- Validate AI-generated changes by running the suite, not just reading the diff.
- Debug test failures methodically (reproduce → analyze → diagnose → fix).
- Iterate on prompts when the first response produces flaky or low-value tests.
- Apply Copilot across the QA lifecycle: test planning → case design → data → automation → regression → CI.
- Recognize when **not** to trust Copilot-generated output as-is (e.g., a generated assertion that would pass regardless of the actual bug).

---

## 🧠 Copilot Expert Mindset

> Copilot is an engineering assistant, not an autonomous authority.

Every activity in this lab should follow this loop:

```
Ask
 ↓
Understand
 ↓
Plan
 ↓
Generate
 ↓
Review
 ↓
Test
 ↓
Validate
 ↓
Iterate
```

**Do not blindly accept generated code.** For every test Copilot generates, participants must be able to answer:
- What behavior does this test actually verify?
- Would this test fail if the feature were broken? (If not, it's not a real test.)
- Does it duplicate an existing test?
- Is it independent of test execution order and other tests' state?

---

## 🧩 Copilot Customization Practice

You will not be required to build all of these during the core lab, but you should understand what each one is for and, where time allows, create at least one.

### Instructions
Instructions (`.github/instructions/copilot-instructions.md`) define **persistent** QA conventions Copilot should apply to every request in this repo.

Example rules you should eventually add to this repository:
- Secure coding: no hardcoded credentials in test code.
- Java standards: consistent naming, no unused imports, one responsibility per Page Object method.
- Testing standards: Page Object Model required; explicit waits only, no `Thread.sleep()`.
- Logging standards: log the test name and failure reason, never log secrets/tokens.
- Dependency management: no new automation library without justification.
- Minimal changes: prefer the smallest diff that satisfies the requirement.
- No secrets: never commit real credentials or environment URLs.

### Prompt Files
Prompt files (`.github/prompts/*.md`) provide reusable, task-specific prompt templates.

Suggested prompt files for this repository:
- `test-plan-prompt.md`
- `test-case-generation-prompt.md`
- `test-data-generation-prompt.md`
- `test-automation-prompt.md`
- `regression-debugging-prompt.md`
- `cross-browser-testing-prompt.md`

### Custom Agents
Agents (`.github/agents/*.agent.md`) define a specialized persona with a multi-step workflow.

Suggested agents for this repository (design these before implementing):
- **Test Design Agent** — takes a feature requirement and produces a risk-based test plan + test cases.
- **Test Automation Agent** — takes approved test cases and implements them as Page-Object-based Selenium/TestNG tests.
- **Debugging Agent** — takes a failing test's stack trace/screenshot and performs root-cause analysis before proposing a fix.

### Skills
Skills (`.github/skills/<name>/SKILL.md`) capture a reusable specialized capability.

Suggested skills for this repository:
- Test planning (risk-based test plan structure for this application)
- Test case generation (the test-case template used in Phase 3)
- Test data generation (fictional data generation rules — see Phase 4)
- Selenium automation (Page Object + explicit-wait conventions)
- Regression analysis (how to triage a failing regression run)
- Cross-browser testing (how browser selection is configured)

---

# 🧪 QA Office Hours — Hands-on Phases

## Phase 1 — Understand the Application

Participants use Copilot to understand:
- Application workflows (how a maintenance record moves from `SCHEDULED` to `COMPLETED`).
- UI components (`instrument-list`, `instrument-detail`, `maintenance-form` — once scaffolded).
- APIs (`/api/instruments`, `/api/maintenance-records` and their request/response shapes).
- The database (the `instrument` and `maintenance_record` tables).
- The existing automation framework structure (Page Objects, base test class, driver factory — once scaffolded).

**Suggested prompt:**
```
Role: QA engineer new to instrument-care-ui/instrument-care-api
Task: Summarize the maintenance-scheduling workflow and the API contract behind it
Constraints: reference actual endpoint paths and status values once they exist
Output: a workflow summary + list of UI/API touchpoints relevant to testing
```

---

## Phase 2 — Generate Test Plan

**Feature under test: Preventive Instrument Maintenance Scheduling**
(Same feature as the developer office-hours repository.)

- Users can schedule a new maintenance record (date, type, engineer).
- Users can view upcoming maintenance sorted by due date.
- Users can update a maintenance record's status through allowed transitions only.
- Invalid requests must be rejected with a clear error.

Participants use Copilot to create a **risk-based test plan** covering:
- **Functional scenarios**: create a record, list upcoming records, valid status transitions.
- **Negative scenarios**: invalid `maintenanceType`, past `dueDate`, unknown `instrumentId`, invalid status transition.
- **Boundary scenarios**: `dueDate` exactly today (accepted) vs. yesterday (rejected).
- **Regression scenarios**: creating a record does not affect unrelated instruments' maintenance history.

**Suggested prompt:**
```
Role: QA test planner for the maintenance-scheduling feature
Task: Produce a risk-based test plan (functional, negative, boundary, regression)
Constraints: prioritize scenarios by business risk; note which are automatable now
Output: prioritized scenario list grouped by category
```

---

## Phase 3 — Generate Test Cases

Participants convert the highest-priority scenarios from Phase 2 into structured test cases containing:

| Field | Description |
|---|---|
| Test ID | e.g., `TC-MAINT-001` |
| Scenario | one-line description |
| Preconditions | required app/data state |
| Test Data | exact fictional values to use |
| Steps | numbered UI/API steps |
| Expected Result | exact expected outcome |
| Priority | High/Medium/Low |
| Test Type | Functional/Negative/Boundary/Regression |

Explain how to review and remove redundant AI-generated test cases: if two cases exercise the same code path with only cosmetic data differences (e.g., two different but equally valid `dueDate` values), keep one and discard the other unless there's a specific boundary reason to keep both.

**Suggested prompt:**
```
Role: QA test case author
Task: Convert the top 8 scenarios from the test plan into structured test cases
Constraints: use the table format above; flag any case that duplicates another
Output: test case table
```

---

## Phase 4 — Test Data

Participants use Copilot to:
- Analyze test-data requirements from the test cases (valid/invalid `maintenanceType`, boundary `dueDate` values, valid/invalid `instrumentId`).
- Generate fictional data only (never real customer/patient data).
- Create positive and negative datasets.
- Parameterize the relevant TestNG tests using `@DataProvider`.

**Suggested prompt:**
```
Role: Test data designer for TestNG DataProvider
Task: Generate a fictional dataset (5+ rows, at least 2 negative) for maintenance scheduling
Constraints: no real credentials/customer data; cover boundary dueDate values
Output: @DataProvider method + backing data
```

---

## Phase 5 — Automate Tests

Participants implement the selected test cases using:
- Selenium WebDriver + Java
- TestNG (`@Test`, `@BeforeMethod`/`@AfterMethod`, `@DataProvider`)
- Page Object Model (`InstrumentListPage`, `MaintenanceFormPage`, etc.)
- Explicit waits (`WebDriverWait`) — never `Thread.sleep()`
- Reusable methods (shared login/navigation steps in a base class or helper)
- Meaningful assertions (assert the actual business outcome, not just "element exists")

**Suggested prompts:**
```
Role: Selenium/TestNG automation engineer
Task: Create InstrumentListPage and MaintenanceFormPage Page Objects
Constraints: no locators/actions in test classes; explicit waits only
Output: 2 Page Object classes
```
```
Role: Selenium/TestNG automation engineer
Task: Automate TC-MAINT-001 through TC-MAINT-004 using the Page Objects above
Constraints: one assertion focus per test; use the DataProvider from Phase 4 where applicable
Output: test class using Page Objects
```

---

## Phase 6 — Regression Debugging

The following planned failure scenarios will be introduced into the automation suite (by design, for this exercise) so participants practice diagnosing before fixing:
- **Locator issue** — a locator that no longer matches the current UI (e.g., renamed element id).
- **Timing issue** — an action performed before the page/element is ready.
- **Assertion mismatch** — an assertion checking the wrong expected value.
- **Test-data problem** — a data-driven test using a value that violates a business rule (e.g., a past `dueDate`).
- **Stale element** — a `WebElement` reference used after a page navigation invalidated it.

Participants must:
1. Execute the regression suite.
2. Analyze each failure (stack trace, screenshot if available, TestNG report).
3. Ask Copilot for a root-cause analysis (RCA) of the failure.
4. Identify the actual root cause (not just the symptom).
5. Fix the test (or confirm it's correctly catching a real application defect).
6. Re-run the regression suite to confirm the fix holds.

> **Diagnose first. Modify second.** Do not let Copilot change assertions or locators without first explaining *why* the original one was wrong.

**Suggested prompt:**
```
Role: QA engineer debugging a failing regression test
Task: Given this stack trace/failure (paste it), identify the root cause
Constraints: reference the exact line/locator/assertion at fault; do not propose a fix yet
Output: root cause explanation only
```

---

## Phase 7 — Cross Browser *(Optional Challenge)*

Design the exercise for:
- Chrome
- Edge
- Firefox

Explain **configuration-driven browser selection** — the browser should be chosen via a config value (e.g., a system property or config file entry), not hardcoded into each test.

**Sample commands (once the framework exists):**
```bash
mvn test -Dbrowser=chrome
mvn test -Dbrowser=edge
mvn test -Dbrowser=firefox
```

---

## Phase 8 — Parallel Execution *(Optional Challenge)*

Participants configure TestNG parallel execution (`parallel="methods"` or `parallel="classes"` in `testng.xml`).

Explain:
- **Thread safety** — shared static state (e.g., a static `WebDriver`) breaks under parallel execution.
- **WebDriver lifecycle** — each thread needs its own driver instance (e.g., via `ThreadLocal<WebDriver>`).
- **Test independence** — tests must not depend on execution order.
- **Shared-state risks** — two parallel tests modifying the same fictional instrument record can produce flaky results; use distinct test data per test.

**Optional challenge:** execute tests across multiple browsers in parallel and confirm no cross-test interference.

---

## Phase 9 — CI/CD *(Optional Challenge)*

Design (do not necessarily implement, unless time allows):

```
Build
 ↓
Start Application
 ↓
Smoke Tests
 ↓
Regression Tests
 ↓
Test Report
 ↓
Artifacts
```

Participants should later implement this as a GitHub Actions workflow (`.github/workflows/qa-ci.yml`) that starts the Spring Boot app (and Angular UI, or a built artifact of it), runs a small smoke suite first, then the full regression suite, and publishes the TestNG/Surefire report as a build artifact.

---

## Phase 10 — Optional Advanced Challenges

### Cloud Browsers
Design (do not necessarily implement) BrowserStack/Sauce Labs-style execution using environment variables for credentials and capabilities (never hardcoded), so the same test suite can run locally or against a cloud grid by changing configuration only.

### BDD
Convert 2-3 of your test cases from Phase 3 into Cucumber/Gherkin `Given/When/Then` scenarios, and discuss how this changes (or doesn't change) the underlying Page Object automation.

These are not required for completing the core lab.

---

## 🧪 QA Timeline (90–120 minutes)

```
10 min — Understand application/framework (Phase 1)
15 min — Test plan (Phase 2)
15 min — Test cases + test data (Phase 3-4)
25 min — Automation (Phase 5)
15 min — Regression debugging (Phase 6)
10 min — Cross-browser (Phase 7, optional)
10 min — Parallel execution (Phase 8, optional)
10 min — CI/CD (Phase 9, optional)
```

If you fall behind, skip Phases 7–10 first (Optional Challenges). Phases 1–6 are the Core Lab.

---

## 🏆 Expert Challenges

1. Complete Phase 5 (automation) without using the provided prompts — write your own from scratch.
2. Create your own prompt file for the phase you found most repetitive.
3. Design and create an agent (`.github/agents/test-automation-agent.agent.md`) that performs Phase 5 end-to-end from an approved test case.
4. Create a reusable skill (`.github/skills/regression-analysis/SKILL.md`) capturing your team's failure-triage conventions.
5. Compare: **manual prompting vs. a prompt file vs. an agent** for the same task (Phase 6, regression debugging) — which produced a faster, more accurate root cause?
6. Ask Copilot to review its own generated test code from Phase 5 and identify potential problems (flakiness risk, weak assertions) it didn't mention the first time.
7. Reduce unnecessary context in one of your prompts and compare the result quality to the original.

---

## ✅ Completion Checklist

```
- [ ] Understood the application and its automation framework
- [ ] Generated a risk-based test plan
- [ ] Generated structured test cases (and removed redundant ones)
- [ ] Prepared fictional test data (including negative/boundary values)
- [ ] Automated the highest-priority test cases (Page Object Model, explicit waits)
- [ ] Executed the regression suite
- [ ] Debugged at least one planned failure using root-cause analysis before fixing
- [ ] Ran cross-browser tests (optional)
- [ ] Executed tests in parallel (optional)
- [ ] Validated the intended CI pipeline design (optional)
- [ ] Used at least one Copilot customization (instructions, prompt file, agent, or skill)
```
