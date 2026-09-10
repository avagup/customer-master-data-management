# Customer Master Data Management & Entity Resolution

## Project Overview

This project demonstrates an end-to-end customer data quality and master data management workflow using Python and Pandas.

The objective was to identify and address customer data-quality issues, standardize customer attributes, detect duplicate customer entities, and consolidate validated duplicates into mastered customer records while preserving source-to-master traceability.

> **Dataset:** Synthetic customer data created for portfolio demonstration purposes.

---

## Business Problem

Customer data can contain missing values, invalid attributes, inconsistent formatting, and duplicate records.

These issues can result in:

- Fragmented customer records
- Inaccurate reporting
- Poor data quality in downstream systems
- Multiple records representing the same customer
- Difficulty maintaining a consistent customer view

This project demonstrates a practical workflow for identifying these issues and creating a consolidated customer master view.

---

## Dataset

| Attribute | Details |
|---|---|
| Records analyzed | 10,100 |
| Original attributes | 18 |
| Core customer attributes | Name, Email, Phone, DOB, Country, State, Signup Date, Customer Status |
| Tools | Python, Pandas |

---

## 1. Data Quality Assessment

The dataset was profiled to identify missing, invalid, and logically inconsistent values before performing duplicate detection.

### Key Findings

| Issue | Records | Treatment |
|---|---:|---|
| Missing Email | 118 | Retained as missing |
| Invalid Email | 122 | Flagged for data-quality review |
| Missing Phone | 255 | Retained as missing |
| Missing State | 102 | Retained as missing |
| Missing First Name | 80 | Retained as missing |
| Future Date of Birth | 25 | Set to missing |
| Future Signup Date | 21 | Set to missing |

### Standardization

The following customer attributes were standardized or validated:

- Country values
- Customer status values
- Email formatting
- Phone numbers converted to digits-only format
- Date fields validated for logical consistency
- Quality flags created for key customer attributes

Invalid or logically impossible dates were not fabricated or replaced with assumed values. They were treated as data-quality exceptions.

---

## 2. Duplicate Detection & Entity Resolution

After standardization, normalized email addresses were used as the primary matching key to identify potential duplicate customer records.

### Results

- **98 duplicate customer groups**
- **196 source records**
- **2 source records per duplicate group**

Each duplicate group was then compared across the available customer attributes, including:

- First name
- Last name
- Phone
- Date of birth
- Country
- State
- Signup date
- Customer status

---

## 3. Match Validation

All **98 duplicate groups showed exact agreement across the compared customer attributes**.

This exact agreement supported consolidation of each duplicate pair into a single mastered customer entity.

No duplicate groups required further attribute-level review within this dataset.

---

## 4. Golden Record Creation

Each validated duplicate group was consolidated into a single mastered customer entity using a unique `Master_Customer_ID`.

### Final Mastered Entities

**98 mastered customer entities** were created from the **196 source records** identified during duplicate detection.

The mastered customer view retains standardized customer attributes while preserving the original `Customer_ID` for source lineage.

Example:

```text
CUST003976
CUST010063
     ↓
MASTER0001
