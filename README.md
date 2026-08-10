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
