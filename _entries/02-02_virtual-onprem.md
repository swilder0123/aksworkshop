---
sectionid: virtualization
sectionclass: h2
title: On-Prem Resourcing
parent-id: virtualization
---

You may run into some heavy duty infrastructure discussion, either in planning a migration or thinking about optimization.

### Topics

- Separation of Duties: Infrastructure Planning and Operations, which may be further subdivided into
	- Server hardware management
	- Networking
	- Storage management
	- Security
- VM operations/provisioning - these are the people who
	- Perform baseline monitoring
	- Provide escalation support for 'server down' calls
	- Boot/reboot/start/stop VMs
- Workload/OS management
	- Monitor OS and basic app issues
	- Create deployments
	- Manage OS deployment templates
	- Deal with clusters and some BC/DR
	- Maintain system updates and report server patch compliance
	- Manage backups
-  Applications/DevOps
	- Monitor applications and investigate application issues
	- Enforce applications security standards
	- Deal with applications infrastructure, i.e., web servers, app server clusters, containers, etc
- Strict role-based access control may mean that admins have very narrowly gated access to the datacenter environment.
- In heavily regulated/high governance environments, expect strong configuration/change controls to delay certain types of requests involving access (i.e., deployment of Azure Migration assessment tools).
- If the VMware environment is run by network operations (fairly typical), network architecture and security discussions can revolve around those topics...
