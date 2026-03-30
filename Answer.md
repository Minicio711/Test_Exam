# Answer No.1

```python
list_a = [1, 2, 3, 5, 6, 8, 9]
list_b = [3, 2, 1, 5, 6, 0]
duplicate_numbers = set(list_a) & set(list_b)
print(f"number duplicate is : {sorted(list(duplicate_numbers))}")
```
<img width="800" height="509" alt="image" src="https://github.com/user-attachments/assets/6f1fa129-7240-4431-b6a1-454e06af0fa6" />


# Answer No.2

```robotframework
*** Settings ***
Library SeleniumLibrary

*** Variables ***
${URL} https://the-internet.herokuapp.com/login
${BROWSER} edge
${USERNAME} tomsmith
${PASSWORD} SuperSecretPassword!

#field
${Field_Username} xpath=//*[@id="username"]
${Field_Password} xpath=//*[@id="password"]
#button
${Login_button} xpath=//*[@id="login"]/button
${Logout_button} xpath=//a[contains(@href, 'logout')]
*** Keywords ***
Open Browser To Login Page
Open Browser ${URL} ${BROWSER}
Maximize Browser Window
Wait Until Page Contains Login Page timeout=10s
Take Screenshot
[Arguments] ${filename}
# สร้างโฟลเดอร์ชื่อเดียวกับเทสเคส แล้วเก็บรูปไว้ข้างใน
Capture Page Screenshot ${TEST_NAME}/${filename}.png
*** Test Cases ***
Login success
[Documentation] To verify that users can login successfully when input a correct username and password.
Open Browser To Login Page
Take Screenshot login_page.png
Input Text ${Field_Username} ${USERNAME}
Input Password ${Field_Password} ${PASSWORD}
Click Button ${Login_button}
Wait Until Page Contains You logged into a secure area! timeout=10s
Take Screenshot login_success.png
Click Element ${Logout_button}
Wait Until Page Contains You logged out of the secure area! timeout=10s
Take Screenshot logout_success.png
[Teardown] Close Browser
Login failed - Password incorrect
[Documentation] To verify that users can login unsuccessfully when they input a correct username but wrong password.
Open Browser To Login Page
Take Screenshot login_page.png
Input Text ${Field_Username} ${USERNAME}
Input Password ${Field_Password} Password!
Click Button ${Login_button}
Wait Until Page Contains Your password is invalid! timeout=10s
Take Screenshot login_fail.png
[Teardown] Close Browser
Login failed - Username not found
[Documentation] To verify that users can login unsuccessfully when they input a username that did not exist.
Open Browser To Login Page
Take Screenshot login_page.png
Input Text ${Field_Username} tomholland
Input Password ${Field_Password} Password!
Click Button ${Login_button}
Wait Until Page Contains Your username is invalid! timeout=10s
Take Screenshot login_fail.png
[Teardown] Close Browser
```
<img width="2573" height="1385" alt="image" src="https://github.com/user-attachments/assets/36d0933c-0640-4a22-8ae7-7d910172cc2a" />


# Answer No.3.1
```
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
<img width="1909" height="1031" alt="image" src="https://github.com/user-attachments/assets/090e7955-7b45-460a-840d-bb45ae8d203b" />


# Answer No.3.2
<img width="1910" height="1025" alt="image" src="https://github.com/user-attachments/assets/b1edb47f-029d-436b-9a1b-e4ea1b58a045" />

# Answer No.4
```
*** Settings ***
Documentation       Test Automation Assignment - Question 4: Mobile Application Testing
...
...                 Design test coverage and create automated test scripts for mobile application.
...                 Download source code from: https://github.com/avjinder/Minimal-Todo
...
...                 **Framework:** Robot Framework + Appium
...
...                 **Test Coverage:**
...                 - App launch verification
...                 - CRUD operations (Create, Read, Update, Delete)
...                 - Input validation (special characters, Unicode, empty input)
...                 - Business logic (duplicate items)
...                 - Edge cases and negative testing
...
...                 **Total Test Cases:** 8 test cases covering main features

Resource            ../../init.resource

Suite Setup         mobile.keywords.Open Mobile Application
Suite Teardown      AppiumLibrary.Close All Applications
Test Setup          mobile.keywords.Prepare Clean State
Test Teardown       mobile.keywords.Capture Screenshot On Failure


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

    mobile.keywords.Add Todo Item    Buy Groceries
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

    mobile.keywords.Add Todo Item    Task to delete
    mobile.keywords.Verify Todo Item Exists    Task to delete
    mobile.keywords.Delete Todo Item    Task to delete
    mobile.keywords.Verify Todo Item Deleted    Task to delete
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
    mobile.keywords.Add Todo Item    ${special_text}
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
    mobile.keywords.Add Todo Item    ${unicode_text}
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

    # ตรวจสอบว่ายังคงอยู่ในหน้าเพิ่ม Todo หรือกลับไปหน้าหลักโดยไม่มีรายการ
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

# Answer No.5
```
pipeline {
agent {-----}
stages {
stage('Checkout Code From Git') {
}
stage('Run Test Automate') {
}
stage('Send Result To Jenkins') {
}
}
}
```

# Answer No.6
```
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

encrypted_str = "ECV" # ใส่คำที่ต้องการเข้ารหัส
k_val = 2 # ใส่เงื่อนไขว่านับถอยหลังกี่ตัวอักษร
print(simpleCipher(encrypted_str, k_val)) # ผลลัพธ์
```
<img width="1983" height="1067" alt="image" src="https://github.com/user-attachments/assets/ecb83292-aa40-47bb-a3ad-456807b516c5" />

