---
sectionid: san
sectionclass: h2
title: Storage Infrastructure - SAN and NAS
parent-id: storage
---

Datacenter storage is almost always connected via a storage area network (SAN). Some servers boot from direct-attached storage (DAS) but SAN-based boot is common, especially for rack-dense servers (blades).

Traditionally, SANs have been special-purpose systems that provide block IO (mountable disk) for server workloads.

A host bus adapter (HBA) implements the SAN's access protocols and provides physical connectivity to the SAN network.

SAN physical connectivity is almost always provided via switched infrastructure over copper twinax or (more commonly), fiber optic cabling.

> [Fibre Channel](https://en.wikipedia.org/wiki/Fibre_Channel) remains the predominant standard for SAN connectivity. Speeds range from 4Gb/sec up to 10Gb/sec for older networks, to common speeds of 32Gb/sec and beyond, depending on the age of the infrastructure.  

> [Hyper-converged infrastructure (HCI)](https://en.wikipedia.org/wiki/Hyper-converged_infrastructure) combines data networking and storage networking high-capacity networking infrastructure. As datacenters continue the path to 40- and 100Gb/sec infrastructure, SANs will follow because of their increasingly common underpinnings.

### SANS and Virtualization

Virtualization environments typically republish storage as virtual disks. This can be done by carving up SAN-based storage pools (typical), or linking SAN LUNs directly to VMs (often used for high-capacity workloads with provisioned I/O).

Some workloads, like clusters or Oracle RAC are only able to function if they can assert distributed lock management (DLM) on storage. These are harder to virtualize and sometimes very difficult to move to the cloud.

### Network Attached Storage (NAS)

Network-attached storage (NAS) systems are typically distinguished by their use case…they provide file-level access via Network File System (NFS) and sometimes Server Message Block (SMB) protocols (for Hyper-V).

NAS system vendors (NetApp, Dell/EMC Isolon, etc) focus on workloads where shared file-level access is key, e.g., high performance computing, web applications, information archival, etc. However NAS systems commonly also provide SAN capabilities, i.e, the ability to support block-level communications for applications and infrastructure that require it (e.g, hypervisors).

> VMware environments often use NAS/NFS to provide shared storage to create VM block storage devices since it allows more easier and more convenient storage pooling.

### Migrating SAN/NAS Data

For migration purposes, most SAN-attached storage operates exactly the same as direct-attached storage. Data will be copied block-for-block to drives mounted to an Azure VM.

However, NAS use cases (e.g., NFS mounts) are different and typically require substituting Blob storage, Azure NetApp Files or some other mechanism like VM-hosted NFS as a shared storage facility.

### Direct-attached Storage

Other than use for system boot and paging, direct-attached storage (DAS) is not commonly used in enterprise datacenters for mission-critical production.

DAS can be identified as storage which has no external storage network interconnect (host bus adapter) and uses SAS or SATA local bus connectivity.

Highly rack dense systems/blades often have no DAS at all, even to support pagefiles (swap volumes).

VMs may leverage DAS if the hypervisor provides 'passthrough' (Hyper-V) or 'raw device mapping' (VMWare) AND the disk volume is locally mounted on the hypervisor host. This is very uncommon in production environments.

> #### NOTE
> There is commonly no special requirement to handle DAS volumes in migration, but confirm that cloud disk storage provides the same functionality for the workload.

It is used for specialized system configurations, including compute clusters, Hadoop clusters, Elastic clusters and other scenarios which latency and/or built-in redundancy makes network hosted storage undesirable or unnecessary.

### iSCSI (background content)

SCSI (Small Computer Systems Interface) is a fundamental storage protocol which underpins most storage device connectivity. SCSI devices have Logical Unit Numbers (LUNs); originally this included a Host (Initiator) and up to 7 targets.

A storage device which can be addressed as a 'block' device via SCSI control commands is sometimes referred to as a 'raw' device. Azure disks are not raw, their low-level access is controlled by a storage

iSCSI is an IP protocol implementation for SCSI. It is network-based Storage Area Network (SAN) and is commonly used in converged network architectures.

A iSCSI initiator allows block-level IO from a server to a network-hosted storage device. This allows network shared storage to be mounted as disk volumes.

In some cases, iSCSI targets provide multi-initiator (shared) access to provide capability for cluster volumes, Oracle RAC configurations, etc. This allows a shared disk to appear on multiple cluster nodes. SCSI arbitration provides write security so nodes don't walk over each other:

A volume level reservation makes an entire disk read/write on a single initiator (node), but read-only on all others
Extent-level reservations make all disks appear writable, but writes must be coordinated so that individual disk regions (extents) are controlled by a single writer at a time. A volume manager like Storage Spaces is required to coordinate multiple nodes (clustered spaces).
Oracle RAC provides volume management but the block devices must be 'raw'; i.e., it must directly manage the disk extents. See the Oracle and RAC tab for more.

Datacenter hardware generally implements iSCSI initiator functionality via system firmware on a storage area network (SAN) host bus adapter (HBA). This is a common way to provide storage for rack-dense systems and blades in a datacenter.

It is also possible to use a software-based initiator. This allows a server to mount an iSCSI drive via the data network. This is typically not done for production workloads.

An HBA-based initiator is more robust and performant than a software initiator.

For migration purposes, an iSCSI hosted volume should be indistinguishable from any other type of SAN volume as long as the initiator is HBA-hosted.

### Basic vs. Dynamic volumes (Windows)

Windows has 2 native types of volumes: Basic, which are created and managed by the OS, and Dynamic, which are managed by a logical volume manager service. Disk volumes on Windows are almost always created as Basic.


#### Basic Volumes

Basic volumes are primary or extended partitions created on basic disks. Basic volumes can be partitioned into multiple logical volumes, although there is very little advantage to doing so.

Basic disks are physical volumes which are directly managed as storage resources by the operating system, i.e., there is no intermediate disk volume manager in place.

Disks which are presented to Windows via SAN storage (or Azure disk volumes) are typically used as basic volumes, since all aggregation, resiliency, etc. is provided by the storage fabric.

#### Dynamic Volumes

Dynamic disks are still supported by Windows, but are deprecated in favor of Storage Spaces.

Dynamic disks are created in the Windows Disk Manager utility. Disk Manager allows the creation of extended (RAID) disk volume types, disk volumes which span physical disks, mirrored disks, etc.

More options for disk management are provided by Storage Spaces.

### Drive letters, mount points and SMB connections

By usual custom in Windows, a volume is presented by the operating system as a drive letter, i.e., a C: drive. In this case you can consider C: to be a mount point for the OS partition. Not all volumes are mapped to drive letters, however...it is common to see multiple hidden volumes on a system. The easiest tool to use to see all your volumes is DiskPart:

![Diskpart list volumes](media/win2k8/diskpart-01.png)

In the case above, notice that Volume 6 is attached to a directory mount point. These are supported in Windows but are infrequently used. In theory, you could mount the user profiles directory to a specific volume (i.e., like mapping /home to a different disk volume in a Unix-style OS), but there is typically little advantage in doing so.

Each volume in Windows has a volume identifier which is unique across space and time. This allows disk volumes to be mounted on a new system without losing their integral identity. A globally unique ID (GUID) is used to identify a volume created in Windows. A volume list (produced by mountvol) looks like this:

![Diskpart mount points](media/win2k8/diskpart-02.png)

The OS kernel has no underlying dependency on drive letters, although one drive letter must be assigned to the system. Any other volume can be mounted on an existing file system or even have no stated mount point (typical for OS utility volumes).

The mount point for any existing volume (except the system drive) can be changed, although any scripts or configurations which depend on existing location may be broken as a result.

![Assign drive letter dialog](/media/win2k8/assign-drive-letter-01.png)

For scripting purposes, it is always recommended to use system-generated variables to find objects of interest, e.g.:

`%systemroot% - C:\WINDOWS` (Note, Windows does not have to be installed on C:)

`%windir%` - same as above

`%systemdrive%` - typically `C:`

`%ProgramFiles%` - Default 64-bit application installation path, typically `c:\Program Files`

`%ProgramFiles(x86)%` - Default 32-bit application install path, typically `c:\Program Files (x86)`

> Note: the `%var%` notation refers to how the variables are referenced at the command line or in .CMD/.BAT scripts

> Find all environment variables by typing 'Set' at the command prompt


Fetch these values from PS like so:

`dir env:\SystemRoot`, or;

`$var = (dir env:\SystemRoot).Value` to deference.

### Mount Points

Creating a volume mount point only requires an existing folder, and works much as it does it Linux. It is possible to relocate some system-managed paths onto volume mounts, (e.g., user folders) although there is typically little advantage in doing so.

There is no completely durable means to mount a network path onto a local file system, although the Windows API does provide support for junctions managed by local services (e.g., OneDrive).

Junction points created by mklink must refer to local paths.

