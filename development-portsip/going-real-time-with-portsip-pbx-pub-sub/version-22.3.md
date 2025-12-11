# Version 22.3

PortSIP PBX exposes a set of **Pub/Sub topics** and structured **message keys** that allow external systems to receive real-time updates.

### **global\_extension\_management\_events**

System Administrators can subscribe to the `global_extension_management_events` topic to receive **instant notifications** whenever an extension is created, updated, or deleted.

#### **Permission**&#x20;

PBX System Administrators with either of the following permissions are allowed to subscribe to this event:

* **Users: View Only**
* **Users: Full Access**

#### **Message Keys**

Each published message includes the following keys:

* **event\_type**\
  Specifies the type of event. Possible values include:
  * `extension_created`
  * `extension_updated`
  * `extension_destroyed`
* **extension\_number**\
  The extension’s number.
* **disp\_name**\
  The display name associated with the extension.
* **email**\
  The email address assigned to the extension.
* **extension\_id**\
  The unique ID of the extension.
* **tenant\_id**\
  The ID of the tenant to which the extension belongs.
* **username**\
  The username associated with the extension.
* **department** _(optional)_\
  The department of extension.\
  If no department is configured, this key will not appear in the JSON payload.

```json
{
  "event_type": "extension_created",
  "extension_number": "1001",
  "disp_name": "Jason Wek",
  "email": "test@test.com",
  "extension_id": "823634238409396",
  "tenant_id": "883634229655633920",
  "username": "testuser1",
  "username": "testuser1",
  "enabled": true
  }
```

The `extension_updated` event includes the same information as the `extnesion_created` event.

***

### **extension\_events**

All extension-related event messages are published under the **extension\_events** topic. This topic provides real-time updates on an extension’s registration state and availability.

#### **Permission**

Users with either of the following permissions are allowed to subscribe to this event:

* **Users: View Only**
* **Users: Full Access**

The **event\_type** key identifies the specific message type, as described below.

#### **extension\_status**

The `extension_status` message is published in the following scenarios:

* **After a successful subscription**\
  When the client successfully subscribes to a specific extension, the PBX immediately returns the current status of that extension.
* **When the extension comes online**\
  A notification is sent when the subscribed extension registers successfully or becomes reachable.
* **When the extension goes offline**\
  A notification is sent when the subscribed extension unregisters or becomes unreachable.

```json
{
  "event_type": "extension_status",
  "status": [
    {
      "extension": "sip:101@test.io",
      "extension_id": "794493219508322304",
      "call_status": "ON_ACLL",
      "online": false,
      "presence" : "AWAY",
      "presence_note": "",
      "push_online": false,
      "time": "1704186780"
    },
    {
      "extension": "sip:102@test.io",
      "extension_id": "794493219718037504",
      "call_status": "ON_CALL",
      "online": false,
      "presence" : "ONLINE",
      "presence_note": "",
      "push_online": false,
      "time": "1704186780"
    }
  ],
  "tenant_id": "794493068765036544"
}
```

The message is delivered in **JSON format** and contains the following fields:

* **event\_type**\
  Identifies the type of extension event.
* **tenant\_id**\
  The ID of the tenant to which the extension belongs.
* **status**\
  A JSON array containing the status details of the extension. Each entry includes:
  * **extension**\
    The SIP URI of the extension.
  * **presence**\
    The presence status of the extension. Possible values include:\
    `ONLINE`, `LUNCH`, `BUSINESS_TRIP`, `DO_NOT_DISTURB`, `AWAY`.
  * **presence\_note**\
    Optional text providing additional presence information.
  * **call\_status**\
    Indicates the extension’s current call state:
    * `ON_CALL` — The extension is currently in an active call.
    * `RINGING` — The extension is currently receiving an incoming call.
    * _(empty)_ — The extension is not involved in any call.
  * **online**\
    Indicates whether the extension is currently registered to the PBX.
  * **push\_online**\
    Indicates whether push notifications are active for the extension.\
    This field is only meaningful when **online = false**.
  * **extension\_id**\
    The unique ID of the extension.
  * **time**\
    The timestamp of the event, represented in UNIX time.

#### **extension\_call**

The `extension_call` message is published in the following scenarios:

* **When an extension initiates an outbound call**\
  Triggered as soon as the extension begins dialing.
* **When an extension receives an inbound call**\
  Published when the extension is being called and starts ringing.
* **When an extension disconnects a call**\
  Generated when the extension ends or terminates an active call.

