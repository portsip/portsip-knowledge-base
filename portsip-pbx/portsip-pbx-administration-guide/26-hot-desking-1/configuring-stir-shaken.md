# Configuring STIR/SHAKEN

In PortSIP PBX, you can configure the system to drop inbound calls on a specified SIP trunk based on **Caller ID verification** - when the trunk provider passes a parameter value in the **P-Asserted-Identity** SIP header in the INVITE message, which by default is named **'verstat'**. Additionally, you can also upload the **STIR/SHAKEN** certificate to sign outbound calls on a specified SIP trunk.

## Drop Calls with Verification Status

To configure call handling based on verification status:

1. Navigate to **Call Manager > Trunks**.
2. Double-click the trunk you want to edit.
3. Click the **Inbound Parameters** tab.
4. In the **STIR/SHAKEN** section, you will find three configurable options, which can be set at the trunk level.

<figure><img src="../../../.gitbook/assets/stire-shaken-1.png" alt=""><figcaption></figcaption></figure>

These options allow you to customize how the PBX handles calls based on STIR/SHAKEN verification status for each trunk.

### **PAI Header Parameter Name**

Set this field to your desired value, it's **'verstat'** by default.

{% hint style="info" %}
This parameter is used for caller ID validation and is typically named **'verstat'**. However, the exact name may vary depending on your trunk provider.
{% endhint %}

### Enable STIR/SHAKEN Validation

This option allows you to enable or disable PortSIP PBX's validation of inbound calls based on **STIR/SHAKEN** Caller ID verification.

### Drop Calls with Verification Status

This option allows you to select which verification status will trigger call drops when **Enable STIR/SHAKEN Validation** is enabled.

For example, selecting **'TN-Validation-Failed'** means that if the **PAI** header contains this verification status, the call will be dropped.

The **PAI** header value will be parsed, and if the specified parameter matches any of the selected values in the **Drop Calls with Verification Status** list, the call will be dropped.

Refer to the list of verification statuses:

* **No-TN-Validation**
* **TN-Validation-Failed**
* **TN-Validation-Passed-B**
* **TN-Validation-Passed-C**
* **TN-Validation-Failed-A**
* **TN-Validation-Failed-B**
* **TN-Validation-Failed-C**

**Note**: Verification statuses are case-insensitive, meaning all variations (e.g., **'No-TN-Validation'**, **'NO-TN-VALIDATION'**, and **'No-tn-Validation'**) are acceptable.

This feature applies only to the inbound calls received from this trunk.

### **Example**

If a call is received from the SIP trunk with the following **PAI header**:

`P-Asserted-Identity: <sip:+15617500080;verstat=TN-Validation-Passed>`\
`P-Attestation-Indicator: B`

Please note that the additional header check is included for **STIR/SHAKEN**. The **P-Asserted-Identity** can contain one of the following values: **'TN-Validation-Passed'**, **'TN-Validation-Failed'**, or **'No-TN-Validation'**. The attestation level is specified in a separate header, such as **P-Attestation-Indicator: B**.

#### **Scenario**

If a user selects **'TN-Validation-Failed-B'** and **'No-TN-Validation'** as values in the **Drop Calls with Verification Status** field, the call will be dropped, since it matches **'TN-Validation-Passed'** with **'B'** as the attestation level.

However, if no attestation indicator is provided, the PBX expects an exact match between the **verstat** value in the **PAI header** and the value specified in the **Drop Calls with Verification Status** field. For example:\
`P-Asserted-Identity: <sip:+15617500080;verstat=TN-Validation-Passed-B>`.































