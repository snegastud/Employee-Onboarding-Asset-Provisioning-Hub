# Employee-Onboarding-Asset-Provisioning-Hub

`Tell me about yourself`

Hi, I’m Snega, and I have around 3.4 years of experience as an SAP BTP Developer, mainly working on enterprise application development using SAP Business Technology Platform.

My core expertise is in SAP CAP using Node.js, where I work with CDS modeling, service development, custom handlers, business logic, and OData services. I also have experience working with SAP Fiori/UI5 for application development.

I have hands-on experience with key SAP BTP services such as SAP HANA Cloud, XSUAA, Destination Service, Application Logging, Alert Notification Service, and Cloud Foundry.

I have worked on developing and deploying SAP BTP applications and implementing business requirements using CAP and Node.js. Overall, my experience is mainly focused on SAP BTP, CAP, Node.js, and Fiori application development.


**Explain the project**

>“Currently, I’m working on an Employee Onboarding and Asset Provisioning application. The main purpose of the application is to simplify the onboarding process between HR, managers, and IT.

>Basically, HR creates the employee and an onboarding request. The manager reviews and approves the request, and after approval, the IT team allocates the required assets such as laptops or ID cards. Once the required assets are allocated, the onboarding is completed and the employee becomes active.

>From the technical side, we developed this application using SAP CAP with Node.js. I worked mainly on the CDS data model, CAP services, business logic and validations, and the Fiori application. We used SAP HANA Cloud as the database and deployed the application on SAP BTP Cloud Foundry.”


**Business flow**

```
HR
 ↓
Create Employee
 ↓
Employee = ONBOARDING
 ↓
Create OnboardingRequest
 ↓
Request = PENDING
 ↓
Manager Approves
 ↓
Request = APPROVED
 ↓
IT selects available Assets
 ↓
Create AssetAllocation
 ↓
Asset = ALLOCATED
 ↓
OnboardingRequest = COMPLETED
 ↓
Employee = ACTIVE

```

>"Basically, the process starts when HR creates a new employee in the system. At that point, we keep the employee status as Onboarding because the employee hasn't completed the onboarding process yet.

>Once the employee is created, we create an onboarding request for that employee, and initially the request is in Pending status. The manager then reviews the request and either approves or rejects it.

>If the manager approves it, the request moves to the asset management part. The IT team checks the available assets and assigns the required assets, like a laptop or ID card, to that employee. We store each assignment in the Asset Allocation table and update the asset status to Allocated.

>Once all the required assets are assigned, we complete the onboarding request and update the employee status from Onboarding to Active. So basically, the flow is HR creates the employee, manager approves the onboarding, IT allocates the assets, and finally the employee becomes Active."

**Relationship design**

 `Employees → OnboardingRequest`

>Relationship: One-to-One (1:1)

>One employee has one onboarding request in your current business design.

`Employees → AssetAllocations`

>Relationship:one to many(1:N)

>One employee can receive multiple assets.

`"For example`, Rahul may receive a laptop, monitor, and ID card. Each of those assignments is stored as a separate asset allocation record. So one employee can have multiple asset allocation records."

`Assets → AssetAllocations`

Relationship: One-to-Many (1:N)

>"The Assets entity stores the company's asset master data, such as laptops, monitors, and ID cards. AssetAllocation stores which asset was assigned to which employee. One asset can have multiple allocation records over its lifetime because it can be returned and later assigned to another employee."

`For example`

"For example, if LAP001 is initially assigned to Rahul and later Rahul returns it, the same laptop can be assigned to another employee.

`End flow`

>"Once the required onboarding activities, including asset allocation, are completed, we mark the onboarding request as Completed and update the employee status to Active."

`Project flow`

```

                 EMPLOYEE ONBOARDING
                           │
                           ▼
                     HR creates
                      Employee
                           │
                           ▼
                Employee Status
                   = ONBOARDING
                           │
                           ▼
                 Create Onboarding
                      Request
                           │
                           ▼
                Request Status
                   = PENDING
                           │
                           ▼
                    Manager Review
                     /           \
                    /             \
               APPROVE           REJECT
                  │                 │
                  ▼                 ▼
              APPROVED           REJECTED
                  │
                  ▼
             ASSET MANAGEMENT
                  │
                  ▼
             IT checks Assets
                  │
                  ▼
        Is Asset AVAILABLE?
             /           \
           Yes            No
            │              │
            ▼              ▼
      Allocate Asset    Choose another
            │
            ▼
     Create AssetAllocation
            │
            ▼
    Asset Status = ALLOCATED
            │
            ▼
   Are all required assets
         allocated?
        /          \
      No            Yes
      │              │
      │              ▼
      │       Complete Onboarding
      │              │
      │              ▼
      │       Request = COMPLETED
      │              │
      │              ▼
      │       Employee = ACTIVE
      │
      └── IT continues allocation

```

