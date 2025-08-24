# Sauce Demo – Cypress QA Automation (Capstone)

A compact, production‑style Cypress test suite that validates critical user flows on **[https://www.saucedemo.com](https://www.saucedemo.com)** as part of a QA capstone project. Includes: login, cart, checkout, logout, and basic accessibility checks, with artifacts (videos/screenshots) generated on headless runs.

---

## Tech Stack

* **Cypress** v15 (E2E)
* **Node.js** (LTS recommended)
* **cypress-axe** + **axe-core** (accessibility)

---

## Prerequisites

1. Node.js installed (run `node -v` and `npm -v`).
2. A clean project folder with this structure (created below via steps):

```
capstone-qa-project/
  cypress/
    e2e/
      sauce-demo.cy.js
      cart.cy.js
      checkout.cy.js
      logout.cy.js
      accessibility.cy.js
    fixtures/
    support/
      e2e.js
  cypress.config.cjs
  package.json
```

---

## Setup & Install

```bash
# 1) Install project dependencies
npm install

# 2) Install Cypress (if not already in package.json)
npm install --save-dev cypress

# 3) Install a11y helpers
npm install --save-dev cypress-axe axe-core
```

### Cypress Config (`cypress.config.cjs`)

```javascript
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    specPattern: 'cypress/e2e/*.cy.js',
    supportFile: 'cypress/support/e2e.js',
    baseUrl: 'https://www.saucedemo.com',
    video: true,
    screenshotOnRunFailure: true,
  },
});
```

### Support File (`cypress/support/e2e.js`)

```javascript
import 'cypress-axe';
// prevent app JS errors from failing tests in this demo environment
Cypress.on('uncaught:exception', () => false);
```

---

## NPM Scripts (add to `package.json`)

```json
{
  "scripts": {
    "cy:open": "cypress open",
    "cy:run": "cypress run",
    "test": "cypress run"
  }
}
```

---

## How to Run

### Interactive (GUI)

```bash
npm run cy:open
```

Select a browser and click any spec (test file) to run.

### Headless (CI‑style with artifacts)

```bash
npm run cy:run
```

* **Videos** saved to `cypress/videos/`
* **Screenshots of failures** saved to `cypress/screenshots/`

---

## Test Scripts (Specs)

> Paste these into the files shown in the tree above under `cypress/e2e/`.

### 1) Login – `sauce-demo.cy.js`

```javascript
describe('Sauce Demo Login', () => {
  beforeEach(() => {
    cy.visit('/');
  });

  it('logs in with valid credentials', () => {
    cy.get('[data-test="username"]').type('standard_user');
    cy.get('[data-test="password"]').type('secret_sauce');
    cy.get('[data-test="login-button"]').click();
    cy.url().should('include', '/inventory.html');
  });

  it('shows error on invalid credentials', () => {
    cy.get('[data-test="username"]').type('wrong_user');
    cy.get('[data-test="password"]').type('wrong_pass');
    cy.get('[data-test="login-button"]').click();
    cy.get('[data-test="error"]').should('be.visible');
  });
});
```

### 2) Cart – `cart.cy.js`

```javascript
describe('Cart Flow', () => {
  beforeEach(() => {
    cy.visit('/');
    cy.get('[data-test="username"]').type('standard_user');
    cy.get('[data-test="password"]').type('secret_sauce');
    cy.get('[data-test="login-button"]').click();
    cy.url().should('include', '/inventory.html');
  });

  it('adds and removes items', () => {
    cy.contains('Add to cart').first().click();
    cy.get('.shopping_cart_badge').should('contain', '1');
    cy.get('.shopping_cart_link').click();
    cy.url().should('include', '/cart.html');
    cy.contains('Remove').click();
    cy.get('.cart_item').should('not.exist');
  });
});
```

### 3) Checkout – `checkout.cy.js`

```javascript
describe('Checkout Flow', () => {
  beforeEach(() => {
    cy.visit('/');
    cy.get('[data-test="username"]').type('standard_user');
    cy.get('[data-test="password"]').type('secret_sauce');
    cy.get('[data-test="login-button"]').click();
    cy.url().should('include', '/inventory.html');
  });

  it('completes checkout', () => {
    cy.contains('Add to cart').first().click();
    cy.get('.shopping_cart_link').click();
    cy.contains('Checkout').click();
    cy.get('[data-test="firstName"]').type('Marlon');
    cy.get('[data-test="lastName"]').type('Logan');
    cy.get('[data-test="postalCode"]').type('32827');
    cy.get('[data-test="continue"]').click();
    cy.url().should('include', '/checkout-step-two.html');
    cy.contains('Finish').click();
    cy.contains('Thank you for your order!').should('be.visible');
  });
});
```

### 4) Logout – `logout.cy.js`

```javascript
describe('Logout', () => {
  it('logs out from menu', () => {
    cy.visit('/');
    cy.get('[data-test="username"]').type('standard_user');
    cy.get('[data-test="password"]').type('secret_sauce');
    cy.get('[data-test="login-button"]').click();
    cy.get('#react-burger-menu-btn').click();
    cy.contains('Logout').click();
    cy.url().should('eq', `${Cypress.config().baseUrl}/`);
  });
});
```

### 5) Accessibility – `accessibility.cy.js`

```javascript
describe('A11y checks - key pages', () => {
  const pages = ['/', '/inventory.html'];
  pages.forEach((path) => {
    it(`has no critical a11y violations on ${path}`, () => {
      cy.visit(path);
      cy.injectAxe();
      cy.checkA11y(null, { includedImpacts: ['critical'] });
    });
  });
});
```

---

## Expected Results (Acceptance)

* Login with `standard_user/secret_sauce` reaches `/inventory.html`.
* Invalid login shows visible error banner.
* Item can be added/removed; cart count updates.
* Checkout completes and shows **“Thank you for your order!”**.
* Axe scan reports **no critical** violations on selected pages.

---

## Artifacts & Deliverables

* Videos: `cypress/videos/`
* Failure screenshots: `cypress/screenshots/`
* Example run command used for submission: `npm run cy:run`

Include these folders and this README in your GitHub repository. Optionally add a short demo screen recording of a GUI run from `npm run cy:open`.

---

## Troubleshooting

* **No specs found** → Ensure specs live in `cypress/e2e/` and match `*.cy.js`.
* **Cypress binary missing** → `npx cypress cache clear && npx cypress install`.
* **Config error** → Keep only `cypress.config.cjs` in the project root; confirm `supportFile` path.

---

## Credits

Created by Marlon Logan for the QA Capstone project. Tests target the public demo at saucedemo.com for educational purposes only.
