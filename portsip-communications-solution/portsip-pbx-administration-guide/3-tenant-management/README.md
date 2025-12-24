# 3 Tenant Management

### PortSIP PBX Multi-Tenant Architecture

**PortSIP PBX** is built on a **true multi-tenant architecture**, purpose-designed for service providers, operators, and enterprises that need to host and manage multiple independent customers on a single, centralized platform.

Unlike traditional single-tenant PBX deployments, this architecture enables one PortSIP PBX system to securely serve **multiple tenants (customers),** each operating as an independent PBX, while efficiently sharing the same underlying infrastructure.

***

### What Is a Tenant?

In PortSIP PBX, a **tenant** represents an independent organization or customer. Each tenant functions as a fully self-contained PBX environment with:

* Its own extensions, users, and tenant administrators
* Independent call routing, trunks, and numbering plans
* Dedicated call queues, IVRs, auto attendants, and voicemail
* Isolated call recordings, CDRs, reports, and analytics
* Tenant-specific policies, features, and branding

From the tenant’s perspective, the system behaves exactly like a **dedicated private PBX**, even though it runs on shared infrastructure.

***

### Key Benefits of the Multi-Tenant Design

#### **True Logical Isolation and Tenant Transparency**

Each tenant is **completely isolated and invisible to other tenants** on the platform. Tenants cannot see, access, or interact with any other tenant’s users, data, numbers, or configuration.

This **tenant-to-tenant transparency** ensures:

* Strong security and data privacy
* No risk of configuration leakage or cross-tenant impact
* A clean, dedicated PBX experience for every customer

For end users and tenant administrators, the presence of other tenants is entirely transparent—they experience the system as if it were deployed exclusively for their organization.

#### **Centralized Management for Service Providers**

Service providers can manage all tenants from a **single PortSIP PBX instance**, dramatically reducing operational complexity, infrastructure costs, and maintenance overhead.

**Scalable by Design**

The platform is engineered to scale horizontally, allowing service providers to host **tens of thousands of tenants** while maintaining consistent performance and reliability.

#### **Role-Based Administration**

PortSIP PBX supports a clear administrative hierarchy:

* **System administrators** manage the global platform and tenants
* **Tenant administrators** manage only their own organization

This separation of responsibilities aligns perfectly with Cloud PBX and UCaaS operational models.

#### **Ideal for Cloud PBX, UCaaS, and CCaaS**

Thanks to its true multi-tenant foundation, PortSIP PBX is ideally suited for delivering:

* Cloud PBX services
* UCaaS platforms
* CPaaS and CCaaS solutions
* Hosted contact center and unified communications services

***

### Summary

PortSIP PBX’s multi-tenant capability is a **core architectural principle**, not an add-on. Each tenant is fully isolated, completely transparent to others, and operates as a dedicated PBX, while service providers benefit from centralized control, scalability, and operational efficiency.



