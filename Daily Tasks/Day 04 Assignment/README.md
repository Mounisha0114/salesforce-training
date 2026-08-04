# Day 4 – Your First Lightning Web Component (LWC)

## Part 1 – Think Before Coding
Before starting the implementation, it is important to understand the purpose of the User Interface (UI), JavaScript, and Apex in Salesforce Lightning Web Components.
### 1. Why do users need a graphical interface?
Users need a graphical interface because it provides an easy and interactive way to work with the application. Instead of writing database queries or code, users can perform tasks using buttons, forms, menus, and pages.
---
### 2. Why can't users directly execute SOQL queries?
Users cannot directly execute SOQL queries because SOQL is a developer query language used to retrieve data from Salesforce. Allowing users to run queries directly could expose sensitive information and create security risks. Instead, Apex classes execute SOQL queries securely and return only the required data to the Lightning Web Component.
---
### 3. Why is JavaScript required in LWC?
JavaScript is used to implement the business logic of the Lightning Web Component. It is responsible for:
- Creating variables
- Handling button click events
- Updating data dynamically
- Performing calculations
- Managing component behavior
Without JavaScript, an LWC would only display static HTML content.
---
### 4. What responsibilities belong to the UI?
The User Interface (UI) is responsible for:
- Displaying information to users
- Accepting user input
- Showing buttons, forms, and messages
- Providing an interactive experience
- Displaying data received from Apex
---
### 5. Which responsibilities should remain in Apex?
Apex is responsible for server-side processing, including:
- Executing SOQL queries
- Performing DML operations (Insert, Update, Delete)
- Implementing business logic
- Validating data
- Communicating with the Salesforce database
- Returning processed data to Lightning Web Components
---
## Part 2 – Understanding Lightning Web Components (LWC)
A Lightning Web Component (LWC) is built using three essential files. Each file has a specific responsibility that helps create a complete and functional component.
### 1. HTML File (`placementHome.html`)
The HTML file is responsible for designing the user interface of the component. It defines what the user sees on the screen.
### Responsibilities
- Page Layout
- Buttons
- Text
- Images
---
### 2. JavaScript File (`placementHome.js`)
The JavaScript file contains the logic of the component. It manages variables, handles user interactions, and controls the component's behavior.
### Responsibilities
- Logic
- Variables
- Events
- Communication
---
### 3. Meta XML File (`placementHome.js-meta.xml`)
The Meta XML file is used to configure the Lightning Web Component. It makes the component available in Lightning App Builder and specifies where it can be used.
### Responsibilities
- Making the component available in Lightning App Builder
- Defining where the component can be used
---
## Part 3 – Hands-on Activity 1
### Objective
In this activity, I created my first **Lightning Web Component (LWC)** named **placementHome**. The component displays a welcome message and is deployed to a Salesforce Lightning Home Page using **Lightning App Builder**.
### Component Name
```
placementHome
```
### Displayed Message
```
Welcome to Vishnu Placement Portal
```
---
## Step 1: Create the Lightning Web Component
Created a new Lightning Web Component named **placementHome**.
---
## Step 2: HTML File (`placementHome.html`)
The HTML file is responsible for displaying the user interface.
```html
<template>
    <lightning-card title="Placement Portal" icon-name="standard:education">
        <div class="slds-p-around_medium">
            <h1 class="slds-text-heading_large">
                Welcome to Vishnu Placement Portal
            </h1>
        </div>
    </lightning-card>
</template>
```
---
## Step 3: JavaScript File (`placementHome.js`)
The JavaScript file contains the component class.
```javascript
import { LightningElement } from 'lwc';
export default class PlacementHome extends LightningElement {
}
```
---
## Step 4: Meta XML File (`placementHome.js-meta.xml`)
The Meta XML file exposes the component so that it can be added to Lightning pages.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<LightningComponentBundle xmlns="http://soap.sforce.com/2006/04/metadata">
    <apiVersion>67.0</apiVersion>
    <isExposed>true</isExposed>

    <targets>
        <target>lightning__HomePage</target>
        <target>lightning__AppPage</target>
        <target>lightning__RecordPage</target>
    </targets>
