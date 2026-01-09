# What is the DID Pool?

A **DID Pool** is a defined range of **DID/DDI numbers** associated with a specific **SIP trunk** in PortSIP PBX.

Because PortSIP PBX is a **true multi-tenant platform** designed for **Cloud PBX and UCaaS service providers**, multiple tenants may configure trunks from the same SIP trunk provider within a single PBX system.

Without proper isolation, this can lead to routing and caller ID conflicts, such as:

* Multiple tenants configuring the **same DID number** in inbound rules, making it unclear which tenant should receive an incoming call.
* A tenant using an **outbound caller ID** that belongs to another tenant, causing incorrect caller identification or call rejection by the trunk provider.

***

### Why DID Pool Is Required

To prevent these conflicts, PortSIP PBX introduces the **DID Pool** mechanism.

The DID Pool ensures that:

* DID numbers are **uniquely allocated per tenant**
* Inbound call routing is **deterministic and unambiguous**
* Outbound caller IDs are restricted to **authorized numbers only**

This design is mandatory for large-scale, multi-tenant Cloud PBX and UCaaS environments.

***

### How DID Pools Work

Instead of adding DID numbers individually, the DID Pool allows you to define **ranges of DID numbers** for a SIP trunk.

Key capabilities include:

* Adding **large DID ranges** efficiently
* Splitting a range into **subranges or individual numbers**
* Assigning ranges, subranges, or individual DIDs to specific tenants

Once assigned, a tenant can:

* Configure inbound call routing rules
* Set outbound caller IDs\
  using **only the DID numbers allocated to them from the DID Pool**

***

### DID Pool Format

PortSIP PBX supports multiple formats for defining DID Pools.\
Multiple entries must be separated by a **semicolon (`;`)**.

#### Supported Formats

```
33802000-33803000;33806000-33807000;33809122
33802000
33802000;33802010;33802112
33806000-33807000;33809122
33802000-33803000;33806000-33807000
```

#### Format Rules

* A **single DID number** can be specified directly
* A **DID range** is defined using a hyphen (`-`)
* Multiple numbers or ranges must be separated by semicolons (`;`)

***

### Important Notes

* **Remove any leading `+`, `0`, or `00`** from DID numbers before entering them into the DID Pool.
* Ensure DID ranges do not overlap across tenants to avoid routing conflicts.
* DID Pool assignments are enforced at both **inbound routing** and **outbound caller ID validation**.

***

### Expected Outcome

With DID Pools correctly configured:

* Incoming calls are routed to the **correct tenant**
* Outbound calls use **valid, tenant-owned caller IDs**
* The PBX operates safely and predictably in a **multi-tenant cloud environment**