```json
{
  "event_type": "extension_call",
  "extension": "sip:101@test.io",
  "call_status": "ON_CALL",
  "tenant_id": "792406615960584192",
  "extension_id": "792406615960584220",
  "time": "1703690276"
}
```

The message is delivered in **JSON format** and contains the following fields:

* **event\_type**\
  Identifies the specific type of call-related event.
* **extension**\
  The SIP URI of the extension associated with this event.
* **call\_status**\
  Indicates the current call state of the extension:
  * `ON_CALL` — The extension is actively engaged in a call.
  * `RINGING` — The extension is receiving an incoming call.
  * _(empty)_ — The extension is not involved in any call.
* **tenant\_id**\
  The ID of the tenant to which the extension belongs.
* **extension\_id**\
  The unique ID of the extension.
* **time**\
  The timestamp of the event, represented in UNIX time.

#### **extension\_presence**

The `extension_presence` message is published in the following scenario:

* **When an extension updates its presence status**\
  Triggered whenever the extension’s presence state changes—for example, switching to _Do Not Disturb_, _Away_, or any other presence mode.

```json
{
  "event_type": "extension_presence",
  "extension": "sip:101@test.io",
  "presence": "AVAILABLE",
  "presence_note": "Driving",
  "tenant_id": "792406615960584192",
  "extension_id": "792406615960584220",
  "time": "1703690547"
}
```

The message is delivered in **JSON format** and includes the following fields:

* **event\_type**\
  Identifies the type of presence-related event.
* **extension**\
  The SIP URI of the extension whose presence has changed.
* **presence**\
  The current presence status of the extension. Possible values include:
  * `ONLINE`
  * `LUNCH`
  * `BUSINESS_TRIP`
  * `DO_NOT_DISTURB`
  * `AWAY`
* **presence\_note**\
  Optional text providing additional presence information.
* **tenant\_id**\
  The ID of the tenant to which the extension belongs.
* **extension\_id**\
  The unique ID of the extension.
* **time**\
  The timestamp of the event, represented in UNIX time.

#### **extension\_agent\_status**

When an extension that serves as a queue agent changes its status in any queue to which the subscriber is subscribed, the PBX publishes an `extension_agent_status` message.\
This notification is triggered whenever the agent’s state changes within any of the queues they are associated with.

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

The message is delivered in **JSON format** and includes the following fields:

* **event\_type**\
  Indicates the type of agent-related event.
* **extension**\
  The SIP URI of the extension (agent).
* **tenant\_id**\
  The ID of the tenant to which the extension belongs.
* **extension\_id**\
  The unique ID of the extension.
* **time**\
  The timestamp of the event, represented in UNIX time.
* **agent\_status**\
  A JSON array containing the agent’s status for each queue they belong to. Each entry includes:
  * **queue\_number**\
    The extension number of the queue.
  * **status**\
    The agent’s current status within that queue.

***

### **cdr\_events**

The `cdr_events` topic delivers real-time Call Detail Record (CDR) notifications for all calls within a tenant. Once a subscription is established, the PBX pushes call lifecycle updates and final CDR data to the subscriber.

#### **Permission**

Users with either of the following permissions are allowed to subscribe to this event:

* **Analytics: View Only**
* **Analytics: Full Access**

The following message types are published under this topic:

* **call\_start**\
  Triggered when an extension receives an incoming call. The initial call information is packaged into a JSON object and delivered to the subscriber.
* **call\_update\_info**\
  Published when call-related information changes (such as call state or routing updates). The updated call information is packaged into a JSON object and pushed to the subscriber.
* **call\_cdr**\
  Sent immediately after a call ends. The final CDR (Call Detail Record) is packaged into a JSON object and delivered to the subscriber.

For a detailed explanation of the CDR JSON structure, please refer to the [Event Reference](../webhook-notifications/event-reference.md) section.

### queue\_management\_events

The tenant Admin and Queue Manager have permission to subscribe to the `queue_management_events`. Once successfully subscribed, the PBX will push all current queue information to the subscriber. After that, during the subscription, whenever a queue is updated, deleted, or added, the subscriber will receive the queue information in real-time.

Below are the message keys.

* event\_type: Indicates the event type, it can have the following values:
  * queue\_created
  * queue\_updated
  * queue\_destroy
* queue\_number: The extension number of the queue.
* tenant\_id: The ID of the tenant to which this queue belongs.

```json
{
  "event_type": "queue_created",
  "queue_number": "8800",
  "tenant_id": "883634229655633920"
}
```

### queue\_events

