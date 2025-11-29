# Install PortSIP Data Flow Server

### Instructions

PortSIP PBX v22.3 introduces a new component: **PortSIP Data Flow Service** — a high-performance analytics engine built on **ClickHouse**.\
This service powers the following advanced features:

* Call Detail Record (CDR) Storage and Analytics
* Comprehensive Call Reports
* Real-Time Data Dashboards
* Queue Wallboards for Contact Center Operations

ClickHouse is optimized for handling **large-scale datasets,** such as billions of CDRs and real-time queue or agent activities. It provides rapid query and analytics capabilities ideally suited for service providers and enterprise deployments.

### Deployment Guidelines

Because ClickHouse is resource-intensive and optimized for analytics workloads, **the PortSIP Data Flow service must be installed on a separate server**.\
Deploying it on the same server as the PBX core service may degrade overall performance due to heavy CPU and memory usage during data processing.

> **Important:**\
> Do **not** install the Data Flow service on the same server as the PortSIP PBX.\
> Running both services together may impact PBX call performance and overall system stability.

#### Hardware Requirements

The PortSIP Data Flow service can be installed on either a **physical server** or a **virtual machine**.\
For optimal performance, ensure your hardware meets or exceeds the specifications below.

**Minimum Requirements**

* **vCPU:** 4 cores
* **Memory:** 16 GB
* **Disk:** 128 GB SSD

**Recommended Requirements**

* **vCPU:** 8 cores
* **Memory:** 32 GB
* **Disk:** 128 GB or higher (preferably NVMe SSD)

#### Hardware Sizing Formula

For large-scale deployments, use the following formula to estimate hardware requirements:

* **vCPU:** ≥ 8
* **Memory:** vCPU × 4 GB
* **Disk:** Based on expected CDR volume and desired retention period

#### Supported Operating Systems

The PortSIP Data Flow service supports **64-bit Linux** only.\
The following operating systems are officially supported:

* **Ubuntu:** 22.04, 24.04
* **Debian:** 11.x, 12.x

