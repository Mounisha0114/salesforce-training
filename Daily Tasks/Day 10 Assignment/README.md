Sprint 10 – LWC Component Architecture

Building Components That Think Together

This sprint focuses on building a Salesforce Lightning Web Component (LWC) application using focused, reusable components that communicate clearly with each other.

The project extends the Student Placement Portal into a more realistic enterprise application.

🎯 Objectives

- Design LWCs as a complete application instead of isolated components.
- Understand Parent → Child communication.
- Understand Child → Parent communication using custom events.
- Use "@api" public properties appropriately.
- Build forms using Salesforce Lightning Base Components.
- Understand client-side and server-side validation.
- Use Lightning Data Service (LDS) where appropriate.
- Understand reactive data and refresh behaviour.
- Create reusable components.
- Handle loading, success, empty, and error states.
- Avoid tightly coupled and oversized components.

🏗️ Component Architecture

StudentPortal
│
├── StudentSummary
│
├── StudentProfile
│
├── EligibleJobs
│   ├── JobCard
│   └── EmptyState
│
├── MyApplications
│   ├── ApplicationCard
│   └── EmptyState
│
└── OfferSummary
    └── StatusBadge

Each component has a focused responsibility while the components cooperate to create one complete application.

🔄 Component Communication

Parent → Child

The parent passes data to a child using public properties with "@api".

Parent
   │
   │ Data
   ↓
Child

Example:

// Child
@api job;

<c-job-details job={selectedJob}></c-job-details>

The child receives the information it needs instead of unnecessarily retrieving the same information again.

Child → Parent

A child communicates with its parent using Custom Events.

Child
   │
   │ Custom Event
   ↓
Parent

Example:

this.dispatchEvent(
    new CustomEvent('viewdetails', {
        detail: {
            jobId: this.job.Id
        }
    })
);

The parent listens for the event:

<c-job-card
    job={job}
    onviewdetails={handleViewDetails}>
</c-job-card>

Key Principle

«Children report events. Parents coordinate behaviour.»

A child should not directly modify the parent's state.

📡 Event Design

Events should communicate accurate information.

For example:

applyclicked

can represent a user's click.

However:

applicationsubmittedsuccessfully

should not be sent by the child if the child has not confirmed that the application was actually created.

The project distinguishes between:

- User Intent
- Business Outcome

📝 Profile Form

The Student Profile component allows students to update:

Field| Type| Required
Phone| Text| Yes
Email| Email| Yes
Branch| Picklist| Yes
CGPA| Number| Yes
Skills| Long Text| No
Preferred Location| Picklist| No

Salesforce Lightning Base Components are used where appropriate:

- "lightning-input"
- "lightning-combobox"
- "lightning-textarea"
- "lightning-checkbox-group"
- "lightning-radio-group"
- "lightning-button"

The principle is:

«Reuse the platform before reinventing the platform.»

✅ Validation Strategy

Client-Side Validation

Client-side validation improves the user experience.

For example:

0 <= CGPA <= 10

Server-Side Validation

Server-side validation protects business integrity.

Client-side validation should not be trusted as the only protection because another client could call the backend directly.

Client Validation
       ↓
Better User Experience

Server Validation
       ↓
Business Integrity

Both have different responsibilities.

⚡ Lightning Data Service

Lightning Data Service (LDS) can be used for standard and supported Salesforce record operations.

It can reduce the amount of custom Apex required.

Before writing Apex, the project considers:

Option A → LDS
Option B → Custom Apex
Option C → Combination

The architecture should be selected based on the requirement, not simply because the development team knows Apex.

🔄 Reactive Data & Refresh

Changes to a Student record can affect:

- Student Summary
- Eligible Jobs
- Application Eligibility

For example:

Student Record
      │
 ┌────┼────┐
 ↓    ↓    ↓
Summary Jobs Applications

If different components maintain separate copies of the same data, they can become inconsistent.

After a profile update:

Profile
   ↓
Student Record Changes
   ↓
Student Summary Refreshes
   ↓
Eligible Jobs Refreshes

Possible strategies include:

- Parent-owned state
- Custom events
- Refreshing wired data
- LDS-supported notifications/reactive updates
- Re-querying data when genuinely necessary

