# QA-02: API Test Suite

**Role:** QA Engineer / Software Tester  

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **QA-02** in your repository README and in your reply email.

## Context
APIs are the core of our platform. We want to see how you design, automate and organise API tests.

Use the public practice API **Restful Booker**: <https://restful-booker.herokuapp.com/apidoc/index.html>.
Treat each booking like a payment order: creating, updating and cancelling must all behave correctly.

## Deliverables

1. **Postman collection** (exported JSON) **or** an automated suite with **Playwright API testing**, **REST Assured**, or **Supertest**, covering:
   - authentication (token creation, invalid credentials, missing token on protected endpoints)
   - create, read, update, partial update and delete of a booking
   - negative tests: invalid data types, missing fields, wrong IDs, very large values
   - assertions on status code, response body, JSON schema, headers and response time
   - test data chaining (the ID from "create" used in later requests)
   - environment variables, no hard-coded tokens
2. A short **test report**: what passed, what failed, and any bugs or odd behaviour you found (with reproduction steps).
3. A `README` explaining how to run the suite from the command line (for example, Newman or `npx playwright test`).

## Bonus
- Run the suite in **GitHub Actions** on every push and publish an HTML report.
- A small **k6 or JMeter** load test (for example, 20 virtual users for 1 minute) on the read endpoint, with p95 latency and error rate in your report.
