# Day 3 Assignment – Validation Rules, Flows & Triggers 

## Part 1 – Interview Warm-up

### 1. What is a Validation Rule?
A Validation Rule is a Salesforce feature that checks whether the data entered into a record meets specific conditions before it is saved. If the data does not satisfy the rule, Salesforce displays an error message and prevents the record from being saved.

---

### 2. What is a Flow?
A Flow is a declarative automation tool in Salesforce that allows users to automate business processes without writing code. It can create, update, delete records, send emails, and perform various actions based on defined logic.

---

### 3. What is an Apex Trigger?
An Apex Trigger is a piece of Apex code that executes automatically before or after events such as insert, update, delete, or undelete on Salesforce records. Triggers are used when complex business logic cannot be achieved using declarative tools.

---

### 4. When would you choose a Flow instead of a Trigger?
I would choose a Flow when the required automation can be implemented without code. Flows are easier to build, maintain, and modify, making them the preferred choice for most standard business automation. Triggers are used only when advanced logic or complex processing is required.

---

### 5. Can a Validation Rule update another field? Why or why not?
No. A Validation Rule cannot update another field because its purpose is only to validate data before saving a record. It can either allow the save operation or display an error message if the validation fails.

---

### 6. Which executes first: Validation Rule, Flow, or Trigger?
The execution order is:

1. **Before-Save Record-Triggered Flow**
2. **Before Apex Trigger**
3. **Validation Rule**
4. **After Apex Trigger**
5. **After-Save Record-Triggered Flow**

---

### 7. What is a Record-Triggered Flow?
A Record-Triggered Flow is a type of Salesforce Flow that automatically runs when a record is created, updated, or deleted. It is commonly used to automate tasks such as updating fields, creating related records, sending emails, and performing other business processes without writing Apex code.

----

## Part 2 – Business Scenario

### Scenario
As a Salesforce Developer, I was asked to automate the Placement Management System for the Placement Cell. The objective is to reduce manual work, improve data quality, and streamline the placement process using Salesforce automation tools.

---

### Business Requirements and Solutions

#### 1. Send an Email When a Student Submits an Application

**Requirement:**
Whenever a student submits a new application, an email should be sent to the Placement Officer.

**Solution:**
- Create an **After-Save Record-Triggered Flow** on the **Application** object.
- Configure the flow to run when a new Application record is created.
- Use the **Send Email** action to send a confirmation email to the Placement Officer.

**Expected Result:**
Every new application automatically triggers an email notification.

---

#### 2. Automatically Populate the Application Date

**Requirement:**
The **Application Date** should be filled automatically when an application is created.

**Solution:**
- Create a **Before-Save Record-Triggered Flow** on the **Application** object.
- Set the **Application Date** field to **$Flow.CurrentDate**.

**Expected Result:**
The Application Date is automatically populated without manual entry.

---

#### 3. Prevent Duplicate Applications

**Requirement:**
A student should not be able to apply for the same job more than once.

**Solution:**
- Create a **Validation Rule** that checks whether an application already exists for the same Student and Job combination.
- If a duplicate application is detected, display an error message and prevent the record from being saved.

**Expected Result:**
Duplicate applications are not allowed.

---

#### 4. Reject Applications with Low CGPA

**Requirement:**
If a student's CGPA is below the minimum CGPA required for the job, the application should not be accepted.

**Solution:**
- Create a **Validation Rule** that compares the student's CGPA with the Job's Minimum CGPA.
- If the student's CGPA is less than the required value, display an error message and stop the save operation.

**Expected Result:**
Only eligible students can submit applications.

---

#### 5. Automatically Create an Offer Letter

**Requirement:**
Whenever the Application Status changes to **Selected**, an Offer Letter record should be created automatically.

**Solution:**
- Create an **After-Save Record-Triggered Flow** on the **Application** object.
- Configure the flow to run only when the **Status** field changes to **Selected**.
- Use the **Create Records** element to create a related **Offer Letter** record.

**Expected Result:**
An Offer Letter is automatically generated for every selected candidate.

---

## Automation Summary

### Before-Save Flow
- Automatically populate **Application Date**.

### After-Save Flow
- Send confirmation email to the Placement Officer.
- Create an **Offer Letter** record when Status becomes **Selected**.

### Validation Rules
- Prevent duplicate applications.
- Ensure the student's CGPA meets the minimum job requirement.

---

## Outcome
By implementing these automations, the Placement Management System becomes more efficient, reduces manual effort, maintains data accuracy, prevents invalid applications, and automatically handles notifications and offer letter creation.

----

## Part 3 – Design Challenge

### Solution Selection

| Requirement | Validation Rule | Flow | Trigger | Why? |
|-------------|-----------------|------|---------|------|
| Reject duplicate applications | Yes | No | No | A Validation Rule prevents users from saving duplicate application records and ensures data quality before the record is saved. |
| Auto-fill Application Date | No | Yes | No | A Before-Save Record-Triggered Flow automatically populates the Application Date when the record is created without writing code. |
| Send Email | No | Yes | No | An After-Save Record-Triggered Flow can send an email notification after a new application is successfully created. |
| Reject low CGPA | Yes | No | No | A Validation Rule checks whether the student's CGPA meets the minimum requirement and prevents saving invalid records. |
| Create Offer Letter record | No | Yes | No | An After-Save Record-Triggered Flow automatically creates an Offer Letter record when the Application Status changes to **Selected**. |

