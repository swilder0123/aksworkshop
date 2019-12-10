---
sectionid: backup
sectionclass: h2
title: Backup and Recovery
parent-id: storage
---

### Shadow Copy

Volume Shadow Copy is a provider+writer/requester architecture which enables on-demand snapshots of a volume while it is in use.

[Volume Shadowing Service (VSS)](https://docs.microsoft.com/en-us/windows/win32/vss/volume-shadow-copy-service-overview) runs as a service on Windows and coordinates activity between runtime services requesting snapshots (commonly a backup agent) and multiple providers which are responsible for putting application data in a consistent state so that it can be safely read.

System snapshots take only a few moments, but it can take minutes or hours to finish a long backup. Windows file systems use a copy-on-write mechanism to make sure that the data that was requested for backup is available to be read when the backup actually happens.

VSS typically works invisibly with no interaction needed.

https://docs.microsoft.com/en-us/windows/win32/vss/volume-shadow-copy-service-portal


### VSS and the SQL Writer Service

On-demand backups for SQL Server (from any backup agent) indirectly invoke the SQL Writer Service. It interacts with VSS in order to provide application consistent snapshots for SQL Server during backups.

### VSS and Oracle DB

Oracle databases running on Windows Server are covered by an Oracle-provided VSS writer which functions similarly to the SQL Writer Service.

### VSS and Hyper-v

Hyper-V provides a platform interface to allow [VSS-coordinated consistent backups](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/virtual/backing-up-and-restoring-virtual-machines), without the need for backup agent in the VM (backups are coordinated via hypervisor integration services).

> Storage paths managed directly by the VM (e.g., iSCSI and passthrough disks) are invisible to Hyper-V and can't be managed via built-in backup integration.

### Application-Consistent Backup and Linux

There is no system-level facility like VSS for application-consistent backups in Linux. However, mission-critical apps running on Linux typically have an option to pause and resume I/O in order to accommodate backups. Azure Backup provides a facility to coordinate system activity in order to facilitate application-consistent backups.

### System State

Windows backup consists of three things:

1. File system -- backup of volumes and directories as viewed in the file system
2. User store -- backup of user profiles, including data, user security keys, user certificate secret keys, etc.
3. System state -- backup of data maintained by the system for specific services, i.e., the registry hives, SYSVOL, Active Directory database (NTDS), etc.
 
### Backup and Restore Scenarios

Backup programs typically allow flexibility around which items are included for backup.

[Backing up system state](https://docs.microsoft.com/en-us/windows/win32/vss/locating-additional-system-files) is critical for restoring certain aspects of a system.

Restoring just data (i.e., migrating data to a new system) is also a common use case, i.e., when migrating data between servers.

Restoring the file system and user profiles is the typical 'new laptop' scenario.

### System state restore scenarios

Doing a full recovery of Active Directory requires the ability to do a system state restore of at least one Active Directory domain controller (DC).

A full system restore of any kind will require the restore of its system state. Key OS objects like the registry will not be properly restored without system state restore.

