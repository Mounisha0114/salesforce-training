````markdown
# Day 11 - External Recruitment Integration

## Overview

Day 11 focuses on integrating the Salesforce Placement Management
System with an external recruitment platform.

The goal is to allow a selected student application to be sent from
Salesforce to an external recruitment API and to track the result of
that integration inside Salesforce.

---

## Business Problem

The Placement Office wants Salesforce to send selected candidates to
the company's recruitment platform.

The integration allows Salesforce to communicate with an external
system using an HTTP REST callout.

---

## Integration Flow

```text
Application
     |
     | Send to Recruitment
     v
Apex Controller
     |
     v
Named Credential
     |
     v
External Recruitment API
     |
     v
HTTP Response
     |
     v
Update Application Integration Status
````

---

## Salesforce Components

### Application Object

The following integration tracking fields were added to
`Application__c`:

* `Integration_Status__c`
* `External_Candidate_Id__c`
* `Last_Integration_Attempt__c`
* `Integration_Error__c`

These fields allow Salesforce users to understand the current state
of an external synchronization.

---

## Integration Status

The `Integration_Status__c` field is a restricted picklist with the
following values:

* Not Sent
* Queued
* Success
* Failed
* Retry

---

## Apex Controller

The main Apex class is:

```text
RecruitmentIntegrationController.cls
```

The controller:

1. Receives the Application Id.
2. Retrieves the Application record.
3. Builds the request payload.
4. Creates an HTTP request.
5. Sends a POST request to the external endpoint.
6. Processes the HTTP response.
7. Updates integration tracking fields.
8. Returns the integration result.

---

## HTTP Callout

The integration uses an HTTP `POST` request.

Example request structure:

```json
{
  "applicationId": "Salesforce Application Id",
  "applicationNumber": "APPID2608",
  "status": "Selected"
}
```

The request uses:

```text
Content-Type: application/json
```

---

## Named Credential

The Apex code does not hard-code the external endpoint.

The callout uses:

```text
callout:Recruitment_Gateway
```

The Named Credential is:

```text
Recruitment Gateway
```

API Name:

```text
Recruitment_Gateway
```

The Named Credential contains the external endpoint configuration.

---

## External Credential

The External Credential created for the integration is:

```text
Recruitment Gateway Credential
```

API Name:

```text
Recruitment_Gateway_Credential
```

Authentication configuration is separated from the Apex business logic.

---

## Permission Set

A permission set was created:

```text
Recruitment Integration Access
```

The permission set provides the required access for the integration
objects and external credential functionality.

---

## Send to Recruitment Action

A custom action was configured on the Application object.

The action allows a user to initiate the recruitment integration from
an Application record.

Example:

```text
Application
    |
    v
Send to Recruitment
    |
    v
RecruitmentIntegrationController
    |
    v
External API
```

---

## Success Handling

When the external API returns a successful response, Salesforce updates:

```text
Integration Status = Success
Integration Error = blank
Last Integration Attempt = current date/time
```

The external candidate reference is also stored in:

```text
External Candidate Id
```

---

## Failure Handling

When the external API returns an unsuccessful response, Salesforce
records the failure.

Example:

```text
Integration Status = Failed
Integration Error = HTTP error information
Last Integration Attempt = current date/time
```

This allows administrators to identify integration failures.

---

## Error Handling

The integration handles exceptions using Apex exception handling.

If an exception occurs, the integration records:

* Failure status
* Error message
* Last integration attempt

The user receives an integration failure notification.

---

## Testing

The integration was tested from an Application record.

Test Application:

```text
Application Number: APPID2608
```

The **Send to Recruitment** action was executed successfully.

The integration successfully reached the configured HTTP endpoint and
returned a successful result.

---

## Development/Test Endpoint

For development and testing, a mock HTTP endpoint was used.

The current endpoint is intended for testing and is **not a production
Recruitment API**.

A real recruitment API endpoint should replace the test endpoint before
production deployment.

---

## Security

Credentials are not stored directly in Apex.

The integration uses Salesforce:

* Named Credentials
* External Credentials
* Permission Sets

This separates integration configuration and authentication from
business logic.

---

## Integration Tracking

Salesforce maintains integration state using the following fields:

| Field                    | Purpose                                  |
| ------------------------ | ---------------------------------------- |
| Integration Status       | Current integration state                |
| External Candidate Id    | External candidate reference             |
| Last Integration Attempt | Date/time of latest attempt              |
| Integration Error        | Error information when integration fails |

---

## Error Scenarios

The integration should consider common external API failures such as:

```text
400 - Bad Request
401 - Authentication Failure
403 - Authorization Failure
500 - Server Error
Timeout
Connection Failure
```

The Salesforce integration layer should record useful error
information so administrators can identify and investigate failures.

---

## Future Improvements

The current implementation can be extended with:

### Queueable Apex

Move the external callout into asynchronous processing so that the
Salesforce user does not need to wait for the external system.

```text
Selected
   |
   v
Queueable Apex
   |
   v
HTTP Callout
   |
   v
External Recruitment API
```

### Retry Processing

Temporary external failures can be retried.

```text
Failure
   |
   v
Retry Required
   |
   v
Queueable
   |
   v
External API
```

### Idempotency

The integration should prevent duplicate candidate creation if the
same Application is submitted more than once.

Possible identifiers include:

* Salesforce Application Id
* External Reference Id
* Idempotency Key

### Monitoring

Failed integrations can be monitored using Salesforce reports,
list views, or administrative dashboards.

---

## Integration Architecture

The broader integration architecture is:

```text
Salesforce
    |
    | Application Selected
    v
Integration Layer
    |
    | HTTP POST
    v
External Recruitment Platform
    |
    | HTTP Response
    v
Integration Status
```

The architecture separates Salesforce business data from external
system communication.

---

## Key Concepts Learned

Day 11 covered:

* REST APIs
* HTTP methods
* JSON request and response structures
* Apex HTTP Callouts
* `HttpRequest`
* `Http`
* `HttpResponse`
* Named Credentials
* External Credentials
* Authentication
* Authorization
* Integration status tracking
* Error handling
* Retry thinking
* Idempotency
* Salesforce Connect and External Objects
* Point-to-point integration
* Middleware concepts
* Synchronous vs asynchronous integration

---

## Engineering Lessons

An integration is more than simply making an HTTP request.

External systems can be:

* Slow
* Unavailable
* Incorrectly configured
* Temporarily overloaded
* Authenticated differently
* Returning unexpected data

Therefore, integration design must consider:

* Timeouts
* Errors
* Authentication
* Retries
* Duplicate requests
* Monitoring
* Data ownership
* API contracts

---

## Day 11 Outcome

The Salesforce Placement Management System successfully communicates
with an external HTTP endpoint.

The Application record stores the integration state and error
information, allowing Salesforce users and administrators to understand
the result of the external synchronization.

The integration can be extended further with Queueable Apex, retry
processing, idempotency, monitoring, and a production recruitment API.

---

## Status

**Day 11 - External Recruitment Integration: COMPLETE**

```
```
