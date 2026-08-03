# Day 1 Assignment – Placement Management System

## Objective

Develop Salesforce developer thinking by converting a business requirement into a working Salesforce solution.

---

## Business Scenario

Our college wants to build a **Placement Management System** in Salesforce. Companies publish job openings with eligibility criteria. Students apply for jobs, and the placement team tracks every application throughout the recruitment process.

---

# Task 1: Design the Data Model

## Required Objects

The following custom objects were identified based on the business requirements:

1. **Student__c**
2. **Job__c**
3. **Application__c**

---

## Object Details

### 1. Student__c

This object stores information about students participating in campus placements.

**Fields:**
- Name
- Department__c
- CGPA__c
- Email__c
- Phone__c

---

### 2. Job__c

This object stores job openings posted by companies.

**Fields:**
- Name
- Company__c
- Location__c
- Minimum_CGPA__c
- Last_Date_To_Apply__c

---

### 3. Application__c

This object stores every job application submitted by students.

**Fields:**
- Student__c (Lookup to Student__c)
- Job__c (Lookup to Job__c)
- Status__c (Applied, Shortlisted, Selected, Rejected)
- Applied_Date__c

---

## Relationships

- **Student__c → Application__c**
  - Relationship Type: Lookup
  - One student can have multiple applications.

- **Job__c → Application__c**
  - Relationship Type: Lookup
  - One job can receive applications from multiple students.

The **Application__c** object serves as a junction object, creating a many-to-many relationship between **Student__c** and **Job__c**.

---

## Why is Application__c Needed?

The **Application__c** object is required because:

- A student can apply for multiple jobs.
- A job can receive applications from multiple students.
- It stores application-specific information such as:
  - Application Status
  - Applied Date
- It enables the placement team to monitor the progress of every application independently.
- It simplifies reporting and tracking of placement activities.

Without **Application__c**, it would not be possible to efficiently manage the many-to-many relationship between students and job openings.

---


# Task 2: SOQL Practice

---

## 1. Retrieve All Students

```apex
List<Student__c> students = [
    SELECT Id, Name, Department__c, CGPA__c
    FROM Student__c
];

System.debug(students);
```

---

## 2. Retrieve Name, Department and CGPA

```apex
List<Student__c> students = [
    SELECT Name, Department__c, CGPA__c
    FROM Student__c
];

for (Student__c s : students) {
    System.debug('Name: ' + s.Name +
                 ', Department: ' + s.Department__c +
                 ', CGPA: ' + s.CGPA__c);
}
```

---

## 3. Retrieve Students with CGPA >= 8

```apex
List<Student__c> students = [
    SELECT Name, Department__c, CGPA__c
    FROM Student__c
    WHERE CGPA__c >= 8
];

System.debug(students);
```

---

## 4. Retrieve CSE Students with CGPA >= 8

```apex
List<Student__c> students = [
    SELECT Name, Department__c, CGPA__c
    FROM Student__c
    WHERE Department__c = 'CSE'
    AND CGPA__c >= 8
];

System.debug(students);
```

---

## 5. Retrieve Top 5 Students by CGPA

```apex
List<Student__c> topStudents = [
    SELECT Name, Department__c, CGPA__c
    FROM Student__c
    ORDER BY CGPA__c DESC
    LIMIT 5
];

System.debug(topStudents);
```

---

## 6. Retrieve Selected Applications

```apex
List<Application__c> applications = [
    SELECT Name,
           Student__r.Name,
           Job__r.Name,
           Status__c
    FROM Application__c
    WHERE Status__c = 'Selected'
];

System.debug(applications);
```

---

## 7. Retrieve Applications with Student and Job Details

```apex
List<Application__c> applications = [
    SELECT Name,
           Student__r.Name,
           Student__r.Department__c,
           Job__r.Name,
           Job__r.Company__c,
           Status__c
    FROM Application__c
];

for (Application__c app : applications) {
    System.debug(
        'Student: ' + app.Student__r.Name +
        ', Department: ' + app.Student__r.Department__c +
        ', Job: ' + app.Job__r.Name +
        ', Company: ' + app.Job__r.Company__c +
        ', Status: ' + app.Status__c
    );
}
```

---

## 8. Count Applications for Each Job

```apex
AggregateResult[] results = [
    SELECT Job__r.Name jobName,
           COUNT(Id) total
    FROM Application__c
    GROUP BY Job__r.Name
];

for (AggregateResult ar : results) {
    System.debug(
        'Job: ' + ar.get('jobName') +
        ', Applications: ' + ar.get('total')
    );
}
```

