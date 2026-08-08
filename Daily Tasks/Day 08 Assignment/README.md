# Placement Management System – Advanced Apex Automation

## 📌 Project Overview

This project is a **Salesforce Placement Management System** developed using **Apex Triggers, Handler Classes, Service Classes, Queueable Apex, Batch Apex, and Scheduled Apex**.

The system automates important placement activities such as application validation, placement notifications, offer post-processing, external synchronization, and placement-category processing.

---

## 🏗️ Salesforce Objects

The project uses the following custom objects:

* `Student__c`
* `Job__c`
* `Application__c`

> **Note:** An `Offer_Letter__c` object was not created. Offer processing is handled through Apex asynchronous processing.

---

## ⚙️ Main Components

### 1. Application Trigger

**Trigger:** `ApplicationTrigger`

The trigger handles:

* Before Insert
* Before Update

It delegates the actual business logic to the trigger handler instead of putting logic directly inside the trigger.

```apex
trigger ApplicationTrigger on Application__c (before insert, before update, after update) {

    if (Trigger.isBefore && Trigger.isInsert) {
        ApplicationTriggerHandler.handleBeforeInsert(Trigger.new);
    }

    if (Trigger.isBefore && Trigger.isUpdate) {
        ApplicationTriggerHandler.handleBeforeUpdate(
            Trigger.new,
            Trigger.oldMap
        );
    }

    if (Trigger.isAfter && Trigger.isUpdate) {
        ApplicationTriggerHandler.handleAfterUpdate(
            Trigger.new,
            Trigger.oldMap
        );
    }
}
```

---

## 2. ApplicationTriggerHandler

**Class:** `ApplicationTriggerHandler`

The handler separates trigger logic from business logic.

### Methods

* `handleBeforeInsert()`
* `handleBeforeUpdate()`
* `handleAfterUpdate()`

The handler calls the appropriate service classes and asynchronous jobs.

---

## 3. ApplicationService

**Class:** `ApplicationService`

This service validates student applications.

### Validations

#### Student Validation

Checks whether a Student is associated with the Application.

#### Job Validation

Checks whether a Job is associated with the Application.

#### CGPA Validation

The student's CGPA is compared with the minimum CGPA required for the Job.

```text
Student CGPA < Job Minimum GPA
        ↓
Application rejected
```

#### Backlog Validation

The student's active backlogs are compared with the number of backlogs allowed for the Job.

```text
Student Backlogs > Allowed Backlogs
        ↓
Application rejected
```

#### Department Validation

The student's department is checked against the eligible department specified for the Job.

---

## 4. Queueable Apex

The project uses Queueable Apex for asynchronous processing.

### `PlacementNotificationJob`

Used for placement-related notification processing.

### `ExternalPlacementSyncJob`

Used for external placement synchronization processing.

### `OfferPostProcessingJob`

Executed when an Application status changes to **Selected**.

The job performs post-processing asynchronously instead of doing additional processing directly inside the trigger transaction.

### Verification

The jobs were successfully executed and appeared as:

```text
PlacementNotificationJob     → Completed
ExternalPlacementSyncJob     → Completed
OfferPostProcessingJob       → Completed
```

---

## 5. Batch Apex

### `PlacementCategoryBatch`

The Batch Apex class processes placement records in batches.

It is useful when processing a larger number of records while respecting Salesforce governor limits.

### Execution Result

The batch job was successfully executed:

```text
Status: Completed
Processed: 1
Errors: 0
```

---

## 6. Scheduled Apex

### `PlacementCategoryScheduler`

The scheduler is responsible for running the placement-category batch process on a scheduled basis.

The scheduled job created for the project is:

```text
Placement Category Daily Job
```

The test scheduled job was removed after verification.

### Current Scheduled Job

```text
Placement Category Daily Job
Type: Scheduled Apex
Status: Scheduled
```

---

## 🔄 Automation Flow

```text
Application Created
        |
        v
ApplicationTrigger
        |
        v
ApplicationTriggerHandler
        |
        v
ApplicationService
        |
        +----> CGPA Validation
        |
        +----> Backlog Validation
        |
        +----> Department Validation
        |
        v
Application Updated
        |
        +----> Placement Notification
        |
        +----> External Sync
        |
        +----> Selected Status
                    |
                    v
          OfferPostProcessingJob
```

### Scheduled Processing

```text
PlacementCategoryScheduler
        |
        v
PlacementCategoryBatch
        |
        v
Placement Category Processing
```

---

## 🛡️ Bulkification

The Apex code follows bulk-processing practices.

Instead of performing SOQL queries inside loops:

* Student IDs are collected into a `Set<Id>`.
* Job IDs are collected into a `Set<Id>`.
* Students are queried once.
* Jobs are queried once.
* Maps are used to access records efficiently.

Example:

```apex
Set<Id> studentIds = new Set<Id>();
Set<Id> jobIds = new Set<Id>();
```

This approach helps prevent Salesforce governor-limit errors such as:

```text
Too many SOQL queries: 101
```

---

## 📊 Asynchronous Apex Verification

The following jobs were verified through **Setup → Apex Jobs**.

| Apex Job                     | Type           | Result    |
| ---------------------------- | -------------- | --------- |
| `PlacementNotificationJob`   | Queueable      | Completed |
| `ExternalPlacementSyncJob`   | Queueable      | Completed |
| `OfferPostProcessingJob`     | Queueable      | Completed |
| `PlacementCategoryBatch`     | Batch Apex     | Completed |
| `PlacementCategoryScheduler` | Scheduled Apex | Scheduled |

---

## 🧪 Testing Performed

The following functionality was tested:

* Application validation
* CGPA eligibility validation
* Backlog eligibility validation
* Department eligibility validation
* Application update processing
* Placement notification processing
* External placement synchronization
* Offer post-processing
* Batch Apex execution
* Scheduled Apex execution
* Async Apex job monitoring

---

## 📁 Apex Components

```text
force-app/
└── main/
    └── default/
        ├── classes/
        │   ├── ApplicationService.cls
        │   ├── ApplicationTriggerHandler.cls
        │   ├── PlacementNotificationJob.cls
        │   ├── ExternalPlacementSyncJob.cls
        │   ├── OfferPostProcessingJob.cls
        │   ├── PlacementCategoryBatch.cls
        │   └── PlacementCategoryScheduler.cls
        │
        └── triggers/
            └── ApplicationTrigger.trigger
```

---

## 🎯 Key Salesforce Concepts Used

* Apex Classes
* Apex Triggers
* Trigger Handler Pattern
* Service Layer Pattern
* SOQL
* Collections
* Maps and Sets
* Bulkification
* Governor Limits
* Queueable Apex
* Batch Apex
* Scheduled Apex
* Asynchronous Processing
* `System.enqueueJob()`
* `Database.executeBatch()`
* `System.schedule()`
* Debug Logs
* Apex Jobs Monitoring

---

## ✅ Final Status

The advanced Apex automation was successfully implemented and tested.

### Completed

* ✅ Application validation
* ✅ Trigger handler architecture
* ✅ Queueable Apex
* ✅ Offer post-processing
* ✅ External placement synchronization
* ✅ Placement notifications
* ✅ Batch Apex
* ✅ Scheduled Apex
* ✅ Async Apex verification
* ✅ Daily scheduled job
* ✅ Test scheduled job removed

---

## 👩‍💻 Author

**Mounisha Rathnavalli Savaram**

**Salesforce Developer Training Project**
