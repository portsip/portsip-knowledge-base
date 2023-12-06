# Going Real-Time with PortSIP PBX Pub/Sub

PortSIP PBX provides the Pub/Sub mechanism which is based on the WebSocket (PortSIP WSI). The user is able to create the WebSocket in any programming language to subscribe to the PBX events, once the subscribed events occur, PortSIP PBX will push the event message to the subscriber automatically, the message is in the JSON format.

Support version: v16.0 or higher

## Service

The PortSIP PBX/UCaaS provides WSI on port **8885** over **WSS**, the server must be allowed this port on the firewall for TCP, which requires WSS(TLS).

## **Topics and Keys**

PortSIP PBX provides the below topics and keys for the Pub/Sub.

### extension\_events

All extension-related event messages will be published by **`extension_events`** . Below, are the various message keys.

* `extension_register`: extension registered to the PBX or unregister from the PBX.

For example, the extension 102 registered to PBX, the subscriber will receive the below message:

```json
{
    "event_type":"extension_register",
    "extension":"sip:102@portsip.io",
    "extension_id":"494521070842286080",
    "registration_contacts":[
        {
            "contact_uri":"<sip:102@113.246.194.98:10061>;+sip.instance=\"<urn:uuid:F36DDB5A-5CC3-A59A-0FB2-E53F7A56F245>\"",
            "expires":1632668422,
            "instance":"<urn:uuid:F36DDB5A-5CC3-A59A-0FB2-E53F7A56F245>",
            "last_update":1632668122,
            "public_address":"113.246.194.98:10061",
            "received_from":"113.246.194.98:10061",
            "reg_id":0,
            "user_agent":"Test SIPMaster for IOS"
        },
        {
            "contact_uri":"<sip:102@113.246.194.98:10243>;+sip.instance=\"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>\"",
            "expires":1632668476,
            "instance":"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>",
            "last_update":1632668176,
            "public_address":"113.246.194.98:10243",
            "received_from":"113.246.194.98:10243",
            "reg_id":0,
            "user_agent":"PortSIP UC Client iOS - v16.0.001"
        }
    ],
    "tenant_id":"236047118140313600",
    "time":"1632668176"
}
```

The `extension` is indicates which extension occurs the register event, the `extension_id` is the id of that extension.  The array`registration_contacts`  includes the current registrations information.&#x20;

In this example, there have two SIP client devices/apps registered to PBX, their agents are: `Test SIPMaster for IOS` and `PortSIP UC Client iOS - v16.0.001`, if both of these clients are unregistered from PBX, the array "`registration_contacts`"  will not present, see below example:

```json
{
    "event_type":"extension_register",
    "extension":"sip:102@portsip.io",
    "extension_id":"494521070842286080",
    "removed_contacts":[
        {
            "contact_uri":"<sip:102@113.246.194.98:10243>;expires=0;+sip.instance=\"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>\"",
            "expires":1632668724,
            "instance":"<urn:uuid:8D5941A6-EB96-4E7A-9EB7-DA940E56513C>",
            "last_update":1632668724,
            "public_address":"113.246.194.98:10243",
            "received_from":"113.246.194.98:10243",
            "reg_id":0,
            "user_agent":"PortSIP UC Client iOS - v16.0.001"
        }
    ],
    "tenant_id":"236047118140313600",
    "time":"1632668724"
}
```

In the above message, the array`registration_contacts` is no longer present, but has an array that is`removed_contacts`  indicates which client app/device is unregistered from PBX, in case, the client App`PortSIP UC Client iOS - v16.0.001`  is unregistered from PBX, now there are no registrations on the PBX so extension 102 is offline.

* `extension_presence`: extension changed his presence status.

Once an extension changed his presence status, the `extesnion_presence` event will occur, see the below example:&#x20;

```json
{"event_type":"extension_presence",
"extension":"sip:102@portsip.io",
"note":"Away",
"tenant_id":"672371581028143104",
"time":"1675136792"}
```

In the above example, extension 102 changed his presence status to `Away`.

* `call_hold`: call was held.

Once an established call is held by the caller or callee, the `call_hold` event will occur, see the below example:&#x20;

```json
{
    "aor":"sip:102@portsip.io",
    "call_id":"09jV-7QM2B-3E0Q-m20exw..",
    "event_type":"call_hold",
    "peer_aor":"sip:103@portsip.io",
    "session_id":"494527456623988736",
    "tenant_id":"236047118140313600",
    "time":"1632669389"
}
```

In the above example, the call is held by extension 102, and another party of the call is extension 103.

* `call_unhold`: call has been resumed from hold.

Once a call is resumed from the held state, the `call_unhold` event occurs.&#x20;

```json
{
    "aor":"sip:102@portsip.io",
    "call_id":"09jV-7QM2B-3E0Q-m20exw..",
    "event_type":"call_unhold",
    "peer_aor":"sip:103@portsip.io",
    "session_id":"494527456623988736",
    "tenant_id":"236047118140313600",
    "time":"1632669492"
}
```

In the above example, the call is un-hold by extension 102, and another party of the call is extension 103.

* `call_start`: call starting.

When the caller starts to make a call to the callee, this event will occur.

