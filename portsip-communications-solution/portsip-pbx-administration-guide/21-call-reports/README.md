# 21 Call Reports

### Call Reporting Overview

In-depth call reporting is essential for gaining real-time insights into **Unified Communications and contact center performance**, as well as understanding customer demand and agent workload.

Key questions call reporting helps answer include:

* How long do agents spend on calls?
* How many calls do they make and receive?
* What is the overall performance of call queues?
* Which agents may be underperforming or experiencing burnout?
* What are the SLA statistics, unanswered calls, abandoned calls, agent activities, queue performance, and callbacks?

With **PortSIP PBX Call Reporting**, you can generate reports for detailed analysis, monitor report status, and share insights across the organization. Reports can be:

* Viewed directly in the browser
* Printed
* Delivered via email

By leveraging these reports, you can make informed business decisions, restructure your contact center, optimize agent productivity, and improve customer satisfaction.

To support ongoing monitoring and analysis, PortSIP provides a wide range of **predefined reports**, which can be customized to meet your operational requirements.

***

### Call Reporting Modes

PortSIP PBX offers **two call reporting modes**, depending on whether the **Data Flow** service is deployed.

***

### Call Reports

Starting with **v22.3.0**, PortSIP PBX introduced the **Data Flow** service, built on **ClickHouse**, to deliver high-performance analytics and reporting.

* Call Reports data is queried from the **Data Flow service**.
* Optimized for **high-speed queries and large-scale datasets**.
* Ideal for dashboards, wallboards, and advanced analytics.
* **Available only when the Data Flow service is installed and running**.

***

### Call Reports (Legacy)

If the **Data Flow service** is not installed, the modern Call Reports feature is unavailable. However, you can still access **Call Reports (Legacy)**.

* Call report data is queried directly from the **PostgreSQL database**.
* Provides basic reporting functionality for historical analysis.
* Intended primarily for compatibility and smaller deployments.



