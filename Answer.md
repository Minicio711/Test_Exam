# Senior Test Engineer (Retail Automate)

---

## Answer No.1 — Find Duplicate Numbers

```python
list_a = [1, 2, 3, 5, 6, 8, 9]
list_b = [3, 2, 1, 5, 6, 0]

duplicate_numbers = set(list_a) & set(list_b)
print(f"number duplicate is : {sorted(list(duplicate_numbers))}")
```

**Expected Output:**
```
number duplicate is : [1, 2, 3, 5, 6]
```

---

## Answer No.2 — Robot Framework + Selenium Login Test

```robot
*** Settings ***
Library    SeleniumLibrary

*** Variables ***
${URL}               https://the-internet.herokuapp.com/login
${BROWSER}           edge
${USERNAME}          tomsmith
${PASSWORD}          SuperSecretPassword!

# Locators
${Field_Username}    xpath=//*[@id="username"]
${Field_Password}    xpath=//*[@id="password"]
${Login_button}      xpath=//*[@id="login"]/button
${Logout_button}     xpath=//a[contains(@href, 'logout')]

*** Keywords ***
Open Browser To Login Page
    Open Browser    ${URL}    ${BROWSER}
    Maximize Browser Window
    Wait Until Page Contains    Login Page    timeout=10s

Take Screenshot
    [Arguments]    ${filename}
    Capture Page Screenshot    ${TEST_NAME}/${filename}.png

*** Test Cases ***
Login success
    [Documentation]    To verify that users can login successfully when input a correct username and password.
    Open Browser To Login Page
    Take Screenshot    login_page.png
    Input Text        ${Field_Username}    ${USERNAME}
    Input Password    ${Field_Password}    ${PASSWORD}
    Click Button      ${Login_button}
    Wait Until Page Contains    You logged into a secure area!    timeout=10s
    Take Screenshot    login_success.png
    Click Element     ${Logout_button}
    Wait Until Page Contains    You logged out of the secure area!    timeout=10s
    Take Screenshot    logout_success.png
    [Teardown]    Close Browser

Login failed - Password incorrect
    [Documentation]    To verify that users can login unsuccessfully when they input a correct username but wrong password.
    Open Browser To Login Page
    Take Screenshot    login_page.png
    Input Text        ${Field_Username}    ${USERNAME}
    Input Password    ${Field_Password}    Password!
    Click Button      ${Login_button}
    Wait Until Page Contains    Your password is invalid!    timeout=10s
    Take Screenshot    login_fail.png
    [Teardown]    Close Browser

Login failed - Username not found
    [Documentation]    To verify that users can login unsuccessfully when they input a username that did not exist.
    Open Browser To Login Page
    Take Screenshot    login_page.png
    Input Text        ${Field_Username}    tomholland
    Input Password    ${Field_Password}    Password!
    Click Button      ${Login_button}
    Wait Until Page Contains    Your username is invalid!    timeout=10s
    Take Screenshot    login_fail.png
    [Teardown]    Close Browser
```

---

## Answer No.3.1 — Postman Test: Verify User Profile (GET)

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Verify user profile data", function () {
    var jsonData = pm.response.json().data;
    pm.expect(jsonData.id).to.eql(12);
    pm.expect(jsonData.email).to.eql("rachel.howell@reqres.in");
    pm.expect(jsonData.first_name).to.eql("Rachel");
    pm.expect(jsonData.last_name).to.eql("Howell");
    pm.expect(jsonData.avatar).to.eql("https://reqres.in/img/faces/12-image.jpg");
});
```

---

## Answer No.3.2 — Postman Test: Create New User (POST)

**Endpoint:** `POST https://reqres.in/api/users`

**Request Body:**
```json
{
    "name": "morpheus",
    "job": "leader"
}
```

**Test Script:**
```javascript
pm.test("Status code is 201 Created", function () {
    pm.response.to.have.status(201);
});

pm.test("Verify created user data", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.name).to.eql("morpheus");
    pm.expect(jsonData.job).to.eql("leader");
    pm.expect(jsonData.id).to.be.a("string");
    pm.expect(jsonData.createdAt).to.be.a("string");
});

pm.test("Response time is less than 3000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(3000);
});

pm.test("Content-Type is application/json", function () {
    pm.expect(pm.response.headers.get("Content-Type")).to.include("application/json");
});
```

