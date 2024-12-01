# Configuring Outbound Rule

## Creating Outbound Rules

An outbound rule determines which trunk is used for outbound calls. The rule is based on criteria such as the user or extension making the call, the number being dialed, the length of the number, or the user group to which the caller belongs.

To add an outbound rule:

1. From the PortSIP PBX Web Portal, navigate to **Call Manager > Outbound Rules** and click the **Add** button. Enter a name for the new rule.
2. **Priority**: Set the priority for the outbound rule. If a call meets multiple rules, a lower number indicates a higher priority. If multiple rules have the same priority, any one of these rules may be selected.
3. **Routing Strategy**: Select how the PBX will route calls through trunks:
   * **Prioritized**: The PBX will use the trunks in the configured order. If the first route fails, the PBX will try the next route.
   * **Randomly**: The PBX will select a trunk at random. If the call fails, the PBX will attempt the next route randomly.
4. **Specify Criteria for the Outbound Rule**: In the **Apply this rule to the following calls** section, choose from the following options:
   * **Calls to numbers starting with prefix**: Apply the rule to calls that start with specific prefixes (e.g., "00" for international calls). You can specify multiple prefixes separated by a semicolon (e.g., "00;123;88").
   * **Calls from extension(s)**: Define a specific extension or extension range for this rule (e.g., "100-120").
   * **Calls to numbers with certain digits**: Apply the rule based on the length of the number (e.g., "8;10;12" for 8, 10, or 12-digit numbers).
   * **Calls from extension group(s)**: Apply the rule to specific user groups rather than individual extensions.
5. **Route Outbound Calls**: In the **Make outbound calls on** section, select up to three routes for the call:
   * **Prioritized Strategy**: The PBX sends calls to trunks in the defined list order. If the call fails, it moves to the next trunk in the list.
   * **Random Strategy**: The PBX ignores the trunk list order and selects a trunk randomly. If the call fails, the PBX randomly selects the next trunk.
6. **Transform Numbers**: You can modify the called number before routing it to the selected trunk:
   * **Strip digits**: Remove a specified number of digits from the called number (e.g., remove "00" from a number like 002345 before routing it).
   * **Prepend**: Add digits to the beginning of the number (e.g., add "+44" to the number 002345 to make it +442345).
7. **Outbound Caller ID**: Set the outbound caller ID for calls routed through this trunk.

## **Office Hours for Outbound Rules**

PortSIP PBX allows the **Tenant Admin** to specify office hours and holidays for outbound rules. If an outbound call is made outside the defined office hours or on a holiday, the outbound rule will be blocked by the PBX, preventing the call from being routed.

* On the **Office Hours** tab, you can configure the office hours for the outbound rule. The PBX will then determine whether to route outgoing calls based on the current time.
  * If the current time falls outside the specified office hours, the call will fail even if the outbound rule matches successfully.
  * If **Use default Global Office Hours** is selected, the PBX will apply the office hours set by the Tenant Admin.
  * If **Use specific Office Hours** is selected, this outbound rule will use customized office hours.
* You can also apply holidays by selecting them from the tenant’s **Global Holiday List**. During holidays, the outbound rule will be blocked, preventing calls from being routed.

If you do not wish to block the outbound rule at any time, avoid selecting any holidays and set office hours to cover all days and times.

For more details on outbound routing, please refer to the topic: [**Office Hours and Holiday Schedule**](configuring-outbound-rule.md#office-hours-for-outbound-rules).

