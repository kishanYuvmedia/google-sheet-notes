# Google Sheets Data Analytics — Custom Apps Script Functions

A practical collection of custom Google Apps Script functions for CRM, sales, lead, customer, order, and business analytics.

## 1. Setup

1. Open Google Sheets.
2. Go to **Extensions → Apps Script**.
3. Delete the default code.
4. Paste the script below.
5. Click **Save**.
6. Return to Google Sheets.
7. Use the functions directly in cells.

---

## 2. Core Analytics Functions

```javascript
/**
 * Calculate percentage.
 * Example:
 * =GET_PERCENTAGE(250,1000)
 */
function GET_PERCENTAGE(value, total) {
  if (total === 0) return 0;
  return (value / total) * 100;
}

/**
 * Calculate growth rate.
 * Example:
 * =GROWTH_RATE(120000,100000)
 */
function GROWTH_RATE(current, previous) {
  if (previous === 0) return 0;
  return ((current - previous) / previous) * 100;
}

/**
 * Lead conversion rate.
 * Example:
 * =CONVERSION_RATE(125,1000)
 */
function CONVERSION_RATE(converted, totalLeads) {
  if (totalLeads === 0) return 0;
  return (converted / totalLeads) * 100;
}

/**
 * Customer segmentation based on purchase value.
 * Example:
 * =CUSTOMER_SEGMENT(E2)
 */
function CUSTOMER_SEGMENT(amount) {
  if (amount >= 100000) return "VIP";
  if (amount >= 50000) return "Premium";
  if (amount >= 10000) return "Regular";
  return "Low Value";
}

/**
 * Age group classification.
 * Example:
 * =AGE_GROUP(B2)
 */
function AGE_GROUP(age) {
  if (age < 18) return "Under 18";
  if (age <= 25) return "18-25";
  if (age <= 35) return "26-35";
  if (age <= 45) return "36-45";
  if (age <= 60) return "46-60";
  return "60+";
}

/**
 * Extract month name.
 * Example:
 * =MONTH_NAME(A2)
 */
function MONTH_NAME(date) {
  if (!(date instanceof Date)) return "";
  return Utilities.formatDate(
    date,
    Session.getScriptTimeZone(),
    "MMMM"
  );
}

/**
 * Rank a value in a range.
 * Example:
 * =SALES_RANK(E2,E2:E100)
 */
function SALES_RANK(value, range) {
  const values = range
    .flat()
    .filter(v => typeof v === "number")
    .sort((a, b) => b - a);

  return values.indexOf(value) + 1;
}

/**
 * Detect statistical outliers using IQR.
 * Example:
 * =OUTLIER_FLAG(E2,E2:E100)
 */
function OUTLIER_FLAG(value, range) {
  const values = range
    .flat()
    .filter(v => typeof v === "number")
    .sort((a, b) => a - b);

  if (values.length < 4) return "Insufficient Data";

  const q1 = values[Math.floor(values.length * 0.25)];
  const q3 = values[Math.floor(values.length * 0.75)];
  const iqr = q3 - q1;

  const lower = q1 - 1.5 * iqr;
  const upper = q3 + 1.5 * iqr;

  return value < lower || value > upper
    ? "OUTLIER"
    : "NORMAL";
}
```

---

# 3. CRM Lead Analytics Functions

These functions are useful for a real-estate CRM or sales enquiry sheet.

## Lead Score

```javascript
/**
 * Calculate a lead score from budget, intent, property type and location.
 *
 * Example:
 * =LEAD_SCORE(B2,C2,D2,E2)
 *
 * B2 = Budget
 * C2 = Buying Intent
 * D2 = Property Type
 * E2 = Location
 */
function LEAD_SCORE(budget, intent, propertyType, location) {
  let score = 0;

  // Budget
  if (Number(budget) >= 10000000) score += 40;
  else if (Number(budget) >= 5000000) score += 30;
  else if (Number(budget) >= 2500000) score += 20;
  else if (Number(budget) >= 1000000) score += 10;

  // Intent
  const intentText = String(intent).toLowerCase();

  if (
    intentText.includes("immediate") ||
    intentText.includes("ready") ||
    intentText.includes("urgent")
  ) {
    score += 30;
  } else if (intentText.includes("3 month")) {
    score += 20;
  } else if (intentText.includes("6 month")) {
    score += 10;
  }

  // Property type
  const propertyText = String(propertyType).toLowerCase();

  if (
    propertyText.includes("commercial") ||
    propertyText.includes("villa")
  ) {
    score += 15;
  } else if (
    propertyText.includes("flat") ||
    propertyText.includes("apartment")
  ) {
    score += 10;
  }

  // Location
  if (String(location).trim() !== "") {
    score += 15;
  }

  return Math.min(score, 100);
}
```

