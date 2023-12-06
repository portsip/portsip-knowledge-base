# Going Real-Time with PortSIP PBX Pub/Sub

PortSIP PBX provides the Pub/Sub mechanism which is based on the WebSocket(PortSIP WSI). The user is able to create the WebSocket in any programming language to subscribe to the PBX events, once the subscribed events occur, PortSIP PBX will push the event message to the subscriber automatically, the message is in the JSON format.

## Sevice

The PortSIP PBX/UCaaS provides WSI on port 8885, the server must be allowed this port on the firewall for TCP, which requires SSL.

## **Topics and keys**

PortSIP PBX provides below topics and keys for the Pub/Sub.

### extension\_events

All extension-related event messages will be published by **`extension_events`** . Below, are the various message keys.

* `extension_register`: extension registered to the PBX or un-register from the PBX.

For example, the extension 102 registered to PBX, the subscriber will receive the below message:

```
{
    "event_type":"extension_register",
    "extension":"sip:102@sipiw.com",
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
    "tenant_name":"admin",
    "time":"1632668176"
}
```

The `extension` is indicates which extension occurs the register event, the `extension_id` is  the id of that extension.  The array`registration_contacts`  is includes the current registrations information. In this example, there have two SIP client devices/apps are registered to PBX, their agents are: `Test SIPMaster for IOS` and `PortSIP UC Client iOS - v16.0.001`, if both of these clients are unregistered from PBX, the array "`registration_contacts`"  will not presents, see below example:

```
{
    "event_type":"extension_register",
    "extension":"sip:102@sipiw.com",
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
    "tenant_name":"admin",
    "time":"1632668724"
}
```

In the above message, the array`registration_contacts` is no longer present, but have an array is`removed_contacts`  indicates which client app/device is unregistered from PBX, in case, the client App`PortSIP UC Client iOS - v16.0.001`  is unregistered from PBX, now there are no any registrations on the PBX so the extension 102 is offline.

* `call_hold`: call was held.

Once an established call is held by the caller or callee, the `call_hold` event will occur, see below example:&#x20;

```
{
    "aor":"sip:102@sipiw.com",
    "call_id":"09jV-7QM2B-3E0Q-m20exw..",
    "event_type":"call_hold",
    "peer_aor":"sip:103@sipiw.com",
    "session_id":"494527456623988736",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632669389"
}
```

In the above example, the call is hold by extension 102, and another party of the call is extension 103.

* `call_unhold`: call has been resumed from hold.

Once a call is resumed from the held state, the `call_unhold` event occurs.&#x20;

```
{
    "aor":"sip:102@sipiw.com",
    "call_id":"09jV-7QM2B-3E0Q-m20exw..",
    "event_type":"call_unhold",
    "peer_aor":"sip:103@sipiw.com",
    "session_id":"494527456623988736",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632669492"
}
```

In the above example, the call is un-hold by extension 102, and another party of the call is extension 103.

* `call_start`: call starting.

When the caller starting to make a call to the callee, this event will occurs.

```
{
    "call_id":"rjFyDxQSbXPxXAd1IR8W-g..",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "caller_display_name":"",
    "cdr_id":"494533487185891328",
    "direction":"ext",
    "event_type":"call_start",
    "request_id":"0",
    "session_id":"494533487177502720",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632670771"
}
```

In the above example, extension 102 is make the call to callee 103,  the  `direciton` is `ext` means the call is between extensions. the `session_id` is a unique ID of this call, after call is completed, we can use this session\_id to query the recording file by REST API.

* `call_established`: the call was answered and successfully connected.

```
{
    "call_id":"K3jV2vbPzplkrC3cIBTGPg..",
    "call_target":"sip:103@sipiw.com",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_established",
    "session_id":"494535522404798464",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632671262"
}
```

In the above example, the call is answered by the callee extension 103.

* `call_ended`: call has ended.

```
{
    "call_id":"K3jV2vbPzplkrC3cIBTGPg..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_ended",
    "session_id":"494535522404798464",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632671272"
}
```

In the above example, the call is ended.

* `call_noanswer`: call is a no answer in the specified time(seconds) then hang up due to timeout.

```
{
    "call_id":"mbZA5vcQHZOkrPZfHjtsdQ..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_noanswer",
    "session_id":"494536565997965312",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632671510"
}
```

* `call_reroute`: the call was re-routed to another target.
* `call_fail`: call has failed.

```
{
    "call_id":"9SyfcKeZvuIVipZmefINbw..",
    "call_target":"",
    "callee":"sip:103@sipiw.com",
    "caller":"sip:102@sipiw.com",
    "event_type":"call_fail",
    "fail_code":480,
    "session_id":"494537752386211840",
    "tenant_id":"236047118140313600",
    "tenant_name":"admin",
    "time":"1632671788"
}
```

In the above example, the 102 make a call to 103, but the 103 is offline, so the call is failed, and the `fail_code`is 480 which is the same as the status code of the SIP standards.

* `target_add`: start a call to a target. For example, extension 101 is registered to PBX from an IP Phone or an App, when someone makes calls to 101, the IP Phone, or App will be added as the target. (The target\_add event will be triggered two times.)
* `target_ringing`: the called target is ringing.
* `target_noanswer`: there is no answer from the called target.
* `target_fail`: call failed from the called target. For example, the App / IP Phone rejected the call.
* `target_ended`: call has ended from the called target. For example, the App / IP Phone hangs up the call.