---

## Answer No.4 — Robot Framework + Appium Mobile Test

```robot
*** Settings ***
Documentation    Test Automation Assignment - Question 4: Mobile Application Testing
...
...    Design test coverage and create automated test scripts for mobile application.
...    Download source code from: https://github.com/avjinder/Minimal-Todo
...
...    **Framework:** Robot Framework + Appium
...
...    **Test Coverage:**
...    - App launch verification
...    - CRUD operations (Create, Read, Update, Delete)
...    - Input validation (special characters, Unicode, empty input)
...    - Business logic (duplicate items)
...    - Edge cases and negative testing
...
...    **Total Test Cases:** 8 test cases covering main features

Resource          ../../init.resource
Suite Setup       mobile.keywords.Open Mobile Application
Suite Teardown    AppiumLibrary.Close All Applications
Test Setup        mobile.keywords.Prepare Clean State
Test Teardown     mobile.keywords.Capture Screenshot On Failure

*** Test Cases ***
TC-001 Verify App Launch Successfully
    [Documentation]    ทดสอบการเปิดแอปพลิเคชันสำเร็จ
    ...
    ...    **Preconditions:**
    ...    - Appium Server กำลังรัน
    ...    - Emulator/Device เชื่อมต่อแล้ว
    ...
    ...    **Test Steps:**
    ...    1. เปิดแอป Minimal Todo
    ...    2. ตรวจสอบว่าแอปเปิดสำเร็จ
    ...
    ...    **Expected Results:**
    ...    - แสดง App Title "Minimal"
    ...    - แสดงข้อความ "You don't have any todos"
    [Tags]    smoke    p0    sanity
    mobile.keywords.Verify App Title Displayed
    mobile.keywords.Verify Empty State Message

TC-002 Add Single Todo Item
    [Documentation]    ทดสอบการเพิ่มรายการ Todo ครั้งละ 1 รายการ
    ...
    ...    **Test Steps:**
    ...    1. คลิกปุ่ม Add Todo
    ...    2. กรอกข้อความ "Buy Groceries"
    ...    3. บันทึกรายการ
    ...
    ...    **Expected Results:**
    ...    - รายการ "Buy Groceries" ถูกเพิ่มและแสดงในรายการ
    [Tags]    functional    p0    crud
    mobile.keywords.Add Todo Item       Buy Groceries
    mobile.keywords.Verify Todo Item Exists    Buy Groceries

TC-003 Add Multiple Todo Items
    [Documentation]    ทดสอบการเพิ่มรายการ Todo หลายรายการ
    ...
    ...    **Test Steps:**
    ...    1. เพิ่มรายการที่ 1: "Buy groceries"
    ...    2. เพิ่มรายการที่ 2: "Do laundry"
    ...    3. เพิ่มรายการที่ 3: "Pay bills"
    ...
    ...    **Expected Results:**
    ...    - ทั้ง 3 รายการแสดงในรายการ
    [Tags]    functional    p1    crud
    VAR    @{todo_items}=
    ...    Buy groceries
    ...    Do laundry
    ...    Pay bills
    FOR    ${item}    IN    @{todo_items}
        mobile.keywords.Add Todo Item    ${item}
    END
    FOR    ${item}    IN    @{todo_items}
        mobile.keywords.Verify Todo Item Exists    ${item}
    END

TC-004 Delete Single Todo Item
    [Documentation]    ทดสอบการลบรายการ Todo ครั้งละ 1 รายการ
    ...
    ...    **Test Steps:**
    ...    1. เพิ่มรายการ "Task to delete"
    ...    2. ลบรายการที่เพิ่ม
    ...
    ...    **Expected Results:**
    ...    - รายการถูกลบและไม่แสดงในรายการ
    ...    - แสดงข้อความ empty state
    [Tags]    functional    p0    crud
    mobile.keywords.Add Todo Item              Task to delete
    mobile.keywords.Verify Todo Item Exists    Task to delete
    mobile.keywords.Delete Todo Item           Task to delete
    mobile.keywords.Verify Todo Item Deleted   Task to delete
    mobile.keywords.Verify Empty State Message

TC-005 Add Todo With Special Characters
    [Documentation]    ทดสอบการเพิ่มรายการที่มีอักขระพิเศษ
    ...
    ...    **Test Data:**
    ...    - อักขระพิเศษ: !@#$%^&*()_+-=[]{}
    ...
    ...    **Expected Results:**
    ...    - ระบบรองรับและแสดงอักขระพิเศษได้ถูกต้อง
    [Tags]    functional    p2    input_validation
    VAR    ${special_text}=    Test!@#$%^&*()_+-=
    mobile.keywords.Add Todo Item              ${special_text}
    mobile.keywords.Verify Todo Item Exists    ${special_text}

TC-006 Add Todo With Unicode Characters
    [Documentation]    ทดสอบการเพิ่มรายการที่มีอักขระ Unicode หลายภาษา
    ...
    ...    **Test Data:**
    ...    - ไทย: สวัสดี
    ...    - ญี่ปุ่น: こんにちは
    ...    - อีโมจิ: ✨ 🎉 📝
    ...
    ...    **Expected Results:**
    ...    - ระบบรองรับและแสดง Unicode ได้ถูกต้อง
    [Tags]    functional    p2    input_validation    i18n
    VAR    ${unicode_text}=    สวัสดี ✨ Hello こんにちは 🎉
    mobile.keywords.Add Todo Item              ${unicode_text}
    mobile.keywords.Verify Todo Item Exists    ${unicode_text}

TC-007 Add Todo With Empty Text
    [Documentation]    ทดสอบการเพิ่มรายการด้วยข้อความว่าง (Negative Test)
    ...
    ...    **Test Steps:**
    ...    1. คลิกปุ่ม Add Todo
    ...    2. ไม่กรอกข้อความ (ปล่อยว่าง)
    ...    3. พยายามบันทึก
    ...
    ...    **Expected Results:**
    ...    - ระบบไม่อนุญาตให้บันทึก หรือ
    ...    - แสดงข้อความแจ้งเตือน
    [Tags]    negative    p1    input_validation    boundary
    mobile.keywords.Click Add Todo Button
    mobile.keywords.Enter Todo Text    ${EMPTY}
    mobile.keywords.Click Save Button
    mobile.keywords.Verify Empty State Message

TC-008 Add Duplicate Todo Items
    [Documentation]    ทดสอบการเพิ่มรายการที่ซ้ำกัน
    ...
    ...    **Test Steps:**
    ...    1. เพิ่มรายการ "Duplicate Task"
    ...    2. เพิ่มรายการ "Duplicate Task" อีกครั้ง
    ...
    ...    **Expected Results:**
    ...    - ระบบอนุญาตให้เพิ่มรายการซ้ำได้
    [Tags]    functional    p2    business_logic
    mobile.keywords.Add Todo Item    Duplicate Task
    mobile.keywords.Add Todo Item    Duplicate Task
    ${count: int}=    mobile.keywords.Count Todo Items    Duplicate Task
    IF    $count < 2
        Fail    Expected at least 2 duplicate items, but found ${count}
    END
```

