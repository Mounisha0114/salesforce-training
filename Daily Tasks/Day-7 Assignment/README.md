# Day 7 – Bulkifying Eligibility Validation using Apex Collections

## Project Overview

This project implements bulk-safe eligibility validation for the Placement Management System in Salesforce using Apex. The solution validates student applications against job eligibility criteria while following Salesforce governor limits and best practices.

---

## Objective

- Validate Student and Job references.
- Validate Student CGPA against Job minimum GPA.
- Validate Student active backlogs against Job allowed backlogs.
- Validate Student department against Job eligible department.
- Use Apex Collections (Set and Map).
- Avoid SOQL queries inside loops.

---

## Technologies Used

- Salesforce
- Apex
- SOQL
- Developer Console

---

## Objects Used

### Student__c
- CGPA__c
- Active_Backlogs__c
- Department__c

### Job__c
- Minimum_Gpa__c
- Allowed_Backlogs_c__c
- Eligible_Department__c

### Application__c
- Student__c (Lookup)
- Job__c (Lookup)

---

## Architecture

```
Application Trigger
        │
        ▼
ApplicationTriggerHandler
        │
        ▼
ApplicationService
        │
        ├── Collect Student IDs
        ├── Collect Job IDs
        ├── Query Students
        ├── Query Jobs
        ├── Store Records in Maps
        └── Validate Applications
```

---

## Features Implemented

### Student Validation
Ensures every application has a Student record.

### Job Validation
Ensures every application has a Job record.

### CGPA Validation
Prevents applications when the student's CGPA is below the job's minimum GPA.

### Backlog Validation
Prevents applications when the student's active backlogs exceed the job's allowed backlogs.

### Department Validation
Allows only students from eligible departments to apply for a job.

---

## Bulkification

This implementation follows Salesforce bulkification best practices.

### Set

Used to collect unique Student and Job IDs.

```apex
Set<Id> studentIds = new Set<Id>();
Set<Id> jobIds = new Set<Id>();
```

### Map

Used to store Student and Job records retrieved from SOQL.

```apex
Map<Id, Student__c> studentMap;
Map<Id, Job__c> jobMap;
```

---

## SOQL Optimization

- One SOQL query for Student records.
- One SOQL query for Job records.
- No SOQL queries inside loops.
- Uses Maps to retrieve records efficiently.

---

## Validations Performed

- Student is required.
- Job is required.
- Student CGPA must meet the Job's minimum GPA.
- Student backlogs must not exceed the Job's allowed backlogs.
- Student department must match the Job's eligible department.

---

## Governor Limit Best Practices

- Uses Sets to avoid duplicate IDs.
- Uses Maps for efficient lookups.
- No SOQL inside loops.
- Bulk-safe implementation.
- Scalable design for multiple records.

---

## Files Included

- ApplicationTrigger.trigger
- ApplicationTriggerHandler.cls
- ApplicationService.cls
- README.md

---

## Test Cases

### Test Case 1
**Scenario:** Student CGPA is greater than the minimum GPA.

**Expected Result:** Application is created successfully.

---

### Test Case 2
**Scenario:** Student CGPA is below the minimum GPA.

**Expected Result:** Validation error is displayed.

---

### Test Case 3
**Scenario:** Student has more active backlogs than allowed.

**Expected Result:**  
`Student has more backlogs than allowed.`

---

### Test Case 4
**Scenario:** Student department is not eligible.

**Expected Result:**  
`Student department is not eligible for this job.`

---

### Test Case 5
**Scenario:** Student or Job is not selected.

**Expected Result:** Appropriate validation error is displayed.

---

## Learning Outcomes

- Learned bulkification using Sets and Maps.
- Implemented bulk-safe Apex code.
- Avoided SOQL queries inside loops.
- Used Trigger → Handler → Service architecture.
- Implemented business validations in Apex.
- Followed Salesforce governor limits and best practices.

---

## Conclusion

This project demonstrates a bulk-safe approach to validating placement applications using Apex Collections. The solution uses Sets and Maps to optimize SOQL queries, validates student eligibility based on business rules, and follows Salesforce best practices for scalable and efficient Apex development.
