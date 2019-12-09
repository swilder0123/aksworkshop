---
sectionid: migration
sectionclass: h2
title: Windows Server Migration to Azure
parent-id: migration
---

Azure Migration uses Azure Site Recovery replication together with a variety of assessment tools to successfully duplicate server on-prem server configurations in Microsoft Azure. These are typically called "lift-and-shift" migrations.

[The Azure Migration Guide](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/migrate/azure-migration-guide/?tabs=Overview) is the principal resource in understanding how Azure Migration works. Specifically it provides a roadmap to important steps for any migration, including assessment, migration, optization and potential later workload transformation.

### Some Other Resources

[Cloud Migration](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/migrate/azure-migration-guide/index?tabs=Overview) is a topic within the [Cloud Adoption Framework (CAF)](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/) that lays out a process for how customers can move existing on-prem assets to Azure. 

[Azure Migrate](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview) is a service built on Azure Site Recovery technology to 'lift-and-shift' on-prem servers to Azure.

[Azure Database Migration Service](https://azure.microsoft.com/en-us/services/database-migration/) migrates compatible on-prem databases to Azure managed data services. The [Data Migration Guide](https://datamigration.microsoft.com/) covers a large set of migration scenarios and provides guidance, partner tools, etc.

Cloud Migration includes [ample documentation on preparing for migrations](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure). Expect many customer scenarios based on datacenter standards, deployment practices, etc.

### Special Challenges for Windows Server 2008 and 2008 R2 Migration

- 32- vs. 64-bit OS migration - Windows Server 2008 was the final version of Windows Server to be available in a 32-bit distribution.
- Features - Key OS features, like Storage Spaces, Clustering, network virtualization, etc. are either completely missing in Windows Server 2008 or have substantially reduced functionality.
- Management tools connectivity - remote admin tools are version sensitive. Typically downlevel client versions are required for remote management tool connectivity.
	
