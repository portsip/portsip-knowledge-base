# Silent Monitoring

### Overview

**Contact center monitoring** is the practice of listening to sales or support agent calls—either **in real time** or **after the call has completed**—to evaluate call handling quality and agent performance.

In simple terms:

**Call Center Monitoring = listening to live or recorded agent calls**

This practice is essential for improving agent communication skills, customer satisfaction, and overall service quality.

***

### Benefits of Real-Time Monitoring

Real-time call monitoring provides several operational advantages:

* Supervisors can step in to assist struggling agents immediately
* Junior agents receive hands-on coaching during live calls
* Customers get faster resolution without requiring follow-up calls
* Supervisors can focus on immediate problem-solving for the customer

***

### Enabling Call Monitoring

To enable call monitoring:

1. Sign in to the **PortSIP PBX Web Portal**.
2. Navigate to **Contact Center > Monitor Service**.
3. Turn on the **Enable Monitor** option.

> **Note**\
> The default monitoring service number is **888**.\
> It is strongly recommended **not to change this number**.

***

### Creating a Monitor Group

Monitor Groups define:

* Which agents can be monitored
* Which supervisors are allowed to monitor them

To create a Monitor Group:

1. Navigate to **Contact Center > Monitor Group**.
2. Click **Add**.

#### Configure the Monitor Group

* **Information**\
  Enter a friendly name for the monitor group.
* **Enable**\
  Enable or disable the monitor group.
* **Group Members**\
  Agents whose calls can be monitored.\
  Click **Add** to include agents.
* **Supervisors**\
  Users who are authorized to monitor the group members.

Click **OK** to save the configuration.

***

### Silent Monitoring (Listen-Only)

#### Example Scenario

You want supervisors **101** and **102** to silently monitor agents **600** and **601**.

#### Configuration Steps

1. Create a Monitor Group named **Queue Management**.
2. Enable the group.
3. Add extensions **600** and **601** as **Group Members**.
4. Add extensions **101** and **102** as **Supervisors**.
5. Click **OK** to save.

***

### Monitoring Agent Calls

When an agent is on an active call:

* A supervisor can start silent monitoring by dialing the **Feature Access Code (FAC)** followed by the agent’s extension.

#### Examples

* Silent monitor agent **600**:\
  &#xNAN;**\*63600**
* Silent monitor agent **601**:\
  &#xNAN;**\*63601**

***

### Supervisor Call Control During Monitoring

While silently monitoring, the supervisor can use DTMF keys to change the monitoring mode:

* **Press 2** – Whisper to the agent (agent hears supervisor; caller does not)
* **Press 3** – Barge in (supervisor joins the call with both agent and caller)
* **Press 4** – Barge-break (supervisor takes over the call; agent is removed)

***

### Direct Monitoring with Mode Selection

Supervisors can also start monitoring with a specific mode directly:

* **Silent monitoring**:\
  \*63601\*1
* **Whisper**:\
  \*63601\*2
* **Barge-in**:\
  \*63601\*3
* **Barge-break**:\
  \*63601\*4

> **Tip**\
> The exact Feature Access Codes (FACs) can be reviewed or customized under\
> **Advanced > Feature Access Codes**.

***

### Best Practices

* Limit monitoring permissions to authorized supervisors only
* Use silent monitoring for quality assurance and coaching
* Use whisper mode for real-time agent guidance without affecting customers
* Reserve barge-in and barge-break for escalation or critical situations
* Inform agents of monitoring policies to comply with local regulations