### cdr\_events

Once a call has ended, the CDR of this call will be pushed to the subscribers, the message topic is: **`cdr_events`**, the message key is below.

* `call_cdr`: once a call has ended, the CDR will be packed in JSON format and pushed to the subscriber.

### queue\_events

Once the queue status is changed, for example, the caller who is in the queue has hung up the call, or the caller who is in the queue is answered by an agent, the related status information will be pushed to the subscribers. The message topic is **queue\_events.** The below is the message keys.

* `queue_status`: if the queue status changed, the information will be packed into a JSON message and pushed to the subscriber.
* `queue_member_state`: if the agent of the queue state is set to ready or not-ready, this message will be pushed.

### trunk\_events

Once a trunk state is changed, for example, the PBX successfully registered to a Trunk, or register to the Trunk fails, or the registration is lost from a trunk, or a connection timeout, PortSIP PBX will push the message information to the subscribers, the message topic is **trunk\_events**, the keys are below.

* `trunk_connected`: PBX is successfully connected to a trunk.
* `trunk_disconnected`: PBX is successfully connected to a trunk.



### **Subscribe and unsubscribe**

In order to subscribe to the events, a user needs to establish a session by opening a WebSocket connection to the listening port (8885) of the PortSIP PBX with authentication credentials. This requires a previously established user account on the PortSIP PBX. The user account can be an extension or a tenant.

You can use the below JSON message to do the authorization:

```
{
"command":"auth",
"extension":"101",
"domain":"test.com",
"password":"111111"
}
```

The **`domain`** is the SIP domain of the extension, the **`password`** is the **`web password`** of extension.

If the tenant wishes to subscribe to the events, authorization is required as below:

```
{
"command" : "auth",
"name": "tenant name",
"password": "111111"
}
```

If there is no error, the response is as below:

```
{"status":0}
```

Otherwise the response including errors as below:

```
{"error":"name or password error","status":-1}
```

After successfully being authenticated, the user can now subscribe to the events.

For instance, the extension 101 wishes to subscribe to extension 102, 103 events(**the 101, 102, 103 must in the same extension group**), just send the below command to subscribe.

```

{
"command":"subscribe",
"topics":[
 { 
 "topic" : "extension_events",
 "keys": ["extension_register", "call_start", "call_fail", "call_reroute", "call_noanswer", "call_hold", "call_unhold", "call_ended", "call_established", "target_add", "target_fail", "target_noanswer", "target_ringing", "target_ended"] 
 } 
],
"extensions":[
"102",
"103"] 
}
```

If we want to subscribe to both extension events and CDR events, use the below command.

```
{
"command":"subscribe",
"topics":[
 { 
 "topic" : "extension_events",
 "keys":["extension_register", "call_start", "call_fail", "call_reroute", "call_noanswer", "call_hold", "call_unhold", "call_ended", "call_established", "target_add", "target_fail", "target_noanswer", "target_ringing", "target_ended"] 
},
 { 
 "topic" : "cdr_events",
 "keys":["call_cdr"] 
 } 
],
"extensions":[
"102",
"103"]  
}
```

If we want to unsubscribe from the events, use the below command.

```
{
"command":"unsubscribe",
"topics":[
 { 
 "topic" : "extension_events",
 "keys":["extension_register", "call_start", "call_fail", "call_reroute", "call_noanswer", "call_hold", "call_unhold", "call_ended", "call_established", "target_add", "target_fail", "target_noanswer", "target_ringing", "target_ended"] 
},
 { 
 "topic" : "cdr_events",
 "keys":["call_cdr"] 
 } 
],
"extensions":[
"102",
"103"]  
}
```

If we just want to unsubscribe from the CDR events, use the below command.

```
{
"command":"unsubscribe",
"topics":[
 { 
 "topic" : "cdr_events",
 "keys":["call_cdr"] 
 } 
] 
}
```

If we want to subscribe to the queue status, use the below command.

Note, the extension only have permission to subscribe to the queues belonging to that extension, if the extension(subscriber) is not a member of the queue, and also not a queue manager of the queue, the events will not push to the extension(subscriber). For example, if extension 101 is the member/agent/queue manager of queue 8001 and 8002, after 101 is subscribed to queue events, both 8001 and 8002 queue status will be pushed to extension 101.

The tenant has permission to subscribe to any queues.

```
{
"command":"subscribe",
"topics":[
 { 
 "topic" : "queue_events",
 "keys":["queue_status"] 
 } 
],
"queues":[
"8001",
"8002"]  
}
```

If we want to subscribe to the trunk events, use the below commands.

{% hint style="info" %}
**Note: only the tenant has the permissions to subscribe to the trunk events.**
{% endhint %}

```
{
"command":"subscribe",
"topics":[
 { 
 "topic" : "trunk_events",
 "keys":["trunk_connected", "trunk_disconnected"] 
 } 
] 
}
```

After successfully being subscribed to the trunk events, once a trunk is connected or disconnected, the PBX will push information to the subscriber.
