# Version 22.9

PortSIP PBX exposes Pub/Sub topics and structured message payloads that enable external systems to receive real-time updates.

#### Global topic access

The following topics can be subscribed to by **System Administrators** and **Dealers** with the **System Tenant: View Only** permission. Dealers can subscribe only to their own tenants and must specify the relevant tenant IDs in the `tenants` array.

* `global_extension_events`
* `global_cdr_events`
* `global_extension_management_events`
* `global_queue_events`
* `global_queue_management_events`

The `global_all_extension_management_events` topic is available **only to System Administrators** with **System Tenant: View Only** permission. It provides extension management events for **all tenants across the PBX** and does **not** require a `tenants` field.

***

### `global_extension_management_events`

System Administrators and Dealers can subscribe to the `global_extension_management_events` topic to receive real-time notifications when extensions are created, updated, or deleted in specified tenants. Dealers may specify only tenants they own.

#### Permissions

System Administrators and Dealers must have the following permission:

* **System Tenant: View Only**

Dealers must specify their own tenant IDs in the `tenants` array.

***

### `global_all_extension_management_events`

Only System Administrators can subscribe to the `global_all_extension_management_events` topic. This topic publishes extension creation, update, and deletion events for **every tenant on the PBX**, without requiring tenant IDs in the subscription request.

#### Permissions

System Administrators must have the following permission:

* **System Tenant: View Only**

The `tenants` field is **not required**. Dealers cannot subscribe to this topic.

#### Message keys

The event types and message keys are the same as those described under `extension_management_events`.

***

### `extension_management_events`

Tenant users can subscribe to the `extension_management_events` topic to receive real-time notifications when extensions are created, updated, or deleted within their tenant.

#### Permissions

Tenant users must have the following permission:

* **Users: View Only**

#### Message keys

| Key                     | Description                                                                                                          |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `event_type`            | Type of extension management event: `extension_created`, `extension_updated`, or `extension_destroyed`.              |
| `extension_number`      | Extension number.                                                                                                    |
| `disp_name`             | Display name associated with the extension.                                                                          |
| `email`                 | Email address assigned to the extension.                                                                             |
| `extension_id`          | Unique identifier of the extension.                                                                                  |
| `tenant_id`             | Identifier of the tenant to which the extension belongs.                                                             |
| `username`              | Username associated with the extension.                                                                              |
| `enabled`               | Boolean indicating whether the extension user is enabled (`true`) or disabled (`false`).                             |
| `department` (optional) | Department associated with the extension. If no department is configured, this key is omitted from the JSON payload. |

**Example: `extension_created`**

```json
{
  "event_type": "extension_created",
  "extension_number": "1001",
  "disp_name": "Jason Wek",
  "email": "test@test.com",
  "extension_id": "823634238409396",
  "tenant_id": "883634229655633920",
  "username": "testuser1",
  "enabled": true
}
```

The `extension_updated` event includes the same information as the `extension_created` event.

***

### `global_extension_events`

System Administrators and Dealers can subscribe to the `global_extension_events` topic to receive extension-related events for specified tenants. This topic provides real-time updates on extension registration and availability. Dealers may subscribe only to their own tenants.

#### Permissions

System Administrators and Dealers must have the following permission:

* **System Tenant: View Only**

Dealers must specify their own tenant IDs in the `tenants` array.

***

### `extension_events`

The `extension_events` topic publishes extension-related events within a tenant, providing real-time updates on extension registration and availability.

#### Permissions

Tenant users must have at least one of the following permissions:

* **Users: View Only**
* **PhoneSystem: View Only**

The `event_type` key identifies the event type, as described below.

***

### `extension_status`

The `extension_status` message is published in the following scenarios:

* **After a successful subscription:** When a client successfully subscribes to a specific extension, the PBX immediately sends the extension's current status.
* **When an extension comes online:** A notification is sent when the subscribed extension registers successfully or becomes reachable.
* **When an extension goes offline:** A notification is sent when the subscribed extension unregisters or becomes unreachable.

```json
{
  "event_type": "extension_status",
  "status": [
    {
      "extension": "sip:101@test.io",
      "extension_id": "794493219508322304",
      "call_status": "ONCALL",
      "online": false,
      "presence": "AWAY",
      "presence_note": "",
      "push_online": false,
      "time": "1704186780"
    },
    {
      "extension": "sip:102@test.io",
      "extension_id": "794493219718037504",
      "call_status": "ONCALL",
      "online": false,
      "presence": "ONLINE",
      "presence_note": "",
      "push_online": false,
      "time": "1704186780"
    }
  ],
  "tenant_id": "794493068765036544"
}
```

