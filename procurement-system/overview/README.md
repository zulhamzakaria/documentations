# Procurement System Overview

## 1. Purpose
An enterprise grade portfolio project for capturing the purchasing life-cycle. 
The system is designed with maintainability in mind and is SaaS ready.  

---
## 2. Scope
**Included:**
- PR -> PO -> GRN
- JWT-based authentication and authorization
- Modular Monolith(MM) with Vertical Slice(VSA) and Domain-Driven Design(DDD)
- Audit trail and status tracking
- Admin
- Suppliers
- Unit testing (Behavioural)
**Not Included:**
- Quotation, DO

---
## 3. Case Study
**Problem:** SMEs struggling with inconsistencies of manual approvals, visibility of purchases and audit reportings

**Solution:** Procurement system that automates workflow
1. Clerk Submits a PR
2. Finance Officer(FO) and Finance Manager(FM) approve in sequence based on a clearly defined policy
3. Upon Approval, a PO is created
4. On receiveng goods, the GRN is logged and the cycle is completed

This ensures that every action is accounted for and historical data is auditable

---
## 4. Trade-offs/Decisions
- **Architecture:** MM over Microservices because of simplicity and maintainability
- **Workflow Design:** Linear approval workflow based on policy at the cost of flexibility and complex approval paths
- **SaaS Readiness:** Implemented TenantId on core aggregates. Single tenant deployment initially for POC
- **Authentication & Security:** JWT authentication is used for stateless access control. Much more simpler for a portfolio project compared to OAuth, SSO
- **Unit Testing:** Unit-tests are done on core domain rules and system behaviors. Limited end-to-end tests to maintain speed and reduce complexity 

---
## 5. Modules
- **PR Module:** Create, submit, approve, or reject purchase requests. Enforces workflow policies.  
- **PO Module:** Generates purchase orders automatically from approved PRs. Tracks status and approvals.  
- **GRN Module:** Logs receipt of goods, completes PR->PO->GRN lifecycle.  
- **Auth Module:** Role-based access(RBA) control with JWT. Ensures only authorized employees perform actions.  
- **Audit & Reporting Module:** Tracks all state changes and user actions for traceability.  
- **Admin Module:** Manages users, roles, approval policies, tenant configuration, and reference data required for system governance.

---
## 6. Tools
- .Net 8, NeonTech db, Azure App Service