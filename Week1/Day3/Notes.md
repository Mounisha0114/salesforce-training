# Day 3 Notes

## Objects, Fields, and Records

### Object
An object in Salesforce is similar to a database table that stores related information.

Examples:
- Student
- Faculty
- Course
- Department

### Field
A field stores a specific type of information inside an object.

Examples:
- Student Name
- Email
- Age
- Course Name

### Record
A record is a single entry inside an object.

Example:
| Name | Age | Department |
|------|------|------------|
| Rahul | 20 | CSE |

---

# Standard vs Custom Objects

## Standard Objects
Objects already provided by Salesforce.

Examples:
- Account
- Contact
- Opportunity
- Lead

## Custom Objects
Objects created according to business requirements.

Examples:
- Student
- Faculty
- Course
- Department

---

# Relationships in Salesforce

Relationships are used to connect objects.

## Lookup Relationship
Creates a loose connection between objects.

Examples:
- Student → Department
- Faculty → Department

## One-to-Many Relationship
One record can be connected to multiple records.

Examples:
- One Department can have many Students
- One Faculty can teach many Courses

## Junction Object
Used to create a many-to-many relationship.

Example:
- StudentCourse object connects Students and Courses

---

# Formula Fields

Formula fields automatically calculate values.

## Examples
1. Full Name
2. Remaining Seats
3. Percentage

## Benefits
- Reduces manual work
- Avoids calculation mistakes
- Keeps values updated automatically

## Example Formula

Full Name:
First_Name__c & " " & Last_Name__c

Remaining Seats:
Total_Seats__c - Enrolled_Students__c

Percentage:
(Marks_Obtained__c / Total_Marks__c) * 100

---

# Validation Rules

Validation rules prevent invalid data from being saved.

## Examples

### 1. Email Cannot Be Empty
Prevents records without email addresses.

### 2. Student Age Cannot Be Negative
Prevents invalid age values.

Validation:
Age__c < 0

### 3. Course Seats Cannot Exceed Limit
Prevents overbooking courses.

---

# Why Structured Data Matters

Companies need structured data because:
- Large data becomes easier to manage
- Reports become accurate
- Searching and filtering become faster
- Duplicate records are reduced
- Team collaboration improves

Random spreadsheets become difficult to maintain when data grows.

---

# Salesforce as a Metadata-Driven Platform

Salesforce is called metadata-driven because:
- Most customizations are done without coding
- Admins can create objects, fields, rules, and automation easily
- Applications can be modified quickly according to business needs
