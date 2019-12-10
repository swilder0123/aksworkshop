---
sectionid: overview
sectionclass: h2
title: Overview
parent-id: storage
---

These notes about storage cover various datacenter topics and are designed to help ask questions about workloads being considered for migration to the cloud.

### Workloads and Disk Latency

Some workloads may benefit from local storage latencies…which can be as low as 20 microseconds (source), see the "Direct attached storage" topic for more detail. These require special handling in any case, but may be troublesome migration candidates.

### Common Questions for Migration

- Are there any unique storage requirements for server(s) considered for migration?
- Is the workload especially sensitive to transaction latency, i.e., <= 2ms?
- Are there any storage control functions exercised by the workload? Does more than one server connect to any connected storage device? Is there any form of clustering involved?
- How do backups get performed?
- Is there a platform-level snapshot?
- How often does backup happen and how many recovery points per day are required?
- Do multiple volumes need to be coordinated in order to ensure a successful snapshot-based backup?
- Is the storage footprint fairly static or does it grow at some set rate?

