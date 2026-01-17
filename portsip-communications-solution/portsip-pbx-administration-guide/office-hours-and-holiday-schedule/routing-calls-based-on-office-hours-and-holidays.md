# Routing Calls Based on Office Hours and Holidays

PortSIP PBX allows you to route calls dynamically based on **office hours**, **holidays**, and other time-based conditions. This flexibility ensures that inbound calls are always directed to the appropriate destination depending on when they are received.

### Inbound Call Routing

PortSIP PBX handles all inbound calls from SIP trunks using **inbound rules**. Within an inbound rule, you can:

* Specify the **SIP trunk** on which the call is received
* Define matching criteria using **Caller ID (CID)** and **DID numbers**
* Configure different routing destinations based on **office hours** and **holidays**

<figure><img src="../../../.gitbook/assets/inbound_rule_1 (1).png" alt=""><figcaption></figcaption></figure>

#### Example routing behavior

In the inbound rule shown in the example:

* Calls received **during a holiday** are routed to **extension 103**
* Calls received **during office hours** are routed to **extension 101**
* Calls received **outside of office hours** are routed to **extension 102**

This allows you to provide different call handling behavior for business hours, after-hours, and holidays using a single inbound rule.

#### Configuring office hours and holidays for an inbound rule

Within an inbound rule, open the **Office Hours** tab to configure time-based routing:

* Choose **Use Default Global Office Hours** to apply the tenant-wide office hours.
* Choose **Use Specific Office Hours** to define custom office hours for this inbound rule.
* Select one or more holidays from the **Global Holiday List** to apply holiday-specific routing.

The PBX evaluates the current time and date against these settings and routes inbound calls to the appropriate destination accordingly.

<figure><img src="../../../.gitbook/assets/inbound_rule_2.png" alt=""><figcaption></figcaption></figure>

***

### Advanced Routing for Inbound Rules

In addition to office hours and holiday routing, PortSIP PBX supports advanced, time-based routing for inbound calls. Advanced routing allows you to direct calls to different destinations based on custom date and time conditions, providing fine-grained control over call handling.

Within an inbound rule, open the **Advanced Routing** tab to configure these routing strategies.

<figure><img src="../../../.gitbook/assets/advanced-routing.png" alt=""><figcaption></figcaption></figure>

***

#### How Advanced Routing Works

An **advanced routing strategy** evaluates the current date and time against a set of conditions. If all conditions match, the call is routed to the specified destination or action.

Advanced routing strategies can be based on:

* Years
* Months
* Days of the month
* Days of the week
* Time shifts

Each strategy defines both **when** it applies and **what action** to take when it matches.

<figure><img src="../../../.gitbook/assets/advanced-routing-1.png" alt=""><figcaption></figcaption></figure>

***

#### Advanced Routing Fields

**Priority**

Determines the order in which routing strategies are evaluated.

* The PBX evaluates strategies from **lowest priority value to highest**.
* **Priority range:** `0–1000`
  * `0` is the **highest priority**.
* If the current date and time do not match a strategy, the PBX evaluates the next strategy.

***

**Years**

Specifies the **years** during which this routing strategy applies.

* The strategy is ignored if the current year does not match.
* Supported formats:
  * Range: `2023–2030`
  * Single year: `2031`
  * Combination: `2023–2030;2032;2035`
* Enter `all` or leave the field empty to apply to **all years**.

***

**Months**

Specifies the **months** during which this routing strategy applies.

* The strategy is ignored if the current month does not match.
* Supported formats:
  * Range: `1–9`
  * Single month: `11`
  * Combination: `1–9;11;12`
* Select or enter `all` to apply to **all months**.

***

**Days (Day of Month)**

Specifies the **days of the month** during which this routing strategy applies.

* The strategy is ignored if the current day does not match.
* Supported formats:
  * Range: `1–21`
  * Single day: `11`
  * Combination: `1–9;11;22`
* Select or enter `all` to apply to **all days of the month**.

***

**Weekdays**

Specifies the **days of the week** during which this routing strategy applies.

* The strategy is ignored if the current weekday does not match.
* Supported formats:
  * Range: `1–5`
  * Single day: `3`
  * Combination: `1–3;4`
* Select or enter `all` to apply to **all weekdays**.
* Weekday numbering:
  * `1` = Monday
  * `7` = Sunday

***

**Time Shifts**

Specifies one or more **time ranges** during which this routing strategy applies.

* The strategy is ignored if the current time does not fall within any defined range.
* Format: `HH:MM–HH:MM`
* Multiple ranges separated by semicolons.
* Example:\
  `09:00–11:30;14:00–18:00`

***

**Action**

Defines what happens when the routing strategy matches.

* **Number** – Routes the call to a specified number.
* **Voicemail** – Routes the call to an extension’s voicemail.
* **Hangup** – Terminates the inbound call.

***

**Destination (Dest)**

Specifies the destination number for the selected action.

* Used when **Action** is **Number** or **Voicemail**
* Ignored when **Action** is **Hangup**

**Example:**\
If **Action** is `Number` and **Dest** is `123456`, the call is routed to `123456`.

> **Note:** To route calls to a shared voicemail, set **Action** to **Number** and enter the shared voicemail number as the destination.

***

#### Strategy Evaluation and Fallback

* Multiple advanced routing strategies can be defined per inbound rule.
* Strategies are evaluated by **priority** (lowest value first).
* The **first matching strategy** is applied.

If no advanced routing strategy matches, the PBX falls back to the **Office Hours and Holiday routing** configured for the inbound rule.

***

### Extension Call Routing

Each extension can have **call forwarding rules** that define how PortSIP PBX handles calls when the user is **unavailable or cannot answer**. These rules can be evaluated based on:

* The **user’s status**
* The **current time** (office hours and holidays)

#### Routing behavior when an extension is offline

When an extension is offline, the PBX evaluates the extension’s **office hours** and **holiday settings**:

*   **Outside office hours or during a holiday**\
    The call is routed to the destination configured in the **When Out of Office Hours** field.\
    The destination can be:

    * Another extension
    * A PSTN phone number
    * A mobile phone number

    _Example:_ Calls are routed to `332061180`.
* **During office hours and not on a holiday**\
  The call is routed to the destination configured in the **In Office Hours** field.\
  &#xNAN;_&#x45;xample:_ Calls are routed to the extension’s voicemail.

#### Extension holidays

An extension can:

* Select holidays from the **tenant-level Global Holiday List**
* Define **custom personal holidays**

Both global and personal holidays are treated as **outside office hours** for that extension and will trigger out-of-office routing behavior.

<figure><img src="../../../.gitbook/assets/user_offline_1.png" alt=""><figcaption></figcaption></figure>

***

### Exceptions for Extensions

You can define **exception rules** for an extension to override the extension’s standard call forwarding behavior under specific conditions.

#### How exception rules work

An exception rule is evaluated based on:

* **Caller ID** – identifies specific callers or caller patterns
* **Received During** – specifies the **time shifts** when the exception applies
* **Forward To** – defines the action or destination when the exception is matched

#### Routing behavior

* If an inbound call **matches an exception rule**, the PBX immediately applies the action defined in that exception.
* When an exception rule is applied, the extension’s **normal forwarding rules are bypassed**.
* If no exception rule matches, the PBX processes the call using the extension’s standard forwarding configuration.





