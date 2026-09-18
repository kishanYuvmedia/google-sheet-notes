# Google Sheets Apps Script -- Functions & Practice Programs

## Project Overview

This practical project teaches how to create **custom functions in
Google Sheets using Google Apps Script**.

### Learning Goals

-   Create custom Google Sheets functions
-   Work with numbers, text, dates, arrays, and ranges
-   Add validation and error handling
-   Build reusable business formulas
-   Practice real-world Google Sheets automation concepts

------------------------------------------------------------------------

# Part 1 -- Basic 10 Custom Functions

## 1. ADD_NUMBERS

**Purpose:** Add two numbers.

``` text
=ADD_NUMBERS(A2,B2)
```

Example:

``` text
=ADD_NUMBERS(10,20)
```

Result: `30`

------------------------------------------------------------------------

## 2. SUBTRACT

**Purpose:** Subtract one number from another.

``` text
=SUBTRACT(A2,B2)
```

Example:

``` text
=SUBTRACT(100,30)
```

Result: `70`

------------------------------------------------------------------------

## 3. MULTIPLY

**Purpose:** Multiply two numbers.

``` text
=MULTIPLY(A2,B2)
```

Example:

``` text
=MULTIPLY(10,5)
```

Result: `50`

------------------------------------------------------------------------

## 4. DIVIDE

**Purpose:** Divide two numbers with zero validation.

``` text
=DIVIDE(A2,B2)
```

Example:

``` text
=DIVIDE(100,4)
```

Result: `25`

------------------------------------------------------------------------

## 5. FULL_NAME

**Purpose:** Combine first name and last name.

``` text
=FULL_NAME(A2,B2)
```

Example:

``` text
=FULL_NAME("Kishan","Gopal")
```

Result: `Kishan Gopal`

------------------------------------------------------------------------

## 6. UPPER_TEXT

**Purpose:** Convert text to uppercase.

``` text
=UPPER_TEXT(A2)
```

Example:

``` text
=UPPER_TEXT("hello world")
```

Result: `HELLO WORLD`

------------------------------------------------------------------------

## 7. LOWER_TEXT

**Purpose:** Convert text to lowercase.

``` text
=LOWER_TEXT(A2)
```

Example:

``` text
=LOWER_TEXT("HELLO WORLD")
```

Result: `hello world`

------------------------------------------------------------------------

## 8. COUNT_WORDS

**Purpose:** Count words inside a cell.

``` text
=COUNT_WORDS(A2)
```

Example:

``` text
=COUNT_WORDS("Google Sheets Apps Script")
```

Result: `4`

------------------------------------------------------------------------

## 9. DISCOUNT_PRICE

**Purpose:** Calculate the final price after discount.

``` text
=DISCOUNT_PRICE(A2,B2)
```

Where:

-   A2 = Original Price
-   B2 = Discount Percentage

Example:

``` text
=DISCOUNT_PRICE(10000,10)
```

Result: `9000`

------------------------------------------------------------------------

## 10. GST_AMOUNT

**Purpose:** Calculate GST amount.

``` text
=GST_AMOUNT(A2,B2)
```

Where:

-   A2 = Price
-   B2 = GST Percentage

Example:

``` text
=GST_AMOUNT(10000,18)
```

Result: `1800`

------------------------------------------------------------------------

# Part 2 -- Advanced 10 Custom Functions

## 11. TOTAL_RANGE

**Purpose:** Calculate the total of a range.

``` text
=TOTAL_RANGE(A2:A20)
```

Example:

``` text
=TOTAL_RANGE(100,200,300)
```

Expected result:

``` text
600
```

Apps Script:

``` javascript
function TOTAL_RANGE(values) {
  let total = 0;

  values.flat().forEach(value => {
    if (typeof value === "number") {
      total += value;
    }
  });

  return total;
}
```

------------------------------------------------------------------------

## 12. AVERAGE_RANGE

**Purpose:** Calculate average from a range.

``` text
=AVERAGE_RANGE(A2:A20)
```

Apps Script:

``` javascript
function AVERAGE_RANGE(values) {
  const numbers = values.flat().filter(v => typeof v === "number");

  if (numbers.length === 0) {
    return 0;
  }

  return numbers.reduce((sum, value) => sum + value, 0) / numbers.length;
}
```

------------------------------------------------------------------------

## 13. FIND_MAX

**Purpose:** Find the highest number in a range.

``` text
=FIND_MAX(A2:A20)
```

Apps Script:

``` javascript
function FIND_MAX(values) {
  const numbers = values.flat()
    .filter(v => typeof v === "number");

  return numbers.length ? Math.max(...numbers) : 0;
}
```

------------------------------------------------------------------------

## 14. FIND_MIN

**Purpose:** Find the lowest number in a range.

``` text
=FIND_MIN(A2:A20)
```

Apps Script:

``` javascript
function FIND_MIN(values) {
  const numbers = values.flat()
    .filter(v => typeof v === "number");

  return numbers.length ? Math.min(...numbers) : 0;
}
```