---

## Answer No.5 — Jenkins Pipeline

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout Code From Git') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-repo/your-project.git'
            }
        }

        stage('Run Test Automate') {
            steps {
                sh '''
                    pip install -r requirements.txt
                    robot --outputdir results tests/
                '''
            }
        }

        stage('Send Result To Jenkins') {
            steps {
                script {
                    robot(
                        outputPath: 'results',
                        passThreshold: 80.0,
                        unstableThreshold: 60.0
                    )
                }
            }
            post {
                always {
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'results',
                        reportFiles: 'report.html',
                        reportName: 'Robot Framework Report'
                    ])
                }
            }
        }
    }

    post {
        success {
            echo 'All tests passed successfully!'
        }
        failure {
            echo 'Some tests failed. Please check the report.'
        }
    }
}
```

---

## Answer No.6 — Simple Caesar Cipher Decoder

```python
def simpleCipher(encrypted, k):
    decrypted = ""
    for char in encrypted:
        # 1. แปลงตัวอักษรเป็นตัวเลข 0-25 (A=0, B=1, ..., Z=25)
        current_pos = ord(char) - ord('A')
        # 2. ถอยหลังไป k ตำแหน่ง โดยใช้ % 26 เพื่อให้วนลูปวงกลมได้
        new_pos = (current_pos - k) % 26
        # 3. แปลงกลับเป็นตัวอักษรภาษาอังกฤษตัวพิมพ์ใหญ่
        decrypted += chr(new_pos + ord('A'))
    return decrypted

