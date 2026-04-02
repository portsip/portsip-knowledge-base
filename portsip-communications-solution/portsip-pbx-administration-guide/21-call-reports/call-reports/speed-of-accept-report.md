# Speed Of Accept Report

### Speed of Accept Report Overview

The **Speed of Accept Report** helps you analyze how long interactions waited in the queue before being accepted.

This report provides a summary of delays associated with interactions that were accepted or pulled from specified queues. It includes both percentages and the number of interactions accepted or pulled, segmented by service time intervals.

The report categorizes interactions into one of ten time buckets based on the time taken for the interaction to be accepted or pulled from the selected queue. "Acceptance" is triggered when the first agent answers the call.

It also provides a breakdown of the percentages of interactions accepted within each bucket, relative to the total number of interactions accepted or pulled from the queue during the reporting interval. The first bucket is defined by the report variable "Accepted Agent ST1 - ST10," which combines the first through tenth service time intervals.

Note that this report only reflects interactions from the selected queues. It does not account for time spent in unselected queue resources, through which the interactions may have passed before being distributed from the selected queue(s).

### Filters

The **Speed of Accept Report** supports the following filters:

* **Range:** Choose the date and time range for the report.
* **Select Queues:** Choose the queues for which you want to generate the report.
* **Accepted Agent ST1 (Seconds) - Accepted Agent ST10 (Seconds):** Specify the time intervals (in seconds) that define how long interactions waited in the queue before being accepted. For example, if "**Accepted Agent ST1 (Seconds**)" is set to 5 seconds and "**ST2**" is set to 15 seconds, interactions answered within 5 seconds will be counted in the "**Accepted Agent ST1**" bucket, while those answered within 15 seconds will be counted in the "**Accepted Agent ST2**" bucket.

***

### Metrics Used in the Speed of Accept Report

| **Metric**                        | **Description**                                                                                                                                                                                                                                                                           |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Accepted Agent ST1**            | The total number of times that interactions entered this queue and were subsequently accepted, answered, or pulled by an agent before the first service time interval threshold.                                                                                                          |
| **Accepted Agent ST2 ... ST10**   | The total number of times that interactions entered the queue and were accepted, answered, or pulled by an agent within the service time interval defined by two service time thresholds.                                                                                                 |
| **% Accepted Agent ST1**          | The percentage of interactions that were accepted, answered, or pulled before the first service time interval threshold, relative to the total number of interactions accepted by agents in the queue.                                                                                    |
| **% Accepted Agent ST2 ... ST10** | The percentage of interactions accepted by agents within each service time interval, relative to the total number of interactions accepted. For example, **% Accepted Agent ST10** represents the percentage of interactions accepted within the ninth and tenth service time thresholds. |

