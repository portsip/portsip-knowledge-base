# What is the DID Pool?

The DID pool is a DID numbers range of a specified SIP trunk which is configured in the PortSIP PBX.

Since the PortSIP PBX is a multi-tenant PBX design for the cloud PBX provider and UCaaS provider. If more than one tenant set up the trunk from the same trunk provider in the PBX, and sets the same DID number for the inbound rule, when a call is arriving at the PBX, the PBX does not know which tenant should route the call to; and when an extension of a tenant makes the call to the trunk, set the outbound caller ID which belongs to another tenant, will cause problems there.

To avoid the problems above, PortSIP PBX introduced the DID pool concept.

The usage of DID pool with the ability to add a range of DID numbers instead of the addition of N DIDs. DID range could be split into subranges that might consist of either individual numbers or smaller subranges.

Ranges, subranges, or individual numbers could be assigned to the tenant, then the tenant can create the call routing rules base on the assigned trunk and DID numbers which are in the DID pool range.

## Format of the DID Pool

The PortSIP PBX separated the multiple DID/DDI numbers by a semicolon, following formats are allowed:

* 33802000-33803000;33806000-33807000;33809122
* 33802000
* 33802000;33802010;33802112
* 33806000-33807000;33809122
* 33802000-33803000;33806000-33807000

Please remove the leading `+`, `0` or `00` from the DID number before entering it.