---

## Explanation

### Reject Duplicate Applications
A **Validation Rule** is the best choice because it stops duplicate records before they are saved, ensuring data integrity.

### Auto-fill Application Date
A **Before-Save Record-Triggered Flow** is ideal because it updates field values efficiently before the record is saved.

### Send Email
An **After-Save Record-Triggered Flow** is appropriate because email notifications should only be sent after the application record has been successfully created.

### Reject Low CGPA
A **Validation Rule** is suitable because it validates the student's CGPA against the job's minimum CGPA and prevents invalid applications from being saved.

### Create Offer Letter Record
An **After-Save Record-Triggered Flow** is the recommended solution because it automatically creates a related Offer Letter record when the application status changes to **Selected**.

---

## Conclusion

For this Placement Management System:

- **Validation Rules** are used to enforce data quality and prevent invalid records.
- **Record-Triggered Flows** are used to automate business processes such as updating fields, sending emails, and creating related records.
- **Apex Triggers** are not required because all the given requirements can be implemented using Salesforce's declarative automation tools.

----

## Part 5 – Validation Rule Challenge

### Objective
Create Validation Rules to ensure data quality in the Placement Management System.

---

## Validation Rule 1 – Student CGPA Validation

### Requirement
The student's CGPA must be greater than or equal to the Job's Minimum CGPA.

### Formula

```text
Student__r.CGPA__c < Job__r.Minimum_CGPA__c
```

### Expected Result
If the student's CGPA is lower than the required minimum CGPA, Salesforce prevents the Application record from being saved.

---

# Validation Rule 2 – Application Date Validation

## Requirement
The Application Date cannot be after the Job Closing Date.

### Formula

```text
Application_Date__c > Job__r.Closing_Date__c
```

### Expected Result
If the Application Date is later than the Job Closing Date, Salesforce displays an error and does not save the record.

---

# Validation Rule 3 – Mandatory Fields Validation

## Requirement
Student, Job, and Status fields cannot be left blank.

### Formula

```text
OR(
ISBLANK(Student__c),
ISBLANK(Job__c),
ISBLANK(TEXT(Status__c))
)
```

### Expected Result
The record cannot be saved until all mandatory fields are completed.

---

# Summary

| Validation Rule | Formula |
|-----------------|---------|
| Student CGPA Validation | `Student__r.CGPA__c < Job__r.Minimum_CGPA__c` |
| Application Date Validation | `Application_Date__c > Job__r.Closing_Date__c` |
| Mandatory Fields Validation | `OR(ISBLANK(Student__c), ISBLANK(Job__c), ISBLANK(TEXT(Status__c)))` |

---

## Outcome

These Validation Rules improve the quality of data in the Placement Management System by:
- Ensuring students meet the minimum CGPA requirement.
- Preventing applications after the job closing date.
- Enforcing completion of mandatory fields before saving records.

----

## Part 6 – Trigger vs Flow Debate

## Objective
Choose the most appropriate Salesforce automation tool (Flow or Apex Trigger) for each scenario and explain the reason.

---

## 1. Update a Field Automatically

**Choice:**  Flow

**Explanation:**
A **Before-Save Record-Triggered Flow** is the best choice for updating field values automatically because it is fast, efficient, and requires no Apex code.

---

## 2. Create a Related Record

**Choice:**  Flow

**Explanation:**
An **After-Save Record-Triggered Flow** can easily create related records, such as an Offer Letter when an Application Status changes to **Selected**. It is simple to configure and maintain.

---

## 3. Send an Email Notification

**Choice:** Flow

**Explanation:**
An **After-Save Record-Triggered Flow** can send email notifications after a record is successfully created or updated. This is the recommended declarative approach for standard email automation.

---

## 4. Call an External REST API

**Choice:** Trigger

**Explanation:**
Calling an external REST API usually requires Apex callouts, authentication, and error handling. An **Apex Trigger** (typically combined with asynchronous Apex such as Queueable or Future methods) is more suitable for this type of integration.

---

## 5. Perform Complex Calculations Involving Multiple Objects

**Choice:**  Trigger

**Explanation:**
When business logic involves multiple related objects, advanced calculations, or complex processing, an **Apex Trigger** provides greater flexibility and control than Flow.

---

## 6. Process 10,000 Imported Records

**Choice:** Trigger

**Explanation:**
For processing a large number of records, **Apex Triggers** are better because they are bulkified and can efficiently handle high-volume data operations while respecting Salesforce governor limits.

---
## Part 8 – Debugging Challenge

## Scenario
A developer created the following automation:

- Apex Trigger updates the **Status** field.
- Record-Triggered Flow updates the **Status** field.
- Workflow Rule updates the **Status** field.

---

## 1. What problem might occur?