</LightningComponentBundle>
```
---
## Step 5: Deployment
The component was deployed to the Salesforce org using the Salesforce CLI.
```bash
sf project deploy start
```
After successful deployment, the component was added to the **Home Page** using **Lightning App Builder** and activated.
---
## Output
The Home Page displays:
```
Placement Portal
Welcome to Vishnu Placement Portal
```
---
## Part 4 – Hands-on Activity 2
## Initial Output
```
Student Name : Rahul
Roll Number : 22B81A0501
Department : CSE
```
---
## Step 1: Update the JavaScript File (`placementHome.js`)
The variables are declared inside the JavaScript file.
```javascript
import { LightningElement } from 'lwc';
export default class PlacementHome extends LightningElement {
    studentName = 'Rahul';
    rollNumber = '22B81A0501';
    department = 'CSE';
}
```
---
## Step 2: Update the HTML File (`placementHome.html`)
The variables are displayed using **Data Binding** with curly braces `{}`.
```html
<template>
    <lightning-card title="Placement Portal" icon-name="standard:education">
        <div class="slds-p-around_medium">
            <h1 class="slds-text-heading_large">
                Welcome to Vishnu Placement Portal
            </h1>
            <br/>
            <p><strong>Student Name :</strong> {studentName}</p>
            <p><strong>Roll Number :</strong> {rollNumber}</p>
            <p><strong>Department :</strong> {department}</p>
        </div>
    </lightning-card>
</template>
```
---
## Step 3: Modify the Variable Values
The values in the JavaScript file were changed as shown below.
```javascript
studentName = 'JAHNAVI';
rollNumber = '22B81A0501';
department = 'CSE';
```
---
## Updated Output
```
Student Name : JAHNAVI
Roll Number : 22B81A0501
Department : CSE
```
---
## Part 5 – Hands-on Activity 3
## Button Label
```
Show Welcome Message
```
---
## Displayed Message
```
Welcome to Salesforce Development.
```
---
## Step 1: Update the JavaScript File (`placementHome.js`)
A variable named `message` is created to store the welcome message. The `showMessage()` method is called when the button is clicked.
```javascript
import { LightningElement } from 'lwc';
export default class PlacementHome extends LightningElement {
    studentName = 'JAHNAVI';
    rollNumber = '22B81A0501';
    department = 'CSE';
    message = '';
    showMessage() {
        this.message = 'Welcome to Salesforce Development.';
    }
}
```
---
## Step 2: Update the HTML File (`placementHome.html`)
A Lightning Button is added to the page. The `onclick` event calls the `showMessage()` method. The message is displayed using Data Binding.
```html
<template>
    <lightning-card title="Placement Portal" icon-name="standard:education">
        <div class="slds-p-around_medium">
            <h1 class="slds-text-heading_large">
                Welcome to Vishnu Placement Portal
            </h1>
            <br/>
            <p><strong>Student Name :</strong> {studentName}</p>
            <p><strong>Roll Number :</strong> {rollNumber}</p>
            <p><strong>Department :</strong> {department}</p>
            <br/>
            <lightning-button
                label="Show Welcome Message"
                variant="brand"
                onclick={showMessage}>
            </lightning-button>
            <p class="slds-m-top_medium">
                {message}
            </p>
        </div>
    </lightning-card>
