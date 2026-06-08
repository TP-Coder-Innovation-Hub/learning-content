# Data Basics

## What

Data is the information your business collects, stores, and uses. Understanding how data is organized and managed helps you make better decisions about technology investments.

## The Filing Cabinet Analogy

Think of data storage as a filing system:

### Relational Database — The Organized Filing Cabinet

Every document has a specific place. Customer files are in one drawer, order files in another, product files in a third. Each file has a number, and the files are connected — order #1234 belongs to customer #567.

Rules are enforced:
- Every order must belong to a real customer
- Product prices must be numbers
- You can't have two customers with the same ID

This is structured, predictable, and great for answering questions like: "How many orders did customer X place last month?"

### Document Database — The Labeled Binders

Instead of strict filing rules, you have binders where each document can look different. One customer document might have a phone number and address. Another might have an email and preferred contact method.

More flexible. Good when your data doesn't fit neatly into rows and columns. Harder to enforce consistency.

### Spreadsheet — The Simplest Database

A spreadsheet is a simple database. Rows are records, columns are fields. If you've used Excel, you understand the basic concept.

## Data Lifecycle

```
Create → Store → Process → Analyze → Archive/Delete
```

1. **Create** — Data enters your system (user signs up, order is placed, sensor reports a reading)
2. **Store** — Data is saved somewhere (database, file, cloud storage)
3. **Process** — Data is transformed or used (calculating totals, generating reports, triggering actions)
4. **Analyze** — Data is examined for patterns and insights (sales trends, customer behavior)
5. **Archive/Delete** — Old data is moved to cheaper storage or removed according to retention policies

## Types of Data

### Structured Data

Fits neatly into rows and columns. Easy to search and analyze.

Examples: Customer records, transactions, inventory levels, employee data.

### Unstructured Data

No fixed format. Harder to search but often rich in information.

Examples: Emails, documents, images, videos, social media posts, call recordings.

### Semi-Structured Data

Has some organization but not strict rows and columns.

Examples: JSON files, XML documents, log files, sensor readings.

## Key Concepts

- **Database** — Where data is stored and organized
- **Query** — A question you ask the database ("How many users signed up last week?")
- **Backup** — A copy of your data in case something goes wrong
- **Migration** — Moving data from one system to another
- **Retention** — How long you keep data before archiving or deleting it
- **Compliance** — Rules about how data must be handled (GDPR, HIPAA, SOC 2)

## Why This Matters for Business

- Data storage costs money. More data and faster access costs more.
- Data quality affects every downstream decision. Bad data leads to bad reports.
- Data has a lifecycle. Keeping everything forever is expensive and potentially illegal.
- The way data is stored affects how quickly you can get answers. Choosing the right structure matters.