**Two modules working process**

>"I worked mainly on two modules, Employee Onboarding and Asset Management. The onboarding module controls the employee's onboarding request and approval process. Once the request is approved, it moves into the asset management part, where IT assigns the required assets to the employee."

"**What business problem were you solving in your project?**"

>"The main problem was that employee onboarding was being handled manually between HR, managers, and the IT team. When a new employee joined, HR had to share the employee details with the concerned teams, the manager had to approve the onboarding, and then IT had to arrange assets like laptops and ID cards.

>Because these activities were handled separately, it was difficult to track the current status. Sometimes approvals were delayed, and IT also had to manually check whether the required assets were available.

>So we developed an Employee Onboarding and Asset Provisioning application where we brought these activities into one application. HR can create the employee and onboarding request, the manager can approve or reject it, and once approved, IT can see the available assets and allocate them to the employee.

>The application maintains the complete status of the onboarding process, so HR and IT can see whether the request is pending, approved, assets are allocated, or onboarding is completed."

**Problems**
• Manual coordination
• Approval delays
• Difficult to track status
• Asset availability not centralized
• No single place to see onboarding progress

**"How did you implement this?"**

>"We implemented the application using SAP CAP with Node.js on SAP BTP. I started with the CDS data model for employees, onboarding requests, assets, and asset allocations.

>Then I exposed those entities through OData V4 services. On top of that, I implemented the business validations and workflow logic in CAP service handlers. For example, before creating an onboarding request, we validate the employee and check for duplicate requests. For approval and asset allocation, we implemented business actions to control the status changes and asset availability.

>We developed the user interface using SAP Fiori/UI5. Different users have different responsibilities, so we used XSUAA for authentication and role-based authorization.

>We also used Application Logging for application and business activity logs, and Alert Notification for important events such as pending or delayed activities. Finally, we deployed the application to BTP Cloud Foundry using MTA."


**"Explain the project end to end."**

>"The application starts with HR creating an employee and an onboarding request. The employee is initially maintained with an onboarding status, and the request is created in pending status.

>Before saving the request, we validate the employee details and make sure there isn't already an active onboarding request for that employee.

>Once the request is submitted, the manager reviews it from the Fiori application. If the manager approves it, the request moves to the asset provisioning stage. If it's rejected, we maintain the rejection status and remarks.

>For an approved request, the IT team checks the available assets. When they allocate an asset, we create an asset allocation record and change the asset status from available to allocated.

>Once all the required assets are assigned, the onboarding request is completed and the employee status is changed from onboarding to active.

>Throughout the process, we maintain the relevant application logs and notifications, so the business users can track what happened and the IT team can react to pending activities."

**Employee onboarding creation workflow**

```
HR
 ↓
Create Employee
 ↓
BEFORE CREATE
 ├─ Validate mandatory fields
 ├─ Check duplicate employee
 └─ Set status = ONBOARDING
 ↓
Employee created
 ↓
AFTER CREATE
 ↓
OnboardingRequest automatically created
 ├─ Employee = newly created employee
 ├─ RequestDate = current date
 ├─ Status = PENDING
 └─ ManagerRemarks = NULL

```
**Manager workflow**

```

HR creates Employee
        ↓
BEFORE CREATE
Validate employee
        ↓
Employee created
        ↓
AFTER CREATE
Automatically create OnboardingRequest
        ↓
Status = PENDING
ManagerRemarks = NULL
        ↓
Manager reviews
        ↓
   ┌─────────────┐
   │             │
Approve       Reject
   │             │
   ↓             ↓
Remark         Reason
required       required
   │             │
   ↓             ↓
APPROVED      REJECTED

```

**Next process: Asset Allocation**

```
OnboardingRequest
      ↓
Status = APPROVED
      ↓
IT checks Assets
      ↓
Find asset with availabilityStatus = AVAILABLE
      ↓
Create AssetAllocation
      ↓
Asset status → ALLOCATED
      ↓
Allocation status → ALLOCATED

```

`validations:`

1.Does the onboarding request exist?
2.Is the request APPROVED?
3.Does the employee exist?
4.Does the asset exist?
5.Is the asset actually AVAILABLE?
6.Is the asset already allocated to this employee?
7.If everything is valid → create allocation.
8.Update asset status to ALLOCATED.


