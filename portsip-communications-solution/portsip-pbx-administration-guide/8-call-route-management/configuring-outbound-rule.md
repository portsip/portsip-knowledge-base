# Configuring Outbound Rule

### Creating Outbound Rules

An **outbound rule** determines which SIP trunk is used to place outbound calls. The PBX evaluates outbound rules based on criteria such as:

* The extension or user placing the call
* The dialed number or its prefix
* The length of the dialed number
* The extension group to which the caller belongs

Outbound rules allow you to control call routing for scenarios such as least-cost routing, carrier failover, and policy-based call control.

#### Procedure: Add an outbound rule

1. Sign in to the **PortSIP PBX Web Portal**.
2. Go to **Call Manager > Outbound Rules**, then click **Add**.
3. Enter a **Name** for the outbound rule.
4. Configure the rule priority:
   * **Priority**\
     Set the priority value for the rule.
     * A **lower number** indicates a **higher priority**.
     * If a call matches multiple outbound rules, the rule with the **highest priority** (lowest number) is applied.
     * If multiple rules share the same priority, **any one of those rules may be selected**.
5. Select the **Routing Strategy**:
   * **Prioritized**\
     The PBX attempts to place the call using the trunks in the configured order.\
     If the call fails on the first trunk, the PBX automatically tries the next trunk.
   * **Randomly**\
     The PBX selects a trunk at random from the configured list.\
     If the call fails, the PBX randomly selects another trunk.

***

#### Specify outbound rule conditions

In the **Apply this rule to the following calls** section, define when this outbound rule applies. You can use one or more of the following conditions:

* **Calls to numbers starting with prefix**\
  Apply the rule to calls that begin with specific prefixes.\
  Example: `00;123;88`
* **Calls from extension(s)**\
  Apply the rule to a specific extension or a range of extensions.\
  Example: `100–120`
* **Calls to numbers with certain digits**\
  Apply the rule based on the total number of digits in the dialed number.\
  Example: `8;10;12` (matches 8-, 10-, or 12-digit numbers)
* **Calls from extension group(s)**\
  Apply the rule to specific extension groups instead of individual extensions.

***

#### Route outbound calls

In the **Make outbound calls on** section, select **up to six trunks** that the PBX can use for this rule.

* **Prioritized strategy**\
  Calls are sent to trunks in the configured order. If a call fails, the PBX tries the next trunk in the list.
* **Random strategy**\
  Calls are sent to a randomly selected trunk. If the call fails, another trunk is selected at random.

***

#### Transform numbers (optional)

Before routing the call to the selected trunk, you can modify the dialed number:

* **Strip digits**\
  Removes a specified number of digits from the beginning of the called number.\
  Example: Strip `00` from `002345`, resulting in `2345`.
* **Prepend digits**\
  Adds digits to the beginning of the called number.\
  Example: Prepend `+44` to `002345`, resulting in `+442345`.

***

#### Outbound Caller ID

* **Outbound Caller ID**\
  Specify the caller ID presented to the called party for outbound calls routed through this rule.

***

### Office Hours for Outbound Rules

PortSIP PBX allows the **Tenant Admin** to control when outbound rules are active by applying **office hours** and **holidays**. If an outbound call is placed **outside the configured office hours** or **during a holiday**, the PBX blocks the outbound rule and prevents the call from being routed.

#### Office Hours

Use the **Office Hours** tab within the outbound rule to define when outbound calls are permitted.

* The PBX evaluates the **current time** against the configured office hours to determine whether the outbound rule can be applied.
* If the current time falls **outside** the specified office hours, the outbound call will **fail**, even if all other rule conditions are met.

You can choose one of the following options:

* **Use default Global Office Hours**\
  The outbound rule follows the **global office hours** configured by the Tenant Admin.
* **Use specific Office Hours**\
  The outbound rule uses **custom office hours** defined specifically for this rule, overriding the global schedule.

#### Holidays

You can also apply **holiday restrictions** to outbound rules by selecting one or more holidays from the tenant’s **Global Holiday List**.

* During selected holidays, the outbound rule is **blocked**, and outbound calls are not routed.
* Holiday restrictions apply regardless of office-hour settings.

#### Allow outbound calls at all times

If you do **not** want to block outbound calls at any time:

* Do **not** select any holidays, and
* Configure office hours to cover **all days and all times**.

This ensures the outbound rule remains active continuously.

For more information about configuring schedules, see [Office Hours and Holiday Schedule.](../office-hours-and-holiday-schedule/)



