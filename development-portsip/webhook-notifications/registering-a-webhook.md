# Registering a Webhook

To start receiving webhook events from PortSIP PBX, you must register an event endpoint in the tenant settings.

1. Sign in to the PortSIP PBX Web Portal as a **Tenant Administrator**.
2. Navigate to **Company** in the left-hand menu.
3. Click the **Event URL** tab.
4. Enter the public HTTPS URL of your webhook endpoint.
5. Click **Save** to apply the configuration.

Once registered, PortSIP PBX will begin sending event notifications to the specified endpoint whenever supported events occur.

> **Note:** Ensure your webhook endpoint is publicly accessible and capable of handling HTTP/HTTPS POST requests from the PBX.

<figure><img src="../../.gitbook/assets/portsip-webhook.png" alt=""><figcaption></figcaption></figure>

***

### CDR Events

PortSIP PBX sends **Call Detail Record (CDR)** data to the configured webhook endpoint whenever a call is completed or updated.

#### Authentication Method

You can secure the CDR webhook using one of the following authentication methods:

* **Disable**\
  No authentication is required for requests sent by the PBX.
* **Basic Authentication**\
  The webhook endpoint requires HTTP Basic Authentication.\
  You must provide a **username** and **password**.
* **Digest Authentication**\
  The webhook endpoint requires HTTP Digest Authentication.\
  You must provide a **username** and **password**.
* **Bearer Authentication**\
  The webhook endpoint requires Bearer token authentication.\
  You must provide a **Bearer Token**.

#### CDR URL

The HTTPS endpoint to which CDR events are delivered.

* The URL must be publicly accessible by PortSIP PBX.
* HTTPS is **strongly recommended** to ensure data security.

#### Enable

Enables or disables the CDR webhook.

* When **enabled**, CDR events are delivered to the configured endpoint.
* When **disabled**, event delivery is paused without removing the configuration.

***

### Extension Events

PortSIP PBX sends **real-time extension and queue-related events** to the configured webhook endpoint, including:

* Extension call status
* Presence status
* Online/offline status
* Queue status updates

#### Authentication Method

You can secure the Extension Events webhook using one of the following authentication methods:

* **Disable**\
  No authentication is required for requests sent by the PBX.
* **Basic Authentication**\
  The webhook endpoint requires HTTP Basic Authentication.\
  You must provide a **username** and **password**.
* **Digest Authentication**\
  The webhook endpoint requires HTTP Digest Authentication.\
  You must provide a **username** and **password**.
* **Bearer Authentication**\
  The webhook endpoint requires Bearer token authentication.\
  You must provide a **Bearer Token**.

#### Event URL

The HTTPS endpoint to which extension and queue events are delivered.

* The URL must be publicly accessible by PortSIP PBX.
* HTTPS is **strongly recommended** to protect event data in transit.

#### Enable

Enables or disables the Extension Events webhook.

* When **enabled**, extension and queue events are sent in real time.
* When **disabled**, event delivery is temporarily or permanently halted.