Once the queue status is changed, for example, the caller who is in the queue has hung up the call, or the caller who is in the queue is answered by an agent, the related status information will be pushed to the subscribers. The message topic is **queue\_events.**&#x20;

Below are the message keys.

* **queue\_status**: If a subscriber successfully subscribed to queue events, the PBX will push the queue’s current status to the subscriber for all waiting for callers and agents. The `type` indicates that the waiting caller is active or is scheduled for a callback. The state of the agent can have the following values:
  * **Ready**: The agent is ready to accept ACD calls.
  * **Queue Call**: The agent is on an ACD call.
  * **Wrap Up**: The agent is in ACW (After Call Work).
  * **Other Call**: The agent is on a non-ACD call.
  * **Not Ready**: The agent is not ready to accept ACD calls.
  * **Online**: The agent has logged out of the queue
  * **Offline**: The agent is offline from the PBX.

```json
{
   "event_type":"queue_status",
   "tenant_id":"676398719834259456",
   "queue_id":"677046776364007424",
   "queue_number":"8001",
   "queue_name":"Sales Department",
   "waiting_list":[
      {
         "position": "2",
         "session_id": "6763987198342",
         "caller_number" : "123456",
         "caller_name":"Thomas Young",
         "timestamp":"1676206249",
         "type" : "normal/callback"
      }，
   "agents":[
      {
         "state":"QUEUE_CALL",
         "agent_extension_number" : "101",
         "session_id" : "677046776364007",
         "caller_number" : "123456",
         "caller_name":"Thomas Young",
         "timestamp":"1676206249"
      }
   ]
   ]
}
```

* **queue\_caller\_status**: if the waiting callers of a queue were changed, the PBX will push the **queue\_caller\_status** event in JSON format to the subscribers. The reason can be:
  * **enqueue**: This caller has just connected with the queue and is now on the waiting list.
  * **agent\_answered**: The caller left the queue since it was answered by an agent.
  * **overflow**: The caller reached the maximum waiting time of the queue and was forwarded to the overflow destination.
  * **hangup**: The caller hung up.
  * **callback**: The caller has successfully scheduled a callback.

```json
{
   "event_type":"queue_caller_status",
   "tenant_id":"676398719834259456",
   "queue_id":"677046776364007424",
   "updated_callers":[
      {
         "position": "2",
         "session_id": "6763987198342",
         "caller_number" : "123456",
         "reason" : "enqueue/agent_answered/overflow/hangup/callback",
         "timestamp":"1676206249",
         "type" : "normal/callback"
      }
   ]
}
```

* **queue\_agent\_status**: If an agent’s status in the queue changes, or a new agent is added to the queue, or an existing agent is removed from the queue, the PBX will push the **queue\_agent\_status** event in JSON format to the subscribers. The **removed\_agents** field indicates the agents that have been newly removed from the queue.

```json
{
   "event_type" : "queue_agent_status",
   "queue_id" : "677046776364007424",
   "tenant_id" : "676398719834259456",
   "updated_agents":[
      {
         "state":"QUEUE_CALL",
         "agent_extension_number" : "101",
         "session_id" : "677046776364007",
         "caller_number" : "123456",
         "caller_name":"Thomas Young",
         "timestamp":"1676206249"
      }
   ],
   "removed_agents":[
      {
         "agent_extension_number" : "102"
      }
      ]
}
```

* **queue\_sla\_breached**: If a caller is waiting in the queue and the SLA is breached, the PBX will push a notification to subscribers in JSON format indicating that the queue SLA has been breached.

```json
{
   "event_type" : "queue_sla_breached",
   "queue_id" : "677046776364007424",
   "tenant_id" : "676398719834259456",
   "caller" : "00431334081002",
   "total_callers_breached" : 6,
   "sla_time_secs" : 90,
   "waiting_callers" : 100,
   "timestamp":"1676206249"
}
```

## **Authentication**

Once connected to the WSI server, use the following JSON message to authenticate:

### **PBX System Administrator Authentication**

```json
{
"command":"auth",
"username":"admin",
"password":"A1s2d3f4",
}
```

The system administrator can currently subscribe only to extension management events. To subscribe to other events, a Tenant Admin or Queue Manager is required.

### Tenant User Authentication

Tenant administrators, users(extensions) can authenticate using either username/password or extension number/extension password.

#### Authenticate with Username

```json
{
"command":"auth",
"username":"testuser1",
"password":"A1s2d3f4",
"domain" : "test.io"
}
```

The **`domain`** is the SIP domain of the extension, the **`password`** is the **`user password`** of extension.