**Return allocation workflow**

```
Employee
   ↓
OnboardingRequest
   ↓
Manager APPROVED
   ↓
Asset Allocation
   ↓
Asset AVAILABLE → ALLOCATED
   ↓
       NEXT
   ↓
Return Asset
   ↓
Allocation ALLOCATED → RETURNED
   ↓
Asset ALLOCATED → AVAILABLE

```

**Interview answer for project related:**

>“It was a new application development project, but I joined the project after the initial development had already started. The team had already set up the basic application structure and some of the initial development. After I joined, I was mainly involved in developing two modules — Employee Onboarding and Asset Management.”

**"How many members were in your project?"**

>“We had around 6 members in the team. We divided the development based on modules, and I was mainly responsible for Employee Onboarding and Asset Management.”

**"How many modules did you build?"**

>“The application had multiple areas, but my main involvement was in two modules — Employee Onboarding and Asset Management.”

**So what did you do after joining?**

>“When I joined, the basic application structure was already there. I first understood the existing CDS model and service structure, and then I started working on the Employee Onboarding and Asset Management modules. I worked on the entities, OData services, business validations, actions and the related Fiori screens.”

**“Are you integrating with S/4HANA in this project?”**

>“No, S/4HANA is not directly involved in the core onboarding and asset management flow I worked on. My work was mainly on the CAP application and its own HANA Cloud data model.”

**“What was your role?”**

>“I was mainly responsible for the Employee Onboarding and Asset Management modules. I worked on the CAP backend, business logic and validations, OData services, and the related Fiori/UI5 screens.”

**Project duration**

>“The project started around early 2025, and I joined around May 2025. By the time I joined, the initial setup and some development were already completed. I then worked mainly on the Employee Onboarding and Asset Management m

**“Why did you join the project in between?”**

>“The project had already started, and one of the team members who was working on that area left the organization. So my team lead assigned that work to me. I took over the existing work, understood the application and the existing code, and then continued the development of the Employee Onboarding and Asset Management modules.

**“So did you build those modules from scratch?”**

>“The overall project was already in development when I joined. For my assigned modules, I took over the existing work and continued the development. I worked on the required functionality, business logic, validations and UI changes based on the requirements.

**“Why did your lead choose you?**

>“At that point, I had experience with SAP CAP and Node.js, so my lead assigned the work to me. I was already familiar with the technology stack, so I could take over the existing implementation and continue the development.”


**“What Fiori screens did you work on?”**

>“On the Fiori side, we mainly had screens for onboarding request management, onboarding approval, asset management, and asset allocation. HR could manage the onboarding information, managers could approve or reject requests, and IT could view and allocate available assets.”

**Fiori applications**

```
Your Fiori application — keep these 4 screens

1. Employee / Onboarding List

HR opens the application and sees the employees/onboarding requests.

It can show:

Employee name
Employee number
Department
Joining date
Onboarding status

2. Onboarding Request Details

When HR/Manager selects a request, they can see:

Employee details
Request date
Request status
Manager remarks

For a manager, this screen has:

Approve | Reject

3. Asset Management

This is mainly for IT.

IT can see the asset master/list:

Asset code
Asset name
Category
Availability status

For example:

Laptop — LAP001 — AVAILABLE

4. Asset Allocation

IT selects the employee and available asset and performs the allocation.

For example:

Employee: Rahul
Asset: LAP001
Allocation Date: 10-Jun-2025
Status: ALLOCATED

After allocation, the asset status changes:

AVAILABLE → ALLOCATED

```

**After you complete the assigned modules,What did you work on next?**

`Next flow`

>Module development → Testing → Bug fixing → Code review → Deployment → Production support.

**“Once you implemented the modules, what was your next task?”**

>“Once I completed the assigned functionality, I moved to testing. I tested the different business scenarios, including the onboarding request, approval flow, asset availability and asset allocation. During testing, if I found any issues, I fixed them and retested the functionality. After that, I raised the changes for code review and then supported the deployment activities.”

**“What kind of testing did you do?”**

>“I mainly tested the APIs and the Fiori application. For the CAP services, I checked the CRUD operations, validations and custom actions. From the UI side, I tested the complete user flow, for example creating an onboarding request, approving it, allocating an available asset and verifying the status changes.”

**“What happened after testing?”**

>“After testing was completed, I shared the changes with my team lead for review. Based on the review comments, I made the required changes. Once everything was approved, we moved the What happened after code review?”changes through the deployment process.”

