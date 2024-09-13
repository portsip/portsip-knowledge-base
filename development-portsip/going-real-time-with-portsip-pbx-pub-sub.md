# WSI: Pub/Sub

PortSIP PBX offers a Pub/Sub mechanism using WebSocket (PortSIP WSI). This allows users to subscribe to PBX events in any programming language. When subscribed events occur, PortSIP PBX automatically pushes event messages to subscribers in JSON format.

Support version: v16.0 or higher

## Service Port

The PortSIP PBX/UCaaS provides WSI on port **8887** over **WSS**, the server must be allowed this port on the firewall for TCP, which requires WSS(TLS).

## **Topics and Message Type**

PortSIP PBX provides the below topics and keys for the Pub/Sub.

### extension\_management\_events

Tenant Administrators and Queue Managers can subscribe to the `extension_management_events` topic. Once subscribed, they'll receive real-time notifications whenever an extension is added, updated, or deleted. The following message keys are included:

* `event_type`**:** Indicates the specific event:
  * &#x20;`extension_created`
  * &#x20;`extension_updated`&#x20;
  * `extension_destroyed`
* `extension_number`**:** The unique identifier for the extension.
* `extension_id`**:** The internal ID of the extension.
* `tenant_id`**:** The ID of the tenant associated with the extension.

```json
{
  "event_type": "extension_created",
  "extension_number": "1001",
  "extension_id": "823634238409396",
  "tenant_id": "883634229655633920"
}
```

The `extension_updated` event includes a `enabled` key that indicates the current status of the extension, which can be either `true` or `false`.

```json
{
  "event_type": "extension_created",
  "extension_number": "1001",
  "extension_id": "823634238409396",
  "tenant_id": "883634229655633920"
  "enabled" : true
}
```

### extension\_events

All extension-related event messages will be published under `extension_events`. The following are the various message keys:

#### extension\_status

This message will be returned under the following scenarios:

* Upon successful subscription to an extension.
* When a subscribed extension comes online.
* When a subscribed extension goes offline.

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
      "presence" : "AVAILABLE",
      "presence_note": "",
      "push_online": false,
      "time": "1704186780"
    }
  ],
  "tenant_id": "794493068765036544"
}
```

The message is in JSON format and includes the following fields:

* `event_type`: Indicates the type of the message.
* `tenant_id`: Represents the ID of the tenant to which the extension belongs.
* `status`: It's a JSON array that includes the extension status, including the following fields:
  * `extension`: Represents the SIP URI of the extension.
  * `presence`: the presence status with the enum string:&#x20;
    * `DO_NOT_DISTURB`
    * `AVAILABLE`
    * `AWAY`
    * `BUSINESS_TRIP`
    * `LUNCH`
  * `presence_note`: Contains the text of the additional presence status.
  * `call_status`: The "**ON\_CALL**" status signifies that the extension is currently engaged in a call. The "**RINGING**" status means that the extension is currently receiving a call and is ringing. If this value is empty, it indicates that the extension is not involved in any call.
  * `online`: Indicates whether the extension is currently registered to the PBX.
  * `push_online`: This field indicates whether mobile push notifications are currently enabled for the extension. This is only valid if `online` is false.
  * `extension_id`: Represents the ID of the extension.
  * `time`: Represents the timestamp of this message in UNIX time.

#### extension\_call

This message will be returned under the following scenarios:

* When an extension starts dialing a call.
* When an extension receives a call.
* When an extension disconnects a call.

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

The message is in JSON format and includes the following fields:

* `event_type`: Indicates the type of the message.
* `extension`: Represents the SIP URI of the extension.
* `call_status`: The "**ON\_CALL**" status signifies that the extension is currently engaged in a call. The "**RINGING**" status means that the extension is currently receiving a call and is ringing. If this value is empty, it indicates that the extension is not involved in any call.
* `tenant_id`: Represents the ID of the tenant to which the extension belongs.
* `extension_id`: Represents the ID of the extension.
* `time`: Represents the timestamp of this message in UNIX time.

#### extension\_presence

This message will be returned under the following scenario:

* When an extension changes its presence status.

```json
{
  "event_type": "extension_presence",
  "extension": "sip:101@test.io",
  "presence" : "AVAILABLE",
  "presence_note": "Drving",
  "tenant_id": "792406615960584192",
  "extension_id": "792406615960584220",
  "time": "1703690547"
}
```

The message is in JSON format and includes the following fields:

* `event_type`: Indicates the type of the message.
* `extension`: Represents the SIP URI of the extension.
* `presence`: the presence status with the enum string:&#x20;
  * `DO_NOT_DISTURB`
  * `AVAILABLE`
  * `AWAY`
  * `BUSINESS_TRIP`
  * `LUNCH`
* `presence_note`: Contains the text of the presence status.
* `tenant_id`: Represents the ID of the tenant to which the extension belongs.
* `extension_id`: Represents the ID of the extension.
* `time`: Represents the timestamp of this message in UNIX time.

#### extension\_agent\_status

When an extension, is also an agent of the queue, who belongs to a queue to which a subscription has been made, changes their status within any of the queues they are associated with, a notification will be sent to the subscriber.

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

The message is in JSON format and includes the following fields:

* `event_type`: Indicates the type of the message.
* `extension`: Represents the SIP URI of the extension.
* `tenant_id`: Represents the ID of the tenant to which the extension belongs.
* `extension_id`: Represents the ID of the extension.
* `time`: Represents the timestamp of this message in UNIX time.
* `agent_status`: A JSON array that contains the status of the queues:
  * `queue_number`: The extension number of the queue
  * `status`: the agent status of the queue

### cdr\_events

Once a call has ended, the CDR of this call will be pushed to the subscribers, this event means subscribing to all calls CDR of a tenant. The message topic is: **`cdr_events`**, the message key is below.

* `call_start`: Once an extension receives a call, the call information will be packed in a JSON object and pushed to the subscriber.
* `call_update_info`: The call information is updated, and the call information will be packed in a JSON object and pushed to the subscriber.
* `call_cdr`: Once a call has ended, the CDR will be packed in a JSON object and pushed to the subscriber.

For more details about the CDR JSON object structure information, please refer to [Event Reference](webhook-notifications/event-reference.md).

### queue\_management\_events

The tenant Admin and Queue Manager have permission to subscribe to the `queue_management_events`. Once successfully subscribed, the PBX will push all current queue information to the subscriber. After that, during the subscription, whenever a queue is updated, deleted, or added, the subscriber will receive the queue information in real-time.

Below are the message keys.

* event\_type: Indicates the event type, it can have the following values:
  * queue\_created
  * queue\_updated
  * queue\_destroy
* queue\_number: The extension number of the queue.
* tenant\_id: The ID of the tenant which this queue is belongs.

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
  * **Logout**: The agent has logged out from the queue.
  * **Offline**: The agent is offline.

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

## **Subscribe and Unsubscribe**

In order to subscribe to the events, a user needs to establish a session by opening a WebSocket connection to the listening port (8887) of the PortSIP PBX with authentication credentials. This requires a previously established user account on the PortSIP PBX. The user account can be an extension or a tenant.

### Server URL

The WSI Server URL varies between PortSIP PBX versions:

* **v22.0 or higher**: `wss://pbx.portsip.com:8887/wsi`
* **v16.x**: `wss://pbx.portsip.com:8885`