encrypted_str = "ECV"
k_val = 2
print(simpleCipher(encrypted_str, k_val))  # Output: CAT
```

**Expected Output:**
```
CAT
```

**Explanation:** Each letter in `"ECV"` is shifted back by 2 positions:
- `E` → `C`
- `C` → `A`
- `V` → `T`


# Senior Test Engineer Assessment (A) — Answers

---

## No.1 – 3 : Basic Software Testing and Test Case Design

---

### Answer No.1 — Test Cases: Money Transfer Feature (Mobile Banking)

#### Transfer Flow Overview
> **Decision to Transfer → Select Transfer Method → Enter Details → Review & Confirm → Completed**

---

| TC ID | Title | Pre-condition | Steps | Expected Result |
|-------|-------|---------------|-------|-----------------|
| TC-001 | Transfer to Own Account – Happy Path | User is logged in; has sufficient balance | 1. Go to Transfer from Home Screen 2. Select "Transfer to Own Account" 3. Select 'To' account and 'From' account 4. Enter a valid Amount 5. Tap Next/Confirm | Transaction data is validated successfully → Review screen is displayed |
| TC-002 | Transfer to Own Account – Confirm on Review Screen | Review screen is displayed after TC-001 | 1. On Review screen, verify all transfer details 2. Tap "Confirm" | Transaction is completed; Completed screen is shown |
| TC-003 | Transfer to Own Account – Invalid Amount (zero) | User is on Enter Details screen | 1. Select 'To' and 'From' account 2. Enter Amount = 0 3. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-004 | Transfer to Own Account – Amount Exceeds Balance | User has insufficient balance | 1. Select 'To' and 'From' account 2. Enter Amount greater than available balance 3. Tap Next | Bottom sheet error message is displayed on Review/Confirm screen |
| TC-005 | Transfer to Own Account – Empty Amount Field | User is on Enter Details screen | 1. Select 'To' and 'From' account 2. Leave Amount field empty 3. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-006 | Transfer to Other Account – Happy Path | User is logged in; has sufficient balance | 1. Go to Transfer from Home Screen 2. Select "Transfer to Other Account" 3. Enter valid Citizen ID 4. Enter valid Amount 5. Tap Next | Transaction data is validated; Review screen is displayed |
| TC-007 | Transfer to Other Account – Confirm Transaction | Review screen is displayed after TC-006 | 1. On Review screen, verify all details 2. Tap "Confirm" | Transaction completed; Completed screen is shown |
| TC-008 | Transfer to Other Account – Invalid Citizen ID Format | User is on Enter Details screen | 1. Select "Transfer to Other Account" 2. Enter Citizen ID with less than 13 digits (e.g. "12345") 3. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-009 | Transfer to Other Account – Citizen ID with Special Characters | User is on Enter Details screen | 1. Enter Citizen ID containing special characters (e.g. "1234-567-890") 2. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-010 | Transfer to Other Account – Invalid Amount (negative) | User is on Enter Details screen | 1. Enter valid Citizen ID 2. Enter Amount = -100 3. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-011 | Transfer to Other Account – Validation Fails on Review | User reaches Review screen with unverifiable data | 1. On Review screen, tap Confirm | Bottom sheet error message is displayed; transaction is not completed |
| TC-012 | Transfer to PromptPay via Citizen ID – Happy Path | User is logged in; has sufficient balance | 1. Go to Transfer from Home Screen 2. Select "Transfer to PromptPay (Citizen ID / Mobile Phone)" 3. Enter valid Citizen ID (13 digits) 4. Enter valid Amount 5. Tap Next | Transaction data is validated; Review screen is displayed |
| TC-013 | Transfer to PromptPay via Mobile Phone – Happy Path | User is logged in; has sufficient balance | 1. Select "Transfer to PromptPay (Citizen ID / Mobile Phone)" 2. Enter valid Mobile Phone Number (10 digits) 3. Enter valid Amount 4. Tap Next | Transaction data is validated; Review screen is displayed |
| TC-014 | Transfer to PromptPay – Confirm Transaction | Review screen is displayed after TC-012/TC-013 | 1. Verify transfer details on Review screen 2. Tap Confirm | Transaction completed; Completed screen is shown |
| TC-015 | Transfer to PromptPay – Invalid Citizen ID | User is on Enter Details screen | 1. Enter Citizen ID with incorrect length or letters 2. Enter valid Amount 3. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-016 | Transfer to PromptPay – Invalid Mobile Phone Number | User is on Enter Details screen | 1. Enter Mobile Phone with incorrect format (e.g. 8 digits) 2. Enter valid Amount 3. Tap Next | Inline error message is displayed; user cannot proceed |
| TC-017 | Transfer to PromptPay – Validation Fails on Review Screen | User reaches Review/Confirm screen | 1. Tap Confirm on Review screen | Bottom sheet error message is displayed; transaction is not completed |
| TC-018 | Transfer to PromptPay – Empty Citizen ID/Mobile Field | User is on Enter Details screen | 1. Leave Citizen ID/Mobile field empty 2. Enter valid Amount 3. Tap Next | Inline error message is displayed; user cannot proceed |

---

### Answer No.2 — Defect Report Template

| Field | Detail |
|-------|--------|
| **Defect ID** | BUG-001 |
| **Title** | [Login] Application fails to login with correct username and password |
| **Reported By** | Napat Phongam |
| **Date** | 2026-04-23 |
| **Environment** | Mobile Banking App – iOS 17 / Android 14, App Version 1.0.0, Staging Environment |
| **Severity** | Critical |
| **Priority** | High |
| **Status** | Open |
| **Module** | Authentication / Login |

**Description:**
The application is unable to authenticate and log in the user despite entering a valid and correct username and password. The login process does not proceed and the user remains on the login screen.

**Steps to Reproduce:**
1. Open the mobile banking application.
2. Enter a valid, registered username.
3. Enter the correct password for that username.
4. Tap the "Login" button.

**Expected Result:**
The application should authenticate the user successfully and navigate to the Home Screen.

**Actual Result:**
The application cannot login to the system even though the username and password entered are correct. The user remains on the login screen and no error message indicating the cause is displayed.

**Attachments:**
- Screenshot of the login screen after failed attempt
- Log file from the session (if available)

**Additional Notes:**
- Issue is reproducible 100% of the time.
- Tested with multiple valid accounts — all fail to login.
- Password reset does not resolve the issue.

---

### Answer No.3 — Priority and Severity Analysis

**Case:** Transfer to Other Bank — PIN reported as incorrect / PIN locked after 3 attempts

| | Level | Reason |
|---|-------|--------|
| **Severity** | **Critical** | The core money transfer feature is completely non-functional. More critically, the system incorrectly locks the customer's PIN after 3 failed attempts — even though the PIN entered is correct. A locked PIN causes the customer to lose access to all PIN-protected functions, which is a permanent and damaging consequence within the session. |
| **Priority** | **High** | Money transfer is a primary feature of any banking application and directly impacts customer trust and business operations. This bug affects every customer attempting to transfer to another bank and causes irreversible account lockout. It must be fixed and deployed as soon as possible, prioritized above non-critical issues. |

**Summary:**
> This is a **Critical Severity / High Priority** defect. The bug not only blocks the core business transaction but also triggers a destructive side effect (PIN lockout) incorrectly. Any defect that locks a customer out of their banking account due to a system error must be treated with the highest urgency.

---

## No.4 – 5 : API Testing

---

### Answer No.4 — Postman Collection: Petstore API

> Collection exported as: **`Napat_Postman_Collection.json`**
> Swagger Reference: [https://petstore.swagger.io/](https://petstore.swagger.io/)

#### Collection Structure

```
Napat_Postman_Collection
├── GET  /pet/{petId}
├── GET  /store/order/{orderId}
└── POST /user
```

#### Request Details

**1. GET /pet/{petId}**
```
Method : GET
URL    : https://petstore.swagger.io/v2/pet/1
Headers: Accept: application/json
```

**2. GET /store/order/{orderId}**
```
Method : GET
URL    : https://petstore.swagger.io/v2/store/order/1
Headers: Accept: application/json
```

**3. POST /user**
```
Method      : POST
URL         : https://petstore.swagger.io/v2/user
Headers     : Content-Type: application/json
              Accept: application/json