</template>
```
---
## Output Before Clicking the Button
```
Placement Portal
Welcome to Vishnu Placement Portal
Student Name : JAHNAVI
Roll Number : 22B81A0501
Department : CSE
[Show Welcome Message]
```
---
## Output After Clicking the Button
```
Placement Portal
Welcome to Vishnu Placement Portal
Student Name : JAHNAVI
Roll Number : 22B81A0501
Department : CSE
[Show Welcome Message]
Welcome to Salesforce Development.
```
---
## Part 6 – Hands-on Activity 4
## Step 1: Update the JavaScript File (`placementHome.js`)
Create a variable named `status` and initialize it with **"Not Applied"**. Create a method named `changeStatus()` to update the status when the button is clicked.
```javascript
import { LightningElement } from 'lwc';
export default class PlacementHome extends LightningElement {
    studentName = 'JAHNAVI';
    rollNumber = '22B81A0501';
    department = 'CSE';
    message = '';
    status = 'Not Applied';
    showMessage() {
        this.message = 'Welcome to Salesforce Development.';
    }
    changeStatus() {
        this.status = 'Applied';
    }
}
```
---
## Step 2: Update the HTML File (`placementHome.html`)
Display the current status and add an **Apply** button. The button calls the `changeStatus()` method when clicked.
```html
<template>
    <lightning-card title="Placement Portal" icon-name="standard:education">
        <div class="slds-p-around_medium">
            <h1 class="slds-text-heading_large">
                Welcome to Vishnu Placement Portal
            </h1>
            <br/>
            <p><strong>Student Name :</strong> {studentName}</p>
            <p><strong>Roll Number :</strong> {rollNumber}</p>
            <p><strong>Department :</strong> {department}</p>
            <br/>
            <lightning-button
                label="Show Welcome Message"
                variant="brand"
                onclick={showMessage}>
            </lightning-button>
            <p class="slds-m-top_medium">
                {message}
            </p>
            <br/>
            <p><strong>Status :</strong> {status}</p>
            <lightning-button
                label="Apply"
                variant="success"
                onclick={changeStatus}>
            </lightning-button>
        </div>
    </lightning-card>
</template>
```
---
## Output Before Clicking the Button
```
Placement Portal
Welcome to Vishnu Placement Portal
Student Name : JAHNAVI
Roll Number : 22B81A0501
Department : CSE
[Show Welcome Message]
Status : Not Applied
[Apply]
```
---
## Output After Clicking the Button
```
Placement Portal
Welcome to Vishnu Placement Portal
Student Name : JAHNAVI
Roll Number : 22B81A0501
Department : CSE
[Show Welcome Message]
Welcome to Salesforce Development.
Status : Applied
[Apply]
```
---
## Part 7 – Mini Project Enhancement
### Step 1: Update the JavaScript File (`placementHome.js`)
```javascript
import { LightningElement } from 'lwc';
export default class PlacementHome extends LightningElement {
    studentName = 'JAHNAVI';
    rollNumber = '22B81A0501';
    department = 'CSE';
    message = '';
    status = 'Not Applied';
    todayDate = new Date().toLocaleDateString();
    companies = 25;
    jobs = 63;
    applications = 5;
    showMessage() {
        this.message = 'Welcome to Salesforce Development.';
    }
    changeStatus() {
        this.status = 'Applied';
    }
}
```
---
## Step 2: Update the HTML File (`placementHome.html`)
```html
<template>
    <lightning-card title="Placement Portal" icon-name="standard:education">
        <div class="slds-p-around_medium">
            <h1 class="slds-text-heading_large">
                Welcome Student
            </h1>
            <h2 class="slds-text-heading_medium">
                Hello {studentName}
            </h2>
            <br/>
            <p><strong>Today's Date :</strong> {todayDate}</p>
            <br/>
            <p><strong>Student Name :</strong> {studentName}</p>
            <p><strong>Roll Number :</strong> {rollNumber}</p>
            <p><strong>Department :</strong> {department}</p>
            <br/>
            <p><strong>Number of Companies :</strong> {companies}</p>
            <p><strong>Number of Jobs :</strong> {jobs}</p>
            <p><strong>Applications Submitted :</strong> {applications}</p>
            <br/>
            <lightning-button
                label="Show Welcome Message"
                variant="brand"
                onclick={showMessage}>
            </lightning-button>
            <p class="slds-m-top_medium">
                {message}
            </p>
            <br/>
            <p><strong>Status :</strong> {status}</p>
            <lightning-button
                label="Apply"
                variant="success"
                onclick={changeStatus}>
            </lightning-button>
        </div>
    </lightning-card>