The JSON message contains the following fields:

| Key                      | Description                                                                                                                             |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| `event_type`             | Type of extension event.                                                                                                                |
| `tenant_id`              | Identifier of the tenant to which the extension belongs.                                                                                |
| `status`                 | Array containing extension status details. Each entry includes the fields below.                                                        |
| `status[].extension`     | SIP URI of the extension.                                                                                                               |
| `status[].presence`      | Presence state of the extension. The complete set of values is `ONLINE`, `LUNCH`, `BUSINESS_TRIP`, `DO_NOT_DISTURB`, and `AWAY`.        |
| `status[].presence_note` | Optional text providing additional presence information.                                                                                |
| `status[].call_status`   | Call status reported by the PBX. Values: `ONCALL` (active call), `RINGING` (incoming call ringing), or an empty value (no active call). |
| `status[].online`        | Indicates whether the extension is currently registered with the PBX.                                                                   |
| `status[].push_online`   | Indicates whether push notifications are active for the extension. This field is meaningful only when `online` is `false`.              |
| `status[].extension_id`  | Unique identifier of the extension.                                                                                                     |
| `status[].time`          | Event timestamp in Unix time.                                                                                                           |

***

### `extension_call`

The `extension_call` message is published in the following scenarios:

* **When an extension initiates an outbound call:** Triggered as soon as the extension starts dialing.
* **When an extension receives an inbound call:** Published when the extension receives a call and starts ringing.
* **When an extension disconnects a call:** Generated when the extension ends or terminates an active call.

```json
{
  "event_type": "extension_call",
  "extension": "sip:101@test.io",
  "call_status": "ONCALL",
  "tenant_id": "792406615960584192",
  "extension_id": "792406615960584220",
  "time": "1703690276"
}
```

The JSON message contains the following fields:

| Key            | Description                                                                                                                             |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `event_type`   | Type of call-related event.                                                                                                             |
| `extension`    | SIP URI of the extension associated with the event.                                                                                     |
| `call_status`  | Call status reported by the PBX. Values: `ONCALL` (active call), `RINGING` (incoming call ringing), or an empty value (no active call). |
| `tenant_id`    | Identifier of the tenant to which the extension belongs.                                                                                |
| `extension_id` | Unique identifier of the extension.                                                                                                     |
| `time`         | Event timestamp in Unix time.                                                                                                           |

***

### `extension_presence`

The `extension_presence` message is published whenever an extension changes its presence state, such as switching to Do Not Disturb, Away, or another presence mode.

```json
{
  "event_type": "extension_presence",
  "extension": "sip:101@test.io",
  "presence": "ONLINE",
  "presence_note": "Driving",
  "tenant_id": "792406615960584192",
  "extension_id": "792406615960584220",
  "time": "1703690547"
}
```

The JSON message contains the following fields:

| Key             | Description                                                                                                             |
| --------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `event_type`    | Type of presence-related event.                                                                                         |
| `extension`     | SIP URI of the extension whose presence has changed.                                                                    |
| `presence`      | Current presence state. The complete set of values is `ONLINE`, `LUNCH`, `BUSINESS_TRIP`, `DO_NOT_DISTURB`, and `AWAY`. |
| `presence_note` | Optional text providing additional presence information.                                                                |
| `tenant_id`     | Identifier of the tenant to which the extension belongs.                                                                |
| `extension_id`  | Unique identifier of the extension.                                                                                     |
| `time`          | Event timestamp in Unix time.                                                                                           |

***

### `extension_agent_status`

When an extension serving as a queue agent changes its status in a queue to which the client is subscribed, the PBX publishes an `extension_agent_status` message. This notification is triggered whenever the agent's state changes in any relevant queue.

```json
{
  "agent_status": [
    {
      "queue_number": "8002",
      "status": "READY"
    }
  ],
  "event_type": "extension_agent_status",
  "extension": "sip:102@test.io",
  "extension_id": "806406815892897792",
  "tenant_id": "806406773714976768",
  "time": "1707028116"
}
```

The JSON message contains the following fields:

