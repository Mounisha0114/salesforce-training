Sprint 11 – Crossing the Salesforce Boundary

Overview

This sprint extends the Placement Management System beyond Salesforce by integrating it with an external recruitment platform.

The integration allows Salesforce to communicate with an external system through REST APIs while keeping authentication, error handling, retries, and data synchronization reliable and secure.

«A good Salesforce application does not live in isolation. It knows what belongs inside the platform, what belongs outside it, and how the two should communicate safely.»

Business Problem

The Placement Management System currently manages students, jobs, applications, interviews, and offers inside Salesforce.

However, recruiting companies may use their own recruitment platforms.

The requirement is:

- When a student is selected, Salesforce should automatically send the candidate information to the external recruitment system.
- When the external company updates information such as interview results, Salesforce should be able to receive that information.

This creates the integration:

Salesforce ↔ External Recruitment System

APIs provide the agreed communication contract between the two systems.

---

Objectives

This sprint covers:

- APIs and API contracts
- REST APIs
- HTTP methods
- Request and response structures
- JSON
- Salesforce HTTP callouts
- Queueable Apex for asynchronous callouts
- Named Credentials
- Authentication and authorisation
- Auth Providers
- Salesforce Connect and External Objects
- Point-to-point and middleware integration
- Synchronous and asynchronous integration
- Error handling
- Retry mechanisms
- Idempotency
- Integration status tracking

---

REST API

REST APIs commonly use HTTP methods to express the intended operation.

Method| Typical Meaning
GET| Retrieve data
POST| Create/process something
PUT| Replace/update a resource
PATCH| Partially update a resource
DELETE| Remove a resource

For example:

GET /jobs
GET /jobs/123
POST /applications
PATCH /applications/123

The HTTP method communicates what operation the caller intends to perform.

---

Request and Response

An API request can contain:

- URL
- HTTP method
- Headers
- Authentication information
- Optional body

The response can contain:

- Status code
- Headers
- Optional body

Common HTTP Status Codes

Status| Meaning
200| Successful request
201| Resource successfully created
204| Successful request with no response body
400| Bad request
401| Authentication required/failed
403| Forbidden
404| Resource not found
500| Server-side error

An integration must be designed to handle failures rather than assuming every request succeeds.

---

JSON

JSON is commonly used to exchange information between APIs.

Example:

{
  "studentId": "STU10045",
  "name": "Ananya",
  "email": "ananya@example.com",
  "cgpa": 8.4,
  "branch": "CSE"
}

JSON contains:

- Keys
- Values
- Objects
- Arrays

The application should understand the data structure before implementing the integration logic.

---

Integration Architecture

The proposed candidate synchronization flow is:

Application Status
        ↓
     Selected
        ↓
     Queueable
        ↓
   Build Request
        ↓
 Named Credential
        ↓
    REST API
        ↓
 Process Response

Queueable Apex is used because sending information to an external system can be secondary to the immediate Salesforce business transaction.

---

Apex HTTP Callout

Salesforce can communicate with external APIs using HTTP callouts.

Conceptual flow:

HttpRequest
     ↓
Configure Request
     ↓
Http
     ↓
send()
     ↓
HttpResponse

Example:

HttpRequest request = new HttpRequest();

request.setEndpoint(
    'callout:Recruitment_API/candidates'
);

request.setMethod('POST');

request.setHeader(
    'Content-Type',
    'application/json'
);

request.setBody(
    JSON.serialize(candidate)
);

Http http = new Http();

HttpResponse response =
    http.send(request);

The important components are:

- HttpRequest – represents what Salesforce wants to send.
- Endpoint – identifies where the request goes.
- Method – specifies the requested operation.
- Headers – provide additional information.
- Body – contains the data being sent.
- Http – performs the request.
- HttpResponse – contains the external system's response.

---

Named Credentials

Credentials should never be hard-coded inside Apex.

Hard-coding credentials can cause:

- Secrets to leak into Git
- Exposure during code reviews
- Accidental copying
- Difficult credential rotation
- Security risks

Instead, Salesforce provides Named Credentials.

Conceptually:

Apex
 ↓
Named Credential
 ↓
Authentication
 ↓
External API

Named Credentials provide managed configuration for an external endpoint and its authentication setup.

---

Authentication vs Authorisation

Authentication

Answers:

«Who are you?»

Authorisation

Answers:

«What are you allowed to do?»

For example:

Authentication
→ Identify the user/system

Authorisation
→ Determine what that identity is permitted to access

A "401" response generally requires investigation of authentication, while a "403" response may indicate insufficient permissions for an authenticated identity.

---

Auth Providers

An Auth Provider can help Salesforce handle authentication with supported external identity providers.

A simplified architecture is:

External Identity Provider
          ↓
     Auth Provider
          ↓
Salesforce Authentication Configuration
          ↓
    Named Credential
          ↓
      Apex Callout
          ↓
     External API

The exact configuration depends on the external provider and authentication protocol.

---

Candidate Data

When a student is selected, the following information can be sent to the external recruitment platform:

- Student ID
- Name
- Email
- Branch
- CGPA
- Job ID
- Company
- Role
- Selection Date

The API contract should define the endpoint, request body, expected response, and possible error responses.

---

API Contract

Example contract:

Endpoint:
POST /candidates

Request:
Candidate information in JSON format

Possible Responses:
200 / 201 → Success
400       → Bad Request
401       → Authentication Failure
403       → Forbidden
500       → Server Error

The API contract should be documented before implementation.

---

Integration Status

The system should track the state of external synchronization.

Example:

Pending
   ↓
Queueable
   ↓
Success → Sent

Failure
   ↓
Retry Required

Useful fields include:

- "Integration_Status__c"
- External Reference
- Last Attempt
- Error Message

