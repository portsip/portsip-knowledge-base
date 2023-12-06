# Configuring Outbound Rule

## Creating Outbound Rules

An outbound rule decides through which trunk an outbound call would be placed. The rule is decided by the user/extension who is making the call, the number that is being dialed, the length of the number, or the user group to which the caller belongs.

To add outbound rules:

* From the PortSIP PBX Web Portal menu, navigate to **Call Manager > Outbound Rules** and click the **Add** button. Enter a name for the new rule in the field.
* **Priority**: set the priority for the outbound rule. If a call meets multiple outbound rules, a lower priority number has a higher level of priority. If multiple rules have the same priority, then any one of these rules may be selected and used.
* **Routing Strategy**: set how the PBX sends the call to the routing trunks, **Prioritized** or **Randomly**.
* Specify the criteria that should be matched for this outbound rule to be triggered. In the **Apply this rule to below calls** section, specify any of the following options.
  * Calls to numbers started with prefix: Apply this rule to all calls started with the number you specify. For example, entering "**00**" to specify that all calls with numbers starting with 00 should trigger this rule. Callers should dial "**00xxx**" to trigger this rule. You can specify more than one prefix, separated by "**;**". For example, "**00;123;88**" specifies prefixes "**00**" and "**123**" and "**88**". If the called number matches one of these prefixes, this rule will be triggered.
  * Calls from extension(s): Select this option to define a particular extension or extension range for which this rule applies. Specify one or more extensions separated by semicolons, or specify a range by using a "**-**". For example 100-120.
  * Calls to numbers with certain digits: Select this option to apply the rule to numbers with a particular digit length, for example, 8 digits. By this method, you can capture calls to local area numbers or national numbers without requiring a prefix. It also supports setting multiple lengths by ";".  For example, "**8;10;12**" specifies called number lengths 8, 10, and 12. If a called number length matches one of these lengths, this rule will be triggered.
  * Calls from extension group(s): Rather than specifying individual extensions, you can select a user group.
*   Specify how to match outbound calls with the criteria. The **Make outbound calls on** section allows you to select up to three routes for the call.&#x20;

    If the **Routing Strategy** is set to "**Prioritized**", the PBX will send calls to the trunk as the configured trunks list order. If the call is failed on the route, PortSIP PBX will automatically try the next route. You can drag and drop the route to adjust the priority.

    If the **Routing Strategy** is set to **Randomly**, the defined trunk list order will be ignored, and PortSIP PBX will choose a trunk randomly to send the call. If the call is failed on the route, PortSIP PBX will automatically try the next route randomly.
* You can transform the number that matches the outbound rule before the call is routed to the selected trunk or provider by using the **Strip Digits** and **Prepend** fields:
  * Strip digits: This allows you to remove one or more digits from the called number. Use this option to remove the prefix before a call is dialed to the trunk, or provider if it is not required. For example, if the extension makes the call to 002345, if you specify to remove two digits, the prefix "**00**" will be removed before it is routed.
  * Prepend: This allows you to add one or more digits at the beginning of the number if this is required by the provider or gateway. For example, if the extension makes the call to 002345, we specify 2 in the **Strip digits** field and set **Prepend** to "**+44**", the final called number that PBX forwards to the SIP Trunk will be `+442345.`
  * Outbound Caller ID: This allows you to set up the outbound caller ID for the call placed on this trunk.

## **Office hours for Outbound Rules**

PortSIP PBX allows tenant admin to specify office hours and holidays for outbound rules. If the current time falls outside of the specified office hours or on a holiday, the outbound rule will be blocked by the PBX.&#x20;

On the **Office Hours** tab, you can set the office hours for the outbound rule so that the outgoing call will be routed to trunks or not depending on the current hour.

For example, if the current time is out of the specified office hours, the call fails even if the outbound rule is successfully matched.

If selected **Use default Global Office Hours**, the PBX will use the office hours set by **Tenant Admin**.  If selected **Use specific Office Hours**, this outbound rule will use the customized office hours.

You can also select holidays from the tenant's **Global Holiday List** to apply to this rule. If you do not want to block the outbound rule at any time, do not select any holidays and specify that all days and times are within office hours.

For more details on the outbound routing, please refer to this topic: [Office Hours and Holiday Schedule](../office-hours-and-holiday-schedule/).

