# Day 5 Apex Introduction

## What is Apex?

Apex is a server-side programming language used in Salesforce to write custom business logic.

It is object-oriented and similar to Java. Apex helps developers automate processes, perform calculations, and integrate Salesforce with external systems.

---

# Why Apex is Needed

Apex is required when:
- Business logic becomes complex
- Multiple objects are involved
- Large data processing is needed
- External system integration is required
- Advanced calculations are needed

Apex gives developers more flexibility and control compared to declarative tools.

---

# Flow vs Apex

| Feature | Flow | Apex |
|--------|------|------|
| Type | No-code (Declarative) | Code-based (Programmatic) |
| Ease of Use | Easy | Requires coding |
| Flexibility | Limited | Highly flexible |
| Performance | Medium | High |
| Complexity Handling | Basic to Medium | Advanced |
| Usage | Admin users | Developers |

---

# Configuration vs Coding

| Feature | Configuration | Coding (Apex) |
|--------|--------------|--------------|
| Type | Click-based setup | Written code |
| Skill Required | Admin skills | Programming skills |
| Flexibility | Limited | Fully flexible |
| Speed | Faster to build | Takes time but powerful |
| Maintenance | Easy | Requires developer |

---

# Real Use Cases of Apex

- Complex fee calculation
- Payment gateway integration
- Advanced eligibility validation
- Bulk student data processing
- Integration with external applications

---

# College Management System

## Objects
- Student__c
- Course__c
- Faculty__c
- Enrollment__c

---

## Relationships

- Student ↔ Course (Many-to-Many)
- Faculty → Course (Lookup Relationship)

---

## Validation Rules

### 1. Email is Required
Prevents incomplete student records.

### 2. Age Must Be Greater Than or Equal to 17
Ensures valid student age data.

### 3. Seats Cannot Be Negative
Prevents invalid course seat information.

---

# Flow Automation

## 1. Send Email After Registration
Automatically sends confirmation email after student registration.

## 2. Notify Faculty When Course Is Full
Automatically alerts faculty when course capacity is reached.

---

# Apex Usage

Apex is used for:
- Custom business logic
- Bulk data processing
- API integration
- Advanced automation
- Complex validations

---

# When Flow is NOT Enough

Flow may not be sufficient when:
- Complex calculations are required
- External API integration is needed
- Large data processing is involved
- Advanced business logic is required

In such cases, Apex is preferred.

---

# Pseudocode Examples

```text
IF seats = 0 THEN block enrollment

IF attendance < 75 THEN send alert

IF marks > 90 THEN give discount
