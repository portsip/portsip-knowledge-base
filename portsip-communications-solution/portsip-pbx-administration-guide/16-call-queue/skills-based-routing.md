# Skills-Based Routing

### Overview

Every call center agent has encountered irate customer situations. To reduce customer frustration, a contact center must resolve issues **quickly and accurately**. One of the most effective ways to achieve this is ensuring that calls are handled by agents with the **right skills**.

This is where **skills-based routing** becomes essential.

***

### What Is Skills-Based Routing?

**Skills-based routing** (also known as skill-based distribution) is a call routing strategy that assigns incoming calls to agents based on the skills most relevant to the customer’s needs.

**Example:**\
Spanish-speaking customers are routed to agents who speak Spanish.

PortSIP PBX supports Skills-Based Routing by automatically routing incoming queue calls to agents in the **highest-skill group first**. If no agents in that group are available (busy, unavailable, or logged out), the call is routed to agents in **lower skill groups**.

This ensures:

* Customers are handled by qualified agents
* Call resolution times are reduced
* Customer frustration is minimized

***

### Route Calls by Caller Language

You can route calls by language using **Inbound Rules** based on the caller ID (CID) mask.

#### Example Scenario

DID number: **00442012345670**

Create two inbound rules using the same DID but different CID masks:

* **Rule 1**
  * CID mask: `0044**********`
  * DID mask: `442012345670`
  * Route to: **English Support Queue (8000)**
* **Rule 2**
  * CID mask: `0033*********`
  * DID mask: `442012345670`
  * Route to: **French Support Queue (9000)**

Assign English-speaking agents to queue **8000** and French-speaking agents to queue **9000**.

**Result:**

* Calls from the UK are routed to English-speaking agents
* Calls from France are routed to French-speaking agents

***

### Route Calls by VIP Priority

PortSIP PBX provides a **VIP Caller** feature to prioritize high-value customers.

When a VIP call is detected:

* The caller is placed at the **front of the queue**
* The call is always handled with **highest priority**

#### Configure VIP Callers

1. Navigate to **Contact Center > VIP Numbers**.
2. Click **Add**.
3. Configure the following fields:
   * **Enabled** – Turn VIP handling on or off
   * **VIP Number** – Enter the customer phone number
   * **Description** – Friendly name (for example, _Microsoft Team_)
   * **Validity Period** – Define how long the VIP entry remains active

> ❗**Note**\
> The VIP list applies globally to **all queues within the tenant**.

***

### Exclusive Agent Routing

Some callers—such as those from specialized industries—require agents with **specific domain expertise**.

The **Exclusive Agent** feature allows you to assign one or more agents as **priority handlers** for specific callers.

#### How Exclusive Agent Routing Works

* When a call arrives from a configured caller number:
  * The call is routed to an exclusive agent with **highest priority**
* If all exclusive agents are **busy, not ready, or signed out**:
  * The call is routed to other available agents in the queue

> ❗**Limitation**\
> Exclusive Agent routing is **not supported** when the queue Ring Strategy is set to **Ring Simultaneously**.

#### Configure Exclusive Agents

1. Navigate to **Contact Center > Exclusive Agent**.
2. Click **Add**.
3. Configure the following:
   * **Description** – For example, _XXX Bank_
   * **Caller Number** – Caller numbers eligible for exclusive handling
   * **Call Queue** – Select the queue and assign exclusive agents

If the call does **not** match the configured caller numbers, the agent behaves as a normal queue agent.

***

### Route Calls by Agent Skill Level

PortSIP PBX supports multiple **skill-based routing strategies** within call queues:

* **Skill-Based Routing – Prioritized Hunt**\
  Agents are rung serially in a configured order, starting with the highest skill group.
* **Skill-Based Routing – Cyclic Hunt**\
  Agents are rung serially, prioritizing the agent who has not been rung for the longest time, starting with the highest skill group.
* **Skill-Based Routing – Least Worked Hunt**\
  Agents are rung serially, prioritizing the agent who has answered the fewest calls, starting with the highest skill group.

When adding agents to a queue, you assign a **skill level**:

* Higher number = higher skill level

<figure><img src="../../../.gitbook/assets/skill_based_routing_1.png" alt=""><figcaption></figcaption></figure>

***

### Last Called Agent Routing (LCA)

**Last Called Agent Routing** routes repeat callers to the agent who last handled their call.

#### How It Works

1. A customer calls the contact center.
2. The PBX stores the agent ID and call time.
3. When the same customer calls again:
   * The call is routed to the **same agent**, if available
4. If the agent is unavailable:
   * The call is routed to another suitable agent

> ❗**Limitation**\
> LCA routing is **not supported** when the queue Ring Strategy is set to **Ring Simultaneously**.

***

### Harass Numbers (Spam Call Protection)

Spam calls are a major issue for contact centers. PortSIP PBX provides **two layers** of protection.

#### Global Number Blacklist

* Calls from blacklisted numbers are **silently rejected**
* Configure under **Blacklist and Codes > Number Blacklist**

***

#### Harass Number (Queue-Specific Protection)

Harass Numbers apply **only to Call Queues** and operate in two levels:

**Level 1 Harass Number**

* A warning prompt is played
* Caller options:
  * Press **1** – Hang up
  * Press **2** – Continue the call

**Level 2 Harass Number**

* A warning prompt is played
* The call is **automatically disconnected** after playback

#### Configure Harass Numbers

1. Navigate to:
   * **Contact Center > Harass Number Level 1**
   * **Contact Center > Harass Number Level 2**
2. Open **Global Settings**
3. Enable or disable:
   * **Enable Level 1 Harass Number**
   * **Enable Level 2 Harass Number**
4. Upload the corresponding prompt files

***

### Best Practices

* Use **skills-based routing** to reduce call transfers and improve first-call resolution
* Combine **VIP routing** with **exclusive agents** for premium customers
* Enable **LCA routing** to improve customer continuity
* Use **harass number protection** to reduce spam without blocking legitimate callers
* Avoid **Ring Simultaneously** when advanced routing logic is required





