# 13 Configuring Ring Group

**Ring Groups** allow you to ring a predefined group of users according to a configured ringing pattern.\
When a call is routed to a Ring Group, the PBX follows the group’s ring strategy to determine how and in what order members are alerted.

PortSIP PBX allows you to configure a Ring Group with:

* Ring group profile information
* User (extension) assignments
* Call distribution, call forwarding, and business continuity settings

***

### Creating a Ring Group

To create a Ring Group:

1. Sign in to the **PortSIP PBX Web Portal**.
2. Navigate to **Call Manager > Advanced Services > Ring Groups**.
3. Click **Add**.

#### Configure Ring Group Settings

* **Ring Group Number**\
  A unique number that identifies the ring group.\
  This must be a new number and **must not conflict with any existing extension**.
* **Ring Group Name**\
  A friendly name to identify the ring group.
* **Ring Time**\
  The amount of time (in seconds) that each extension will ring.
* **Ring Strategy**\
  Select how calls are distributed to group members:
  * **Ring Simultaneously**\
    All group members ring at the same time.
  * **Prioritized Hunt**\
    Ring available members one by one in a fixed, configured order.
  * **Cyclic Hunt**\
    Ring available members in rotation.\
    The member who has not been rung for the longest time is given priority.
  * **Least Worked Hunt**\
    Ring available members based on call activity.\
    The member who has answered the fewest calls from this group is given priority.
  * **Skill-Based Routing – Prioritized Hunt**\
    Ring agents serially based on skill level and configured order.\
    Calls are offered to the highest skill group first, then to lower skill groups if unanswered.
  * **Skill-Based Routing – Cyclic Hunt**\
    Ring agents serially, prioritizing the agent who has not been rung for the longest time, starting with the highest skill group.
  * **Skill-Based Routing – Least Worked Hunt**\
    Ring agents serially, prioritizing the agent who has answered the fewest calls, starting with the highest skill group.
  * **Paging / Intercom**\
    Create a paging or intercom group (see the sections below).
* **Skip member(s) who’s calling**\
  If enabled, the calling group member will not be rung.
* **Night Mode:** Set the group with Night Mode activated or deactivated. For more details, please refer to [Night Mode](32-night-mode.md).

***

#### Call Handling Options

* **Destination if no answer**\
  Defines how calls are handled if no group member answers (for example, forward to voicemail, another number, or another service).
* **Destination for Night Mode**\
  Defines call behavior when Night Mode is active for the tenant.\
  For details, refer to the **Night Mode** option above of this guide.

***

#### Group Members

On the **Group Members** page, select the extensions that should belong to this Ring Group.

***

#### Outbound Caller ID

When an **Outbound Caller ID** is configured for the Ring Group:

* If no members answer and the call is forwarded to an external number via a SIP trunk,
* The Ring Group’s outbound caller ID can be used to replace specific SIP fields.

For more details, refer to [Outbound parameters and Inbound parameters](7-trunk-management/configuring-sip-trunk.md#outbound-parameters-and-inbound-parameters).

***

### Paging

When the **Paging / Intercom** ring strategy is selected, the Ring Group functions as a **paging group**.

* Calls are automatically answered on the called extensions
* Audio is played through the phone speaker
* Called users do **not** need to pick up the handset
* The caller does **not** hear audio back from the paged users

Paging is commonly used for announcements.

***

### Intercom

Intercom uses the same **Paging / Intercom** ring strategy but allows two-way communication when initiated.

* Called extensions auto-answer and hear the caller
* By default, the caller does not hear audio back
* A called user can:
  * Press **\*** to start talking
  * Press **#** to stop talking

***

#### Ways to Use Paging and Intercom

**Method 1: Ring Group Paging / Intercom**

Assume:

* A Ring Group with number **9000**
* Ring Strategy set to **Paging / Intercom**

Behavior:

* Dialing **9000** causes all group members to auto-answer
* Members hear the caller, but the caller does not hear them
* Any member can press **\*** to speak and **#** to stop speaking

***

**Method 2: Direct Extension Intercom**

Assume:

* Extension **100** wants to intercom with extension **101**
* The Feature Access Code (FAC) for Intercom is **\*46**

Behavior:

* Extension **100** dials **\*46101**
* Extension **101** auto-answers and two-way audio is established

The Intercom FAC can be viewed or changed under\
**Advanced > Feature Access Codes**.