------------------------------------------------------------------------

## 15. PERCENTAGE

**Purpose:** Calculate percentage of one value against another.

``` text
=PERCENTAGE(A2,B2)
```

Example:

``` text
=PERCENTAGE(25,100)
```

Result:

``` text
25%
```

Apps Script:

``` javascript
function PERCENTAGE(value, total) {
  if (Number(total) === 0) {
    return 0;
  }

  return (Number(value) / Number(total)) * 100;
}
```

------------------------------------------------------------------------

## 16. AGE_FROM_DOB

**Purpose:** Calculate age from date of birth.

``` text
=AGE_FROM_DOB(A2)
```

Apps Script:

``` javascript
function AGE_FROM_DOB(dob) {
  const birthDate = new Date(dob);
  const today = new Date();

  let age = today.getFullYear() - birthDate.getFullYear();

  const monthDifference = today.getMonth() - birthDate.getMonth();

  if (
    monthDifference < 0 ||
    (monthDifference === 0 &&
      today.getDate() < birthDate.getDate())
  ) {
    age--;
  }

  return age;
}
```

------------------------------------------------------------------------

## 17. TEXT_LENGTH

**Purpose:** Count characters in a cell.

``` text
=TEXT_LENGTH(A2)
```

Apps Script:

``` javascript
function TEXT_LENGTH(text) {
  return String(text).length;
}
```

------------------------------------------------------------------------

## 18. REMOVE_SPACES

**Purpose:** Remove unnecessary spaces from text.

``` text
=REMOVE_SPACES(A2)
```

Apps Script:

``` javascript
function REMOVE_SPACES(text) {
  return String(text).trim().replace(/\s+/g, " ");
}
```

Example:

``` text
Input:
   Kishan     Gopal

Output:
Kishan Gopal
```

------------------------------------------------------------------------

## 19. PRICE_WITH_GST

**Purpose:** Calculate final price including GST.

``` text
=PRICE_WITH_GST(A2,B2)
```

Apps Script:

``` javascript
function PRICE_WITH_GST(price, gstPercent) {
  price = Number(price);
  gstPercent = Number(gstPercent);

  return price + (price * gstPercent / 100);
}
```

Example:

``` text
=PRICE_WITH_GST(10000,18)
```

Result:

``` text
11800
```

------------------------------------------------------------------------

## 20. GRADE_FROM_MARKS

**Purpose:** Automatically generate a grade based on marks.

``` text
=GRADE_FROM_MARKS(A2)
```

Apps Script:

``` javascript
function GRADE_FROM_MARKS(marks) {
  marks = Number(marks);

  if (marks >= 90) return "A+";
  if (marks >= 80) return "A";
  if (marks >= 70) return "B";
  if (marks >= 60) return "C";
  if (marks >= 50) return "D";
  if (marks >= 40) return "E";

  return "F";
}
```

------------------------------------------------------------------------

# Part 3 -- Complete Advanced Program List

After learning the 20 custom functions, practice these larger Apps
Script programs.

## Program 1 -- Employee Salary Calculator

Create a sheet containing:

  Employee     Basic Salary    HRA   Allowance   Deduction   Net Salary
  ---------- -------------- ------ ----------- ----------- ------------
  Amit                30000   5000        3000        2000            ?
  Rahul               40000   7000        5000        3000            ?

Calculate:

``` text
Net Salary = Basic + HRA + Allowance - Deduction
```

------------------------------------------------------------------------

## Program 2 -- Student Result System

Create:

  Student     English   Maths   Science   Total   Average Grade
  --------- --------- ------- --------- ------- --------- -------
  Amit             80      75        90       ?         ? ?

Requirements:

-   Calculate total
-   Calculate average
-   Generate grade
-   Identify pass/fail

------------------------------------------------------------------------

## Program 3 -- Invoice Calculator

Create an invoice with:

-   Product
-   Quantity
-   Unit Price
-   Subtotal
-   Discount
-   GST
-   Grand Total

Formula:

``` text
Subtotal = Quantity × Unit Price
Discount Amount = Subtotal × Discount %
GST = Discounted Amount × GST %
Grand Total = Discounted Amount + GST
```

------------------------------------------------------------------------

## Program 4 -- Attendance Calculator

Create:

  Employee     Present   Absent   Total Days   Attendance %
  ---------- --------- -------- ------------ --------------
  Amit              24        2           26              ?
  Rahul             22        4           26              ?

Calculate attendance percentage automatically.

------------------------------------------------------------------------

## Program 5 -- Sales Commission Calculator

Create:

  Salesperson      Sales   Commission %   Commission
  ------------- -------- -------------- ------------
  Amit            100000              5            ?
  Rahul           150000              7            ?

Calculate commission automatically.

------------------------------------------------------------------------

## Program 6 -- Product Stock Status

Create:

  Product     Stock   Minimum Stock Status
  --------- ------- --------------- --------
  Laptop         15              10 ?
  Mouse           4              10 ?

Return:

