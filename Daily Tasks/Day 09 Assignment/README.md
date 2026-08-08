# Placement Management System — README.md

````markdown
# Placement Management System

A Salesforce-based Placement Management System built using **Lightning Web Components (LWC), Apex, SOQL, and Salesforce custom objects**.

The system allows students to view eligible job opportunities and apply for jobs while enforcing placement eligibility and application rules.

---

## 🚀 Project Overview

The Placement Management System provides a centralized platform for managing:

- Student information
- Job opportunities
- Job eligibility
- Student applications
- Application status
- Placement statistics
- Lightning Web Component communication
- Apex business logic

The project demonstrates how Salesforce LWC components communicate with Apex controllers and services to implement a real-world placement workflow.

---

## 🛠️ Technologies Used

- Salesforce
- Lightning Web Components (LWC)
- Apex
- SOQL
- Salesforce Custom Objects
- Salesforce Lightning App Builder
- VS Code
- Salesforce CLI / SFDX

---

## 📁 Project Structure

```text
force-app/
└── main/
    └── default/
        ├── classes/
        │   ├── ApplicationController.cls
        │   ├── ApplicationService.cls
        │   ├── PlacementService.cls
        │   ├── StudentController.cls
        │   └── StatisticsService.cls
        │
        └── lwc/
            ├── applicationStatus/
            │   ├── applicationStatus.html
            │   ├── applicationStatus.js
            │   └── applicationStatus.js-meta.xml
            │
            ├── eligibleJobs/
            │   ├── eligibleJobs.html
            │   └── eligibleJobs.js
            │
            ├── jobCard/
            │   ├── jobCard.html
            │   └── jobCard.js
            │
            ├── parentComponent/
            │   ├── parentComponent.html
            │   └── parentComponent.js
            │
            └── childComponent/
                ├── childComponent.html
                └── childComponent.js
````

---

## 🗂️ Main Salesforce Objects

### Student__c

Stores student information.

Important fields:

* Name
* Department__c
* CGPA__c

### Job__c

Stores available job opportunities.

Important fields:

* Name
* Company_Name__c
* Minimum_Gpa__c
* Eligible_Department__c
* Allowed_Backlogs__c
* Closing_Date__c
* Requirements__c

### Application__c

Stores student job applications.

Important fields:

* Student__c
* Job__c
* Status__c
* Applied_date__c

---

# ⚙️ Apex Implementation

## ApplicationController

`ApplicationController` acts as the Apex controller called from the LWC.

Responsibilities:

* Validate Student Id
* Validate Job Id
* Create an Application record
* Set application status to `Applied`
* Set application date

Main method:

```apex
submitApplication(Id jobId, Id studentId)
```

---

## ApplicationService

`ApplicationService` contains application-related business logic.

Responsibilities:

* Validate application fields
* Check for existing applications
* Create applications
* Prevent invalid application requests

Main method:

```apex
applyForJob(Id studentId, Id jobId)
```

The service checks whether an application already exists for the same student and job.

---

## StudentController

`StudentController` retrieves student information using SOQL.

The controller retrieves:

```text
Id
Name
Department__c
CGPA__c
```

---

## PlacementService

`PlacementService` contains the placement workflow logic.

Implemented operations include:

### Student Retrieval

Retrieves the student record required for eligibility checking.

### Job Retrieval

Retrieves job information such as:

* Minimum GPA
* Eligible Department

### Duplicate Application Check

Checks whether a student has already applied for a particular job.

### Application Creation

Creates a new `Application__c` record.

### Application Status Update

Updates the application status.

Supported statuses include the statuses configured in the Salesforce application workflow, such as:

```text
Applied
Shortlisted
Interview Scheduled
Selected
Rejected
```

---

## StatisticsService

`StatisticsService` contains logic related to placement statistics.

When an application reaches the `Selected` status, the service can be used to process placement statistics.

---

# 💻 Lightning Web Components

## Eligible Jobs

The `eligibleJobs` component retrieves available jobs using:

```javascript
@wire(getEligibleJobs)
```

Each job is passed to the `jobCard` component.

---

## Job Card

The `jobCard` component displays job information including:

* Company
* Department
* Minimum CGPA
* Allowed Backlogs
* Closing Date
* Requirements

It also contains the Application Status component.

The Job Id is passed to the child component:

```html
<c-application-status
    job-id={job.Id}
    student-id="STUDENT_ID">
