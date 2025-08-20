# Capstone QA Project – Part 1: Planning and Foundation

## 📌 Project Overview
This repository contains the foundation work for my Capstone QA project.  
The focus of Part 1 is to establish a strong QA strategy, perform manual and automated testing, and document results for a selected e-commerce application.

---

## 🛠️ Application Under Test
- **Chosen App:** Saleor Demo Storefront (https://demo.saleor.io)  
- **API:** Saleor GraphQL API (https://demo.saleor.io/graphql/)  
- **Environments:**  
  - Web: Desktop + Mobile viewport testing  
  - Mobile: Responsive layout checks via Cypress  
  - API: Backend verification with GraphQL queries

capstone-qa-project/
├─ test-plan/
│ └─ TestPlan.md # Full QA strategy and test plan
├─ manual-testing/
│ ├─ TestCases_Web.md # Manual test cases for web
│ ├─ TestResults.md # Results of manual testing
│ └─ TestCases_Mobile.md # Mobile viewport test cases
├─ cypress/
│ ├─ e2e/
│ │ ├─ web_flow.cy.js # Cypress E2E tests
│ │ ├─ checkout.cy.js # Checkout test flow
│ │ └─ api_graphql.cy.js # API tests (GraphQL)
│ └─ support/
│ └─ e2e.js # Cypress support + accessibility setup
├─ reports/
│ ├─ accessibility/ # Accessibility test results
│ └─ performance/ # Lighthouse performance reports
├─ jira/
│ └─ defects.md # Documented defects with Jira links
└─ README.md # This file


---

## 🧪 Test Plan Deliverables
- **Functional Testing**  
  - User login, product search, add-to-cart, checkout flow  
- **API Testing**  
  - GraphQL queries for products and cart validation  
- **Performance Testing**  
  - Lighthouse performance scores using Cypress Audit  
- **Accessibility Testing**  
  - Axe accessibility checks for core pages  

---

## 📝 Manual Testing
- Core features tested:  
  - User Registration  
  - Product Search  
  - Add to Cart  
  - Remove from Cart  
  - Checkout Flow  
- Documented results are in `/manual-testing/TestResults.md`.  
- Defects logged in Jira and linked in `/jira/defects.md`.

---

## 🤖 Automated Testing (Cypress)
- **E2E Web Flows:** login, search, checkout  
- **Cross-browser:** Chrome + Firefox  
- **Mobile:** Responsive viewports (iPhone, Pixel)  
- **Accessibility:** Axe plugin  
- **Performance:** Lighthouse plugin  

Run tests with:
```bash
npx cypress open