``` text
IN STOCK
LOW STOCK
OUT OF STOCK
```

------------------------------------------------------------------------

## Program 7 -- Customer Data Cleaning

Create a function that:

-   Removes extra spaces
-   Converts names to proper case
-   Removes unwanted characters
-   Formats phone numbers
-   Formats email addresses

Example:

``` text
"  kishan   gopal  "
```

Output:

``` text
"Kishan Gopal"
```

------------------------------------------------------------------------

## Program 8 -- Invoice Number Generator

Create an Apps Script function that generates invoice numbers.

Example:

``` text
INV-2026-0001
INV-2026-0002
INV-2026-0003
```

Requirements:

-   Current year
-   Sequential number
-   Fixed prefix

------------------------------------------------------------------------

## Program 9 -- Employee Performance Report

Create a report using:

-   Employee name
-   Projects completed
-   Tasks completed
-   Tasks pending
-   Productivity %
-   Performance status

Example statuses:

``` text
Excellent
Good
Needs Improvement
```

------------------------------------------------------------------------

## Program 10 -- Monthly Sales Dashboard Data

Create a sheet containing:

-   Date
-   Salesperson
-   Product
-   Category
-   Quantity
-   Sales Amount

Use Apps Script/custom functions to calculate:

-   Total sales
-   Average sales
-   Highest sale
-   Lowest sale
-   Total quantity
-   Sales by employee
-   Sales by category
-   Monthly totals

------------------------------------------------------------------------

# Part 4 -- 10 Practice Questions

## Question 1

Create a custom function:

``` text
=CALCULATE_TOTAL(A2,B2,C2)
```

It should add three numbers.

------------------------------------------------------------------------

## Question 2

Create:

``` text
=FINAL_PRICE(price, discount, gst)
```

Calculate the final price after discount and GST.

------------------------------------------------------------------------

## Question 3

Create:

``` text
=CHECK_PASS(marks)
```

Return:

``` text
PASS
```

when marks are 40 or higher, otherwise:

``` text
FAIL
```

------------------------------------------------------------------------

## Question 4

Create:

``` text
=EMAIL_DOMAIN(email)
```

Input:

``` text
kishan@gmail.com
```

Output:

``` text
gmail.com
```

------------------------------------------------------------------------

## Question 5

Create:

``` text
=PHONE_FORMAT(number)
```

Convert a 10-digit Indian phone number into:

``` text
+91 98765 43210
```

------------------------------------------------------------------------

## Question 6

Create:

``` text
=STUDENT_RESULT(marks)
```

Return:

``` text
A+
A
B
C
D
F
```

according to marks.

------------------------------------------------------------------------

## Question 7

Create:

``` text
=STOCK_STATUS(stock, minimumStock)
```

Return:

``` text
OUT OF STOCK
LOW STOCK
IN STOCK
```

------------------------------------------------------------------------

## Question 8

Create:

``` text
=DISCOUNT_AMOUNT(price, discountPercent)
```

For:

``` text
Price = ₹20,000
Discount = 15%
```

calculate the discount amount.

------------------------------------------------------------------------

## Question 9

Create:

``` text
=GST_TOTAL(price, gstPercent)
```

For:

``` text
Price = ₹50,000
GST = 18%
```

calculate:

1.  GST amount
2.  Final price including GST

------------------------------------------------------------------------

## Question 10

Create an Apps Script custom function:

``` text
=EMPLOYEE_STATUS(completedTasks, pendingTasks)
```

Return:

``` text
Excellent
Good
Needs Improvement
```

based on the task completion ratio.

------------------------------------------------------------------------

# Part 5 -- Suggested Google Sheet Structure

Create these sheets:

``` text
01_Basic_Functions
02_Advanced_Functions
03_Employee_Salary
04_Student_Result
05_Invoice
06_Attendance
07_Sales
08_Inventory
09_Customer_Data
10_Practice_Questions
```

------------------------------------------------------------------------

# Part 6 -- Final Project

## Business Management Sheet

Build one Google Sheets application containing:

### Employee Management

-   Employee name
-   Department
-   Salary
-   Attendance
-   Performance
-   Net salary

### Sales Management

-   Customer
-   Product
-   Quantity
-   Price
-   Discount
-   GST
-   Final amount

### Inventory Management

-   Product
-   SKU
-   Stock
-   Minimum stock
-   Stock status

### Customer Management

-   Customer name
-   Phone
-   Email
-   City
-   Data cleaning

### Dashboard

Show:

``` text
Total Employees
Total Sales
Average Sale
Total Products
Low Stock Products
Average Attendance
```

------------------------------------------------------------------------

# Skills Covered

By completing this project, you will practice:

-   JavaScript functions
-   Parameters and return values
-   Number calculations
-   String manipulation
-   Arrays and ranges
-   Date handling
-   Conditional logic
-   Validation
-   Error handling
-   Google Sheets custom functions
-   Business calculations
-   Data cleaning
-   Basic reporting
-   Dashboard data preparation
