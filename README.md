# 🚀 Java Selenium Cucumber BDD Automation Framework

This repository contains a **Behavior Driven Development (BDD)** based test automation framework built with **Java, Selenium WebDriver, Maven, and Cucumber**.  
It is designed for scalable, maintainable, and readable UI test automation.  

---

## 📂 Project Structure

automation-framework/
│── src
│ └── test
│ ├── java
│ │ ├── pages/ # Page Object classes (locators + actions)
│ │ ├── runners/ # Cucumber TestRunner classes
│ │ ├── step_definitions/ # Glue code for Gherkin steps
│ │ └── utilities/ # Helpers, config readers, and drivers
│ └── resources
│ ├── features/ # Gherkin feature files
│ └── filesToUpload/ # Test data files (uploads etc.)
│
│── configuration.properties # Configurable browser & credentials
│── pom.xml # Maven build & dependencies
│── README.md # Documentation

---

## 🟢 BDD Behaviour Driven Development
**BDD** allows collaboration between business, QA, and developers by writing test cases in plain English (**Gherkin**).  
It improves communication and ensures tests represent user behavior.  

---

## 🎯 Benefit of BDD
- Easy collaboration with non-technical stakeholders  
- Living documentation for the system  
- Faster feedback loop during development  
- Test scenarios double as acceptance criteria  

---

## ✍️ Gherkin
**Gherkin syntax** is used to write test scenarios in `.feature` files.  
Example:  

Feature: As a user, I should be able to log out.

  Scenario: Validate user can logout
    Given user is logged in
    When user clicks on logout button
    Then validate user is on the login page

---

⚙️ Step Definitions
Step Definitions link Gherkin steps to Java methods (inside step_definitions/).

Example:
@Given("user is logged in")
    public void user_is_logged_in() {
        Driver.getDriver().get(ConfigurationReader.getProperty("url"));
        loginPage.login("posmanager75@info.com","posmanager");

    }

    @When("user clicks on logout button")
    public void user_clicks_on_logout_button() {
        inBoxPage.userMenu.click();
        inBoxPage.logoutBtn.click();
    }

    @Then("validate user is on the login page")
    public void validate_user_is_on_the_login_page() {
        String actualUrl = Driver.getDriver().getCurrentUrl();
        String expectedUrl = ConfigurationReader.getProperty("url");
        Assert.assertEquals(expectedUrl,actualUrl);
    }

---

📦 Cucumber Maven Project
Uses Maven for dependencies and build.

Cucumber integrates with JUnit for execution.

---

🔧 Setting up Project
Clone repo:

git clone <repo-url>
Import into IntelliJ IDEA as a Maven project.

Run tests with:

mvn clean test

---

🧩 Adding Selenium Dependencies and Classes
pom.xml includes required dependencies:

xml
<dependency>
  <groupId>org.seleniumhq.selenium</groupId>
  <artifactId>selenium-java</artifactId>
  <version>4.x.x</version>
</dependency>
<dependency>
  <groupId>io.cucumber</groupId>
  <artifactId>cucumber-java</artifactId>
  <version>7.x.x</version>
</dependency>
<dependency>
  <groupId>io.cucumber</groupId>
  <artifactId>cucumber-junit</artifactId>
  <version>7.x.x</version>
</dependency>

NOTE: The versions of dependencies may vary depending when you will be cloning this project
---

🥒 Cucumber Java with Selenium
This framework glues Cucumber (feature files) with Selenium (UI automation) through Step Definitions.

---

🌐 First Selenium Scenario
Located inside features/.

gherkin
Scenario Outline: Verify login with different user types
    When user logs in as "<userType>"
    Then user should see "Odoo" in page title
    Examples:
      | userType          |
      | pos_manager       |
      | sales_manager     |
      | crm_manager       |
      | inventory_manager |
      | expenses_manager  |

---

🔄 Hooks
In utilities/ → Hooks.java:

@Before → Setup browser

@After → Take screenshot & close browser

---

📝 More Scenarios and IntelliJ Step Definition Generation
Right-click missing steps → Generate Step Definitions in IntelliJ.

🎛 Using Parameters with Cucumber Expression
gherkin
When User searches for {string}

---

🔄 Using Background to Reuse Given Condition
gherkin
Background:
  Given User is logged in
🏷 Cucumber Tags
Tag scenarios with @Smoke, @Regression for selective runs.

---

🧮 Tag Expressions
Run tagged scenarios:

mvn test -Dcucumber.filter.tags="@Smoke and not @WIP"

---

▶️ Creating TestRunner
Inside runners/ directory:

@RunWith(Cucumber.class)
@CucumberOptions(
        plugin = {
                "pretty",
                "html:target/cucumber-report.html",
                "rerun:target/rerun.txt",
                "me.jvt.cucumber.report.PrettyReports:target/cucumber",
                "json:target/cucumber.json",
        },

        features = "src/test/resources/feature",
        glue = "com/brite/step_definition",
        dryRun = false,
        tags = "@Login",
        publish = true
)
public class CukesRunner { }

---

📊 Data Tables
Use structured data directly in steps:

 Scenario Outline:Verify users see the calendar as a daily
    When user logs in as "<userType>"
    Then User should see "<account_Name>" on the page
    When User clicks on calendar module
    Then User should be able to see Day module
    When User clicks on Day module
    Then User should be able to see Day header
    And User should be able to see time column

    Examples:
      | userType          | account_Name       |
      | pos_manager       | POSManager50       |
      | sales_manager     | SalesManager6      |
      | crm_manager       | EventsCRMManager10 |
      | inventory_manager | InventoryManager11 |
      | expenses_manager  | ExpensesManager10  |

---

🧩 Scenario Outline with Example

Scenario Outline: Verify login with invalid credentials
    When user logs in with invalid "<username>","<password>"
    Then user should see error message "Wrong login/password"
    Examples:
      | username             | password      |
      | posmanager5@info.com | wrongPassword |
      | wrong@UserName.com   | posmanager

---

📸 Adding Screenshot to Failed Scenarios
Implemented in Hooks.java using Selenium TakesScreenshot.

---

🔁 Rerunning Failed Scenario
Enable rerun plugin in CucumberOptions.

---

📑 Using Third-Party HTML Reporter
Supports ExtentReports and Cucumber HTML Reports for reporting.

⚡ Simple Maven Commands
Run all tests:

mvn clean test
Run with specific tag:

mvn test -Dcucumber.filter.tags="@Smoke"

---

⚙️ configuration.properties
The configuration.properties file is used to store environment-specific settings
This ensures flexibility, reusability, and cleaner test scripts.

Example keys:

properties
browser=chrome
baseUrl=https://example.com
username=testuser
password=securePassword123
timeout=30

---

🌟 Why BDD 

Business Need ➝ Feature File (Gherkin) ➝ Step Definitions ➝ Automated Test
📘 Business defines acceptance criteria in plain English

💻 Developers & QA implement step definitions with Selenium

✅ Automation validates functionality & produces reports

---

✅ Summary
This framework provides a scalable and collaborative approach to UI automation using Java + Selenium + Cucumber BDD.
It ensures readability, maintainability, and reusability of test cases.

---