| Key                           | Description                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------- |
| `event_type`                  | Type of agent-related event.                                                  |
| `extension`                   | SIP URI of the extension acting as the agent.                                 |
| `tenant_id`                   | Identifier of the tenant to which the extension belongs.                      |
| `extension_id`                | Unique identifier of the extension.                                           |
| `time`                        | Event timestamp in Unix time.                                                 |
| `agent_status`                | Array containing the agent's status in each queue to which the agent belongs. |
| `agent_status[].queue_number` | Extension number of the queue.                                                |
| `agent_status[].status`       | Agent's current status in the queue.                                          |

***

### `global_cdr_events`

System Administrators and Dealers can subscribe to the `global_cdr_events` topic to receive real-time Call Detail Record (CDR) notifications for calls in specified tenants. Dealers may subscribe only to their own tenants. Once subscribed, the PBX sends call lifecycle updates and final CDR data to the subscriber.

#### Permissions

System Administrators and Dealers must have the following permission:

* **System Tenant: View Only**

Dealers must specify their own tenant IDs in the `tenants` array.

***

### `cdr_events`

The `cdr_events` topic delivers real-time CDR notifications for calls within a tenant. Once subscribed, the PBX pushes call lifecycle updates and final CDR data to the subscriber.

#### Permissions

Tenant users must have at least one of the following permissions:

* **Analytics: View Only**
* **Analytics: Full Access**

#### Message types

| `event_type`       | Description                                                                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `call_start`       | Published when an extension receives an incoming call. The initial call information is delivered as a JSON object.                                      |
| `call_update_info` | Published when call-related information changes, such as call state or routing information. The updated call information is delivered as a JSON object. |
| `call_cdr`         | Published immediately after a call ends. The final CDR is delivered as a JSON object.                                                                   |

For details of the CDR JSON structure, see the **Event Reference** section.

***

### `global_queue_management_events`

System Administrators and Dealers can subscribe to the `global_queue_management_events` topic for specified tenants. Dealers may subscribe only to their own tenants. Immediately after a successful subscription, the PBX sends information about existing queues in the subscribed tenants. It then sends real-time notifications when those queues are created, updated, or deleted.

#### Permissions

System Administrators and Dealers must have the following permission:

* **System Tenant: View Only**

Dealers must specify their own tenant IDs in the `tenants` array.

***

### `queue_management_events`

Tenant users can subscribe to the `queue_management_events` topic. Immediately after a successful subscription, the PBX sends information about all existing queues to the subscriber. The PBX then sends real-time notifications with the corresponding queue details whenever a queue is created, updated, or deleted.

#### Permissions

Tenant users must have the following permission:

* **Analytics: View Only**

#### Message keys

| Key            | Description                                                                        |
| -------------- | ---------------------------------------------------------------------------------- |
| `event_type`   | Queue management event type: `queue_created`, `queue_updated`, or `queue_destroy`. |
| `queue_number` | Extension number assigned to the queue.                                            |
| `tenant_id`    | Identifier of the tenant that owns the queue.                                      |

**Example: `queue_created`**

```json
{
  "event_type": "queue_created",
  "queue_number": "8800",
  "tenant_id": "883634229655633920"
}
```

***

### `global_queue_events`

The `global_queue_events` topic delivers real-time queue status updates for specified tenants to System Administrators and Dealers. Dealers may subscribe only to their own tenants. For example, when a caller waiting in a queue hangs up or an agent answers a queued call, the PBX immediately sends the updated queue status to subscribers.

#### Permissions

System Administrators and Dealers must have the following permission:

* **System Tenant: View Only**

Dealers must specify their own tenant IDs in the `tenants` array.

***

### `queue_events`

The `queue_events` topic delivers real-time updates whenever a queue's status changes. For example, when a waiting caller hangs up or an agent answers a queued call, the PBX immediately sends the updated queue status to subscribers.

#### Permissions

Tenant users must have the following permission:

* **Analytics: View Only**

***

### `queue_status`

After a successful subscription, the PBX sends the queue's current status, including all waiting callers and agents assigned to the queue.

Each caller entry indicates whether the caller is waiting for an agent or has scheduled a callback. The `type` field contains either `normal` or `callback` (one value per caller). Each agent entry reports the agent's current state:

| Agent state | Description                                                                |
| ----------- | -------------------------------------------------------------------------- |
| Ready       | The agent is available to receive Automatic Call Distribution (ACD) calls. |
| Queue Call  | The agent is handling an ACD (queue) call.                                 |
| Wrap Up     | The agent is performing After Call Work (ACW).                             |
| Other Call  | The agent is on a non-ACD call.                                            |
| Not Ready   | The agent is unavailable to receive ACD calls.                             |
| Online      | The agent has logged out of the queue.                                     |
| Offline     | The agent is offline from the PBX.                                         |