The WebSocket client application needs to connect to the appropriate URL. Please replace `pbx.portsip.com` with your actual PBX domain.

After successfully connecting to the WSI server, you can use the following JSON message for authorization:

```json
{
"command":"auth",
"username":"testuser1",
"password":"A1s2d3f4",
"domain" : "test.io"
}
```

The **`domain`** is the SIP domain of the extension, the **`password`** is the **`user password`** of extension.

You can also use the **`SIP extension number`** with the **`SIP password`** to do the authorization:

```json
{
"command":"auth",
"extension_number":"101",
"password":"A1s2d3f4",
"domain" : "test.io"
}
```

If there is no error, the response is as below:

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

After successfully being authenticated, the user can now subscribe to the events.

For instance, if extension 101 wishes to subscribe to extension 102, and 103 events, just send the below command to subscribe.

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

If we want to subscribe to both extension events and CDR events, use the below command.

```json
{
   "command":"subscribe",
   "topics":[
      "extension_events",
      "cdr_events"
   ],
   "extensions":[
      "102",
      "103"
   ]
}
```

If we want to subscribe to CDR events only, use the below command.

```json
{
   "command":"subscribe",
   "topics":[
      "cdr_events"
   ]
}
```

Note, that the extension only has permission to subscribe to the queues belonging to that extension, if the extension(subscriber) is not an agent of the queue, and also not a queue manager of the queue, the events will not push to the extension(subscriber).&#x20;

For example, if extension 101 is the agent/queue manager of queues 8001 and 8002 after 101 is subscribed to queue events, both 8001 and 8002 queue status will be pushed to extension 101.

The admin user role of the tenant and queue manager has permission to subscribe to any queues.

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

Subscribe to the `queue_management_events`:

```json
{
   "command":"subscribe",
   "topics":[
      "queue_management_events"
   ]
}
```

If we want to unsubscribe from the events, use the below command, all subscriptions will be terminated.

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

Tenant Administrators and Queue Managers can subscribe to the `extension_management_events` topic using the following JSON request:

```json
{
   "command":"subscribe",
   "topics":[
      "extension_management_events"
   ]
}
```

To unsubscribe from the topic, send the following request:

```json
{
   "command":"unsubscribe",
   "topics":[
      "extension_management_events"
   ]
}
```


