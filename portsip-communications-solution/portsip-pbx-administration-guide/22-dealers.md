# 22 Dealers

### Overview

**Cloud PBX** and **UCaaS** solutions enable businesses to scale rapidly, stay agile, and focus on their core operations without the complexity of managing IT infrastructure.

In this ecosystem:

* **Service providers** focus on delivering reliable and stable hosted services
* **Distributors and resellers** manage sales and customer relationships
* End users benefit from a seamless, professionally managed communication service

PortSIP PBX is a **multi-tenant platform** designed specifically for modern service providers.\
Its **Dealers** feature streamlines the management of distributors and resellers, improving operational efficiency and supporting scalable business growth.

***

### Dealer Levels

PortSIP PBX supports **three dealer levels**:

* **Distributor**
* **Sub-Distributor**
* **Reseller**

<figure><img src="../../.gitbook/assets/dealer_1.png" alt=""><figcaption></figcaption></figure>

#### Dealer Hierarchy and Permissions

Each dealer level can manage only the levels below it:

* **Service Provider**
  * Can create **Distributors**, **Sub-Distributors**, and **Resellers**
* **Distributor**
  * Can create **Sub-Distributors** and **Resellers**
* **Sub-Distributor**
  * Can create **Resellers**
* **All dealer levels**
  * Can create and manage their own **end users (tenants)**

This hierarchical model ensures clear ownership, responsibility, and operational boundaries.

***

### Adding a Dealer

To add a dealer in PortSIP PBX:

1. Sign in to the **PortSIP PBX Web Portal** as a **System Administrator**.
2. Click the **Dealers** menu.
3. Click **Add**.

<figure><img src="../../.gitbook/assets/dealer_2.png" alt=""><figcaption></figcaption></figure>

#### Configure Dealer Information

Enter the required information and configure the following options:

* **Level**\
  Select the dealer level:
  * Distributor
  * Sub-Distributor
  * Reseller
* **Tenant Limit**\
  (Optional) Set the maximum number of tenants the dealer can create.
* **Extension Limit**\
  (Optional) Set the maximum number of extensions allowed across the dealer’s tenants.

Click **OK** to create the dealer.

After creation, the dealer can sign in to the PortSIP PBX Web Portal to manage their dealers and tenants according to their assigned level.

***

### Dealer Sign-In

A dealer can sign in to the PortSIP PBX Web Portal using their **username and password**.

<figure><img src="../../.gitbook/assets/dealer_3.png" alt=""><figcaption></figcaption></figure>

After signing in, the dealer will have access to:

* **Dealers**\
  Manage sub-dealers (based on permission level)
* **Tenants**\
  Create and manage end-customer tenants

This allows each dealer to independently manage their business while operating within the limits defined by the service provider.

<figure><img src="../../.gitbook/assets/dealer_4.png" alt=""><figcaption></figcaption></figure>