</template>
```
---
## Output
The Placement Management System dashboard displays:
```
Placement Portal
Welcome Student
Hello JAHNAVI
Today's Date : 31/07/2026
Student Name : JAHNAVI
Roll Number : 22B81A0501
Department : CSE
Number of Companies : 25
Number of Jobs : 63
Applications Submitted : 5
[Show Welcome Message]
Welcome to Salesforce Development.
Status : Applied
[Apply]
```
---
## Part 8 – Understanding Data Binding
## Initial Output
```
Hello Rahul
```
---
## Step 1: Update the HTML File (`placementHome.html`)
Use the `studentName` variable inside the HTML template.
```html
<template>
    <lightning-card title="Placement Portal" icon-name="standard:education">
        <div class="slds-p-around_medium">
            <h2>Hello {studentName}</h2>
        </div>
    </lightning-card>
</template>
```
---
## Step 2: Update the JavaScript File (`placementHome.js`)
Initially, set the value of `studentName` to **Rahul**.
```javascript
import { LightningElement } from 'lwc';
export default class PlacementHome extends LightningElement {
    studentName = 'Rahul';
}
```
---
## Initial Output
```
Hello Rahul
```
---
## Step 3: Change the Variable Value
Modify the value of `studentName` in the JavaScript file.
```javascript
studentName = 'JAHNAVI';
```
---
## Updated Output
```
Hello JAHNAVI
```
---
## Part 9 – Interview Questions
### 1. What is Lightning Web Components (LWC)?
Lightning Web Components (LWC) is Salesforce's modern user interface framework used to build fast, reusable, and interactive applications. It is based on standard web technologies such as **HTML, JavaScript, and CSS**.
---
### 2. Why did Salesforce introduce LWC?
Salesforce introduced Lightning Web Components to:
- Improve application performance
- Use modern web standards
- Simplify component development
- Create reusable components
- Provide a better user experience than Aura Components
---
### 3. Difference between LWC and Aura
| LWC | Aura |
|-----|------|
| Modern framework | Older framework |
| Built using HTML, JavaScript, and CSS | Uses Aura-specific syntax |
| Faster and lightweight | Comparatively slower |
| Better performance | Lower performance |
| Easier to learn and maintain | More complex to develop |
---
### 4. What are the three files inside an LWC?
Every Lightning Web Component consists of three main files:
- **HTML File (`placementHome.html`)** – Defines the user interface.
- **JavaScript File (`placementHome.js`)** – Contains variables, logic, and event handling.
- **Meta XML File (`placementHome.js-meta.xml`)** – Makes the component available in Lightning App Builder and defines where it can be used.
---
### 5. Why is JavaScript required?
JavaScript is required to:
- Create variables
- Handle button click events
- Implement business logic
- Update data dynamically
- Control component behavior
Without JavaScript, the component can only display static content.
---
### 6. What is Data Binding?
Data Binding is the process of displaying JavaScript variable values in the HTML template using curly braces `{}`.
When the value of a JavaScript variable changes, the UI automatically updates without modifying the HTML code.
---
### 7. Can LWC directly execute SOQL?
No. Lightning Web Components cannot execute SOQL queries directly.
LWC calls an Apex class, and the Apex class executes the SOQL query to retrieve data from the Salesforce database.
---
### 8. Why does LWC need Apex?
LWC uses Apex to:
- Execute SOQL queries
- Perform database operations (Insert, Update, Delete)
- Implement business logic
- Validate data
- Return Salesforce data to the component
---
### 9. Where is the component deployed?
A Lightning Web Component can be deployed using **Lightning App Builder** on:
- Home Page
- App Page
- Record Page
---
### 10. Explain the component you built today.
Today, I developed a **Placement Management System** using Lightning Web Components.
The component displays:
- Placement Portal
- Welcome Student message
- Student Name
- Roll Number
- Department
- Today's Date
- Number of Companies
- Number of Jobs
- Applications Submitted
- Show Welcome Message button
- Apply button
- Application Status
The component demonstrates important LWC concepts such as **Data Binding**, **Event Handling**, **JavaScript Variables**, and **Lightning App Builder Deployment**. All values are currently hard-coded, and in future sessions they can be retrieved dynamically from Salesforce using Apex.

---