#### Authenticate with Extension Number

You can authenticate using the SIP extension number and its corresponding password as well.

```json
{
"command":"auth",
"extension_number":"101",
"password":"A1s2d3f4",
"domain" : "test.io"
}
```

If there is no error, the response is as follows:

```json
{
  "command": "auth",
  "id": "1",
  "status": 0
}
```

Otherwise, the response includes errors as below:

```json
{
  "command": "auth",
  "error": "access_token , username , domain or password error",
  "id": "2",
  "status": -1
}
```

## **Subscribe and Unsubscribe**

After successful authentication, the user can subscribe to events.

### Subscribe to CDR Event

To subscribe to **Call Detail Record (CDR)** events, send the following command. Once subscribed, all CDRs for the tenant the subscriber belongs to will be pushed to the subscriber. Only the **Tenant Administrator** has permission to subscribe.

```json
{
   "command":"subscribe",
   "topics":[
      "cdr_events"
   ]
}
```

### Subscribe to Global CDR Event

The **System Administrator** can subscribe to the global **CDR** event to receive CDR events from multiple tenants. To subscribe to the global CDR event for specific tenants, send the following command.

The "**tenants**" array should use the tenant's ID in string format as the elements.

```sh
{
   "command":"subscribe",
   "topics":[
      "global_cdr_events"
   ],
  "tenants":[
      "23435",
      "74235345"
   ]
}
```

### Subscribe to Extension Event

To subscribe to events for specific extensions, send the following command.

The "**extensions**" array should use the extension number as the elements.

```json
{
   "command":"subscribe",
   "topics":[
      "extension_events"
   ],
   "extensions":[
      "102",
      "103"
   ]
}
```

### Subscribe to Global Extension Event

The PBX administrators can subscribe to this event by specifying the tenant IDs as a JSON array.

The "**tenants**" array should use the tenant's ID in string format as the elements.

```json
{
   "command":"subscribe",
   "topics":[
      "global_extension_events"
   ],
   "tenants":[
      "23435",
      "74235345"
   ]
}
```

### Subscribe to Global Extension Management Event

PBX System Administrators can subscribe to extension management events by specifying the tenant IDs as a JSON array.

The "**tenants**" array should use the tenant's ID in string format as the elements.

```json
{
   "command":"subscribe",
   "topics":[
      "global_extension_management_events"
   ],
   "tenants": [
   "12345",
   "235346"
   ]
}
```

### Subscribe Queue Event

#### **Subscription Limitations**

* **Extension-Specific Subscriptions:** Extensions can only subscribe to queues that they belong to or manage. If an extension is not an agent or queue manager of a queue, it cannot subscribe to that queue's events.
* **Tenant Admin and Queue Manager Permissions:** Tenant Admin and Queue Manager users have the permission to subscribe to any queue wiin a tenant.

If extension 101 is an agent or queue manager of queues 8001 and 8002, and it subscribes to queue events using the following command.&#x20;

The "**queues**" array should use the queue number as the elements.

```json
{
   "command":"subscribe",
   "topics":[
      "queue_events"
   ],
   "queues":[
      "8001",
      "8002"
   ]
}
```

### Subscribe Global Queue Event

Only the PBX administrators can subscribe to this event.&#x20;

The "**tenants**" array should use the tenant's ID in string format as the elements.

```json
{
   "command":"subscribe",
   "topics":[
      "global_queue_events"
   ],
   "tenants":[
      "325435",
      "534888"
   ]
}
```

### Subscribe to Queue Management Event

Tenant Admin and Queue Manager users can subscribe to the `queue_management_events` event.

```json
{
   "command":"subscribe",
   "topics":[
      "queue_management_events"
   ]
}
```

### Subscribe to Global Queue Management Event

Only the PBX Administrator can subscribe to this event.&#x20;

The "**tenants**" array should use the tenant's ID in string format as the elements.

```json
{
   "command":"subscribe",
   "topics":[
      "global_queue_management_events"
   ],
   "tenants":[
   "325435",
   "534888"
   ]
}
```

### Unsubscribe

To unsubscribe from all events, send the following command.&#x20;

The "**queues**" array and "**extensions**" array should use the queue number, extension number as the elements.

```json
{
   "command":"unsubscribe",
   "topics":[
      "queue_events"
   ],
   "queues":[
      "8001",
      "8002"
   ]
}
```

```json
{
   "command":"unsubscribe",
   "topics":[
      "extension_events"
   ],
   "extensions":[
      "101",
      "102"
   ]
}
```



