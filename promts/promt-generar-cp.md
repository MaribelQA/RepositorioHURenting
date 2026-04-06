# Test Case Template

## 📌 Instructions for AI

* Replace all placeholders written between [brackets]
* Follow the structure strictly
* Do not omit any section unless explicitly marked as optional
* Each test case must validate only ONE behavior
* Use clear, concise, and test-focused language
* Generate multiple test cases when applicable
* Include positive, negative, and edge cases
* The "Test Steps" section MUST be formatted as a table with columns:
  Step | Action | Expected Result
* Each step must include its corresponding expected result
* Do NOT use bullet points or numbered lists for test steps
* Output MUST be generated in Spanish
 
---

## 🆔 Test Case Identification

**Test Case ID**: TC_[MODULE]*[FEATURE]*[###]
**Title**: [Action] should [expected outcome] when [condition]

---

## 📝 Description

[Brief explanation of what is being validated and why]

---

## ⚙️ Preconditions

* [System state required before execution]
* [User must be logged in / data exists / etc.]

## 🔢 Test Steps

| Step | Action                                      | Expected Result                            |
| ---- | ------------------------------------------- | ------------------------------------------ |
| 1    | [Describe the action performed by the user] | [Expected system behavior after this step] |
| 2    | [Next action]                               | [Expected result]                          |
| N    | [...]                                       | [...]                                      |

---

## 📊 Test Data

* Input: [Data used in test]
* Output: [Expected output]

---

## 🔀 Test Type

* [Functional | Negative | Edge | Regression | Smoke]

---

## 🔥 Priority

* [High | Medium | Low]

---

## 🌍 Environment

* [QA | Staging | Production | Local]

---

## 📎 Notes (Optional)

* [Any additional context or assumptions]

---

## 🧩 Example (Reference Only - Do NOT reuse)

**Test Case ID**: TC_AUTH_LOGIN_001
**Title**: User should log in successfully when valid credentials are provided

**Description**: Validate that a registered user can log in with valid credentials

**Preconditions**:

* User account exists

**Test Steps**:

| Step | Action                         | Expected Result                 |
| ---- | ------------------------------ | ------------------------------- |
| 1    | Navigate to login page         | Login page is displayed         |
| 2    | Enter valid email and password | Credentials are accepted        |
| 3    | Click login button             | User is redirected to dashboard |

**Test Data**:

* Input: [valid_user@test.com](mailto:valid_user@test.com) / Password123
* Output: login exitoso

**Test Type**:

* Functional

**Priority**:

* High

**Environment**:

* QA