Request Body:
{
    "id": 1001,
    "username": "napat_test",
    "firstName": "Napat",
    "lastName": "Phongam",
    "email": "napat.test@example.com",
    "password": "Test@1234",
    "phone": "0812345678",
    "userStatus": 1
}
```

> See the exported collection file: **`Napat_Postman_Collection.json`** (attached separately)

---

### Answer No.5 — API Response Codes

#### GET /pet/{petId}

| Scenario | Input | Response Code | Description |
|----------|-------|---------------|-------------|
| Valid pet ID exists | `petId = 1` | **200 OK** | Pet data returned successfully |
| Pet ID not found | `petId = 999999` | **404 Not Found** | Pet with given ID does not exist |
| Invalid ID format | `petId = "abc"` | **400 Bad Request** | Invalid ID supplied |

**Test Script (Postman):**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response has pet id", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.id).to.eql(1);
});
```

---

#### GET /store/order/{orderId}

| Scenario | Input | Response Code | Description |
|----------|-------|---------------|-------------|
| Valid order ID (1–10) | `orderId = 1` | **200 OK** | Order data returned successfully |
| Order ID not found | `orderId = 999` | **404 Not Found** | Order not found |
| Invalid ID format | `orderId = "xyz"` | **400 Bad Request** | Invalid ID supplied |

