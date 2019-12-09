---
sectionid: migration
sectionclass: h2
title: Questions to Ask
parent-id: migration
---

Even if an Azure environment is built out and ready to host migrated workloads, a variety of questions could delay individual server migrations. You can assess the readiness of moving a particular workload by how well you understand these questions.

It is always good to have questions to ask! So here are a few:

### Environment
- Where is the workload running? (On-prem, colo, other cloud). Is there a budgetary need to move workloads out of this location, e.g., expiring lease? (Investigate timing if so.)
- What is the relationship with the workload owner(s)? Are they in the room for the discussion? Are they on board with putting their workload in the cloud?
- Are there other workloads similar to this which have already been migrated to Azure?
- What's the the regulatory environment around the customer/workload (FINRA, FedRamp, etc.) and how will it impact architecture/design for the environment?

### Application
- Is there an application inventory for all the servers considered for migration?
- Are there third-party applications loaded on the server(s)? Is the ISV still supporting it?
- Are the apps available in the Marketplace? Could the app(s) be restaged on a Azure native image?
- Lift and shift vs replatforming: Is there an option to move to a SaaS options for the app?
- WS: Can the app run on Windows 2012 or later? Or is it necessary to leave it on Windows Server 2008/R2?

### OS
- Windows 2008 was offered as a 32-bit version. Are any migrations of 32-bit servers planned?
- How is the server being updated, e.g., Windows Update, SCCM, Ubuntu Advantage, etc. Is there a plan in place to extend this to the cloud or will updates move to Azure Update Management?
- How is the system being monitored e.g., via on on-prem agent (SCOM, SolarWinds, etc.). Will this be maintained on Azure?
- Do security/audit events roll up into a SIEM, i.e., Splunk, IBM Qradar, etc? Will this need to be maintained as is?
- Does this server require hands-on (console) access? Does the end-user customer expect to be able to access it via RDP/ssh?
	
### Process
- Is there a migration window for the workload? How long is it?
	
### Migration 
- What is the timing for migration? Is there a end-date that must be met?
- Are all of the people involved or affected migration aware of the process?
