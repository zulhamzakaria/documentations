# Procurement System Domain

## 1. Purpose and Scope
Core business domains (entities, VOs, aggregates, policies), rules and behaviours, workflows, correctness. 
Limited to back-end only. Subject to changes as the project evolves

---
## 2. Bounded Contexts
- **Procurement (PR/PO)**
- **Receiving (GRN)**
- **Administration**
- **Supplier Management**
- **Identity & Access**

---
## 3. Procurement Domain
**Aggregates**
	1. Purchase Request (Root)
		- Intent: For purchasing items
		- Flow: Request -> Submitted -> Approved/Rejected
		- Invariants:
			* Must have one PR item
			* Positive total amount
			* Must follow approval flow		
	2. Purchase Order (Root)
		- Intent: Represents a confirmed order
		- Flow: Created -> Approved -> Issued -> Closed
		- Invariants:
			* Must have one PO item
			* Must follow approval flow
**Entities**
	1. PR/PO Items
		- Properties:
			* Description
			* Quantiy
			* Price (Money)
**Value Objects (VO)**
	1. Money
		- Represents monetary value
		- For avoiding primitive obsession
		- Properties:
			* Amount
			* Currency
		- Prevents:
			* Negative Amount
			* Amount mismatch
	2. Quantity
		- Ensures that it is always positive
**Policies**
	1. PRApproval
		- Defines the approval workflows
	2. POApproval
		- Defines the approval workflows
**Workflow rules**
	1. PRApproval
		- Must be done before creating PO
		- Linear with limited scope for simplicity
		- FO can both reject and approve. FM can only approve
		- Rejected PRs go back to Clerk for modifications
		- Defined by Aministration module
	2. POApproval
		- Must be done before order issuance to the Supplier
		- Linear with limited scope for simplicity
		- FO can both reject and approve. FM can only approve
		- Rejected POs go back to Clerk for modifications
		- Defined by Aministration module
**Documents and Deliverables
Generate POs?

---
## 4. Receiving Domain
**Aggregate**
	1. GRN
		- Intent: For confirming orders reception
		- Invariants:
			* Must be from existing PO
			* Quantity should match
			
---
## 5. Supplier Management Domain 
**Aggregate**
	1. Supplier
		- Intent: Storing approved vendors
		- Data is read-only to Procurement
		- Attributes:
			* Name
			* Basic Details for commmunication
			* Active/Inactive status
		- Invariants
			* Only active Suppliers can be referenced
						
---
## 6. Administration Domain 
**Aggregate**
	1. Workflow Policy
		- Intent: Storing linear approvals workflows for PR/PO
		- Data is read-only to Procurement
	2. Tenant
		- Intent: Storing Tenant for SaaS scoping
	3. Employees
		- System users
		- Attributes
			* Name
			* EmployeeNo
			* Roles
			* Status
			
---
## 7. Identity and Access
Authentication is handled by JWT. Authorization is role/claim-based. Tenant is for SaaS scoping
			
---
## 8. Cross Domain Rules
TenantId is required accross all aggregates. Aggregates are contained inside the bounded context. 
Interaction between aggregates are done thru interfaces
			
---
## 9. Testing Considerations
Validates domain behaviours in state transitions, policy enforcements, and invariants. 
			
---
## 10. Design Decisions
Linear workflow for simplicity. VOs over primitive obsession. Aggregates define correctness. Actions orchestrate.