**What happened after code review?”**

>“After the code review, if there were any comments, I addressed them and pushed the updated changes. Once the reviewer approved the pull request, the changes were merged into the appropriate branch. Then the CI/CD pipeline was triggered to build and deploy the application to the test or QA environment. After deployment, we performed another round of testing and supported the QA team in resolving any issues.”


**“Did you deploy directly to production?”**

>“No, we normally didn't move changes directly to production. First, the changes went to the test or QA environment. After successful testing and business validation, the changes were promoted to the production environment through the deployment process.”

**“What kind of logs did you analyze?”**

>“Mostly, I checked the application logs when there was an issue in the CAP backend. For example, if an API was failing or a business action was not working as expected, I checked the application logs to identify the error, such as validation errors, database errors, authorization issues, or exceptions from the service handler.”

`The types of logs you can mention`


| Log type                    | What you check                                             |
| --------------------------- | ---------------------------------------------------------- |
| **Application logs**        | CAP Node.js errors, exceptions, business logic failures    |
| **Request/API logs**        | API request, response, HTTP status such as 400/401/403/500 |
| **Database-related errors** | SQL/HANA errors, failed queries, constraint issues         |
| **Authorization errors**    | 401/403, missing scopes/roles                              |
| **UI/browser console**      | Fiori/UI5 JavaScript or OData errors                       |

**Local / Development — Debugging with Breakpoints**

`step we were followed`

```
Step 1 — Start your CAP application

In your CAP project:

cds watch

Your CAP service starts locally.

For example:

http://localhost:4004
Step 2 — Find the service handler

Suppose your logic is in:

srv/
   onboarding-service.js

You may have something like:

this.on('create', 'AssetAllocations', async (req) => {

    // business logic

});
Step 3 — Put a breakpoint

Open the project in VS Code.

Put a breakpoint on the line where you want execution to stop.

For example:

this.on('create', 'AssetAllocations', async (req) => {

    const assetId = req.data.asset_ID;  // ← breakpoint

});
Step 4 — Start the Node.js debugger

You can start your CAP application in debug mode, for example:

cds watch --inspect

Then attach the VS Code debugger to the Node.js process.

Step 5 — Trigger the request

Now open your Fiori application and perform the actual operation:

Employee → Select Asset → Allocate

The Fiori application sends an OData request to your CAP backend.

The debugger should stop at your breakpoint.

Step 6 — Inspect the data

Now you can inspect values such as:

req.data.employeeAsset_ID
req.data.asset_ID
req.data.allocationStatus

You can also step through the code using:

Step Over → execute the next line
Step Into → enter a function
Step Out → come back from a function
Continue → continue execution
Step 7 — Find the actual problem

For example, you might discover:

```

**QA / Production — You normally don't use breakpoints**

For a deployed application, you generally investigate using:

`Application logs + API response + request details`

**command**

cf log application name --recent

real world process in production:

```
PRODUCTION ISSUE
       ↓
User/QA reports issue
       ↓
Collect request details
(Employee / Request / Asset / Time)
       ↓
Check production logs
       ↓
Identify error/root cause area
       ↓
Identify production build / Git commit
       ↓
Check corresponding code
       ↓
Recreate equivalent test data
       ↓
Reproduce in DEV
       ↓
Use debugger/breakpoints
       ↓
Find root cause
       ↓
Fix code
       ↓
Test
       ↓
Code Review
       ↓
Deploy to QA
       ↓
QA verifies
       ↓
Production release

```

Now imagine the issue is a code problem

Suppose production logs show:

Error while creating AssetAllocation
Cannot read properties of undefined

You identify:

Production
   ↓
Build 245
   ↓
Git commit abc123
   ↓
Service handler
   ↓
Problematic code

You check that code version in your development environment.

Then you reproduce the same business scenario using test data.

For example:

Create onboarding request
        ↓
Approve request
        ↓
Select asset
        ↓
Allocate asset
        ↓
Error occurs

Then you put a breakpoint in the CAP handler and inspect the variables.


**“How did you handle production issues?”**

>“Usually the issue came through the support or incident-management process. The support team would create an incident with details such as the application, error message, affected functionality and time of occurrence, and the incident would be assigned to our development team. Once it was assigned to me, I first understood the issue and tried to reproduce it. For production-specific investigation, depending on our access, I checked the application logs in the production Cloud Foundry environment and identified the relevant error and application version. I then reproduced the same scenario in the development environment using test data, debugged the CAP service with breakpoints, fixed the issue, and moved the change through code review and QA before production deployment.”

