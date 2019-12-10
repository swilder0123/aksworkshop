---
sectionid: vmware
sectionclass: h3
title: VMware vs. Hyper-V Resourcing
parent-id: resourcing
---

### Azure vs VMware Resourcing:

In general, Azure does not overcommit fabric resources. vmWare admins can sometimes perceive this as waste. 

However, unlike on-prem environments Azure is required to prioritize consistent performance for all VMs. Azure is not able to predict VM workload activity or set execution policy based on any knowledge of the VM workload. This means:
- In most SKU families, each physical CPU core is committed to a single VM. Dv3, Ev3 and other newer families effectively allow a type of oversubscription via hyperthreading to better balance system utilization of new memory-dense hardware.
- Every page of customer-allocated memory is committed to a single VM.
- Every page of disk storage is committed to a single disk, and every disk is committed to a single VM. Some disk types (UltraSSD) are beginning to provide multi-initiator (shared) access.