This allows administrators to identify what happened during synchronization.

---

Error Handling and Retry

The external system may be temporarily unavailable.

For example:

Salesforce
    ↓
External API
    X
Unavailable

The student's selection should not necessarily fail just because the external synchronization failed.

A temporary "500 Internal Server Error" may require a retry instead of permanently marking the candidate as failed.

However, retries can create duplicate records. Therefore, the integration must consider idempotency.

---

Idempotency

Idempotency asks:

«If the same request is processed twice, will it create duplicate data?»

Possible strategies include:

- External Reference ID
- Salesforce Application ID
- Idempotency Key
- Existing-record lookup
- Synchronization status

The integration should identify the unique business transaction before implementing retries.

---

Salesforce Connect and External Objects

Sometimes Salesforce does not need to copy external data into Salesforce.

Instead:

External System
      ↓
External Object
      ↓
Salesforce UI

The data remains primarily in the external system.

This can be useful when Salesforce users need to view external information without storing all of it inside Salesforce.

Copy Data vs Access Data

Copy data into Salesforce when Salesforce needs to own, manipulate, automate, or report on the data.

Use External Objects when the requirement is primarily to access external data without copying everything.

The decision depends on:

- Data ownership
- Data volume
- Latency
- Reporting
- Security
- Integration requirements
- Business criticality

---

Integration Patterns

Point-to-Point

Salesforce
     ↕
External System

This is simple and can be appropriate when there is only one external system.

Middleware

Salesforce
     ↕
Middleware
     ↕
External Systems

Middleware can provide:

- Transformation
- Routing
- Orchestration
- Monitoring
- Retries
- Protocol conversion

As the number of external systems increases, middleware can help manage integration complexity.

---

Synchronous vs Asynchronous

Synchronous

Salesforce
    ↓
External API
    ↓
Response
    ↓
Salesforce

Use when the user genuinely needs the response immediately.

Example:

«Verify an external certification number.»

Asynchronous

Salesforce
    ↓
Queueable
    ↓
External API

Use when the external operation does not need to block the user.

Example:

«Send selected student information to a company's recruitment system.»

For large scheduled synchronizations, concepts such as Scheduled Apex, Batch Apex, integration, error handling, and retry strategies should be considered.

---

Security Principles

This sprint emphasizes:

1. Never hard-code credentials.
2. Use Named Credentials.
3. Separate configuration from business code.
4. Understand authentication and authorisation.
5. Handle authentication failures.
6. Protect sensitive integration information.

---

Project Structure

Recommended repository structure:

Sprint-11-Integration
│
├── README.md
│
├── architecture/
│   ├── integration-flow.png
│   ├── sequence-diagram.png
│   └── integration-pattern.png
│
├── force-app/
│
├── api-contract/
│   └── candidate-api.md
│
├── screenshots/
│
└── learning-notes/
    └── sprint-11.md

The README should document the business problem, external system, data flow, authentication, error handling, retry strategy, idempotency, and integration pattern.

---

Mini Project – External Recruitment Gateway

The project should:

1. Send Selected Candidates

Application
    ↓
Queueable
    ↓
External API

2. Track Integration Status

Recommended fields:

- Integration Status
- External Candidate ID
- Last Integration Attempt
- Integration Error

3. Use Named Credentials

No hard-coded secrets.

4. Handle Responses

At minimum:

- Success
- 400
- 401
- 403
- 500
- Unexpected Error

5. Handle Retry

Document what happens when the external system is temporarily unavailable.

6. Prevent Duplicates

Document what uniquely identifies a candidate submission.

7. Document the API Contract

Include:

- Endpoint
- Method
- Request JSON
- Response JSON
- Authentication approach
- Error handling
- Retry strategy
- Idempotency strategy

If a mock API is used, clearly document that it is a mock API.

---

Key Engineering Principles

API = Communication Contract

Systems communicate through agreed contracts instead of directly accessing each other's databases.

Configuration ≠ Business Code

Endpoints and authentication configuration should be separated from Apex logic.

Business Commitment ≠ External Communication

The Salesforce transaction should establish the essential business truth first. External synchronization can happen afterward when appropriate.

Design for Failure

External systems can be:

- Slow
- Unavailable
- Incorrectly configured
- Overloaded
- Changed unexpectedly
- Authenticated differently
- Returning unexpected data

Therefore, integrations need timeouts, errors, authentication, retries, duplicate protection, monitoring, and contracts.

---

Final Architecture

Students
   ↓
Lightning Web Components
   ↓
Apex Services
   ↓
┌───────────────────────┐
│ Salesforce Async Apex │
│ Queueable / Batch     │
└───────────┬───────────┘
            ↓
       External APIs
            ↓
     External Systems

The overall goal is not simply to make one API request work. The integration should allow two independent systems to continue working correctly even when one system fails.

---

Learning Outcomes

After completing this sprint, you should be able to:

- Explain APIs as contracts between systems.
- Explain REST and HTTP methods.
- Understand request and response structures.
- Read JSON.
- Understand Salesforce HTTP callouts.
- Explain why callouts may use asynchronous processing.
- Explain Named Credentials.
- Distinguish authentication from authorisation.
- Explain Auth Providers.
- Explain Salesforce Connect and External Objects.
- Compare point-to-point and middleware integration.
- Choose synchronous or asynchronous integration based on requirements.
- Handle failures and retries.
- Explain idempotency.
- Track integration status.
- Connect Triggers, Services, Queueable Apex, and external APIs.
- Document an API contract.
- Defend an integration architecture.

Conclusion

Sprint 11 takes the Placement Management System from a Salesforce-only application to an enterprise-style application capable of communicating with external systems.

The key lesson is:

«An integration is successful not when one API call works, but when two independent systems can continue working correctly even when the other system does not.»
