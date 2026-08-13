# Placement Management System

A Salesforce-based Placement Management System developed using Apex, Lightning Web Components (LWC), and Salesforce DX.

## Project Overview

The Placement Management System helps manage student placement information, student profiles, eligible jobs, applications, and placement-related information through a Salesforce application.

## Technologies Used

- Salesforce Platform
- Apex
- Lightning Web Components (LWC)
- SOQL
- Salesforce CLI
- Git
- GitHub
- Visual Studio Code

## Main Features

- Placement Portal
- Student Details
- Student Profile
- Eligible Jobs
- Placement Management Dashboard
- Application Management
- Placement Services
- Notification Services
- Recruitment Integration
- Student and Placement Statistics

## Project Structure

```text
PlacementManagementSystem/
│
├── force-app/
│   └── main/
│       └── default/
│           ├── classes/
│           ├── lwc/
│           ├── messageChannels/
│           └── triggers/
│
├── config/
├── scripts/
├── .gitignore
├── package.json
├── sfdx-project.json
├── jest.config.js
└── README.md
````

## Salesforce Components

### Apex Classes

The project contains Apex classes for:

* Student management
* Student profile management
* Application management
* Placement management
* Alumni services
* Eligible job processing
* Notifications
* Recruitment integration
* Statistics

### Lightning Web Components

The application contains LWC components for:

* Placement Home
* Student Details
* Student Profile
* Eligible Jobs
* Job Card
* Application Status
* Placement Dashboard
* Application Integration
* Parent and Child component demonstrations
* Data Binding demonstration
* Welcome Message

## Deployment

The project was deployed to the Salesforce `placementorg` target org using Salesforce CLI.

Deployment command:

```bash
sf project deploy start --target-org placementorg
```

Deployment status was successfully verified.

## Deployment Verification

The following functionality was verified after deployment:

* Salesforce CLI authentication
* Metadata deployment
* Placement Portal UI
* Student Details
* Student Profile
* Eligible Jobs
* Placement Management Dashboard

## Apex Test Verification

Apex tests were executed against the target Salesforce org.

Command:

```bash
sf apex test run --target-org placementorg --wait 10 --result-format human
```

### Test Result

* Tests Run: 1
* Passed: 1
* Pass Rate: 100%
* Fail Rate: 0%

The Apex test execution completed successfully.

## Git and GitHub Workflow

The project is maintained using Git and GitHub.

### Repository

GitHub repository:

```text
https://github.com/Mounisha0114/PlacementManagementSystem
```

### Git Workflow Used

```bash
git add .
git commit -m "Commit message"
git push origin main
```

Feature development was performed using a separate branch:

```bash
git checkout -b feature/placement-verification
```

The feature branch was pushed to GitHub and merged into the `main` branch through a Pull Request.

After merging, the feature branch was deleted locally and remotely.

## Verification Status

| Verification              | Status    |
| ------------------------- | --------- |
| Salesforce Authentication | Completed |
| Metadata Deployment       | Completed |
| Placement Portal          | Verified  |
| Student Profile           | Verified  |
| Eligible Jobs             | Verified  |
| Placement Dashboard       | Verified  |
| Apex Test                 | Passed    |
| Git Commit                | Completed |
| GitHub Push               | Completed |
| Pull Request              | Merged    |
| Main Branch Sync          | Completed |

## Troubleshooting

### Source Tracking Error

If the following error occurs:

```text
Error (NonSourceTrackedOrgError): This org does not have source tracking.
```

Use a deployment command that explicitly specifies the source directory or metadata instead of relying on source tracking.

Example:

```bash
sf project deploy start --target-org placementorg
```

### Check Git Status

```bash
git status
```

The working tree should be clean after committing changes.

### Check Remote Repository

```bash
git remote -v
```

### Check Current Branch

```bash
git branch
```

## Conclusion

The Placement Management System was successfully developed, committed to Git, pushed to GitHub, deployed to Salesforce, and verified through the Salesforce UI and Apex test execution.

The project is maintained in the GitHub `main` branch and the deployment verification workflow has been completed.

```