## Lead Priority

```javascript
/**
 * Convert lead score into priority.
 *
 * Example:
 * =LEAD_PRIORITY(F2)
 */
function LEAD_PRIORITY(score) {
  score = Number(score);

  if (score >= 80) return "HOT";
  if (score >= 50) return "WARM";
  return "COLD";
}
```

## Property Segment

```javascript
/**
 * Classify property based on budget.
 *
 * Example:
 * =PROPERTY_SEGMENT(B2)
 */
function PROPERTY_SEGMENT(budget) {
  budget = Number(budget);

  if (budget >= 10000000) return "Luxury";
  if (budget >= 5000000) return "Premium";
  if (budget >= 2500000) return "Mid Market";
  return "Budget";
}
```

## Budget Segment

```javascript
/**
 * Categorize customer budget.
 *
 * Example:
 * =BUDGET_SEGMENT(B2)
 */
function BUDGET_SEGMENT(budget) {
  budget = Number(budget);

  if (budget >= 10000000) return "1Cr+";
  if (budget >= 5000000) return "50L-1Cr";
  if (budget >= 2500000) return "25L-50L";
  if (budget >= 1000000) return "10L-25L";
  return "Below 10L";
}
```

---

# 4. Sales Analytics

## Sales Performance

```javascript
/**
 * Calculate sales achievement percentage.
 *
 * Example:
 * =SALES_ACHIEVEMENT(750000,1000000)
 */
function SALES_ACHIEVEMENT(actual, target) {
  if (Number(target) === 0) return 0;
  return (Number(actual) / Number(target)) * 100;
}
```

## Average Deal Value

```javascript
/**
 * Calculate average deal value.
 *
 * Example:
 * =AVERAGE_DEAL_VALUE(B2:B100)
 */
function AVERAGE_DEAL_VALUE(values) {
  const numbers = values
    .flat()
    .filter(v => typeof v === "number" && !isNaN(v));

  if (numbers.length === 0) return 0;

  return numbers.reduce((sum, value) => sum + value, 0) / numbers.length;
}
```

## Pipeline Value

```javascript
/**
 * Calculate pipeline value from deal values and probability.
 *
 * Example:
 * =PIPELINE_VALUE(B2:B100,C2:C100)
 *
 * B = Deal Value
 * C = Probability percentage
 */
function PIPELINE_VALUE(values, probabilities) {
  let total = 0;

  for (let i = 0; i < values.length; i++) {
    const value = Number(values[i][0]) || 0;
    const probability = Number(probabilities[i][0]) || 0;

    total += value * (probability / 100);
  }

  return total;
}
```

---

# 5. Date Analytics

## Days Since Lead Created

```javascript
/**
 * Calculate lead age in days.
 *
 * Example:
 * =LEAD_AGE(A2)
 */
function LEAD_AGE(createdDate) {
  if (!(createdDate instanceof Date)) return "";

  const today = new Date();
  const diff = today.getTime() - createdDate.getTime();

  return Math.floor(diff / (1000 * 60 * 60 * 24));
}
```

## Follow-up Status

```javascript
/**
 * Check whether a follow-up is overdue.
 *
 * Example:
 * =FOLLOWUP_STATUS(H2)
 */
function FOLLOWUP_STATUS(followupDate) {
  if (!(followupDate instanceof Date)) return "No Date";

  const today = new Date();

  if (followupDate < today) return "OVERDUE";
  if (
    followupDate.getFullYear() === today.getFullYear() &&
    followupDate.getMonth() === today.getMonth() &&
    followupDate.getDate() === today.getDate()
  ) {
    return "TODAY";
  }

  return "UPCOMING";
}
```

---

# 6. Data Cleaning Functions

## Clean Text

```javascript
/**
 * Remove extra spaces and normalize text.
 *
 * Example:
 * =CLEAN_TEXT(A2)
 */
function CLEAN_TEXT(value) {
  if (value === null || value === undefined) return "";

  return String(value)
    .trim()
    .replace(/\s+/g, " ");
}
```

## Normalize Phone Number

```javascript
/**
 * Extract digits from a phone number.
 *
 * Example:
 * =NORMALIZE_PHONE(A2)
 */
function NORMALIZE_PHONE(phone) {
  if (phone === null || phone === undefined) return "";

  const digits = String(phone).replace(/\D/g, "");

  if (digits.length > 10) {
    return digits.slice(-10);
  }

  return digits;
}
```

