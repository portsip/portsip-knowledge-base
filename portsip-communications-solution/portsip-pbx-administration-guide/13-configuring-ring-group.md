# 13 Configuring Ring Group

Ring groups are used to ring specific groups of users in a pre-configured pattern. Calls routed to a Ring Group will follow the configured ring pattern for that group.

PortSIP PBX allows you to create a ring group with the below settings.&#x20;

* Ring group profile information.&#x20;
* User assignments.&#x20;
* Settings for call distribution, call forwarding, and business continuity.

## **13.1 Creating a Ring Group**

To add a Ring Group:

* In the PortSIP PBX Web Portal, select **Call Manager** > **Advanced Services** > **Ring Groups** and click the **Add** button
* Now enter the ring group fields:
  * **Ring Group Number** – This number identifies the ring group from other extensions. Specify a new one as needed. Do not specify an existing extension number
  * **Ring Group Name** – Enter a friendly name for the ring group
  * **Ring Time** – Specify how long the extension should ring for.
  * **Ring strategy** – Select the appropriate ring strategy for this ring group:
    * **Ring Simultaneously**: All Ring Group members will ring at the same time.
    * **Prioritized Hunt**: Ring each available member of the group by specific order.
    * **Cyclic Hunt**: Ring each available member of the group by the sequence the members are added into the group. The member who has not been rung from a call would take priority.
    * **Least Worked Hunt**: Ring each available member of the group by the order the members are added to the group. The member who has not answered a call from this group will take priority.
    * **Skill Based Routing Prioritized Hunt**: Ring each available agent in the queue serially in the configured order. Assign the call to agents in the highest-level skill group first. If the call is not answered in the current skill group, move on to less experienced agents in subsequent skill groups.
    * **Skill Based Routing Cyclic Hunt**: Ring each available agent in the queue serially. Ring the agent who hasn't been rung from a call from this queue in the longest amount of time first. Assign the call to agents in the highest-level skill group first. If the call is not answered in the current skill group, move on to less experienced agents in subsequent skill groups.
    * **Skill Based Routing Least Worked Hunt**: Ring each available agent in the queue serially. Ring the agent who hasn't answered a call from this queue in the longest amount of time first. Assign the call to agents in the highest-level skill group first. If the call is not answered in the current skill group, move on to less experienced agents in subsequent skill groups.
    * **Paging/Intercom**: This is a Paging or Intercom group (see the next section for more details).
* **Skip member(s) who's calling** - If the member of the group is on call, then don't ring this&#x20;
* In the section "**Destination if no answer**", you can define what should happen if the call is not answered by the ring group.
* In the **Destination for Night Mode** section, you can define how incoming calls should be handled when Night Mode is active for a tenant. For detailed configuration instructions, please refer to the [**Night Mode**](32-night-mode.md) section of this guide.
* On the "**Group Members**" page, select the extensions that should be members of this ring group.&#x20;
* **Outbound Caller ID** - Once the outbound caller ID for the ring group is set, when no members answer the call and forward the call to an external number on a trunk by the **Destination if no answer** option, the outbound caller ID could be a replacement for certain SIP field. For more details, please refer to [Outbound parameters and Inbound parameters](7-trunk-management/#7.2-outbound-parameters-and-inbound-parameters).

## **13.2 Paging**

When creating the ring group, selecting the **Ring Strategy** with **Paging/intercom** would allow someone to ring a group of extensions and make an announcement via the phone speaker. The called party will not need to pick up the handset as the audio will be played via the phone's speaker. The person who’s paging will not hear any audio back from the people being paged.

## **13.3 Intercom**

When creating the ring group, selecting the **Ring Strategy** with **Paging/intercom** would allow someone to ring a group of extensions and make an announcement via the phone speaker. The called party will not need to pick up the handset as the audio will be played via the phone speaker. The person paging will not hear any audio back from the people being paged.

If the extension user wants to talk with the caller, he/she should press the `*` **button to start talking, and stop by pressing the \`#\`** button.

There are two ways to commit Paging and Intercom:

1. Assume that you have created a ring group for which the group number is 9000, and selected the **Ring Strategy** with **Paging/intercom**. When dialing 9000, all members of ring group 9000 will answer the call automatically and can hear from the caller but the caller cannot hear back from members. If one of the members wishes to talk with the caller, just press the `*`, and stop talking by pressing `#` key.
2. If extension 100 wants to intercom with extension 101, just dial **\*46101**, and extension user 101 will answer the call automatically and talk with caller 100. In this example, **`*46`** is the value of **FAC**, it can be found by selecting the menu **Advanced > Feature Access Codes**.

