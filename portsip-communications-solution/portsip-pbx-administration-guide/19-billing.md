# 19 Billing

### Overview

PortSIP PBX includes a comprehensive Call Accounting and Billing system that allows you to generate detailed call cost reports, both manually and automatically.

Billing can be applied at either the tenant or user (extension) level and supports both real-time balance enforcement and post-call cost calculation.

***

### Enabling Billing

1. Sign in to the PortSIP PBX Web Portal as a **Tenant Administrator**.
2. Navigate to **Billing > Settings**.
3. Enable or disable the billing feature as required.

***

### Billing Settings

#### Enable Call Billing

Turn this option **on** to activate the billing system.

#### Billing Profile

Defines where call charges are applied:

* **Tenant** – Charges are deducted from the tenant’s balance
* **User** – Charges are deducted from the extension user’s balance

#### Charge Mode

Defines how charges are calculated and enforced:

* **Online Charging**
  * The PBX checks the available balance **before and during** the call
  * If sufficient balance exists, the call proceeds
  * Call cost is deducted and recorded in the CDR after call completion
  * Balance verification applies to:
    * Tenant balance (if Billing Profile = Tenant)
    * User balance (if Billing Profile = User)
* **Offline Charging**
  * No balance check is performed
  * Call cost is calculated after the call and recorded in the CDR
  * No balance deduction or call blocking occurs

<figure><img src="../../.gitbook/assets/billing_1.png" alt=""><figcaption></figcaption></figure>

***

### Account Recharge

#### Tenant Balance

* **Current Balance**\
  Displays the tenant’s current balance. This field is **read-only**.
* **Recharge**\
  Allows the tenant administrator to add funds to the tenant account.

***

#### Recharge for a User

To recharge an individual user’s balance:

1. Navigate to **Call Manager > Users**.
2. Edit the desired user.
3. Open the **Balance** tab.
4. Enter the recharge amount and save.

***

### Managing Billing Rules

To manage billing rules:

1. Navigate to **Billing > Billing Rules**.
2. From this page, you can:
   * Create new rules
   * Modify existing rules
   * Delete rules
   * Import or export billing rules

#### Rule Matching Logic

* Billing rules are matched using the **called number prefix**
* If the prefix matches, the call is billed
* **Prefixes must be unique** and cannot be duplicated

***

### Billing Rule Parameters

When creating a billing rule, the following parameters can be configured:

* **FreeSeconds** – Free call duration (seconds)
* **ConnectFee** – Fixed charge applied upon successful call connection
* **PostCallSurcharge** – Percentage surcharge (e.g., `0.01` = 1%)
* **GracePeriod** – Minimum call duration required for billing (seconds)
* **Price1** – Rate per minute for the initial billing interval
* **PriceN** – Rate per minute for subsequent billing intervals
* **Interval1** – Initial billing interval (seconds)
* **IntervalN** – Subsequent billing interval (seconds)

The simplest parameters are _ConnectFee_, which is a fixed amount of money charged for each successful call regardless of its duration; and _PostCallSurcharge_ which is additional charge applied. It is calculated on the percentage of the amount charged, that is if the call costs 1 dollar and the _PostCallSurcharge_ is 0.01, the actual amount charged will be 1.01 dollar.

The following picture illustrates how the calls are charged. The process starts with comparing value of the _GracePeriod_ parameter with the duration of the call. The _GracePeriod_ parameter determines the minimum duration of the call that will be subject of charge. Calls with durations of less than this value are not charged at all. _GracePeriod_ value of 0 second and 1 second provide almost the same behavior except that when the _GracePeriod_ is 1 second, connected calls with zero duration won't be charged with connection fee.

A _ConnectFee_ is charged immediately upon connection, and all calls shorter than Interval’ will be rounded to _Interval1_ seconds. _FreeSeconds_ are granted after the _Interval1_, so this part of the call is not charged, and calls shorter than (_Interval1_ + _FreeSeconds_) will be rounded to _Interval1_ seconds. If call is longer than (_Interval1_ + _FreeSeconds_) remaining portion will be rounded up to multiple _IntervalIN_ seconds. After that, the _PostCallSurcharge_ is applied to the total amount charged.

<figure><img src="../../.gitbook/assets/billing_2.png" alt=""><figcaption></figcaption></figure>





***

### Call Cost Formula

The total call cost is calculated using:

* **ConnectFee**
* **Price1 × Interval1**
* **PriceN × IntervalN**
* **PostCallSurcharge**

Where:

* All time values are rounded according to billing intervals
* Surcharge is applied after base cost calculation

The call illustrated in the figure will be charged using the following formula:

<figure><img src="../../.gitbook/assets/billing_3.png" alt=""><figcaption></figcaption></figure>

* Connect fee - monetary units
* Price 1 - monetary units
* Price N - monetary units
* Interval 1 - seconds
* Interval N - seconds
* Post call surcharge - fractional based on the setting in the Tariff/Destination Set (E.g. for 10% fractional = 0.1)





