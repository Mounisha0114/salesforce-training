# Placement Management System

## Project Overview

The Placement Management System is a Salesforce-based application developed to automate the campus placement process. The system enables students to apply for jobs, validates eligibility criteria, prevents duplicate applications, updates application status, and provides meaningful feedback using Apex, SOQL, DML, and Triggers.

This project was implemented as part of the Salesforce Developer Bridge Program Engineering Sprint.

---

## Technologies Used

- Salesforce Platform
- Apex
- SOQL (Salesforce Object Query Language)
- DML (Data Manipulation Language)
- Salesforce Triggers
- Lightning Web Components (LWC)
- Salesforce CLI
- Visual Studio Code

---

# Custom Objects

## Student__c

Fields:
- Name
- CGPA__c
- Department__c

---

## Job__c

Fields:
- Name
- Minimum_Gpa__c

---

## Application__c

Fields:
- Student__c (Lookup)
- Job__c (Lookup)
- Status__c

---

# Engineering Sprint 7 – Retrieving Student Information

## Business Requirement

Before validating an application, the software retrieves the student's information using SOQL. Only the fields required for eligibility validation are queried.

### Apex Method

```apex
public static Student__c getStudent(Id studentId){

    Student__c student = [
        SELECT Id,
               Name,
               CGPA__c,
               Department__c
        FROM Student__c
        WHERE Id = :studentId
        LIMIT 1
    ];

    return student;
}
```

### Expected Behaviour

- Retrieve the student record.
- Fetch only the required fields.
- Prepare data for eligibility validation.

---

# Engineering Sprint 8 – Retrieving Job Information

## Business Requirement

Retrieve the selected job record before making any eligibility decision.

### Apex Method

```apex
public static Job__c getJob(Id jobId){

    Job__c job = [
        SELECT Id,
               Name,
               Minimum_Gpa__c
        FROM Job__c
        WHERE Id = :jobId
        LIMIT 1
    ];

    return job;
}
```

### Expected Behaviour

- Retrieve the selected job.
- Fetch minimum GPA required.
- Prepare information for eligibility checking.

---

# Engineering Sprint 9 – Preventing Duplicate Applications

## Business Requirement

A student should not be able to submit multiple applications for the same job.

### Apex Method

```apex
public static Boolean alreadyApplied(Id studentId, Id jobId){

    Integer applicationCount = [
        SELECT COUNT()
        FROM Application__c
        WHERE Student__c = :studentId
        AND Job__c = :jobId
    ];

    return applicationCount > 0;
}
```

### Trigger Validation

```apex
if(countApp > 0){
    app.addError('Student has already applied for this job.');
}
```

### Test Scenarios

| Scenario | Expected Result |
|----------|-----------------|
| First Application | Continue |
| Duplicate Application | Reject |
| Different Job | Continue |

---

# Engineering Sprint 10 – Creating the Application

## Business Requirement

After successful validation, create and save a new Application record.

### Apex Method

```apex
public static String createApplication(Id studentId, Id jobId){

    if(alreadyApplied(studentId, jobId)){
        return 'Student has already applied for this job.';
    }

    Application__c app = new Application__c();

    app.Student__c = studentId;
    app.Job__c = jobId;
    app.Status__c = 'Applied';

    insert app;

    return 'Application submitted successfully.';
}
```

### Expected Behaviour

- Create Application record.
- Save using DML.
- Return success message.

---

# Engineering Sprint 11 – Updating Application Status

## Business Requirement

Update the application status after recruiter action.

Possible Status Values

- Applied
- Shortlisted
- Interview Scheduled
- Selected
- Rejected

### Apex Method

```apex
public static void updateStatus(Id applicationId, String status){

    Application__c app = [
        SELECT Id,
               Status__c
        FROM Application__c
        WHERE Id = :applicationId
        LIMIT 1
    ];

    app.Status__c = status;

    update app;
}
```

### Expected Behaviour

- Retrieve existing Application.
- Update Status.
- Save changes.

---

# Engineering Sprint 12 – Completing the Business Transaction

## Complete Business Flow

```
Receive Request
      ↓
Retrieve Student
      ↓
Retrieve Job
      ↓
Check Duplicate
      ↓
Validate Eligibility
      ↓
Create Application
      ↓
Save Record
      ↓
Display Confirmation
```

The project successfully performs the complete enterprise transaction.

---

# Application Trigger

### Trigger Name

ApplicationTrigger

### Validations Implemented

- Student must be selected.
- Job must be selected.
- Prevent duplicate applications.
- Validate Student CGPA against Job Minimum GPA.

---

# Features Implemented

- Student Retrieval using SOQL
- Job Retrieval using SOQL
- Duplicate Application Prevention
- Eligibility Validation
- CGPA Validation
- Application Creation
- Application Status Update
- User Validation Messages
- Enterprise Business Transaction Flow

---

# Business Rules

- Student must exist.
- Job must exist.
- Student CGPA must satisfy the minimum GPA requirement.
- Duplicate applications are not allowed.
- Application status can be updated after recruitment stages.

---

# Project Structure

```
force-app
└── main
    └── default
        ├── classes
        │     PlacementService.cls
        │
        ├── triggers
        │     ApplicationTrigger.trigger
        │
        └── lwc
              placementHome
              placementDashboard
              applicationStatus
              dataBindingDemo
```

---

# Debugging and Engineering Best Practices

- Retrieve only required fields using SOQL.
- Perform all validations before DML operations.
- Avoid duplicate SOQL queries.
- Keep business logic modular.
- Separate retrieval, validation, and record creation into different methods.

---

# Learning Outcomes

During this sprint, the following Salesforce concepts were implemented:

- Apex Programming
- SOQL Queries
- DML Operations
- Salesforce Triggers
- Business Logic Implementation
- Validation Rules using Apex
- Enterprise Transaction Processing
- Code Reusability
- Governor Limit Awareness
- Salesforce Best Practices

---

# Future Enhancements

- Email notification after successful application.
- Automatic interview scheduling.
- Offer Letter generation.
- Dashboard with real-time statistics.
- Bulk application processing.
- Trigger Handler Pattern.
- Comprehensive Apex Test Classes.

---

# Author

**Name:** Mounisha Rathnavalli Savaram

**Roll Number:** 23PA1A05M0

**College:** Vishnu Institute of Technology

**Department:** Computer Science and Engineering

---

# Conclusion

The Placement Management System successfully implements the complete placement application lifecycle using Salesforce technologies. The project demonstrates the practical use of SOQL, DML, Apex Classes, Triggers, and Lightning Web Components while following enterprise software development principles such as modularity, efficient data retrieval, validation before data modification, and maintainable service design.
