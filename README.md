# Employee-Onboarding-Asset-Provisioning-Hub

`Tell me about yourself`

"Good morning, and thank you for the opportunity. I'm Snega, and I have around three years of experience as an SAP BTP Developer. During this time, I've mainly worked on building enterprise applications using SAP CAP with Node.js, SAP Fiori/UI5, CDS, OData V4, and SAP HANA Cloud.

My day-to-day work involves designing CDS data models, developing OData services, implementing business logic using CAP event handlers and custom actions, and building Fiori applications based on business requirements.

Apart from application development, I've also worked with various SAP BTP services. I use XSUAA for authentication and role-based authorization, Destination Service to connect with external REST and OData services, Application Logging Service to capture application logs, Alert Notification Service to notify business users about important events, and SAP Build Work Zone to publish applications for different business roles. I've also deployed applications to Cloud Foundry using MTA and worked with Application Autoscaler to handle application scalability.


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
AssetAllocation
      ↓
Check allocation exists
      ↓
Check status = ALLOCATED
      ↓
Allocation → RETURNED
      ↓
Asset → AVAILABLE

```


