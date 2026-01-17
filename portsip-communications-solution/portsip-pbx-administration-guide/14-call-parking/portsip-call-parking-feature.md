# PortSIP Call Parking Feature

### What Is the Problem with Traditional Call Parking?

As described earlier, **traditional call parking** works well in **single-tenant PBX** deployments. In these environments, call parking is typically implemented by creating dedicated **parking extensions (park spots)**, which users can monitor and retrieve calls from.

However, this model does **not scale** in modern **cloud PBX** deployments.

#### Challenges in a Cloud, Multi-Tenant Environment

Today’s PBX deployments are predominantly **cloud-based and multi-tenant**, where:

* A service provider hosts a **single PBX instance**
* The PBX serves **hundreds or thousands of tenants**
* Each tenant operates a **logically isolated virtual PBX**, while sharing the same infrastructure

A single cloud PBX instance may support **1,000 to 10,000 tenants**.

In a traditional call parking model:

* Each tenant must create its own set of parking spots
* Parking spots are implemented as **PBX extensions**
* Each parking extension must:
  * Register to the PBX
  * Periodically send SIP `REGISTER` messages
  * Be monitored via **Dialog Event subscriptions**

**Scalability impact**

For example:

* If each tenant creates **10 parking spots**
* A PBX with **1,000 tenants** requires **10,000 parking extensions**

This results in:

* Tens of thousands of SIP registrations
* Thousands of Dialog Event subscriptions
* Significant consumption of **CPU**, **memory**, and **network bandwidth**

As tenants grow and require more parking spots, the impact multiplies further.\
This approach **severely degrades PBX performance** and is **unacceptable for service providers** operating at scale.

***

### PortSIP Solution

PortSIP PBX is designed specifically for **cloud PBX and service provider environments**. It is a **true multi-tenant PBX**, where:

* A single PBX instance can host **thousands of tenants**
* Each tenant is fully isolated
* System resources are used efficiently and predictably

To address the scalability issues of traditional call parking, **PortSIP implements call parking in a fundamentally different way**.

#### Key Design Principles

PortSIP’s call parking implementation:

* Does **not** require creating parking spot extensions
* Does **not** require SIP registration for parking spots
* Does **not** require Dialog Event subscriptions

This design eliminates the core scalability bottlenecks found in traditional implementations.

***

### How PortSIP Call Parking Works

#### Parking a Call

* There is **no need to create parking spots**.
* The **target extension number itself** acts as the parking reference.
* To park a call to extension `103`, the user simply transfers the call to:

```
*68103
```

Where:

* `*68` is the **Feature Access Code (FAC)** for call parking
* `103` is the extension number associated with the parked call

***

#### Call Park Notification

Once the call is parked:

* The target extension device (IP phone or softphone) receives an **out-of-dialog SIP NOTIFY**
* The NOTIFY uses the **`park-info` event**
* The message includes:
  * Who parked the call
  * Where the call is parked
  * How the call can be retrieved

The device can parse this information and **alert the user** that a call is parked.

***

#### Retrieving the Call

* The user retrieves the parked call by pressing the corresponding **button or soft key**
* No polling, subscription, or manual dialing is required

***

### Advantages of the PortSIP Call Parking Design

PortSIP’s approach provides the following benefits:

* **No parking spot extensions required**\
  Eliminates the need to create, register, and maintain thousands of virtual extensions.
* **No Dialog Event subscriptions**\
  Removes subscription overhead and long-lived SIP dialogs.
* **Rich NOTIFY information**\
  Devices receive detailed parking information and can present one-click retrieval to users.
* **Cloud-scale performance**\
  Call parking does not consume excessive CPU, memory, or bandwidth—even in very large deployments.
* **Service-provider friendly**\
  Suitable for large tenants and massive multi-tenant cloud PBX environments without performance degradation.

***

### Summary

Traditional call parking models were designed for on-premise, single-tenant PBXs.\
PortSIP’s call parking is **cloud-native by design**, enabling service providers to deliver call parking at scale—without compromising system performance.



