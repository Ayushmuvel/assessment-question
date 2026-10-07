# QA-02: API Test Suite

**Role:** QA Engineer / Software Tester · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **QA-02** in your repository README and in your reply email.

## Scenario

APIs are the core of our platform. Use the public practice API **Restful Booker** (<https://restful-booker.herokuapp.com/apidoc/index.html>) and treat each booking like a payment order: creating, updating and cancelling must all behave correctly.

## Must have

- A **Postman collection** (exported JSON) **or** an automated suite (Playwright API testing, REST Assured or Supertest) covering:
  - auth: create a token, invalid credentials, and a protected call with no token
  - create → read → update → delete of a booking, with the ID passed from "create" to later requests
  - at least 5 negative tests (missing fields, wrong data types, unknown ID)
  - assertions on status code, response body fields and response time
  - environment variables, no hard-coded tokens
- A short **test report**: what passed, what failed, and any bugs or odd behaviour you found, with steps to reproduce.
- How to run the suite from the command line (for example, Newman or `npx playwright test`).

## Bonus (optional)

- JSON schema validation.
- A GitHub Actions workflow that runs the suite on every push.
- A small k6 or JMeter load test on the read endpoint, reporting p95 latency and error rate.

## Answer in your README (a few lines each)

1. Besides the status code, what do you check in an API response, and why?
2. If this were a real payments API, which extra tests would you add?
