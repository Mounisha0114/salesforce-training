# Salesforce Placement Management System – Bulk Processing & Bulk-Safe Apex

## Overview

This project demonstrates the implementation of **Bulk Processing** and **Bulk-Safe Apex Triggers** in Salesforce for a Placement Management System. It follows Salesforce best practices by designing Apex code that efficiently processes multiple records while complying with Governor Limits.

The project focuses on validating student eligibility for job applications using bulkified Apex, Trigger Handlers, and collection-based processing.

---

## Objectives

- Understand Salesforce Governor Limits.
- Design scalable Apex code.
- Implement bulk-safe Trigger architecture.
- Process multiple records efficiently.
- Eliminate SOQL and DML operations inside loops.
- Use Lists, Sets, and Maps effectively.
- Validate student eligibility before creating applications.

---

## Technologies Used

- Salesforce Platform
- Apex
- Apex Triggers
- SOQL
- Lists
- Sets
- Maps
- Developer Edition

---

## Objects Used

### Student__c

Fields:
- Student Name
- CGPA
- Department
- Email
- DOB

### Job__c

Fields:
- Job Title
- Company
- Minimum CGPA
- Location
- Salary
- Closing Date

### Application__c

Fields:
- Student
- Job
- Application Date
- Status
- Remarks

---

## Project Architecture

Application Trigger

↓

ApplicationEligibilityService

↓

Collect Student IDs

↓

Collect Job IDs

↓

Single Student Query

↓

Single Job Query

↓

Store Results in Maps

↓

Validate Applications

↓

Save or Display Error

---

## Bulk Processing Pattern

1. Receive all Application records.
2. Collect Student IDs.
3. Collect Job IDs.
4. Query Students once.
5. Query Jobs once.
6. Store records in Maps.
7. Validate each Application.
8. Prevent invalid Applications using addError().
9. Perform no SOQL inside loops.
10. Perform no DML inside loops.

---

## Governor Limits Considered

This project follows Salesforce Governor Limits by:

- Using only one SOQL query for Students.
- Using only one SOQL query for Jobs.
- Avoiding SOQL inside loops.
- Avoiding DML inside loops.
- Processing records using collections.
- Supporting bulk operations for up to 200 records.

---

## Collections Used

### List

Stores multiple Application records received from Trigger.new.

### Set

Stores unique Student IDs and Job IDs.

### Map

Stores queried Student and Job records for fast lookup using record Ids.

---

## Eligibility Validation Logic

Student is eligible if:

Student CGPA >= Job Minimum CGPA

If the student does not satisfy the eligibility criteria, the application is blocked using:

```apex
app.addError('Student is not eligible because CGPA is below the required minimum CGPA.');