♻️ Reusable Components

Reusable components reduce duplication and provide meaningful capabilities.

Examples:

StatusBadge
EmptyState
LoadingIndicator
JobCard
ApplicationStatus

Example architecture:

ApplicationCard
      ↓
  StatusBadge

InterviewCard
      ↓
  StatusBadge

OfferCard
      ↓
  StatusBadge

A reusable component should have a meaningful purpose rather than being created merely for a small piece of markup.

🚦 UI States

The application handles different states consistently.

Loading

Loading your profile...

Editing

Normal profile form.

Saving

Saving...

Success

Profile updated successfully.

Error

We could not update your profile.
Please review the highlighted fields.

Empty

No eligible opportunities are available right now.

Keep your profile updated and check again
as new companies are added.

🔁 Student Placement User Journey

The complete application workflow is:

Student Login
     ↓
Student Summary
     ↓
Update Profile
     ↓
Profile Saved
     ↓
Eligible Jobs Refresh
     ↓
Select Job
     ↓
Job Details
     ↓
Apply
     ↓
Application Created
     ↓
My Applications Refresh
     ↓
Student Sees New Status

🧩 Engineering Principles

1. Focused Components

Each component should have a clear responsibility.

2. Clear Communication

Components should communicate through defined interfaces and events.

3. Data Ownership

The application should have clear ownership of changing data.

4. Server-Side Business Rules

Business validation must remain authoritative on the server.

5. Platform First

Use Salesforce capabilities such as Lightning Base Components and LDS when they fit the requirement.

6. Avoid God Components

A single component should not control every piece of state, event, and business behaviour.

🐛 Architecture Review

Important questions when reviewing the application:

1. Which components retrieve data?
2. Which components display data?
3. Which components own state?
4. Which components only report events?
5. Where does business logic live?
6. Are multiple components unnecessarily retrieving the same information?
7. Are any components becoming too large?

🧪 Definition of Done

- [ ] Student can view their profile.
- [ ] Student can update their profile.
- [ ] Profile validation works.
- [ ] Eligible Jobs reflect current student information.
- [ ] Job Cards are reusable.
- [ ] Child components communicate through events.
- [ ] Parents deliberately pass information to children.
- [ ] Application submission works.
- [ ] Duplicate applications are handled.
- [ ] My Applications reflects new applications.
- [ ] Loading states are visible.
- [ ] Empty states are meaningful.
- [ ] Errors are handled professionally.
- [ ] Business rules remain server-side.
- [ ] Components have clear responsibilities.
- [ ] Complete data flow can be explained.

📁 Suggested Repository Structure

Sprint-10-LWC-Architecture
│
├── README.md
│
├── architecture/
│   ├── component-tree.png
│   └── data-flow.png
│
├── force-app/
│
├── screenshots/
│   ├── profile.png
│   ├── eligible-jobs.png
│   ├── application-success.png
│   └── empty-state.png
│
└── learning-notes/
    └── sprint-10.md

🎤 Interview Preparation

Important questions covered in this sprint:

1. How does a parent communicate data to a child?
2. How does a child communicate an action to a parent?
3. Why should a child not directly manipulate parent state?
4. What is the purpose of "@api"?
5. What are custom events in LWC?
6. When would you use LDS instead of Apex?
7. Why is client-side validation insufficient for business rules?
8. What is reactive data?
9. Why can a component show stale information after an update?
10. What is a reusable component?
11. When does component reuse become over-engineering?
12. How would you design communication between sibling components?

💡 Key Takeaway

This sprint moves beyond individual Salesforce technologies and focuses on how technologies cooperate.

The complete flow can be understood as:

User
 ↓
LWC
 ↓
Component Event / Data Binding
 ↓
Apex
 ↓
Service
 ↓
Database
 ↓
Result
 ↓
UI Refresh

A good Salesforce application is not simply a collection of components. It is a system in which components communicate with clear responsibilities and clear contracts.

🚀 Next Step

The next sprint moves beyond Salesforce and introduces:

- REST APIs
- Callouts
- Named Credentials
- Authentication
- Integration Patterns
- External Systems
