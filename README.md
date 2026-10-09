# Selenium Web Table Automation Testing

## Project Description

This project automates the testing of a web table using **Python, Selenium WebDriver, and Microsoft Edge**. The automation script interacts with the AssertQA Web Tables practice page to retrieve table information, verify records, search for employees, extract email addresses, identify the highest due amount, validate website links, and count data rows.

## Objectives

- Verify and display all table column headings.
- Retrieve the first and last data rows.
- Search for an employee using their last name.
- Extract and display email addresses from the table.
- Identify the employee record with the highest due amount.
- Verify whether a specified website link exists.
- Count the total number of data rows.

## Technologies Used

- **Programming Language:** Python
- **Automation Tool:** Selenium WebDriver
- **Browser:** Microsoft Edge
- **Wait Mechanism:** WebDriverWait and Expected Conditions
- **Locator Strategies:** XPath and HTML tags

## Test Cases

| Test Case | Description |
|---|---|
| TC01 | Retrieve and display column headings |
| TC02 | Display the first data row |
| TC03 | Display the last data row |
| TC04 | Search for an employee by last name |
| TC05 | Extract all email addresses |
| TC06 | Find the highest due amount and corresponding employee record |
| TC07 | Verify whether a website link exists |
| TC08 | Count the total number of data rows |

### Program:
```

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
driver = webdriver.Edge()
wait = WebDriverWait(driver, 10)
driver.get("https://assertqa.com/practice/webtables")
driver.maximize_window()
table = wait.until(
    EC.visibility_of_element_located((By.TAG_NAME, "table"))
)
headers = table.find_elements(By.XPATH, ".//thead/tr/th")
rows = table.find_elements(By.XPATH, ".//tbody/tr")
print("TC01: Column Headings")
for header in headers:
    print(header.text)
print("TC02: First Data Row")
if rows:
    print(rows[0].text)
else:
    print("No data rows found")
print("TC03: Last Data Row")
if rows:
    print(rows[-1].text)
else:
    print("No data rows found")
print("TC04: Search by Last Name")
last_name = input("Enter employee last name: ")
found = False
for row in rows:
    if last_name.lower() in row.text.lower():
        print("Matching record:", row.text)
        found = True
if not found:
    print("No matching employee found")
print("TC05: All Email Addresses")
for row in rows:
    cells = row.find_elements(By.TAG_NAME, "td")
    for cell in cells:
        if "@" in cell.text:
            print(cell.text)

print("TC06: Highest Due Amount")
highest_due = None
highest_due_row = None
for row in rows:
    cells = row.find_elements(By.TAG_NAME, "td")
    for index, header in enumerate(headers):
        if header.text.strip().lower() == "due":
            due_index = index
            break
    else:
        due_index = None
    if due_index is not None and due_index < len(cells):
        due_text = cells[due_index].text.strip()
        due_value = float(
            due_text.replace("$", "").replace(",", "")
        )
        if highest_due is None or due_value > highest_due:
            highest_due = due_value
            highest_due_row = row.text
if highest_due_row is not None:
    print("Employee record:", highest_due_row)
    print("Highest Due amount:", highest_due)
else:
    print("Due column or valid Due values not found")
print("TC07: Verify Website Link")
website = input("Enter website URL or text to search: ")
links = driver.find_elements(By.XPATH, "//a")
link_found = False
for link in links:
    href = link.get_attribute("href") or ""
    text = link.text.strip()
    if website.lower() in href.lower() or website.lower() in text.lower():
        link_found = True
        print("PASS: Website link exists")
        print("Link:", href)
        break
if not link_found:
    print("FAIL: Website link does not exist")
print("\nTC08: Count Data Rows")
print("Total data rows:", len(rows))
driver.quit()
```

## Expected Outcome
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6fae8b7b-53d6-4c05-8c8f-a47558f8949a" />


The script retrieves information from the web table and displays the results in the terminal. It helps practice Selenium element identification, table traversal, text extraction, conditional logic, loops, user input, and basic data analysis.

## Conclusion

This project demonstrates practical web table automation techniques used in QA testing. It provides experience in validating tabular data, searching records, extracting information, and performing automated checks using Selenium WebDriver.
