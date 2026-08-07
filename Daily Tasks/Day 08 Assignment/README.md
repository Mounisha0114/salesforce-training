# Engineering Sprint 8 – Designing Asynchronous Workflows That Remain Reliable

## Objective

The objective of this sprint is to understand and implement asynchronous processing in Salesforce by using Queueable Apex, Queueable Chaining, Batch Apex, and Scheduled Apex. The goal is to separate immediate business operations from background processing while building scalable, maintainable, and reliable applications.

---

# Tasks Completed

## Task 1 – Queueable Apex

Created a Queueable Apex class named **OfferPostProcessingJob**.

### Implemented

- Accepted Offer Letter Id through the constructor.
- Retrieved the Offer Letter record using SOQL.
- Executed background processing.
- Simulated External Synchronization.
- Simulated Notification Processing.
- Simulated Analytics Processing.

### Verified Using

- Developer Console Debug Logs
- Setup → Apex Jobs

---

## Task 2 – Queueable Chaining

Created two Queueable Apex classes:

- ExternalPlacementSyncJob
- PlacementNotificationJob

### Implemented

- Executed External Synchronization.
- Started PlacementNotificationJob after successful completion.
- Demonstrated Queueable Chaining.

### Verified Using

- Developer Console Debug Logs
- Setup → Apex Jobs

---

## Task 3 – Batch Apex

Created Batch Apex class:

**PlacementCategoryBatch**

### Implemented

- start()
- execute()
- finish()

### Executed Using

- Execute Anonymous Window

### Verified Using

- Setup → Apex Jobs
- Developer Console Debug Logs

---

## Task 4 – Scheduled Apex

Created Scheduled Apex class:

**ExpiredJobScheduler**

### Implemented

- Scheduled Batch Apex execution using a Cron Expression.
- Automatically executed the Batch Apex job.

### Verified Using

- Setup → Scheduled Jobs
- Setup → Apex Jobs

---

# Execution Flow

```
Student Accepts Offer Letter
          │
          ▼
Synchronous Processing
          │
          ▼
Queueable Apex
          │
          ▼
External Synchronization
          │
          ▼
Queueable Chaining
          │
          ▼
Notification Processing
          │
          ▼
Analytics Processing
          │
          ▼
Batch Apex
          │
          ▼
Scheduled Apex
```

---

# Classes Created

- OfferPostProcessingJob
- ExternalPlacementSyncJob
- PlacementNotificationJob
- PlacementCategoryBatch
- ExpiredJobScheduler

---

# Result Verification

The implemented classes were executed successfully using the Execute Anonymous Window.

Execution was verified through:

- Developer Console Debug Logs
- Setup → Apex Jobs
- Setup → Scheduled Jobs

---

# Architecture Review Answers

### 1. Immediate Validation using Batch Apex

Not recommended because validation should occur immediately during the synchronous transaction.

### 2. Processing 300,000 Records using Future Method

Not suitable because Future Methods are not intended for processing very large datasets.

Batch Apex is the appropriate solution.

### 3. Scheduled Apex directly processing a huge dataset

Not recommended.

Scheduled Apex should start a Batch Apex job instead of processing all records directly.

### 4. Queueable Job performing multiple responsibilities

This violates the Single Responsibility Principle.

Each Queueable class should perform only one responsibility.

### 5. Moving inefficient synchronous code into Queueable Apex

Moving inefficient code into Queueable Apex does not automatically improve performance.

The underlying business logic should also be optimized.

---

# Architecture Challenge Solution

```
User
 │
 ▼
Synchronous Transaction
 │
 ▼
Queueable Apex
 │
 ▼
Queueable Chaining
 │
 ▼
Scheduled Apex
 │
 ▼
Batch Apex
 │
 ▼
Business Logic
```

This architecture separates immediate user operations from background processing, making the application scalable, maintainable, and efficient.

---

# Interview Questions and Answers

### What is Asynchronous Apex?

Asynchronous Apex executes code in the background without making users wait.

### When should processing remain synchronous?

When immediate validation or an immediate response is required.

### Why use Queueable Apex?

- Structured background processing
- Supports Queueable Chaining
- Better monitoring and flexibility than Future Methods

### When should Batch Apex be used?

When processing very large datasets.

### Methods in Batch Apex

- start()
- execute()
- finish()

### What is Scheduled Apex?

Scheduled Apex executes jobs automatically at a specified time.

### Can Scheduled Apex and Batch Apex work together?

Yes.

Scheduled Apex can initiate a Batch Apex job.

### Does Asynchronous Apex remove Governor Limits?

No.

Governor Limits still apply.

### Why should Batch Apex be bulkified?

To process records efficiently while remaining within Governor Limits.

### What happens if an asynchronous job fails?

The failure can be investigated using Apex Jobs and Debug Logs.

### What is Queueable Chaining?

Queueable Chaining allows one Queueable job to enqueue another Queueable job after successful completion.

### Before moving work to asynchronous processing, what should be considered?

- Business requirement
- Processing time
- Failure handling
- Monitoring
- Duplicate execution

---

# Sprint Retrospective

## Hardest Concept

Queueable Chaining because it requires understanding execution order.

## Most Important Learning

Knowing **when** to use Queueable Apex is more important than simply knowing how to write it.

## Governor Limits

Governor Limits continue to apply in asynchronous processing.

## New Challenges in Background Processing

- Monitoring
- Job failures
- Duplicate execution
- Execution order
- Retry handling

## Activities That Can Move to Background Processing

- External System Synchronization
- Notification Processing
- Analytics Processing

---

# Conclusion

In this sprint, asynchronous processing was implemented using Queueable Apex, Queueable Chaining, Batch Apex, and Scheduled Apex. Different execution models were selected based on business requirements, and the execution was successfully verified using Salesforce monitoring tools. The sprint demonstrated how asynchronous processing improves scalability, maintainability, and overall application performance.

---

---

# Submission Details

- **Name:** Mounisha Savaram
- **Topic:** Engineering Sprint 8 – Designing Asynchronous Workflows That Remain Reliable
- **Sprint:** Engineering Sprint 8
- **Submitted On:** 06-Aug-2026
