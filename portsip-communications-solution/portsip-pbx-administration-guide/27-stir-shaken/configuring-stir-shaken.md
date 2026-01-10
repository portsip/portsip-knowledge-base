# Configuring STIR/SHAKEN

PortSIP PBX allows you to enforce **STIR/SHAKEN-based call handling** on a per-trunk basis. You can:

* **Drop inbound calls** based on Caller ID verification status provided by the SIP trunk
* **Sign outbound calls** using your own STIR/SHAKEN certificate

Inbound verification is performed using parameters passed in the **P-Asserted-Identity (PAI)** SIP header, while outbound signing adds an **Identity** header to SIP INVITE messages.

***

### Dropping Inbound Calls Based on Verification Status

#### Configuration Steps

1. Navigate to **Call Manager > Trunks**.
2. Double-click the SIP trunk you want to configure.
3. Select the **Inbound Parameters** tab.
4. In the **STIR/SHAKEN** section, configure the options described below.

These settings apply **only to inbound calls received on this trunk**.

<figure><img src="../../../.gitbook/assets/stire-shaken-1.png" alt=""><figcaption></figcaption></figure>

***

#### PAI Header Parameter Name

* Default value: **verstat**
* Used to extract the verification status from the **P-Asserted-Identity** header
* The parameter name may vary depending on your trunk provider

Example:

```
P-Asserted-Identity: <sip:+15617500080;verstat=TN-Validation-Passed>
```

***

#### Enable STIR/SHAKEN Validation

* Enables or disables inbound STIR/SHAKEN verification on this trunk
* Must be enabled to enforce call dropping based on verification status

***

#### Drop Calls with Verification Status

* Select one or more verification statuses that should cause the PBX to **drop inbound calls**
* When enabled, the PBX parses the PAI header and compares the verification result

If a match is found, the call is immediately rejected.

**Supported Verification Status Values**

* **No-TN-Validation**
* **TN-Validation-Failed**
* **TN-Validation-Passed-B**
* **TN-Validation-Passed-C**
* **TN-Validation-Failed-A**
* **TN-Validation-Failed-B**
* **TN-Validation-Failed-C**

> **Note**\
> Verification status values are **case-insensitive**.\
> For example: `No-TN-Validation`, `NO-TN-VALIDATION`, and `No-tn-Validation` are treated the same.

***

#### Verification Logic Example

Inbound SIP header:

```
P-Asserted-Identity: <sip:+15617500080;verstat=TN-Validation-Passed>
P-Attestation-Indicator: B
```

Explanation:

* `verstat` indicates the verification result
* Attestation level (`A`, `B`, or `C`) is provided separately

**Scenario**

If the following values are selected in **Drop Calls with Verification Status**:

* **TN-Validation-Failed-B**
* **No-TN-Validation**

The call **will be dropped**, because:

* The verification result is `TN-Validation-Passed`
* The attestation indicator is `B`
* Combined, this matches `TN-Validation-Passed-B`

If **no attestation indicator** is present, the PBX requires an **exact match** in the PAI header, for example:

```
P-Asserted-Identity: <sip:+15617500080;verstat=TN-Validation-Passed-B>
```

***

### Signing Outbound Calls with STIR/SHAKEN

To sign outbound calls on a trunk, you must follow the process of obtaining your own STIR/SHAKEN certificate. This involves the following steps:

1. Acquire a [US FCC 499-A](https://apps.fcc.gov/cores/userLogin.do) Filer ID and an Operating Company Number (OCN).
2. After securing these, you can then proceed to apply for your STIR/SHAKEN token and certificate, which are required for authenticating calls.

***

#### Obtaining a STIR/SHAKEN Certificate

To implement STIR/SHAKEN, follow these steps:

1. **Obtain an FCC 499 ID**\
   Apply for and receive an [FCC 499-A Filer ID](https://apps.fcc.gov/cores/userLogin.do), which is necessary for companies providing telecom services in the U.S.
2. **Get an Operating Company Number (OCN)**\
   An [OCN is a 4-character identifier](https://www.neca.org/) assigned to telecom companies in North America. You’ll need this to proceed with certification.
3. **Acquire Your iConnectiv Token**\
   [iConnectiv ](https://authenticate.iconectiv.com/)is the designated policy administrator for the STIR/SHAKEN framework. Obtain your token through their portal.
4. **Partner with a Certificate Authority (CA)**\
   Collaborate with a trusted Certificate Authority (CA) to issue your STIR/SHAKEN certificate, enabling you to sign and authenticate your outbound calls.

***

### Uploading the STIR/SHAKEN Certificate

Certificates can be uploaded at either the **System Administrator** or **Tenant Administrator** level.

#### Certificate Scope

* **System Administrator certificate**\
  Used to sign calls on trunks created by the System Administrator
* **Tenant certificate**\
  Used to sign calls on trunks created by the Tenant Administrator

***

#### Uploading at the System Administrator Level

1. Navigate to **Advanced > Settings**.
2. Select the **STIR/SHAKEN Certificates** tab.
3. Open the public certificate file and paste its contents into **Public Certificate**.
4. Open the private key file and paste its contents into **Private Key**.
5. Click **OK** to save.

***

#### Uploading at the Tenant Administrator Level

1. Navigate to **Advanced > STIR/SHAKEN**.
2. Paste the public certificate into **Public Certificate**.
3. Paste the private key into **Private Key**.
4. Click **OK** to save.

<figure><img src="../../../.gitbook/assets/stir-shaken-3.png" alt=""><figcaption></figcaption></figure>

***

### Enabling STIR/SHAKEN Signing on a Trunk

To enable call signing for a specific trunk:

1. Navigate to **Call Manager > Trunks**.
2. Double-click the trunk to edit.
3. Select the **Options** tab.
4. Enable **STIR/SHAKEN Signature Required**.
5. Click **OK** to save.

<figure><img src="../../../.gitbook/assets/stire-shaken-2.png" alt=""><figcaption></figcaption></figure>

***

### Outbound Call Signing Behavior

When an outbound call is placed over a trunk with **STIR/SHAKEN Signature Required** enabled:

* PortSIP PBX signs the call using the uploaded certificate
* An **Identity** header is added to the SIP INVITE
* The signature allows downstream carriers to verify the caller ID

#### Example SIP INVITE

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





