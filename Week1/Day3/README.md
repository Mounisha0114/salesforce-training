# Salesforce Day 3
## Data Modeling and Business Logic

---

# 1. Difference Between App, Object, Record, and Field

| Term | Meaning | Example |
|------|----------|----------|
| App | Collection of related tools and objects | College Management App |
| Object | Database table storing data | Student Object |
| Record | Single row of data inside object | Student: Rahul |
| Field | Individual data attribute | Name, Email, Age |

---

# 2. Standard vs Custom Objects

## Standard Objects
Pre-built Salesforce objects provided by Salesforce.

### Examples
- Account
- Contact
- Opportunity
- Lead

## Custom Objects
Objects created according to specific business requirements.

### Examples
- Student
- Faculty
- Course
- Department

---

# 3. College Management Data Model

## Objects
- Student
- Faculty
- Course
- Department
- StudentCourse (Junction Object)

---

## Relationships

### One Department → Many Students
Each department can contain multiple students.

### One Department → Many Faculty
Each department can contain multiple faculty members.

### One Faculty → Many Courses
One faculty member can teach multiple courses.

### Many Students ↔ Many Courses
A student can enroll in many courses, and a course can contain many students.

This is handled using a Junction Object:
StudentCourse

---

# Data Model Diagram

Department
│
├── Students
├── Faculty
│
Faculty
│
├── Courses
│
Students ↔ StudentCourse ↔ Courses

---

# 4. Formula Fields

## 1. Full Name

### Purpose
Automatically combines first name and last name.

### Why Should It Be Automated?
- Avoids manual typing
- Maintains consistency

### Formula
First_Name__c & " " & Last_Name__c

---

## 2. Remaining Seats

### Purpose
Calculates available seats in a course.

### Why Should It Be Automated?
- Prevents overbooking
- Shows real-time availability

### Formula
Total_Seats__c - Enrolled_Students__c

---

## 3. Percentage

### Purpose
Calculates student percentage automatically.

### Why Should It Be Automated?
- Saves time
- Reduces calculation errors

### Formula
(Marks_Obtained__c / Total_Marks__c) * 100

---

# 5. Validation Rules

## 1. Email Cannot Be Empty

### Problem Prevented
Avoids missing communication details.

---

## 2. Student Age Cannot Be Negative

### Problem Prevented
Prevents invalid student records.

### Validation Rule
Age__c < 0

---

## 3. Course Seats Cannot Exceed Limit

### Problem Prevented
Prevents enrolling more students than available seats.

---

# 6. Reflection – Why Structured Enterprise Data Matters

Structured data is important because companies handle large amounts of information every day. Random spreadsheets create duplicate records, inconsistent information, and confusion between teams.

Enterprise systems like Salesforce organize data using:
- Objects
- Fields
- Relationships
- Validation Rules
- Formula Fields

This helps companies:
- Improve efficiency
- Reduce errors
- Generate accurate reports
- Automate repetitive tasks
- Scale operations easily

---

# Reflective Questions

## 1. Why can’t companies manage everything using Excel sheets?
Excel sheets become difficult to manage when data grows large and multiple teams work together.

---

## 2. Why are relationships important between objects?
Relationships connect related data and improve organization and reporting.

---

## 3. What problems happen if data is inconsistent?
It can cause incorrect reports, duplicate records, and poor business decisions.

---

## 4. Why should repetitive calculations be automated?
Automation saves time and reduces human errors.

---

## 5. Why should invalid data be blocked early?
To maintain accurate, clean, and reliable data.

---

## 6. Why is Salesforce called a metadata-driven platform?
Because most customization is done through configuration instead of coding.
