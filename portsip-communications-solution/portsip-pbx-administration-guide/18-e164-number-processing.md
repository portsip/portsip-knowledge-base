# 18 E164 Number Processing

E.164 is an [international standard](https://en.wikipedia.org/wiki/International_standard) ([ITU-T](https://en.wikipedia.org/wiki/ITU-T) Recommendation), titled _The international public telecommunication numbering plan_, that defines a [numbering plan](https://en.wikipedia.org/wiki/Telephone_numbering_plan) for the worldwide [public switched telephone network](https://en.wikipedia.org/wiki/Public_switched_telephone_network) (PSTN) and some other data [networks](https://en.wikipedia.org/wiki/Telecommunications_network).

E.164 defines a general format for international [telephone numbers](https://en.wikipedia.org/wiki/Telephone_number). Plan-conforming telephone numbers are limited to only digits and to a maximum of fifteen digits.[\[1\]](https://en.wikipedia.org/wiki/E.164#cite_note-:0-1) The specification divides the digit string into a country code of one to three digits, and the subscriber telephone number of a maximum of twelve digits.

In PortSIP PBX, E164 processing converts user-dialed numbers (including those with a leading +) into a standardized format that can be interpreted by your outbound rules and provider.

Using E164 processing is optional, but it can help solve the issue of handling numbers dialed with a leading +. It converts these numbers into a format that can be interpreted by the rules based on the call type (local, national, or international).

***

### E.164 Settings

The **E.164 Settings** control how PortSIP PBX normalizes and transforms dialed phone numbers—especially numbers entered in international (E.164) format—before outbound routing rules are applied.

#### Accessing E.164 Settings

1. Sign in to the PortSIP PBX Web Portal as a **Tenant Admin**.
2. Navigate to **Blacklist and Codes > Codes and E164 > E164**.
3. The E.164 configuration page appears (as shown in the screenshot below).

<figure><img src="../../.gitbook/assets/e164.png" alt=""><figcaption></figcaption></figure>

***

### Configuration Options

#### International Code

Defines the international dialing prefix used when placing calls to other countries **without using the “+” symbol**.

* Example (United States):
  * International Code: `011`
  * Dialed number: `+44123456789`
  * Converted number: `01144123456789`

This allows users to dial international numbers using E.164 format while remaining compatible with carriers that require a numeric international prefix.

***

#### Process E.164 Numbers

Controls whether the PBX processes numbers that begin with a leading **“+”**.

* **Enabled**:\
  Numbers dialed in E.164 format (e.g., `+44123456789`) are normalized according to the E.164 rules.
* **Disabled**:\
  The PBX does **not** modify the dialed number, even if it starts with `+`.

> **Recommendation:**\
> Enable this option in most deployments to ensure consistent number normalization.

***

#### Remove Special Characters

**Remove the following characters from processed numbers:**\
`(`, `)`, and spaces

* When enabled, the PBX removes these characters from dialed numbers.
* Example:
  * Dialed number: `+1 (813) 456 78910`
  * Converted number: `+181345678910`

This ensures compatibility with SIP trunks and outbound routing rules.

***

#### Remove Country Code for Same Country

When enabled, the PBX removes the country code if the call is made **within the same country**.

* Example (United States):
  * Dialed number: `+12345678910`
  * Converted number: `2345678910`

This is useful when your carrier expects national-format numbers for domestic calls.

***

#### Select Country

Select the country where your PBX tenant is located.

* This setting allows the PBX to correctly identify:
  * Your country code
  * Domestic vs. international calls

**Example:**\
Select **United States (US)** for all examples in this section.

***

#### Area Code

Enter your local area code.

**Remove if Same Area Code**

When enabled, the PBX removes the area code for local calls if it matches the configured area code.

* Example:
  * Area Code: `813`
  * Dialed number: `+181345678910`
  * Converted number: `45678910`

This is useful in regions where local dialing does not require an area code.

***

#### National Code

Adds a national dialing prefix to the beginning of the number during processing.

* Example:
  * National Code: `8`
  * Dialed number: `+12345678910`
  * Converted number: `82345678910`

This is typically required in countries where domestic calls must include a national prefix.

***

#### Add Prefix

Adds a custom prefix to the processed number.

* Common use cases:
  * Selecting specific outbound rules
  * Carrier-specific routing requirements

> **Important:**\
> E.164 rules are processed **before outbound routing rules**.

* Example:
  * Prefix: `2`
  * Dialed number: `+12345678910`
  * Converted number: `22345678910`

***

### Non-E.164 Calls

A **Non-E.164 call** is a call dialed **without a leading “+”**.

#### International Calls (Non-E.164 Format)

If the dialed number:

* Does **not** begin with `+`, and
* Begins with the configured **International Code**

then the call is treated as an **international call**.

**Call Processing Flow**

1. The PBX identifies the call as international based on the **International Code**.
2. The PBX checks the destination country code against:
   * **Blacklist**
   * **Codes > Codes and E164 > Allowed Country Code**
3. The PBX verifies the caller’s **role and permissions**:
   * If the caller does **not** have permission to make international calls, the call is rejected.

**Example**

* International Code: `00`
* Dialed number: `004412345678`
* Extracted country code: `44` (United Kingdom)

The PBX will:

* Verify whether country code `44` is allowed
* Check whether the caller is permitted to place international calls

If either check fails, the call is blocked.

***

#### Domestic Calls (Non-E.164 Format)

If the dialed number:

* Does **not** begin with `+`, and
* Does **not** begin with the International Code

then the call is treated as a **domestic call**.

The PBX checks:

* The caller’s role and permissions for domestic calling

If the caller is not authorized to place domestic calls, the call is rejected.

***

### E.164 Calls

An **E.164 call** is any call where the dialed number begins with a leading **“+”**.\
These calls are always treated as **international-format numbers**, but how they are processed depends on whether **Process E.164 Numbers** is enabled.

***

### When **Process E.164 Numbers** Is Disabled

If **Process E.164 Numbers** is disabled, the PBX does **not** normalize or rewrite the dialed number.

#### Call Processing Behavior

1. The call is identified as an **international call**.
2. The PBX checks the destination country code against:
   * **Blacklist**
   * **Codes > Codes and E164 > Allowed Country Code**
3. The PBX verifies the caller’s **role and permissions**:
   * If the caller is not permitted to make international calls, the call is rejected.

#### Example

* Dialed number: `+4412345678`
* Country code extracted: `44`

The PBX checks whether country code **44** is allowed and whether the caller has international calling permissions.\
If either check fails, the call is blocked.

***

### When **Process E.164 Numbers** Is Enabled

When enabled, the PBX **removes the leading “+”** and applies E.164 normalization rules before outbound routing.

After removing the `+`, the PBX evaluates the number using the following logic.

***

#### 1. Dialed Number Starts with the **Selected Country Code**

If the dialed number begins with the configured **Select Country** code, the call is treated as a **national (domestic) call**.

**Processing Steps**

1. Remove the leading `+`.
2. Remove the country code.
3. If **Remove if same country** is enabled, the country code is discarded.
4. If **Remove Area Code** is enabled, the area code is also removed.
5. Generate the new dialed number using the rule:

```
Prefix + National Code + Remaining Number
```

6. Check the caller’s **domestic calling permissions**.
   * If the caller is not permitted to make domestic calls, the call is rejected.

**Example**

Configuration:

* Select Country: `44`
* Area Code: `31`
* Remove if same country: **Enabled**
* Remove Area Code: **Enabled**
* National Code: `11`
* Add Prefix: `22`

Dialed number:

```
+4431867762
```

Processing result:

* Remove `+` → `4431867762`
* Remove country code `44` → `31867762`
* Remove area code `31` → `867762`
* Add National Code `11` → `11867762`
* Add Prefix `22` → `2211867762`

**Final dialed number:** `2211867762`

<figure><img src="../../.gitbook/assets/e164_1.png" alt=""><figcaption></figcaption></figure>

***

#### 2. Dialed Number Does NOT Start with the Selected Country Code

If the E.164 number does **not** begin with the configured country code, the PBX treats it as an **international destination**.

**Processing Steps**

1. Remove the leading `+`.
2. Generate the new dialed number using the rule:

```
Prefix + International Code + Dialed Number
```

3. Determine call type:
   * If **Add Prefix** is empty → **International call**
   * If **Add Prefix** is configured → **National call**
4. For international calls:
   * Check **Allowed Country Code**
   * Check caller’s **international calling permissions**

If any check fails, the call is rejected.

**Example**

* International Code: `00`
* Add Prefix: _(empty)_
* Dialed number: `+3312345678`

Processing result:

* Converted number: `003312345678`
* Country code checked: `33`

The PBX verifies:

* Country code `33` is allowed
* Caller has international calling permissions

If either condition fails, the call is blocked.

***

### Summary: E.164 Call Processing Logic

| Scenario                        | Call Type     | Permission Check |
| ------------------------------- | ------------- | ---------------- |
| `+` dialed, processing disabled | International | International    |
| `+` dialed, same country        | Domestic      | Domestic         |
| `+` dialed, different country   | International | International    |
| Prefix added                    | National      | Domestic         |

<figure><img src="../../.gitbook/assets/e164_2.png" alt=""><figcaption></figcaption></figure>



