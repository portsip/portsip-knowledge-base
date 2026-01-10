# SIP Header Manipulation

**SIP header manipulation** allows you to add, remove, or modify SIP message attributes on the PortSIP PBX, including SIP headers and specific header elements.

Common use cases include:

* Resolving interoperability issues between SIP endpoints
* Passing custom or business-specific information across call legs
* Addressing incompatibilities between systems such as **Softswitch–PSTN gateways** or **different IP PBX platforms** in multi-site deployments

In many of these scenarios, calls may fail or behave unexpectedly due to differences in SIP header handling. SIP header manipulation enables the PBX to normalize or enrich SIP messages to ensure successful call setup and processing.

***

### How SIP Header Manipulation Works

To enable SIP header manipulation, you define **rule sets** that specify how SIP headers should be handled.

A **SIP header element** refers to a sub-part of a SIP header, such as:

* The header value
* Header parameters
* URI parameters

The header name itself is excluded. For each header or header element, you can specify the action the system should perform (for example, relay, add, modify, or remove).

***

### 1. Relay SIP Headers

The **Relay SIP Headers** feature allows the PBX to forward selected SIP headers from one call leg to the next.

#### Example Use Case

If an incoming INVITE from a SIP trunk includes a custom SIP header such as:

```
X-Case-Number
```

and the call is routed through:

```
IVR → Call Queue → Agent
```

The PBX can relay this header when sending the INVITE to the agent.\
This allows the agent’s application to read the header, retrieve the case number, and query related customer information from a CRM system—enabling faster and more informed customer service.

***

#### Configuration Steps

1. Sign in to the PBX Web Portal as a **Tenant Administrator**.
2.  Navigate to:

    ```
    Advanced > Customer Headers
    ```
3. In the **Relay SIP Headers** section, specify:
   * The SIP headers to relay
   * The call types to which the relay applies

**Available Options**

* **All**\
  Relay the header for all calls.
* **Trunk Calls**\
  Relay the header only for calls to or from SIP trunks.
* **User Calls**\
  Relay the header only for calls between internal users.

***

#### Default Relayed Headers

By default, PortSIP PBX relays the following headers for **all calls**:

* **X-CID**\
  Used to correlate call legs for debugging and troubleshooting.\
  This header is commonly referenced in the [Trace Server](debug-sip-message.md).
* **X-Session-Id**\
  A unique identifier for a call across all call legs.\
  For example, when a call flows from a trunk through IVR and a queue to an agent, the same session ID is present on each leg, allowing the PBX to associate the call with its CDRs.
* **X-Trunk-Name**\
  Indicates the SIP trunk used for the call, making it easy to identify trunk routing in SIP messages.

<figure><img src="../../.gitbook/assets/relay_headers.png" alt=""><figcaption></figcaption></figure>

To stop relaying a header, click the **minus (–) icon** next to the header name.

> **Example**\
> If `X-Trunk-Name` and `X-CID` are removed, but `X-Case-Number` is configured, the PBX will relay only the `X-Case-Number` header when it is present in the incoming SIP message.

<figure><img src="../../.gitbook/assets/relay_headers_1.png" alt=""><figcaption></figcaption></figure>

***

### 2. Add SIP Headers

The **Add SIP Headers** feature allows the PBX to insert new SIP headers into outgoing SIP messages.

#### Example Use Case

When an incoming INVITE is received from a SIP trunk and routed to an agent, the PBX can add custom headers that provide contextual information to downstream applications.

***

#### Configuration Steps

1. Sign in as a **Tenant Administrator**.
2.  Navigate to:

    ```
    Advanced > Customer Headers
    ```
3. In the **Add SIP Headers** section:
   * Enter the SIP header name and value
   * Select the call types to which the header applies

**Available Options**

* **All**\
  Add the header to all calls.
* **Trunk Calls**\
  Add the header only to calls to or from SIP trunks.
* **User Calls**\
  Add the header only to calls between internal users.

***

#### Example Configuration

With the following configuration:

* Add header `X-Server-Name: PBX01` for **all calls**
* Add header `X-Data-Center: US` for **trunk calls only**

The PBX will automatically insert these headers into the INVITE sent to the called party, based on the call type.

***

### 3. Manipulate Known SIP Headers

In addition to custom headers, PortSIP PBX allows you to **modify or remove standard (known) SIP headers**.

For details, refer to the documentation section:\
[Handle Outbound Calls Through SIP Trunk](7-trunk-management/handle-outbound-calls-through-sip-trunk.md)

> ❗**Warning**\
> Modifying or removing standard SIP headers can break call signaling and cause calls to fail.
>
> Only make changes if you fully understand the impact. If you are unsure, contact **PortSIP Support** for guidance.

***

### Best Practices and Warnings

* Use SIP header manipulation primarily for **interoperability** and **integration** scenarios.
* Avoid unnecessary changes to standard SIP headers.
* Always test changes in a **non-production environment** before deploying to live systems.
* Document all custom headers used for CRM or application integrations.