## Normalize Email

```javascript
/**
 * Normalize email address.
 *
 * Example:
 * =NORMALIZE_EMAIL(A2)
 */
function NORMALIZE_EMAIL(email) {
  if (email === null || email === undefined) return "";

  return String(email)
    .trim()
    .toLowerCase();
}
```

## Check Duplicate

```javascript
/**
 * Check whether a value appears more than once.
 *
 * Example:
 * =DUPLICATE_CHECK(A2,A2:A1000)
 */
function DUPLICATE_CHECK(value, range) {
  const target = String(value).trim().toLowerCase();

  const count = range
    .flat()
    .filter(v => String(v).trim().toLowerCase() === target)
    .length;

  return count > 1 ? "DUPLICATE" : "UNIQUE";
}
```

---

# 7. Statistical Analytics

## Median

```javascript
/**
 * Calculate median.
 *
 * Example:
 * =MEDIAN_VALUE(B2:B100)
 */
function MEDIAN_VALUE(values) {
  const numbers = values
    .flat()
    .filter(v => typeof v === "number")
    .sort((a, b) => a - b);

  if (numbers.length === 0) return 0;

  const middle = Math.floor(numbers.length / 2);

  if (numbers.length % 2 === 0) {
    return (numbers[middle - 1] + numbers[middle]) / 2;
  }

  return numbers[middle];
}
```

## Standard Deviation

```javascript
/**
 * Calculate population standard deviation.
 *
 * Example:
 * =STD_DEV(B2:B100)
 */
function STD_DEV(values) {
  const numbers = values
    .flat()
    .filter(v => typeof v === "number");

  if (numbers.length === 0) return 0;

  const mean =
    numbers.reduce((sum, value) => sum + value, 0) /
    numbers.length;

  const variance =
    numbers.reduce(
      (sum, value) => sum + Math.pow(value - mean, 2),
      0
    ) / numbers.length;

  return Math.sqrt(variance);
}
```

---

# 8. Customer Analytics

## Customer Value Segment

```javascript
/**
 * Segment customers by total revenue.
 *
 * Example:
 * =CUSTOMER_VALUE_SEGMENT(E2)
 */
function CUSTOMER_VALUE_SEGMENT(amount) {
  amount = Number(amount);

  if (amount >= 500000) return "Platinum";
  if (amount >= 250000) return "Gold";
  if (amount >= 100000) return "Silver";
  return "Bronze";
}
```

## Customer Activity

```javascript
/**
 * Classify customer based on days since last activity.
 *
 * Example:
 * =CUSTOMER_ACTIVITY(A2)
 */
function CUSTOMER_ACTIVITY(lastActivityDate) {
  if (!(lastActivityDate instanceof Date)) {
    return "Unknown";
  }

  const today = new Date();
  const diff =
    today.getTime() - lastActivityDate.getTime();

  const days = Math.floor(
    diff / (1000 * 60 * 60 * 24)
  );

  if (days <= 7) return "Active";
  if (days <= 30) return "Warm";
  if (days <= 90) return "Inactive";
  return "Dormant";
}
```

---

# 9. CRM Analytics Sheet Structure

A useful CRM sheet can contain:

| Column | Field |
|---|---|
| A | Lead ID |
| B | Lead Date |
| C | Customer Name |
| D | Phone |
| E | Email |
| F | City |
| G | Source |
| H | Property Type |
| I | Budget |
| J | Intent |
| K | Lead Score |
| L | Priority |
| M | Status |
| N | Sales Agent |
| O | Follow-up Date |
| P | Follow-up Status |
| Q | Deal Value |
| R | Probability |
| S | Pipeline Value |
| T | Lead Age |

Example formulas:

```text
K2 =LEAD_SCORE(I2,J2,H2,F2)

L2 =LEAD_PRIORITY(K2)

P2 =FOLLOWUP_STATUS(O2)

S2 =Q2*(R2/100)

T2 =LEAD_AGE(B2)
```

---

# 10. Dashboard KPIs

For a CRM dashboard, calculate:

### Lead KPIs

```text
Total Leads
New Leads
Qualified Leads
Hot Leads
Warm Leads
Cold Leads
Converted Leads
Lost Leads
```

### Sales KPIs

```text
Total Revenue
Pipeline Value
Average Deal Value
Conversion Rate
Sales Achievement %
Monthly Revenue
Monthly Growth %
```

### Customer KPIs

```text
Total Customers
New Customers
Active Customers
Inactive Customers
Dormant Customers
VIP Customers
Average Customer Value
```

