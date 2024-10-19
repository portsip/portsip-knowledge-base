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

## Signing Outbound Calls on a Trunk

To sign outbound calls on a trunk, you must follow the process of obtaining your own STIR/SHAKEN certificate. This involves the following steps:

1. Acquire a [US FCC 499-A](https://apps.fcc.gov/cores/userLogin.do) Filer ID and an Operating Company Number (OCN).
2. After securing these, you can then proceed to apply for your STIR/SHAKEN token and certificate, which are required for authenticating calls.

### Obtain a STIR/SHAKEN Certificate

To implement STIR/SHAKEN, follow these steps:

1. **Obtain an FCC 499 ID**\
   Apply for and receive an FCC 499-A Filer ID, which is necessary for companies providing telecom services in the U.S.
2. **Get an Operating Company Number (OCN)**\
   An OCN is a 4-character identifier assigned to telecom companies in North America. You’ll need this to proceed with certification.
3. **Acquire Your iConnectiv Token**\
   iConnectiv is the designated policy administrator for the STIR/SHAKEN framework. Obtain your token through their portal.
4. **Partner with a Certificate Authority (CA)**\
   Collaborate with a trusted Certificate Authority (CA) to issue your STIR/SHAKEN certificate, enabling you to sign and authenticate your outbound calls.

### Uploading the STIR/SHAKEN Certificate

You can upload the STIR/SHAKEN certificate either at the System Administrator level or the Tenant level, depending on the scope of the calls.

* The PBX will use the System Administrator's certificate to sign calls on trunks that are added by the System Administrator.
* Tenant-level certificates are used to sign calls on trunks that are added by the Tenant Administrator.

#### Uploading a Certificate at the System Administrator Level

1. Navigate to **Advanced > Settings** in the menu.
2. Select the **STIR/SHAKEN Certificates** tab.
3. Open your STIR/SHAKEN certificate file using Windows Notepad, then copy and paste its contents into the **Public Certificate** field.
4. Similarly, open your STIR/SHAKEN private key file using Windows Notepad, and copy and paste the contents into the **Private Key** field.
5. Click **OK** to save your changes.

#### Uploading a Certificate at the Tenant Administrator Level

1. Navigate to **Advanced > STIR/SHAKEN** in the menu.
2. Open your STIR/SHAKEN certificate file using Windows Notepad, then copy and paste its contents into the **Public Certificate** field.
3. Similarly, open your STIR/SHAKEN private key file using Windows Notepad, and copy and paste the contents into the **Private Key** field.
4. Click **OK** to save your changes.

### Enabling STIR/SHAKEN Call Signing on a Trunk

To enable the STIR/SHAKEN signature on a specific trunk, follow these steps:

1. Go to **Call Manager > Trunks**.
2. Double-click the trunk you wish to edit.
3. Select the **Options** tab.
4. Toggle on the **STIR/SHAKEN Signature Required** option.
5. Click **OK** to save your changes.

<figure><img src="../../../.gitbook/assets/stire-shaken-2.png" alt=""><figcaption></figcaption></figure>

Now that you’ve enabled the STIR/SHAKEN signature, when an extension makes an outbound call over a trunk where the **STIR/SHAKEN Signature Required** option is enabled, the PortSIP PBX will sign the call using the uploaded certificates. It will then add an **Identity** header to the INVITE message, which contains the call signature. The INVITE message will look similar to the following example:

{% code overflow="wrap" %}
```
INVITE sip:18001234567@example.com:5060 SIP/2.0
Via: SIP/2.0/UDP example.com:5060
From: "Alice" <sip:14045266060@5.6.7.8:5060>;tag=123456789
To: "Bob" <sip:18001234567@1.2.3.4:5060>
Call-ID: 1-12345@5.6.7.8
CSeq: 1 INVITE
Max-Forwards: 70
Identity: eyJhbGciOiJFUzI1NiIsInBwdCI6InNoYWtlbiIsInR5cCI6InBhc3Nwb3J0IiwieDV1IjoiaHR0cHM6Ly9jZXJ0aWZpY2F0ZXMuZXhhbXBsZS5jb20vMTIzNDU2Nzg5LnBlbSJ9.eyJhdHRlc3QiOiJBIiwiZGVzdCI6eyJ0biI6WyIxODAwMTIzNDU2NyJdfSwiaWF0IjoxNTQ4ODU5OTgyLCJvcmlnIjp7InRuIjoiMTQwNDUyNjYwNjAifSwib3JpZ2lkIjoiM2E0N2NhMjMtZDdhYi00NDZiLTgyMWQtMzNkNWRlZWRiZWQ0In0.S_vqkgCk88ee9rtk89P6a6ru0ncDfSrdb1GyK_mJj-10hsLW-dMF7eCjDYARLR7EZSZwiu0fd4H_QD_9Z5U2bg;info=<https://pbx.example.com:8887/creds/3054739567934621/cert.pem>alg=ES256;ppt=shaken
```
{% endcode %}