```json
{
  "event_type": "queue_status",
  "tenant_id": "676398719834259456",
  "queue_id": "677046776364007424",
  "queue_number": "8001",
  "queue_name": "Sales Department",
  "waiting_list": [
    {
      "position": "2",
      "session_id": "6763987198342",
      "caller_number": "123456",
      "caller_name": "Thomas Young",
      "timestamp": "1676206249",
      "type": "normal"
    }
  ],
  "agents": [
    {
      "state": "QUEUE_CALL",
      "agent_extension_number": "101",
      "session_id": "677046776364007",
      "caller_number": "123456",
      "caller_name": "Thomas Young",
      "timestamp": "1676206249"
    }
  ]
}
```

***

### `queue_caller_status`

The `queue_caller_status` event is published whenever a queue's waiting caller list changes. Each caller entry contains exactly one `reason` value from the following list; its `type` field is either `normal` or `callback`:

| Reason           | Description                                                                                                |
| ---------------- | ---------------------------------------------------------------------------------------------------------- |
| `enqueue`        | The caller entered the queue and was added to the waiting list.                                            |
| `agent_answered` | The caller left the queue because an agent answered the call.                                              |
| `overflow`       | The caller reached the configured maximum waiting time and was routed to the queue's overflow destination. |
| `hangup`         | The caller hung up while waiting in the queue.                                                             |
| `callback`       | The caller successfully scheduled a callback request.                                                      |

```json
{
  "event_type": "queue_caller_status",
  "tenant_id": "676398719834259456",
  "queue_id": "677046776364007424",
  "updated_callers": [
    {
      "position": "2",
      "session_id": "6763987198342",
      "caller_number": "123456",
      "reason": "enqueue",
      "timestamp": "1676206249",
      "type": "normal"
    }
  ]
}
```

***

### `queue_agent_status`

The `queue_agent_status` event provides real-time updates about agents assigned to a queue. Whenever an agent's status changes, an agent is added to the queue, or an existing agent is removed, the PBX publishes a JSON message to subscribers.

The `updated_agents` array contains the current status entries for the affected agents. The `removed_agents` array identifies agents removed from the queue since the previous update.

```json
{
  "event_type": "queue_agent_status",
  "queue_id": "677046776364007424",
  "tenant_id": "676398719834259456",
  "updated_agents": [
    {
      "state": "QUEUE_CALL",
      "agent_extension_number": "101",
      "session_id": "677046776364007",
      "caller_number": "123456",
      "caller_name": "Thomas Young",
      "timestamp": "1676206249"
    }
  ],
  "removed_agents": [
    {
      "agent_extension_number": "102"
    }
  ]
}
```

***

### `queue_sla_breached`

The `queue_sla_breached` event indicates that a caller's waiting time has exceeded the configured Service Level Agreement (SLA) threshold. When an SLA breach occurs, the PBX sends a JSON notification to subscribers.

```json
{
  "event_type": "queue_sla_breached",
  "queue_id": "677046776364007424",
  "tenant_id": "676398719834259456",
  "caller": "00431334081002",
  "total_callers_breached": 6,
  "sla_time_secs": 90,
  "waiting_callers": 100,
  "timestamp": "1676206249"
}
```

***

## Authentication

Once connected to the WSI server, authenticate using one of the following JSON messages.

#### PBX System Administrator authentication

```json
{
  "command": "auth",
  "username": "admin",
  "password": "A1s2d3f4"
}
```

System Administrators with **System Tenant: View Only** permission can subscribe to the five tenant-scoped global topics listed above, as well as `global_all_extension_management_events`. Dealers with the same permission can subscribe to the five tenant-scoped global topics, restricted to their own tenants. **Dealers use the same WSI authentication message format as System Administrators**, with their own credentials. Tenant-scoped topics are subject to their respective tenant-level permissions.

#### Dealer authentication

Dealers authenticate using the same WSI `auth` message format as PBX System Administrators, supplying their own username and password. Their permissions and accessible tenants are determined by their Dealer account.

```json
{
  "command": "auth",
  "username": "dealer_username",
  "password": "dealer_password"
}
```

#### Tenant user authentication

Tenant administrators and extension users can authenticate using either a username and user password or an extension number and extension password.

**Authenticate with a username**

```json
{
  "command": "auth",
  "username": "testuser1",
  "password": "A1s2d3f4",
  "domain": "test.io"
}
```

