# Push CDR

### Push CDR to Webhook

You can collect **Call Detail Records (CDRs)** by sending an HTTP request to an external service or application each time a call is completed.

A **webhook** is an event-driven mechanism that allows PortSIP PBX to notify an external system when specific events occur, such as call start or call completion.

Only the **Tenant Administrator** has permission to enable or disable this feature.

#### Configuration

1. Select **Company** from the left menu.
2. On the **Event URL** page, enable **CDR Events**.
3. Choose an **Authentication Method**.
4. Enter your **Webhook URL**.

Once configured, PortSIP PBX will push CDR-related events to the specified third-party webhook endpoint.

***

#### Example: Call Start Event

Below is an example generated when **extension 102** receives a call.

```json
{
  "event_type": "call_start",
  "call_id": "Q0sXhDgxnowmQp1P9swHLw..",
  "callee": "sip:102@test.io",
  "callee_display_name": "Jason",
  "caller": "sip:101@test.io",
  "caller_display_name": "Tom",
  "cdr_id": "798119942409949185",
  "direction": "ext",
  "session_id": "798119942409949184",
  "tenant_id": "798119491077668864",
  "time": "1705051422"
}
```

***

#### Example: Call Detail Record (CDR)

The following example represents a completed call.\
The call flow is: **caller > trunk > PBX IVR > Queue > Agent**

```json
{
   "event_type":"call_cdr",
   "answered_time":"1679476851",
   "call_id":"b6PEEYEcn5speSovMAybCw..",
   "callee":"5000",
   "callee_domain":"test.io",
   "caller":"1888722",
   "caller_display_name":"",
   "caller_domain":"192.168.0.16",
   "did_cid":"18800606",
   "direction":"in",
   "ended_reason":"callee_disconnect",
   "ended_time":"1679476870",
   "outbound_caller_id":"",
   "ring_time":"1679476851",
   "session_id":"690852418417594368",
   "start_time":"1679476851",
   "status_code":"0",
   "tenant_id":"690833127458734080",
   "tenant_name":"PortSIP Inc.",
   "trunk_name":"AudioCodes"
   "user_data":"",
   "call_targets":[
      {
         "answered_time":"1679476851",
         "call_type":"normal_call",
         "callee":"5000",
         "caller":"1888722",
         "destination":"sip:5000@192.168.0.16:8922",
         "end_reason":"referred",
         "ended_time":"1679476857",
         "id":"690833127458734080",
         "outbound_caller_id":"",
         "ring_time":"1679476851",
         "start_time":"1679476851",
         "status_code":0,
         "trunk_name":"AudioCodes"
      },
      {
         "answered_time":"1679476857",
         "call_type":"normal_call",
         "callee":"8000",
         "caller":"1888722",
         "destination":"sip:8000@192.168.0.16:8916",
         "end_reason":"referred",
         "ended_time":"1679476862",
         "id":"690833127458734080",
         "outbound_caller_id":"",
         "ring_time":"1679476857",
         "start_time":"1679476857",
         "status_code":0,
         "trunk_name":"AudioCodes"
      },
      {
         "answered_time":"0",
         "call_type":"normal_call",
         "callee":"102",
         "caller":"1888722",
         "destination":"sip:102@192.168.0.16:9070",
         "end_reason":"None",
         "ended_time":"1679476862",
         "id":"690833127458734080",
         "outbound_caller_id":"",
         "ring_time":"1679476858",
         "start_time":"1679476858",
         "status_code":0,
         "trunk_name":"AudioCodes"
      },
      {
         "answered_time":"1679476862",
         "call_type":"normal_call",
         "callee":"103",
         "caller":"1888722",
         "destination":"sip:103@192.168.0.16:9530",
         "end_reason":"None",
         "ended_time":"1679476870",
         "id":"690833127458734080",
         "outbound_caller_id":"",
         "ring_time":"1679476858",
         "start_time":"1679476858",
         "status_code":0,
         "trunk_name":"AudioCodes"
      }
   ]
}
```

***

#### CDR Fields Description

The CDR information is provided in **JSON format** and includes the following fields:

* **answered\_time**\
  The timestamp (Unix format) when the call was answered.\
  In this example, it represents when the PBX IVR answered the call.\
  A value of `0` indicates the call was not answered.
* **call\_id**\
  The Call-ID of the SIP INVITE received by PortSIP PBX from the trunk.
* **callee**\
  The first destination when the call arrived at the PBX (for example, IVR 5000).
* **callee\_domain**\
  The SIP domain (tenant SIP domain) of the callee.
* **caller**\
  The caller number received from the trunk.
* **caller\_display\_name**\
  The display name from the SIP `From` header.
* **caller\_domain**\
  The host part of the SIP `From` header.
* **did\_cid**\
  The DID number dialed from the trunk (user part of the SIP `To` header).
* **direction**\
  Indicates call direction:
  * `in`: inbound from trunk
  * `out`: outbound to trunk
  * `in2out`: inbound call forwarded to trunk
* **end\_reason**\
  Indicates why the call ended. Possible values include:\
  `caller_disconnect`, `callee_disconnect`, `replaced`, `referred`, `redirected`,\
  `blind_transfer_complete`, `attended_transfer_complete`, `cancelled`
* **ended\_time**\
  The time when the call ended (Unix timestamp).
* **event\_type**\
  Indicates that this event is a CDR.
* **outbound\_caller\_id**\
  The outbound caller ID used for outbound trunk calls.
* **ring\_time**\
  Ring start time for the call target.
* **session\_id**\
  Session ID also present in the SIP `X-Session-ID` header; can be used to query call recordings.
* **start\_time**\
  The timestamp when the call arrived at PortSIP PBX.
* **status\_code**\
  SIP status code if the call failed (e.g., `404`, `408`).\
  A value of `0` indicates no error.
* **tenant\_id**\
  The ID of the tenant that handled the call.
* **tenant\_name**\
  The name of the tenant that handled the call.
* **trunk\_name**\
  The name of the trunk used for the call.
* **user\_data**\
  Custom data passed when the call is initiated via REST API.

***

#### Call Targets

A call may include **multiple call targets**, representing each routing step or ringing endpoint.

**Example Scenarios:**

* An extension registered on both an IP phone and an app creates multiple targets.
* A call rings extension 101, goes unanswered, and is forwarded to extension 102.
* A call passes through IVR, queue, and multiple agents.

In this example:

* The first target is **IVR 5000**.
* The caller presses DTMF `1`.
* The call is forwarded to **Queue 8000**.
* Agents **102** and **103** ring while the caller waits.

***

#### Example: IVR Target (5000)

```json
{
  "answered_time":"1679476851",
  "call_type":"normal_call",
  "callee":"5000",
  "caller":"1888722",
  "destination":"sip:5000@192.168.0.16:8922",
  "end_reason":"referred",
  "ended_time":"1679476857",
  "id":"690833127458734080",
  "outbound_caller_id":"",
  "ring_time":"1679476851",
  "start_time":"1679476851",
  "status_code":0,
  "trunk_name":"AudioCodes"
}
```

***

### Push CDR to WebSocket Subscriber

You can also collect CDRs by subscribing to **CDR events via WebSocket**.

A WebSocket subscription allows an external service or application to receive real-time events directly from PortSIP PBX without polling.

For more details, please refer to the [WSI: Pub/Sub](../../../development-portsip/going-real-time-with-portsip-pbx-pub-sub.md) documentation.



