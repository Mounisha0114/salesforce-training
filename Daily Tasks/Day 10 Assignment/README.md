# README.md

````markdown
# Placement Management System – Day 10

## Overview

Day 10 focuses on implementing the Student Profile and Eligible Jobs functionality in the Salesforce Placement Management System.

The system allows students to:

- View their profile
- Edit and save profile information
- Check eligible placement opportunities
- Apply for eligible jobs
- Prevent duplicate applications
- Automatically update eligible jobs when profile details change

---

## Student Profile

The Student Profile contains the following information:

- Student Name
- Department
- CGPA
- Email
- Phone
- Active Backlogs
- Skills
- Preferred Location

### Example Student

| Field | Value |
|---|---|
| Student Name | Ramya |
| Department | CSE |
| CGPA | 9.00 |
| Active Backlogs | 0 |

The student can click **Edit**, modify profile information, and click **Save**.

A success message is displayed after the profile is saved successfully.

---

## Job Eligibility

Jobs are displayed based on the student's profile.

A student is eligible when:

```text
Student CGPA >= Job Minimum CGPA
AND
Student Department = Job Eligible Department
AND
Student Active Backlogs <= Job Allowed Backlogs
AND
Job Closing Date >= Current Date
````

### Example

Student:

```text
Department = CSE
CGPA = 9.00
Active Backlogs = 0
```

Job:

```text
Job Title = SDE Developer
Company = Amazon
Eligible Department = CSE
Minimum CGPA = 8.00
Allowed Backlogs = 0
Closing Date = 21/08/2026
```

Since all eligibility conditions are satisfied, the job is displayed.

---

## GPA Eligibility Test

The eligibility system was tested by changing the student's CGPA.

### When CGPA is 7.00

The job requires a minimum CGPA of 8.00.

```text
7.00 < 8.00
```

Therefore:

```text
No eligible jobs available.
```

### When CGPA is changed back to 9.00

```text
9.00 >= 8.00
```

The eligible job appears again.

This confirms that GPA-based job eligibility is working correctly.

---

## Apply for Job

Eligible students can click the **Apply** button.

The application process is:

```text
Apply
   ↓
Submitting...
   ↓
Application submitted successfully
   ↓
Applied
```

An `Application__c` record is created with:

* Student
* Job
* Applied Date
* Status

The application status is set to:

```text
Applied
```

---

## Duplicate Application Prevention

Before creating an application, the system checks whether the student has already applied for the same job.

If an application already exists, the system returns:

```text
ALREADY_SUBMITTED
```

The user receives:

```text
Application already submitted for this job.
```

This prevents duplicate applications.

---

## Salesforce Components

### Lightning Web Components

```text
studentProfile
eligibleJobs
jobCard
```

### Apex Classes

```text
StudentProfileController
EligibleJobsController
```

### Salesforce Objects

```text
Student__c
Job__c
Application__c
```

### Lightning Message Channel

```text
StudentProfileUpdated__c
```

The Lightning Message Channel is used to notify the Eligible Jobs component when the student profile is updated.

---

## Project Flow

```text
Student Profile
       |
       v
Edit Profile
       |
       v
Save Profile
       |
       v
Profile Updated
       |
       v
Eligible Jobs Reload
       |
       v
Check Eligibility
       |
       v
Display Eligible Jobs
       |
       v
Apply for Job
       |
       v
Create Application
       |
       v
Application Status = Applied
```

---

## Testing Completed

| Functionality                    | Status    |
| -------------------------------- | --------- |
| Student profile display          | Completed |
| Edit student profile             | Completed |
| Save student profile             | Completed |
| GPA validation                   | Completed |
| Department eligibility           | Completed |
| CGPA eligibility                 | Completed |
| Backlog eligibility              | Completed |
| Closing date eligibility         | Completed |
| Eligible job display             | Completed |
| Dynamic GPA eligibility          | Completed |
| Job application                  | Completed |
| Duplicate application prevention | Completed |

---

## Screenshots

The following screenshots should be included as evidence for Day 10:

### 1. Student Profile

Shows the student's:

* Name
* Department
* CGPA
* Active Backlogs
* Email
* Phone

### 2. Job Record

Shows:

* Job Title
* Company
* Eligible Department
* Minimum CGPA
* Allowed Backlogs
* Closing Date

### 3. Eligible Jobs

Shows the eligible job with the **Apply** button.

### 4. No Eligible Jobs

Shows:

```text
No eligible jobs available.
```

when the student's CGPA is below the job's minimum CGPA.

### 5. Application Success

Shows the successful application and the **Applied** button.

---

## Day 10 Result

Day 10 successfully implements the core student placement workflow:

```text
Profile Management
        +
Eligibility Checking
        +
Dynamic Job Filtering
        +
Job Application
        +
Duplicate Prevention
```

The Placement Management System can now identify eligible jobs for a student and allow the student to apply for suitable placement opportunities.

```
```
