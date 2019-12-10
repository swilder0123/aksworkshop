---
sectionid: applications
sectionclass: h2
title: Application Considerations
parent-id: storage
---

### Oracle and RAC

A common configuration for Oracle is built on 'Real Application Clusters' (Oracle RAC). RAC is a key technology to support scale and HA for Oracle databases and applications.

The Oracle Automatic Storage Manager (ASM) Cluster File System (ACFS) provides the underlying cluster FS for RAC.

RAC requires [two things](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/oracle/oracle-vm-solutions#oracle-real-application-cluster-oracle-rac) which are generally not available in the public cloud (including Azure): network multicast and shared disk.

Server storage for ACFS must be [unformatted and 'raw'](https://docs.oracle.com/en/database/oracle/oracle-database/18/cwwin/disk-format-requirements.html#GUID-F44B348B-0070-4CBC-B24A-000284EA5551).

Azure disk can't be used by ACFS...which means that RAC cannot be directly supported on Azure VMs. It is [possible to configure ASM](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/oracle/configure-oracle-asm).

Third-party storage support for RAC is available: [FlashGrid SkyCluster](https://www.flashgrid.io/oracle-rac-in-azure/) also provides a working solution. 

> NOTE: FlashGrid on Azure is [not supported by Oracle](https://www.oracle.com/technetwork/database/options/clustering/overview/rac-cloud-support-2843861.pdf).

> #### Resource
> See the ACFS whitepaper at https://www.oracle.com/technetwork/database/database-technologies/cloud-storage/acfs/learnmore/oracle-acfs-19c-5302856.html.


### SQL Server

This section is under construction!