When multiple automation tools update the same **Status** field, they can conflict with each other. One automation may overwrite another, causing unexpected results, making the process difficult to debug and maintain.

---

## 2. Could automation repeatedly execute?

**Yes.**

If one automation updates the **Status** field, it can trigger the others again. This may lead to repeated execution, unnecessary processing, or even an infinite loop (recursion), depending on the implementation.

---

## 3. How would you redesign this solution?

I would redesign the solution by following Salesforce best practices:

- Use **one primary automation tool** to update the **Status** field instead of multiple tools.
- Prefer a **Record-Triggered Flow** for standard business automation because it is declarative, easier to maintain, and recommended by Salesforce.
- Use an **Apex Trigger** only if the business logic is too complex to implement with Flow.
- Remove the **Workflow Rule**, as Workflow Rules are being replaced by Flow in Salesforce.
- Ensure only one automation is responsible for updating the **Status** field to avoid conflicts and repeated execution.

---

## Conclusion

Using multiple automation tools on the same field can cause conflicts, repeated execution, and maintenance issues. A cleaner design is to use a single automation tool—preferably a **Record-Triggered Flow** for standard automation or an **Apex Trigger** only when advanced logic is required.

---- 

## Part 9 – Interview Questions

## 1. What is the difference between Workflow, Process Builder, and Flow?

- **Workflow Rules** are used for simple automation such as field updates, email alerts, and task creation.
- **Process Builder** supports more advanced automation than Workflow, including multiple criteria and creating related records.
- **Flow** is the most powerful declarative automation tool. It can update records, create records, delete records, send emails, perform calculations, call Apex, and automate complex business processes. Salesforce recommends using **Flow** for new automation.

---

## 2. Why is Flow replacing Workflow Rules?

Flow is replacing Workflow Rules because it offers more features, greater flexibility, better performance, and supports almost every type of automation. Instead of maintaining multiple automation tools, Salesforce now recommends using Flow as the single automation solution.

---

## 3. What is a Record-Triggered Flow?

A Record-Triggered Flow is a Flow that automatically runs when a record is created, updated, or deleted. It is commonly used to automate business processes without writing Apex code.

---

## 4. What are Before-Save and After-Save Flows?

- **Before-Save Flow** runs before the record is saved. It is mainly used to update fields quickly and efficiently.
- **After-Save Flow** runs after the record is saved. It is used for actions such as creating related records, sending emails, posting notifications, and updating related objects.

---

## 5. When should Apex be preferred over Flow?

Apex should be preferred when:
- Complex business logic is required.
- Calling external REST or SOAP APIs.
- Performing advanced calculations.
- Handling very large data volumes.
- Implementing functionality that Flow cannot support.

---

## 6. Can Flow call Apex?

**Yes.**

A Flow can call Apex by using an **Invocable Apex Method**, allowing developers to combine declarative automation with custom Apex logic when needed.

---

## 7. What are the advantages of declarative automation?

- No coding required.
- Faster development.
- Easy to maintain.
- Easier for administrators to update.
- Reduces development time.
- Follows Salesforce best practices.

---

## 8. Explain one Flow that you built.

I built a **Before-Save Record-Triggered Flow** on the **Application** object. Whenever a new application is created, the flow automatically sets the **Application Date** to the current date. This eliminates manual data entry and ensures every application has the correct submission date.

I also created an **After-Save Record-Triggered Flow** that sends a confirmation email to the Placement Officer whenever a new application is submitted.

---

## 9. Explain one Validation Rule that you created.

I created a Validation Rule to ensure that a student's **CGPA** is greater than or equal to the minimum CGPA required for the selected job.

**Formula:**

```text
Student__r.CGPA__c < Job__r.Minimum_CGPA__c
```

If the student's CGPA is below the required value, Salesforce displays an error message and prevents the application from being saved.

---

## 10. If given the choice, why did you use Flow instead of Apex?

I chose **Flow** because it can implement the required business automation without writing code. Flow is easier to build, maintain, and modify, making it the recommended Salesforce automation tool. Apex is used only when the automation requires complex business logic or features that Flow cannot provide.

---

## Conclusion

This assignment demonstrates an understanding of Salesforce automation tools, including Validation Rules, Record-Triggered Flows, and Apex. It also highlights when to use declarative automation versus programmatic automation based on business requirements.

----

# Today's Outcome

Today, I successfully completed the Salesforce automation tasks for the Placement Management System. I learned how to use **Validation Rules** to maintain data quality by preventing duplicate applications, validating student CGPA, checking application dates, and enforcing mandatory fields. I also implemented **Record-Triggered Flows** to automatically populate the Application Date, send email notifications to the Placement Officer, and create Offer Letter records when a candidate is selected.

In addition, I analyzed different business scenarios to determine when to use **Validation Rules, Flows, or Apex Triggers**, understood the differences between declarative and programmatic automation, explored Salesforce's order of execution, and prepared answers to common Salesforce interview questions related to automation. This hands-on practice improved my understanding of Salesforce automation and reinforced best practices for designing efficient and maintainable solutions.
