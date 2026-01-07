# Procurement System Architecture

## 1. Overview
**Intent:**
- Domain-bounded modularity in which each module owns its own domain rules 
- Maintainability
- Ease of expansion
- Published Modular Monolith(MM) application

---
## 2. Style & Principles
**Modular Monolith:** Well-defined isolated modules that wont break the cohesiveness while keeping tight coupling at bay.
Easier to maintain compared to Microservices considering theres only one codebase. Modules are group based on intent and not technical layers.
**DDD:** Domains that take control of the correctness instead of relying on Services. Domain takes care of business rules and invariants.
Supporting entities are fashioned as Value Objects.
**VSA:** Orchestration based on actions. Functionality based on use-case. Avoid large shared service class

---
## 3. Module Decomposition
**ProcurementModule:** Owns Purchase Request(PR) & Purchase Order(PO). Handles PR->PO pregression. Implements Workflows and Approval.
**ReceivingModule:** Takes care of GRN. Acknowledges the product reception. Happens after PO is done. Does not manage Inventory.
**SupplierManagement:** Handles the Suppliers data for items for Procurement. For data keeping .
**Administration:** System level configurations. Includes Dashboard and system visibility. Devoid of business rules. For data keeping.
**Identity and Access:** Implements JWT for Authentication. Authorization is role-based. System wide. Owns Tenant
**Auditing and History:** Produces summaries of transactions. Highly flexible and is achored by requests and queries. Readonly. 

---
## 4. Module Boundaries & Interaction
Modules are isolated. Each module has its own set of data and rules. Interactions are done thru orchestrators in the form of Actions/Services.
Lightweight Tenant enforcement.  

---
## 5. Project Organization
Slices are implemented based on intents and actions. For example, Procurement/PO/CreatePO. Actions are the orchestrators themselves.
Each module conforms to Clean Architecture. For example, a module has Domain, Infrastructure, Application and API. Slices live inside
Application

---
## 6. Cross-cutting Concerns
**Authentication:** Provided by JWT tokens. Required for every API calls. Validate Tenant context.
**Authorization:** Role-based Access Control. Effective on the Domain level. Can be fused with JWT via roles/claims
**Validation and Error Handling:** Deploys the Result<T> for returning Domain and Validation/Generic errors. 
Use extension class for handling API returns. Use middleware for exceptions handling. Ensure consistent error response.
**Auditing:** Summaries of purchases done and approval flows mainly. Supports visibility, traceability and reporting. 
May expand to include summaries of other relatable query like Active/Inactive Employees.   
**Unit Testing:** Unit tests based on behaviours and business rules. Mimics BDD mindset closest possible

---
## 7. SaaS Readiness Considerations
Scope is limited to only one tenant now but app should be able to handle multiple Tenants. Maybe increase the scope to two Tenants?
Data isolation is based on Tenants. JWT carries Tenant's claim. Data is domain-scoped. For future improvements, Tenants can be split to different
database (can be handled with Options Pattern)

---
## 8. Deployment & Runtime Model
App is published as a single deployable unit and then deployed to Azure App Service. Database is hosted by NeonTech and accessed by EF Core.
Deployment is kept simple to focus on application and domain design.
**Things to consider:** Database per Tenant vs Single database with Tenant-scoping

---
## 9. Out-of-Scope
Inventory. Intergration with the existing Invoice system. Delivery Orders module (open to expansion)

---
## 10. Summaries
Serves as basic guideline for developing the system and future expansions. Subject to changes.

---

