# Event Reference

PortSIP sends the events to the webhook in JSON object encoded using UTF-8, the events are defined as the below.

## CDR Events

### Call Target Definition

The definition of the **call target**: In PortSIP PBX,  a **call target** refers to an endpoint. For example, if user 102 registers with the PBX from an IP phone and an app simultaneously, when user 101 calls 102, the PBX will send INVITE messages to both the IP phone and the app. In this scenario, the CDR will have two call targets: one for the IP phone and another for the app.&#x20;

When the PBX sends an INVITE to a target, and the call target is ringing, answered, hung up, or the call to the target fails, the PortSIP PBX will send this call target event to the webhook.

### Call Start Event

This event is sent to the webhook when a call is starting, it contains the below fields:

<table><thead><tr><th width="279">Key name</th><th>Value</th></tr></thead><tbody><tr><td>event_type</td><td>call_start</td></tr><tr><td>time</td><td>The Unix timestamp in string format represents the call start time.</td></tr><tr><td>session_id</td><td>The session ID of the call.</td></tr><tr><td>cdr_id</td><td>The ID of this CDR.</td></tr><tr><td>direction</td><td><p></p><p>The call direction can have the following values:</p><ul><li><strong>ext</strong>: Indicates a call between extensions.</li><li><strong>in</strong>: Indicates an inbound call from a trunk.</li><li><strong>in2out</strong>: Indicates an inbound call routed to the trunk.</li><li><strong>out</strong>: Indicates an outbound call to the trunk.</li></ul></td></tr><tr><td>user_data</td><td>If the call is initiated from the REST API, additional information may be passed to this call as <strong>user_data</strong>.</td></tr><tr><td>call_id</td><td>The Call-ID of the INVITE SIP message.</td></tr><tr><td>caller_display_name</td><td>The display name of the caller.</td></tr><tr><td>caller</td><td>The caller number.</td></tr><tr><td>callee_display_name</td><td>The display name of the callee.</td></tr><tr><td>callee</td><td>The callee number.</td></tr><tr><td>tenant_id</td><td>The numeric tenant ID in string format</td></tr><tr><td>did</td><td>If the call is an inbound call, this key stores the DID.</td></tr><tr><td>outbound_caller_id</td><td>If the call is an outbound call, this key stores the outbound caller ID.</td></tr></tbody></table>

### Call Info Update Event

This event is sent to the webhook when call information is updated, it contains the below fields:

<table><thead><tr><th width="275">Key name</th><th>Value</th></tr></thead><tbody><tr><td>event_type</td><td>call_update_info</td></tr><tr><td>time</td><td>The Unix timestamp in string format represents the call start time.</td></tr><tr><td>session_id</td><td>The session ID of the call.</td></tr><tr><td>cdr_id</td><td>The ID of this CDR.</td></tr><tr><td>direction</td><td><p>The call direction can have the following values:</p><ul><li><strong>ext</strong>: Indicates a call between extensions.</li><li><strong>in</strong>: Indicates an inbound call from a trunk.</li><li><strong>in2out</strong>: Indicates an inbound call routed to the trunk.</li><li><strong>out</strong>: Indicates an outbound call to the trunk.</li></ul></td></tr><tr><td>caller_display_name</td><td>The display name of the caller.</td></tr><tr><td>caller</td><td>The caller number.</td></tr><tr><td>callee_display_name</td><td>The display name of the callee.</td></tr><tr><td>callee</td><td>The callee number.</td></tr><tr><td>tenant_id</td><td>The numeric tenant ID in string format</td></tr><tr><td>did</td><td>If the call is an inbound call, this key stores the DID.</td></tr><tr><td>outbound_caller_id</td><td>If the call is an outbound call, this key stores the outbound caller ID.</td></tr></tbody></table>

### Call Target Event