---

# Task 3: Apex Class – PlacementService.cls

## Objective

Create an Apex service class named **PlacementService** to manage student eligibility and job applications for the Placement Management System.

---

## Apex Class: PlacementService.cls

```apex
public with sharing class PlacementService {

    // Returns students whose CGPA is greater than or equal to the given value
    public static List<Student__c> getEligibleStudents(Decimal minimumCGPA) {
        return [
            SELECT Id, Name, Department__c, CGPA__c
            FROM Student__c
            WHERE CGPA__c >= :minimumCGPA
        ];
    }

    // Returns students belonging to a specific department
    public static List<Student__c> getStudentsByDepartment(String department) {
        return [
            SELECT Id, Name, Department__c, CGPA__c
            FROM Student__c
            WHERE Department__c = :department
        ];
    }

    // Creates a new job application for a student
    public static Application__c createApplication(Id studentId, Id jobId) {

        Application__c app = new Application__c(
            Student__c = studentId,
            Job__c = jobId,
            Status__c = 'Applied',
            Applied_Date__c = Date.today()
        );

        insert app;
        return app;
    }
}
```

---

## Methods

## Test 1 – Get Eligible Students

Retrieves all students whose CGPA is **greater than or equal to 8.0**.

```apex
List<Student__c> students = PlacementService.getEligibleStudents(8.0);
System.debug(students);
```
- **Test 1:** Displays all students with CGPA **≥ 8.0**.
  
---

## Test 2 – Get Students by Department

Retrieves all students belonging to the **CSE** department.

```apex
List<Student__c> students = PlacementService.getStudentsByDepartment('CSE');
System.debug(students);
```
- **Test 2:** Displays all students in the **CSE** department.

---

## Test 3 – Create a Job Application

Creates a new application for a student.

> **Note:** Replace the placeholder IDs with actual **Student__c** and **Job__c** record IDs from your Salesforce org.

```apex
Id studentId = 'PASTE_STUDENT_RECORD_ID';
Id jobId = 'PASTE_JOB_RECORD_ID';

Application__c app = PlacementService.createApplication(studentId, jobId);

System.debug(app);
```
- **Test 3:** Creates a new **Application__c** record with:
## Execute Anonymous Code example

```apex
Id studentId = 'a06dL00000Qv70zQAB';
Id jobId = 'a08dL00000fac8vQAA';

Application__c app =
    PlacementService.createApplication(studentId, jobId);

System.debug(app);
```
---
# Task 4: Trigger Challenge

## Objective

Create a **before insert** trigger on **Application__c** to prevent students from applying for a job if their **CGPA is less than the Job's Minimum CGPA**.

---

## Trigger: ApplicationTrigger.trigger

```apex
trigger ApplicationTrigger on Application__c (before insert) {

    // Collect Student and Job IDs
    Set<Id> studentIds = new Set<Id>();
    Set<Id> jobIds = new Set<Id>();

    for (Application__c app : Trigger.new) {
        if (app.Student__c != null)
            studentIds.add(app.Student__c);

        if (app.Job__c != null)
            jobIds.add(app.Job__c);
    }

    // Fetch Students
    Map<Id, Student__c> studentMap = new Map<Id, Student__c>(
        [SELECT Id, Name, CGPA__c
         FROM Student__c
         WHERE Id IN :studentIds]
    );

    // Fetch Jobs
    Map<Id, Job__c> jobMap = new Map<Id, Job__c>(
        [SELECT Id, Name, Minimum_CGPA__c
         FROM Job__c
         WHERE Id IN :jobIds]
    );

    // Validate Eligibility
    for (Application__c app : Trigger.new) {

        Student__c student = studentMap.get(app.Student__c);
        Job__c job = jobMap.get(app.Job__c);

        if (student != null && job != null &&
            student.CGPA__c < job.Minimum_CGPA__c) {

            app.addError(
                'Application cannot be created. Student CGPA is below the minimum CGPA required for this job.'
            );
        }
    }
}
```

---

## How It Works

1. Collects all **Student** and **Job** record IDs.
2. Retrieves Student and Job records using SOQL.
3. Compares:
   - **Student CGPA**
   - **Job Minimum CGPA**
4. If the student's CGPA is lower than the required minimum, the trigger prevents the record from being inserted using `addError()`.

---
