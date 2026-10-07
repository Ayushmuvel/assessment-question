# QA-03: UI Automation for a Checkout Flow

**Role:** QA Engineer / Software Tester · **Time:** about 3 hours (bonus is optional)

Read the [role overview](README.md) and the [submission rules](../README.md) before you start.
Mention the assessment ID **QA-03** in your repository README and in your reply email.

## Scenario

Checkout is the most important journey in any payment product. Use the public demo store **Sauce Demo** (<https://www.saucedemo.com/>). Test users are listed on its login page.

## Must have

- Automated UI tests with **Playwright**, **Cypress** or **Selenium WebDriver**, using the **Page Object Model**:
  - login: valid user, locked-out user, wrong password
  - add two items to the cart and complete checkout
  - verify the order total (item total + tax = total)
  - checkout form validation (missing first name, last name, postal code)
- A screenshot on failure.
- A list of the bugs you find when logged in as `problem_user`, with steps to reproduce.
- How to run the tests from the command line.

## Bonus (optional)

- Run in two browsers (for example, Chromium and Firefox).
- A GitHub Actions workflow with an HTML report.
- Data-driven tests (users from a JSON file).

## Answer in your README (a few lines each)

1. How do you keep UI tests from being flaky?
2. Which tests here would you run on every commit, and which only before a release?