```json
{
    "call_id":"rjFyDxQSbXPxXAd1IR8W-g..",
    "callee":"sip:103@portsip.io",
    "caller":"sip:102@portsip.io",
    "caller_display_name":"",
    "cdr_id":"494533487185891328",
    "direction":"ext",
    "event_type":"call_start",
    "request_id":"0",
    "session_id":"494533487177502720",
    "tenant_id":"236047118140313600",
    "time":"1632670771"
}
```

In the above example, extension 102 makes the call to callee 103,  the  `direciton` is `ext` means the call is between extensions. the `session_id` is a unique ID of this call, after the call is completed, we can use this session\_id to query the recording file by REST API.

* `call_established`: the call was answered and successfully connected.

```json
{
    "call_id":"K3jV2vbPzplkrC3cIBTGPg..",
    "call_target":"sip:103@portsip.io",
    "callee":"sip:103@portsip.io",
    "caller":"sip:102@portsip.io",
    "event_type":"call_established",
    "session_id":"494535522404798464",
    "tenant_id":"236047118140313600",
    "time":"1632671262"
}
```

In the above example, the call is answered by the callee extension 103.

* `call_ended`: call has ended.

```json
{
    "call_id":"K3jV2vbPzplkrC3cIBTGPg..",
    "call_target":"",
    "callee":"sip:103@portsip.io",
    "caller":"sip:102@portsip.io",
    "event_type":"call_ended",
    "session_id":"494535522404798464",
    "tenant_id":"236047118140313600",
    "time":"1632671272"
}
```

In the above example, the call is ended.

* `call_noanswer`: call is a no answer in the specified time(seconds) then hang up due to timeout.

```json
{
    "call_id":"mbZA5vcQHZOkrPZfHjtsdQ..",
    "call_target":"",
    "callee":"sip:103@portsip.io",
    "caller":"sip:102@portsip.io",
    "event_type":"call_noanswer",
    "session_id":"494536565997965312",
    "tenant_id":"236047118140313600",
    "time":"1632671510"
}
```

* `call_reroute`: the call was rerouted to another target.
* `call_fail`: call has failed.

```json
{
    "call_id":"9SyfcKeZvuIVipZmefINbw..",
    "call_target":"",
    "callee":"sip:103@portsip.io",
    "caller":"sip:102@portsip.io",
    "event_type":"call_fail",
    "fail_code":480,
    "session_id":"494537752386211840",
    "tenant_id":"236047118140313600",
    "time":"1632671788"
}
```

In the above example, the 102 makes a call to 103, but the 103 is offline, so the call fails, and the `fail_code`is 480 which is the same as the status code of the SIP standards.

* `target_add`: start a call to a target. For example, extension 101 is registered to PBX from an IP Phone and an App, when someone makes calls to 101, the IP Phone and app will be added as the target. (The target\_add event will be triggered twice.)
* `target_ringing`: the called target is ringing.
* `target_noanswer`: there is no answer from the called target.
* `target_fail`: call failed from the called target. For example, the App / IP Phone rejected the call.
* `target_ended`: call has ended from the called target. For example, the App / IP Phone hangs up the call.

### cdr\_events

Once a call has ended, the CDR of this call will be pushed to the subscribers, the message topic is: **`cdr_events`**, the message key is below.

* `call_cdr`: once a call has ended, the CDR will be packed in JSON format and pushed to the subscriber.

For more details please refer to this [topic](20-cdr-and-call-recordings/#cdr).

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

In order to subscribe to the events, a user needs to establish a session by opening a WebSocket connection to the listening port (8885) of the PortSIP PBX with authentication credentials. This requires a previously established user account on the PortSIP PBX. The user account can be an extension or a tenant.

You can use the below JSON message to do the authorization:

```json
{
"command":"auth",
"username":"testuser",
"domain":"portsip.io",
"password":"111111"
}
```

The **`domain`** is the SIP domain of the extension, the **`password`** is the **`user password`** of extension.

If there is no error, the response is as below:

```json
{"status":0}
```

Otherwise, the response includes errors as below:

```json
{"error":"name or password error","status":-1}
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
"103"] 
}
```

If we want to subscribe to both extension events and CDR events, use the below command.

```json
{
"command":"subscribe",
"topics":
[
  "extension_events,
  "cdr_events"
],
"extensions":
[
"102",
"103"
]  
}
```

If we want to unsubscribe from the events, use the below command.

```json
{
"command" : "unsubscribe",
"topics":
[
  "extension_events", 
  "topic" : "cdr_events"
],
"extensions":
[
"102",
"103"
]  
}
```

If we just want to unsubscribe from the CDR events, use the below command.

```json
{
"command":"unsubscribe",
"topics":[ "cdr_events" ] 
}
```

If we want to subscribe to the queue status, use the below command.

Note, that the extension only has permission to subscribe to the queues belonging to that extension, if the extension(subscriber) is not a member of the queue, and also not a queue manager of the queue, the events will not push to the extension(subscriber). For example, if extension 101 is the member/agent/queue manager of queue 8001 and 8002, after 101 is subscribed to queue events, both 8001 and 8002 queue status will be pushed to extension 101.

The admin user role of the tenant and queue manager has permission to subscribe to any queues.

```json
{
"command":"subscribe",
"topics":[ "queue_events" ],
"queues":
[
"8001",
"8002"
]  
}
```

