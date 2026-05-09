## DAY-2 Notes  

### Salesforce Platform Overview
- Salesforce is a cloud-based CRM platform  
- Built using core components:
  - Apps  
  - Objects  
  - Tabs  
- Uses multi-tenant architecture (multiple users share same system)  

---

### What is an App?
- An App is a collection of related features  
- Includes objects, tabs, and tools  
- Designed for a specific business purpose  
- Example:
  - Sales App  
  - Service App  

---

### What is an Object?
- An Object is a database table used to store data  
- Contains:
  - Records (rows)  
  - Fields (columns)  
- Types:
  - Standard Objects (Account, Contact, Opportunity)  
  - Custom Objects (created by user)  

---

### What is a Tab?
- A Tab is a user interface element  
- Used to access objects and features  
- Helps users navigate the platform  

---

### CRM in Salesforce Platform
- CRM concepts are implemented as Objects:
  - Account → Company  
  - Contact → Person  
  - Opportunity → Deal  
- These objects are grouped inside Apps  
- Tabs are used to access them  

---

### Configuration vs Coding  

#### Configuration (No Code)
- Done using point-and-click tools  
- Used for simple requirements  
- Examples:
  - Creating fields  
  - Validation rules  

#### Coding (Apex)
- Used for complex logic  
- Provides flexibility  
- Examples:
  - Custom automation  
  - Integration with external systems  

---

### When to Use Configuration
- Requirement is simple  
- Can be solved using built-in tools  
- No complex logic required  

---

### When to Use Coding
- Complex logic is needed  
- Custom functionality required  
- Integration with other systems  

---

### Salesforce Architecture
- Based on multi-tenant model  
- Multiple organizations share same platform  
- Data is securely separated  
- Updates are automatic  

---

### How Developers Extend Salesforce
- Apex → backend logic  
- APIs → integration with external systems  
- UI (Lightning) → interface customization  

---

### System Example (College Admission)
- App:
  - College Admission App  

- Objects:
  - Student  
  - Application  
  - Course  
  - Admission  

- User Interaction:
  - Student submits application  
  - Admin reviews application  
  - Admission status updated  
  - Data accessed through tabs  

---

### Key Takeaways
- Salesforce platform is built using Apps, Objects, Tabs  
- CRM concepts are implemented as Objects  
- Configuration is used for simple tasks  
- Coding is used for complex logic  
- Multi-tenant architecture enables scalability  
