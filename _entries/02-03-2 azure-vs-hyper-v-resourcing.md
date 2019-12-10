---
sectionid: hyper-v
sectionclass: h3
title: Azure vs. Hyper-V Resourcing
parent-id: resourcing
---

Compared to typical Vmware environments, Hyper-V is much less aggressive in oversubscription and overprovisioning. This means that data collection for migration should be easier, since there are fewer variables to think about.

1. Hyper-V uses less speculation in CPU scheduling compared to Vmware ESXi. For instance, Hyper-V enlightened guests provide direct input to the hypervisor's scheduler for NUMA scheduling and spinlock avoidance.
2. Hyper-V provides memory ballooning but does not support memory compression, transparent page sharing or memory paging. Memory ballooning is disabled in VMs which are configured for virtual NUMA.

In Hyper-V, there is relatively little penalty for overprovisioning CPUs. Only VM threads with work to do will get scheduled, and datacenter workloads are typically not CPU-bound.

As with VMware, Migrating VMs to Azure becomes very expensive when CPUs are overprovisioned because the other resources allocated to Azure VMs (memory and bandwidth) are typically tied to the number of cores.

> Lift-and-shift VMs can be very wasteful in the public cloud. Always consider a on-prem VM as overprovisioned for CPU in Azure unless collected performance data suggests otherwise


