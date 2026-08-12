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
