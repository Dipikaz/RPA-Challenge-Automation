# RPA-Challenge-Automation
**Project Link:** [https://rpachallenge.com/](https://rpachallenge.com/)  
**Automation Tool:** Playwright (Python)  

## 1. Project Objective

The goal of this project is to build a **robust, maintainable, and resilient automation framework** for the RPA Challenge.  
The application features **dynamic forms** where input fields change position on each page load, creating challenges for traditional test automation.  


This project demonstrates **senior-level SDET thinking** by handling:

- Dynamic UI and Shadow DOM elements  
- Negative and positive scenarios  
- Chaos/resilience testing (simulating unexpected user behavior)  
- Flaky test prevention and parallel execution  
- CI/CD readiness with automated reporting  


## 2. Why This Project Matters

Modern web applications (like Salesforce, React-based dashboards, and low-code platforms) often have:

- Dynamic rendering of UI components  
- Frequent DOM updates  
- Unstable network responses  

Most junior testers write brittle tests that fail under these conditions.  
This project shows how to **design stable, maintainable, and realistic tests** in a modern automation framework.



## 3. Key Challenges & My Approach

| Challenge | Why It Matters | How I Handled It |
|-----------|---------------|----------------|
| Dynamic fields move randomly | Index-based locators break tests | Used label/role-based locators (`get_by_label`, `get_by_role`) |
| Empty field submission | Users must see validation | Negative tests asserting visible error messages |
| Page refresh mid-form | Could lose progress | Chaos tests validating safe reset and correct UI behavior |
| Flaky timing due to UI rendering | Random failures | Playwright auto-wait + `expect()` assertions; no hard sleeps |
| Rapid or duplicate submissions | System instability | Assertions ensure only one submission succeeds |
| Parallel execution | Session leakage between tests | Isolated BrowserContext per session |


## 4. Test Highlights

**1️⃣ Successful Dynamic Form Submission**  
- Filled all fields correctly → Form submitted successfully, next iteration loaded  
- **Skills demonstrated:** Dynamic locators, auto-waits, functional validation  

**2️⃣ Empty Form Submission**  
- Clicked Submit without filling → Validation errors displayed  
- **Skills demonstrated:** Negative testing, resilient locators, observable UI assertion  

**3️⃣ Page Refresh Mid-Form**  
- User refreshed halfway → Form reset safely, progress handled  
- **Skills demonstrated:** Chaos testing, stability validation  

**4️⃣ Rapid Double Submission**  
- Double-clicked Submit → Only one submission accepted, no duplicate or crash  
- **Skills demonstrated:** Chaos testing, test determinism, UI assertion  

**5️⃣ Parallel Execution**  
- Ran multiple browser sessions concurrently → No session interference  
- **Skills demonstrated:** Parallel execution, BrowserContext isolation, scalability


## 5. Automation Approach

- **Locator Strategy:** Accessibility-first (`get_by_label()`, `get_by_role()`)  
- **Framework Design:** Page Object Model (POM), reusable utilities, configuration-driven  
- **Assertions:** Validate **behavior** instead of DOM structure  
- **Flaky Prevention:** Auto-waiting, fresh locators, no static sleeps  
- **Chaos Handling:** Page refresh, rapid submission, partial submission scenarios  
- **Parallel Execution:** Isolated BrowserContext per test  
- **Observability:** Screenshot and trace capture on failure, console logs stored  
- **CI/CD Ready:** Headless execution, HTML reports, artifact upload


## 6. Sample Automation Snippet

```python
from playwright.sync_api import Page, expect

def test_empty_form_submission(page: Page):
    page.goto("https://rpachallenge.com/")
    page.get_by_role("button", name="Start Challenge").click()
    
    # Wait for dynamic form
    expect(page.get_by_role("form")).to_be_visible()
    
    # Click submit without filling fields
    page.get_by_role("button", name="Submit").click()
    
    # Validate error message
    expect(page.get_by_text("Error", exact=True)).to_be_visible()


