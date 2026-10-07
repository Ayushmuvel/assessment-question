# QA-03: UI Automation for a Checkout Flow

**Role:** QA Engineer / Software Tester  

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **QA-03** in your repository README and in your reply email.

## Context
Checkout is the most important journey in any payment product. Regressions here cost real money.

Use the public demo store **Sauce Demo**: <https://www.saucedemo.com/> (test users are listed on the login page).

## Deliverables

1. Automated UI tests with **Playwright**, **Cypress**, or **Selenium WebDriver**, using the **Page Object Model**:
   - login with valid, locked-out and invalid users
   - add items to the cart and remove them
   - complete a checkout and verify the order total (item total + tax = total)
   - checkout form validation (missing first name, last name, postal code)
   - sorting products by price and name
   - the `problem_user` and `performance_glitch_user` accounts: describe and assert what goes wrong
2. Tests must run in **at least two browsers** (for example, Chromium and Firefox).
3. Screenshots or video on failure.
4. A **bug report list** for the defects you found with `problem_user`.
5. A `README` with how to run the tests and how the framework is organised.

## Bonus
- GitHub Actions workflow that runs the tests and uploads the report.
- Data-driven tests (users and products from a JSON file).
- One mobile viewport test.
