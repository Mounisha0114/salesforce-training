
# `codes.md`

````markdown
# Day 11 - Integration Codes

## 1. RecruitmentIntegrationController.cls

```apex
public with sharing class RecruitmentIntegrationController {

    @AuraEnabled
    public static String sendApplication(Id applicationId) {

        if (applicationId == null) {
            throw new AuraHandledException(
                'Application Id is required.'
            );
        }

        try {

            Application__c app = [
                SELECT Id,
                       Name,
                       Status__c,
                       Student__c,
                       Job__c
                FROM Application__c
                WHERE Id = :applicationId
                LIMIT 1
            ];

            HttpRequest request = new HttpRequest();

            request.setEndpoint(
                'callout:Recruitment_Gateway'
            );

            request.setMethod('POST');

            request.setHeader(
                'Content-Type',
                'application/json'
            );

            Map<String, Object> requestBody =
                new Map<String, Object>();

            requestBody.put('applicationId', app.Id);
            requestBody.put('applicationNumber', app.Name);
            requestBody.put('status', app.Status__c);

            request.setBody(
                JSON.serialize(requestBody)
            );

            Http http = new Http();

            HttpResponse response =
                http.send(request);

            if (
                response.getStatusCode() >= 200 &&
                response.getStatusCode() < 300
            ) {

                app.Integration_Status__c = 'Success';
                app.External_Candidate_Id__c =
                    String.valueOf(app.Id);
                app.Last_Integration_Attempt__c =
                    Datetime.now();
                app.Integration_Error__c = null;

                update app;

                return 'SUCCESS';

            } else {

                app.Integration_Status__c = 'Failed';
                app.Last_Integration_Attempt__c =
                    Datetime.now();

                app.Integration_Error__c =
                    'HTTP ' +
                    response.getStatusCode() +
                    ': ' +
                    response.getBody();

                update app;

                return 'FAILED';
            }

        } catch (Exception e) {

            try {

                Application__c errorApp = [
                    SELECT Id,
                           Integration_Status__c,
                           Last_Integration_Attempt__c,
                           Integration_Error__c
                    FROM Application__c
                    WHERE Id = :applicationId
                    LIMIT 1
                ];

                errorApp.Integration_Status__c = 'Failed';

                errorApp.Last_Integration_Attempt__c =
                    Datetime.now();

                errorApp.Integration_Error__c =
                    e.getMessage();

                update errorApp;

            } catch (Exception updateException) {
                // Preserve the original integration error.
            }

            throw new AuraHandledException(
                'Integration failed: ' +
                e.getMessage()
            );
        }
    }
}
````

---

## 2. Anonymous Apex - Test Integration Fields

```apex
Application__c app = [
    SELECT Id, Name
    FROM Application__c
    WHERE Name = 'APPID2608'
    LIMIT 1
];

app.Integration_Status__c = 'Success';
app.External_Candidate_Id__c = 'TEST-ID';
app.Last_Integration_Attempt__c = Datetime.now();
app.Integration_Error__c = null;

update app;

System.debug('UPDATED: ' + app.Id);
```

---

## 3. Anonymous Apex - Verify Integration Attempt

```apex
Application__c app = [
    SELECT Id, Last_Integration_Attempt__c
    FROM Application__c
    WHERE Name = 'APPID2608'
    LIMIT 1
];

Datetime beforeTime =
    app.Last_Integration_Attempt__c;

app.Last_Integration_Attempt__c =
    Datetime.now();

app.Integration_Status__c =
    'Success';

update app;

Application__c checkApp = [
    SELECT Last_Integration_Attempt__c,
           Integration_Status__c
    FROM Application__c
    WHERE Id = :app.Id
];

System.debug(
    'BEFORE: ' + beforeTime
);

System.debug(
    'AFTER: ' +
    checkApp.Last_Integration_Attempt__c
);

System.debug(
    'STATUS: ' +
    checkApp.Integration_Status__c
);
```

---

## 4. Named Credential

```text
Label:
Recruitment Gateway

Name:
Recruitment_Gateway

External Credential:
Recruitment Gateway Credential

External Credential Name:
Recruitment_Gateway_Credential

Endpoint:
callout:Recruitment_Gateway
```

---

## 5. Integration Status Picklist Values

```text
Not Sent
Queued
Success
Failed
Retry
```

---

## 6. Integration Fields

```text
Integration_Status__c
External_Candidate_Id__c
Last_Integration_Attempt__c
Integration_Error__c
```

---

## 7. Callout Method

```apex
request.setEndpoint(
    'callout:Recruitment_Gateway'
);

request.setMethod('POST');

request.setHeader(
    'Content-Type',
    'application/json'
);
```

---

## 8. JSON Request Body

```apex
Map<String, Object> requestBody =
    new Map<String, Object>();

requestBody.put('applicationId', app.Id);
requestBody.put('applicationNumber', app.Name);
requestBody.put('status', app.Status__c);

request.setBody(
    JSON.serialize(requestBody)
);
```

````

### GitHub files

Keep them like this:

```text
README.md
codes.md
````

