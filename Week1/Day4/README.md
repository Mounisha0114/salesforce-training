# Day 4 Flow Builder

## day 4 topics
- What Flow Builder is
- Why businesses automate processes
- Types of Flows in Salesforce
- Difference between manual and automated systems
- No-code automation in Salesforce

---

# 1. What is Flow Builder?

Flow Builder is a no-code automation tool in Salesforce used to automate business processes visually without writing code.

Using Flow Builder, businesses can:
- Automate repetitive tasks
- Update records automatically
- Send notifications and emails
- Create guided user screens
- Improve productivity and consistency

---

# 2. Types of Flows

## Screen Flow

A Screen Flow is a flow that interacts with users through screens and forms.

### Example:
- Student registration form
- Fee payment form
- Course enrollment process

### Features:
- Takes user input
- Displays screens
- Guides users step-by-step

---

## Record Triggered Flow

A Record Triggered Flow runs automatically when a record is created, updated, or deleted.

### Example:
- Send email after student registration
- Update remaining course seats automatically
- Notify faculty when a course becomes full

### Features:
- Runs automatically
- No manual action required
- Used for background automation

---

# 3. My Automation Ideas (College Management System)

## 1. Auto Email After Student Registration

When a student registers, Salesforce automatically sends a confirmation email.

### Why Automation Helps:
Reduces manual communication work and provides instant response.

---

## 2. Auto Update Remaining Seats

When a student enrolls in a course, available seats are automatically reduced.

### Why Automation Helps:
Prevents overbooking and maintains accurate seat count.

---

## 3. Notify Faculty When Course Is Full

Faculty receives notification automatically when course capacity is reached.

### Why Automation Helps:
Improves communication and quick decision-making.

---

## 4. Generate Student ID Automatically

Salesforce automatically creates a unique student ID during admission.

### Why Automation Helps:
Avoids duplicate IDs and reduces manual effort.

---

## 5. Send Fee Deadline Reminder

Students automatically receive reminders before fee payment deadlines.

### Why Automation Helps:
Improves fee collection and reduces missed payments.

---

# Flow Design Thinking

## Automation Chosen:
Auto Email After Student Registration

---

# Flow Diagram

```text
Trigger:
Student Record Created
        │
        ▼
Check Student Email Exists?
        │
   ┌────┴────┐
   │         │
  Yes        No
   │          │
   ▼          ▼
Send Email   Stop Flow
   │
   ▼
Update Status
   │
   ▼
End

```

# 5. Manual vs Automated Process

## Process Chosen: Student Registration Confirmation

### Manual Process
In the manual process, the admin checks student registration details one by one and sends confirmation emails manually. The admin also updates records and verifies course details separately. This process takes more time and requires continuous human effort.

### Problems in Manual Process
- Time consuming
- Repetitive work
- Higher chance of human errors
- Delayed communication
- Difficult to manage large number of students

### Automated Process Using Salesforce
Using Salesforce Flow Builder, the system automatically sends confirmation emails after registration, updates records, and notifies related departments instantly. The process becomes faster, accurate, and efficient.

### Benefits of Automation
- Saves time
- Reduces manual work
- Improves accuracy
- Faster communication
- Better productivity

---

# 6. Reflection – Why Automation Matters in Enterprise Systems

Automation is important in enterprise systems because companies handle large amounts of data and repetitive business processes every day. Manual work can cause delays, errors, and inefficiency. Automation helps businesses complete tasks faster, maintain consistency, and improve productivity.

In Salesforce, automation tools like Flow Builder allow organizations to automate workflows without coding. This helps employees focus on important business decisions instead of repeating the same tasks manually. Automation also improves customer experience by providing quick responses and reliable processes.