As per the [Call Target Definition](event-reference.md#call-target-definition), the call target event includes the below information.

<table><thead><tr><th width="270"></th><th></th></tr></thead><tbody><tr><td>event_type</td><td><p></p><p>For call target events, this event type can have the following values:</p><ul><li><strong>cdr_target_add</strong>: Sends the INVITE to the call target.</li><li><strong>cdr_target_ringing</strong>: Indicates that a call target is ringing.</li><li><strong>cdr_target_answer</strong>: Indicates that a call target has answered the call.</li><li><strong>cdr_target_noanswer</strong>: Indicates that a ringing call target did not answer the call within the allotted time.</li><li><strong>cdr_target_fail</strong>: Indicates that the call to a call target failed (e.g., network not reachable, rejected by call target).</li><li><strong>cdr_target_end</strong>: Indicates that an established call has now ended.</li></ul></td></tr><tr><td>time</td><td>The Unix timestamp in string format represents the call start time.</td></tr><tr><td>session_id</td><td>The session ID of the call.</td></tr><tr><td>cdr_id</td><td>The ID of this CDR.</td></tr><tr><td>tenant_id</td><td>The numeric tenant ID in string format</td></tr><tr><td>direction</td><td><p>The call direction can have the following values:</p><ul><li><strong>ext</strong>: Indicates a call between extensions.</li><li><strong>in</strong>: Indicates an inbound call from a trunk.</li><li><strong>in2out</strong>: Indicates an inbound call routed to the trunk.</li><li><strong>out</strong>: Indicates an outbound call to the trunk.</li></ul></td></tr><tr><td>destiantion</td><td>The call target address, for example: <code>sip:102@192.168.0.18:5066</code></td></tr><tr><td>caller</td><td>The caller number.</td></tr><tr><td>did</td><td>If the call is an inbound call, this key stores the DID.</td></tr><tr><td>outbound_caller_id</td><td>If the call is an outbound call, this key stores the outbound caller ID.</td></tr></tbody></table>

### Call End Event

When a call is completed (either hung up or failed), the PortSIP PBX sends this event to the webhook. This event includes all information about the call. Below is an example of a CDR.

&#x20;The call flow is as follows: caller 1888722 dials 18800606 > trunk > PBX IVR (5000) > Queue (8000) > Agent (102, 103).

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
   "did":"18800606",
   "outbound_caller_id":"",
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
   "talk_duration":"16",
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
         "talk_duration":"16",
         "trunk_name":"AudioCodes"
      }
   ]
}
```

<table><thead><tr><th width="270"></th><th></th></tr></thead><tbody><tr><td>event_type</td><td>call_cdr</td></tr><tr><td>start_time</td><td>The Unix timestamp in string format represents the call start time.</td></tr><tr><td>ring_time</td><td>The Unix timestamp in string format represents the first call target ring.</td></tr><tr><td>answered_time</td><td>The Unix timestamp in string format represents the first call target answered.</td></tr><tr><td>ended_time</td><td>The Unix timestamp in string format represents the call was ended.</td></tr><tr><td>session_id</td><td>The session ID of the call.</td></tr><tr><td>cdr_id</td><td>The ID of this CDR.</td></tr><tr><td>call_id</td><td>The Call-ID of the INVITE SIP message.</td></tr><tr><td>callee</td><td>The callee number.</td></tr><tr><td>callee_domain</td><td>The domain of the callee.</td></tr><tr><td>caller</td><td>The caller number.</td></tr><tr><td>caller_display_name</td><td>The display name of the caller.</td></tr><tr><td>caller_domain</td><td>The domain of the caller.</td></tr><tr><td>call_status</td><td><p>The call final status can have the following values:</p><ul><li><strong>noanswer</strong>: The call was not answered.</li><li><strong>fail</strong>: The call failed.</li><li><strong>answered</strong>: The call was answered.</li></ul></td></tr><tr><td>trunk_name</td><td>If the call is an inbound call or an outbound call through the trunk, the trunk name is stored in this key.</td></tr><tr><td>tenant_id</td><td>The numeric tenant ID in string format</td></tr><tr><td>tenant_name</td><td>The tenant name.</td></tr><tr><td>status_code</td><td>The final status code of the call is a SIP standard-defined status code.</td></tr><tr><td>direction</td><td><p>The call direction can have the following values:</p><ul><li><strong>ext</strong>: Indicates a call between extensions.</li><li><strong>in</strong>: Indicates an inbound call from a trunk.</li><li><strong>in2out</strong>: Indicates an inbound call routed to the trunk.</li><li><strong>out</strong>: Indicates an outbound call to the trunk.</li></ul></td></tr><tr><td>end_reason</td><td><p></p><p>The end reason of the call can have the following values:</p><ul><li><strong>None</strong>: Unknown reason.</li><li><strong>caller_disconnected</strong>: The caller hung up.</li><li><strong>callee_disconnected</strong>: The callee hung up.</li><li><strong>replaced</strong>: Ended by attended transfer or call pickup.</li><li><strong>referred</strong>: The call ended due to a blind transfer.</li><li><strong>redirected</strong>: The call ended because of a 302 Redirect.</li><li><strong>blind_transfer_complete</strong>: The call was hung up because the blind transfer was completed.</li><li><strong>attended_transfer_complete</strong>: The call was hung up before the attended transfer was completed.</li><li><strong>cancelled</strong>: The call was cancelled by the caller.</li></ul></td></tr><tr><td>did</td><td>If the call is an inbound call, this key stores the DID.</td></tr><tr><td>outbound_caller_id</td><td>If the call is an outbound call, this key stores the outbound caller ID.</td></tr><tr><td>ring_duration</td><td>The ring time of the call.</td></tr><tr><td>talk_duration</td><td>The talk time of the call.</td></tr></tbody></table>

Since a call may be forwarded multiple times to different [call targets](event-reference.md#call-target-definition) during its duration, PortSIP PBX includes all call targets in the CDR when the call is completed. We can simply parse the **call\_targets** JSON array object to access the call target information.

<table><thead><tr><th width="270"></th><th></th></tr></thead><tbody><tr><td>call_type</td><td><p></p><p>Indicates the call type. The values can be:</p><ul><li><strong>callback_call</strong>: An automatic callback call.</li><li><strong>none</strong>: Unknown call type.</li><li><strong>normal_call</strong>: A normal call.</li><li><strong>queue_call</strong>: The call is of the queue.</li><li><strong>queue_callback_call</strong>: The call is a callback to the caller from an agent.</li><li><strong>ring_group_call</strong>: The call of the group.</li></ul></td></tr><tr><td>start_time</td><td>The Unix timestamp in string format represents the call start time.</td></tr><tr><td>ring_time</td><td>The Unix timestamp in string format represents the first call target ring.</td></tr><tr><td>answered_time</td><td>The Unix timestamp in string format represents the first call target answered.</td></tr><tr><td>ended_time</td><td>The Unix timestamp in string format represents the call was ended.</td></tr><tr><td>cdr_id</td><td>The ID of CDR for this call target.</td></tr><tr><td>callee</td><td>The callee number.</td></tr><tr><td>caller</td><td>The caller number.</td></tr><tr><td>destination</td><td>The address of the call target, for example: <code>sip:102@192.168.0.11.</code></td></tr><tr><td>status</td><td><p>The call final status can have the following values:</p><ul><li><strong>noanswer</strong>: The call was not answered.</li><li><strong>answered</strong>: The call was answered.</li></ul></td></tr><tr><td>trunk_name</td><td>If the call is an inbound call or an outbound call through the trunk, the trunk name is stored in this key.</td></tr><tr><td>cost</td><td>If the billing is enabled, store the call cost in this key.</td></tr><tr><td>status_code</td><td>The final status code of the call is a SIP standard-defined status code.</td></tr><tr><td>direction</td><td><p>The call direction can have the following values:</p><ul><li><strong>ext</strong>: Indicates a call between extensions.</li><li><strong>in</strong>: Indicates an inbound call from a trunk.</li><li><strong>in2out</strong>: Indicates an inbound call routed to the trunk.</li><li><strong>out</strong>: Indicates an outbound call to the trunk.</li></ul></td></tr><tr><td>end_reason</td><td><p></p><p>The end reason of the call can have the following values:</p><ul><li><strong>None</strong>: Unknown reason.</li><li><strong>caller_disconnected</strong>: The caller hung up.</li><li><strong>callee_disconnected</strong>: The callee hung up.</li><li><strong>replaced</strong>: Ended by attended transfer or call pickup.</li><li><strong>referred</strong>: The call ended due to a blind transfer.</li><li><strong>redirected</strong>: The call ended because of a 302 Redirect.</li><li><strong>blind_transfer_complete</strong>: The call was hung up because the blind transfer was completed.</li><li><strong>attended_transfer_complete</strong>: The call was hung up before the attended transfer was completed.</li><li><strong>cancelled</strong>: The call was cancelled by the caller.</li></ul></td></tr><tr><td>did</td><td>If the call is an inbound call, this key stores the DID.</td></tr><tr><td>outbound_caller_id</td><td>If the call is an outbound call, this key stores the outbound caller ID.</td></tr><tr><td>ring_duration</td><td>The ring time of the call.</td></tr><tr><td>talk_duration</td><td>The talk time of the call.</td></tr></tbody></table>