**Test Script (Postman):**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Order has required fields", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("id");
    pm.expect(jsonData).to.have.property("status");
});
```

---

#### POST /user

| Scenario | Input | Response Code | Description |
|----------|-------|---------------|-------------|
| Valid user body | Complete JSON body | **200 OK** | User created successfully |
| Missing required fields | Empty or partial body | **400 Bad Request** | Validation error |

**Test Script (Postman):**
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("User created successfully", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.code).to.eql(200);
    pm.expect(jsonData.type).to.eql("unknown");
});
```

---

## No.6 – 8 : SQL Test

---

### Answer No.6 — Display All Product Names

```sql
SELECT product_name
FROM Products;
```

---

### Answer No.7 — Display ID, Name, and Citizen Registered in 2022

```sql
SELECT id, name, citizen
FROM Customers
WHERE YEAR(registered_date) = 2022;
```

> **Alternative syntax (compatible with more SQL engines):**
```sql
SELECT id, name, citizen
FROM Customers
WHERE registered_date >= '2022-01-01'
  AND registered_date < '2023-01-01';
```

---

### Answer No.8 — Display Product & Customer Info by Customer ID

```sql
SELECT
    p.product_id,
    p.product_name,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_full_name,
    c.citizen                              AS customer_citizen
FROM Products p
JOIN Customers c ON p.customer_id = c.customer_id
WHERE c.customer_id = '001110001';
```

> **If the schema uses a separate Orders/Transactions linking table:**
```sql
SELECT
    p.product_id,
    p.product_name,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_full_name,
    c.citizen                              AS customer_citizen
FROM Products p
JOIN Orders o    ON p.product_id  = o.product_id
JOIN Customers c ON o.customer_id = c.customer_id
WHERE c.customer_id = '001110001';
```