</c-application-status>
```

---

## Application Status

The `applicationStatus` component provides the Apply functionality.

Initial status:

```text
Not Applied
```

When the student successfully applies:

```text
Applied
```

The component calls:

```javascript
submitApplication()
```

from `ApplicationController`.

Errors returned from Apex are displayed to the user.

---

# 🔄 Application Flow

The application workflow is:

```text
Student
   ↓
Eligible Jobs
   ↓
Job Card
   ↓
Application Status
   ↓
Apply Button
   ↓
ApplicationController
   ↓
ApplicationService
   ↓
Eligibility / Validation
   ↓
Application__c
   ↓
Status = Applied
```

---

# 🔐 Eligibility Validation

The system enforces job eligibility rules configured in Salesforce.

For example, if a job requires a minimum CGPA of `8.0` and the student's CGPA is `7.9`, the application is rejected.

Example validation message:

```text
Application Rejected!
Student CGPA is below the minimum requirement.
```

This validation is performed by the Salesforce business logic/validation configured for the application process.

---

# 🔁 Duplicate Application Handling

Before creating a new application, the service checks whether an application already exists for the same:

```text
Student + Job
```

This prevents duplicate applications from being processed as new applications.

---

# 🔗 Parent-Child Component Communication

The project also demonstrates communication between LWC components.

### Parent Component

Maintains:

```javascript
studentId
message
```

### Child Component

Receives the Student Id using:

```javascript
@api studentId;
```

The child retrieves student information using Apex and dispatches a custom event to the parent.

Example:

```javascript
const event = new CustomEvent('notify');
this.dispatchEvent(event);
```

The parent handles the event and updates its message.

---

# 🖥️ Placement Portal

The final Placement Portal page contains sections such as:

* Welcome Message
* Data Binding Demo
* Eligible Jobs
* Student Details
* Placement information

Example dashboard information:

```text
Today's Date
Number of Companies
Number of Jobs
Applications Submitted
```

---

# 🧪 Testing Performed

The following functionality was tested:

### Job Display

* Eligible jobs are displayed on the Placement Portal.
* Job details are displayed through Job Cards.

### Apply Functionality

* Apply button is displayed for each job.
* Job Id is passed from Job Card to Application Status.
* Application request reaches Apex.
* Application record is created for an eligible student.

### Eligibility Validation

Tested an ineligible student whose CGPA was below the job's minimum requirement.

The application was correctly rejected with:

```text
Application Rejected!
Student CGPA is below the minimum requirement.
```

After updating the student's CGPA to meet the requirement, the application could be submitted successfully.

### Deployment

LWC and Apex components were deployed successfully to the Salesforce org.

---

# 🐛 Debugging

Debugging was performed using:

* VS Code deployment output
* Salesforce Developer Console
* Execute Anonymous
* Browser Console
* Salesforce debug logs

During testing, issues involving:

* Missing Job Id
* Missing Student Id
* Invalid Salesforce Id
* Student eligibility
* LWC deployment errors

were identified and resolved.

---

# 📌 Key Learning Outcomes

This project demonstrates practical experience with:

* Apex classes
* Apex service classes
* Apex controllers
* SOQL queries
* DML operations
* Salesforce custom objects
* Lightning Web Components
* `@api`
* `@wire`
* Apex-to-LWC communication
* Parent-child LWC communication
* Custom events
* Lightning App Builder
* Application validation
* Duplicate application handling
* Debugging and deployment

---

# ▶️ How to Run

1. Open the Salesforce project in VS Code.
2. Authenticate the Salesforce org.
3. Deploy the project to the org.
4. Open Salesforce Lightning App Builder.
5. Open the `Placement_Home` page.
6. Verify the required LWC components are placed on the page.
7. Save and activate the Lightning page.
8. Open the Placement Portal.
9. Verify eligible jobs are displayed.
10. Select an eligible job and click **Apply**.
11. Verify that the Application record is created with status `Applied`.

---

# 📄 Project Status

**Status: Completed and tested**

The main Placement Portal functionality, eligible job display, application workflow, Apex services, LWC components, validation, and deployment have been implemented and tested.

---

## 👩‍💻 Author

**Mounisha Savaram**

Salesforce Developer Bridge Program

**Project:** Placement Management System

```
```