### Team KPIs

```text
Leads Per Agent
Conversions Per Agent
Revenue Per Agent
Average Deal Per Agent
Agent Conversion Rate
```

---

# 11. Recommended Dashboard Charts

Create these charts in Google Sheets:

1. Leads by Month
2. Revenue by Month
3. Leads by Source
4. Conversion Rate by Source
5. Leads by City
6. Revenue by City
7. Lead Status Distribution
8. Hot/Warm/Cold Leads
9. Agent Performance
10. Property Type Distribution
11. Budget Segment Distribution
12. Pipeline by Stage
13. Monthly Growth
14. Average Deal Value
15. Follow-up Status

---

# 12. Useful Native Google Sheets Functions

Custom Apps Script should be combined with native functions.

### COUNTIF

```excel
=COUNTIF(M2:M1000,"Converted")
```

### SUMIF

```excel
=SUMIF(M2:M1000,"Converted",Q2:Q1000)
```

### AVERAGEIF

```excel
=AVERAGEIF(M2:M1000,"Converted",Q2:Q1000)
```

### UNIQUE

```excel
=UNIQUE(F2:F1000)
```

### FILTER

```excel
=FILTER(A2:T1000,L2:L1000="HOT")
```

### QUERY

```excel
=QUERY(A1:T1000,
"SELECT F, COUNT(A)
 WHERE F IS NOT NULL
 GROUP BY F
 ORDER BY COUNT(A) DESC",
1)
```

### SORT

```excel
=SORT(A2:T1000,11,FALSE)
```

---

# 13. Real Estate CRM Analytics Example

For a real-estate lead dataset, useful segmentation is:

```text
Budget
├── Below 10L
├── 10L-25L
├── 25L-50L
├── 50L-1Cr
└── 1Cr+

Lead Priority
├── HOT
├── WARM
└── COLD

Property
├── Apartment
├── Villa
├── Plot
├── Commercial
└── Farm House

Intent
├── Immediate
├── 3 Months
├── 6 Months
└── Just Exploring
```

---

# 14. Advanced Analytics Roadmap

After the basic functions are working, add:

```text
1. RFM Customer Segmentation
2. Cohort Analysis
3. Customer Lifetime Value
4. Churn Prediction
5. Lead Conversion Prediction
6. Sales Forecasting
7. Moving Average
8. Month-over-Month Growth
9. Year-over-Year Growth
10. Pareto Analysis
11. ABC Analysis
12. Outlier Detection
13. Correlation Analysis
14. Regression Analysis
15. Agent Performance Score
16. Source ROI Analysis
17. Marketing Attribution
18. Pipeline Forecast
19. Customer Retention Rate
20. Repeat Purchase Rate
```

---

# 15. Important Best Practices

- Keep raw data separate from calculated data.
- Use a unique Lead ID or Customer ID.
- Normalize phone numbers and email addresses.
- Avoid manually editing calculated columns.
- Use Data Validation for status fields.
- Keep dates as real date values, not text.
- Use Pivot Tables for large summaries.
- Use QUERY for dynamic reporting.
- Use Apps Script for reusable business logic.
- Keep dashboard calculations separate from raw CRM data.
- Add filters/slicers to dashboards.
- Validate missing and duplicate records before analysis.

---

# 16. Suggested Project Structure

```text
Google Sheets Analytics Project
│
├── Raw_Data
│   └── CRM / Sales / Customer records
│
├── Clean_Data
│   └── Cleaned and normalized records
│
├── Calculations
│   └── Scores, segments, KPIs
│
├── Pivot_Data
│   └── Pivot tables and summaries
│
├── Dashboard
│   └── Charts and KPI cards
│
└── Apps_Script
    └── Custom analytics functions
```

## Recommended starting functions

For a CRM/data analytics project, start with:

```text
GET_PERCENTAGE()
GROWTH_RATE()
CONVERSION_RATE()
LEAD_SCORE()
LEAD_PRIORITY()
PROPERTY_SEGMENT()
BUDGET_SEGMENT()
LEAD_AGE()
FOLLOWUP_STATUS()
CUSTOMER_SEGMENT()
CUSTOMER_VALUE_SEGMENT()
CUSTOMER_ACTIVITY()
DUPLICATE_CHECK()
NORMALIZE_PHONE()
NORMALIZE_EMAIL()
OUTLIER_FLAG()
MEDIAN_VALUE()
STD_DEV()
PIPELINE_VALUE()
SALES_ACHIEVEMENT()
```

These provide a strong base for building a **Google Sheets CRM + Analytics + Dashboard system**.