The `domain` value is the extension's SIP domain, and `password` is the extension user's password.

**Authenticate with an extension number**

You can also authenticate using an SIP extension number and its corresponding password.

```json
{
  "command": "auth",
  "extension_number": "101",
  "password": "A1s2d3f4",
  "domain": "test.io"
}
```

**Successful response**

```json
{
  "command": "auth",
  "id": "1",
  "status": 0
}
```

**Error response**

```json
{
  "command": "auth",
  "error": "access_token , username , domain or password error",
  "id": "2",
  "status": -1
}
```

***

## Subscribe and Unsubscribe

After successful authentication, clients can subscribe to events.

#### Subscribe to CDR events

To receive Call Detail Record (CDR) events, send the following command. Once subscribed, the client receives all CDR events for its tenant. Only the Tenant Administrator has permission to subscribe.

```json
{
  "command": "subscribe",
  "topics": ["cdr_events"]
}
```

#### Subscribe to global CDR events

System Administrators and Dealers can subscribe to global CDR events for specified tenants. Dealers must specify only their own tenants. Provide tenant IDs as strings in the `tenants` array.

```json
{
  "command": "subscribe",
  "topics": ["global_cdr_events"],
  "tenants": ["23435", "74235345"]
}
```

#### Subscribe to extension events

To receive events for specific extensions, include their extension numbers as strings in the `extensions` array.

```json
{
  "command": "subscribe",
  "topics": ["extension_events"],
  "extensions": ["102", "103"]
}
```

#### Subscribe to global extension events

System Administrators and Dealers can subscribe to global extension events for specified tenants by providing their IDs as strings in the `tenants` array. Dealers may specify only their own tenants.

```json
{
  "command": "subscribe",
  "topics": ["global_extension_events"],
  "tenants": ["23435", "74235345"]
}
```

#### Subscribe to global extension management events

System Administrators and Dealers can subscribe to extension management events for specified tenants by providing their IDs as strings in the `tenants` array. Dealers may specify only their own tenants.

```json
{
  "command": "subscribe",
  "topics": ["global_extension_management_events"],
  "tenants": ["12345", "235346"]
}
```

#### Subscribe to extension management events across all tenants

Only System Administrators with **System Tenant: View Only** permission can subscribe to `global_all_extension_management_events`. The subscription covers all tenants on the PBX; **do not include a `tenants` field**.

```json
{
  "command": "subscribe",
  "topics": ["global_all_extension_management_events"]
}
```

#### Subscribe to queue events

**Subscription restrictions**

* **Extension-specific subscriptions:** An extension can subscribe only to queues in which it is an agent or queue manager. An extension that has neither role in a queue cannot subscribe to that queue's events.
* **Tenant Admin and Queue Manager access:** Tenant Admin and Queue Manager users can subscribe to any queue within their tenant.

For example, if extension `101` is an agent or queue manager for queues `8001` and `8002`, it can subscribe to those queues using the following command. The `queues` array contains queue numbers as strings.

```json
{
  "command": "subscribe",
  "topics": ["queue_events"],
  "queues": ["8001", "8002"]
}
```

#### Subscribe to global queue events

System Administrators and Dealers can subscribe to global queue events for specified tenants. Dealers may subscribe only to their own tenants. Provide tenant IDs as strings in the `tenants` array.

```json
{
  "command": "subscribe",
  "topics": ["global_queue_events"],
  "tenants": ["325435", "534888"]
}
```

#### Subscribe to queue management events

Tenant Admin and Queue Manager users can subscribe to the `queue_management_events` topic.

```json
{
  "command": "subscribe",
  "topics": ["queue_management_events"]
}
```

#### Subscribe to global queue management events

System Administrators and Dealers can subscribe to global queue management events for specified tenants. Dealers may subscribe only to their own tenants. Provide tenant IDs as strings in the `tenants` array.

```json
{
  "command": "subscribe",
  "topics": ["global_queue_management_events"],
  "tenants": ["325435", "534888"]
}
```

#### Unsubscribe

To unsubscribe from the specified queue or extension events, send an `unsubscribe` command. Use queue numbers in the `queues` array and extension numbers in the `extensions` array.

**Unsubscribe from queue events**

```json
{
  "command": "unsubscribe",
  "topics": ["queue_events"],
  "queues": ["8001", "8002"]
}
```

**Unsubscribe from extension events**

```json
{
  "command": "unsubscribe",
  "topics": ["extension_events"],
  "extensions": ["101", "102"]
}
```



