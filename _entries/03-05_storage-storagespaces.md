---
sectionid: storagespaces
sectionclass: h2
title: Storage Infrastructure: Storage Spaces and Storage Spaces Direct (S2D)
parent-id: storage
---

### What Is Storage Spaces?

Storage Spaces is the volume manager available in Windows Server since Windows Server 2012. No version of Storage Spaces is available for Windows Server 2008.

### Storage Spaces in Azure

Storage Spaces is a good fit for cloud deployment in Azure. Note that the speed of the system interconnect is critical; use a VM deployment

Storage Spaces Direct on Azure VMs: https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm, or;
Storage configuration for SQL Server VMs: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-storage-configuration.

NOTE: SQL Server configuration from the Azure portal will automatically configure local Storage Spaces pools if your VM needs them!

### Storage Spaces Resiliency Models

Storage Spaces provides a flexible volume management environment for direct-attached volumes (see below). It supports drive pooling and flexible volume provisioning across three different resiliency models:

1. Simple Spaces - aka simple stripe sets, these provide capacity with no added resiliency (akin to RAID 0)
2. Mirror Spaces - mirror sets provide at least one full copy on every write. Full recovery can be quickly achieved on device failure. It is possible to protect up to two simultaneous drive failures via mirroring (akin to RAID 1)
3. Parity Spaces - more storage-efficient than mirror spaces, but somewhat increases I/O write latency (by default, this is akin to RAID 5)

### Storage Spaces Components

[Storage Pools](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/deploy-standalone-storage-spaces#step-1-create-a-storage-pool) - Raw direct attached devices (primordial disks) are eligible to be added to storage pools. Additional disk capacity adds to the total storage capacity of the pool.

[Virtual disks](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/deploy-standalone-storage-spaces#step-2-create-a-virtual-disk) - Synonymous with Spaces, virtual disks govern the resiliency model and provisioning model (thin or fixed) for the disks which will be created from the pool.

Volumes - These are mountable as drives by the local OS and will follow the resiliency model as laid out in the virtual disk/Space.

### Clustered Spaces

Windows Server 2012 and later provide the ability to [clustered Storage Spaces](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/understand-quorum). This adds:

- A cluster resource model allows a Space to span members of a cluster. Underlying storage can be addressed by any cluster node using a form of centralized contention management performed via the cluster.

- Clustered Spaces are created on clustered pools of storage. In a clustered pool, every primordial disk must be visible to all cluster nodes, since any node could end up writing to an extent on any physical disk.

### Scale-Out File Server (SOFS)

Cluster Shared Volumes can be created from Clustered Spaces. These can be used to provide durable disk storage volumes for Hyper-V, SQL Server and other server workloads.

### Storage Spaces Direct (S2D)

Clustered Spaces in 2012/R2 was limited by a requirement to connect storage via a dedicated storage interconnect (shared SaaS). This approach was replaced by a new facility called [Storage Spaces Direct (S2D)](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview) in Windows Server 2016.

S2D is not a replacement for Storage Spaces. It simply provides a new model for setting up storage on a cluster. It also enables hyper-converged infrastructure, which is useful in some deployment scenarios for Hyper-V (e.g., branch servers).

Unlike the earlier version of clustered Storage Spaces, S2D allows any locally attached storage to be presented to a virtual cluster-level storage pool. Clustered Spaces in S2D can then be built using locally-attached storage, without the needed for a cluster-level shared storage interconnect (this is virtualized within the cluster interconnect).

> Functionality of S2D is explained at https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview#how-it-works.

