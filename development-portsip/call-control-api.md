# Call Control APIs

This section describes the Call Control API portion of the PortSIP PBX [REST API ](rest-apis/)and provides guidance for developers building 3rd applications. You can use this API to write agent applications that provide a variety of call-related features, from agent state management and call control to supervisor monitoring and call recording.

## Authentication

Before calling the REST API to make the call, and control the call, we need to authenticate with the PortSIP PBX to obtain the `access_token`. Please refer to the  [REST API](rest-apis/).

## Initiate a call directly

<mark style="color:green;">`GET`</mark> `/sessions/directly`

Use the GET method to initiate a call directly. This API does not require an `access_token`. Some hardware is not programmable and only allows pasting the URL to initiate the call. This API is designed for that purpose.

**Example**

<pre class="language-url" data-overflow="wrap"><code class="lang-url"><strong>/api/sessions/directly?extension_number=1001&#x26;password=A1s2d3f4&#x26;caller=1001&#x26;callee=1002&#x26;domain=test.io&#x26;send_sdp=true
</strong></code></pre>

**URL Parameters**

| Name                  | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| extension\_number     | An extension number                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| password              | The extension password is used by the PBX for authentication instead of the `access_token`. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| caller                | The caller number, it's can be either an extension number or a PSTN number. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| callee                | The callee number, it's can be either an extension number or a PSTN number. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| domain                | The sip domain of the tenant. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| send\_sdp             | Include the SDP in the INVITE message to initiate the call, true or false. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| caller\_display\_name | Specify the display name for caller. (optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| trunk                 | This is a trunk name used to indicate that this call should be routed through the specified trunk. If the specified trunk is not available, the call will fail.(optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| rewrite\_from         | Specify when to send this call to the trunk. Use `rewrite_from` to rewrite the user of the From header in the INVITE message. Usually use this to set the outbound caller ID for the outbound call.(optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| rewrite\_pai          | Specify when to send this call to the trunk. Use `rewrite_pai` to rewrite the user of the `P-Asserted-Identity` header in the INVITE message. (optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| rewrite\_rpi          | Specify when to send this call to the trunk. Use `rewrite_rpi` to rewrite the user of the `Remote-Party-ID` header in the INVITE message. (optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| user\_data            | Use this key to pass the additional information to the call. This information will be stored in the CDR, allowing you to easily track and analyze the call.(optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| additional\_header    | <p></p><p>Specifies whether to add an additional SIP header (RFC 5373) to the INVITE SIP message. The possible values are:</p><ul><li><strong>ANSWER_MODE</strong>: Adds the Answer-Mode header to the INVITE SIP message.</li><li><strong>CALL_INFO</strong>: Adds the Call-Info header with <code>answer-after=0</code> to the INVITE SIP message.</li><li><strong>ALERT_INFO_AUTO_ANSWER_DELAY0</strong>: Adds the Alert-Info header with <code>info=alert-autoanswer;delay=0</code> to the INVITE SIP message.</li><li><strong>ALERT_INFO_AUTO_ANSWER</strong>: Adds the Alert-Info header with <code>info=Auto Answer</code> to the INVITE SIP message.</li><li><strong>ALERT_INFO_INTERCOM</strong>: Adds the Alert-Info header with <code>info=intercom</code> to the INVITE SIP message.</li></ul><p>(optional)</p> |
| header\_direction     | <p></p><p>Specifies which party of the call should receive the additional_header. If the additional_header is not specified, this parameter will be ignored. The possible values are:</p><ul><li><strong>CALLER</strong>: Adds the specified header in additional_header to the INVITE SIP message sent to the caller.</li><li><strong>CALLEE</strong>: Adds the specified header in additional_header to the INVITE SIP message sent to the callee.</li><li><strong>ALL</strong>: Adds the specified header in additional_header to the INVITE SIP messages sent to both the caller and callee.</li></ul><p>(optional)</p>                                                                                                                                                                                                 |
|                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "id": "884757570370146304"
}
```
{% endtab %}
{% endtabs %}

## Initiate a call

<mark style="color:green;">`POST`</mark> `/sessions`

Use this REST API to initiate a call, the `access_token` is required for this API.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |
|               |                    |

**Body**

| Name                  | Type         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| caller                | string       | The caller number, it's can be either an extension number or a PSTN number. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| callee                | string       | The callee number, it's can be either an extension number or a PSTN number. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| send\_sdp             | boolean      | Include the SDP in the INVITE message to initiate the call, true or false. (required)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| caller\_display\_name |              | Specify the display name for caller. (optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| trunk                 | string       | This is a trunk name used to indicate that this call should be routed through the specified trunk. If the specified trunk is not available, the call will fail.(optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| rewrite\_from         | string       | Age of the user                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| rewrite\_pai          | string       | Specify when to send this call to the trunk. Use `rewrite_pai` to rewrite the user of the `P-Asserted-Identity` header in the INVITE message. (optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| rewrite\_rpi          |              | Specify when to send this call to the trunk. Use `rewrite_rpi` to rewrite the user of the `Remote-Party-ID` header in the INVITE message. (optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| user\_data            | string       | Use this key to pass the additional information to the call. This information will be stored in the CDR, allowing you to easily track and analyze the call.(optional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| additional\_header    | string enum  | <p></p><p>Specifies whether to add an additional SIP header (RFC 5373) to the INVITE SIP message. The possible values are:</p><ul><li><strong>ANSWER_MODE</strong>: Adds the Answer-Mode header to the INVITE SIP message.</li><li><strong>CALL_INFO</strong>: Adds the Call-Info header with <code>answer-after=0</code> to the INVITE SIP message.</li><li><strong>ALERT_INFO_AUTO_ANSWER_DELAY0</strong>: Adds the Alert-Info header with <code>info=alert-autoanswer;delay=0</code> to the INVITE SIP message.</li><li><strong>ALERT_INFO_AUTO_ANSWER</strong>: Adds the Alert-Info header with <code>info=Auto Answer</code> to the INVITE SIP message.</li><li><strong>ALERT_INFO_INTERCOM</strong>: Adds the Alert-Info header with <code>info=intercom</code> to the INVITE SIP message.</li></ul><p>(optional)</p> |
| additional\_header    | string enum. | <p></p><p>Specifies which party of the call should receive the additional_header. If the additional_header is not specified, this parameter will be ignored. The possible values are:</p><ul><li><strong>CALLER</strong>: Adds the specified header in additional_header to the INVITE SIP message sent to the caller.</li><li><strong>CALLEE</strong>: Adds the specified header in additional_header to the INVITE SIP message sent to the callee.</li><li><strong>ALL</strong>: Adds the specified header in additional_header to the INVITE SIP messages sent to both the caller and callee.</li></ul><p>(optional)</p>                                                                                                                                                                                                 |

**Example of the body**

```json
{
"caller" : "1001",
"callee" : "1002",
 "send_sdp" : true
}
```

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
    "id": "884757570370146304"
}
```
{% endtab %}
{% endtabs %}

## Hang up a call

<mark style="color:green;">`POST`</mark> `/sessions/{id}/destroy`

Use this API to hang up a call by specifying the session ID. Pass the call session ID to the URL parameter ID.

**Example**

```url
/api/sessions/884761305393664000/destroy
```

**Headers**

<table><thead><tr><th width="525">Name</th><th>Value</th></tr></thead><tbody><tr><td>Content-Type</td><td><code>application/json</code></td></tr><tr><td>Authorization</td><td><code>Bearer &#x3C;token></code></td></tr></tbody></table>

**Response**

{% tabs %}
{% tab title="200" %}
```json
```
{% endtab %}
{% endtabs %}

## Hold a call

<mark style="color:green;">`POST`</mark> `/sessions/{id}/hold`

Use this API to hold a call by specifying the session ID. Pass the call session ID to the URL parameter ID.

**Example**

```url
/api/sessions/884761305393664000/hold
```

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Body**

<table><thead><tr><th>Name</th><th width="305">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>extension_number</code></td><td>string</td><td>The <code>extension_number</code> can be either a PortSIP PBX extension number or a PSTN number. Since a call can have multiple call legs, if this parameter is specified, the PBX will match the call leg by <code>extension_number</code> and session ID, then perform the hold on the matched call leg. If the ID parameter in the URL is set to <code>1</code>, the hold will be performed on the last call associated with this <code>extension_number</code>. (required)</td></tr><tr><td></td><td></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

**Response**

{% tabs %}
{% tab title="200" %}
```json
```
{% endtab %}
{% endtabs %}

## Unhold a call

<mark style="color:green;">`POST`</mark> `/sessions/{id}/unhold`

Use this API to unhold a call by specifying the session ID. Pass the call session ID to the URL parameter ID.

**Example**

```url
/api/sessions/884761305393664000/unhold
```

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Body**

<table><thead><tr><th>Name</th><th width="305">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>extension_number</code></td><td>string</td><td>The <code>extension_number</code> can be either a PortSIP PBX extension number or a PSTN number. Since a call can have multiple call legs, if this parameter is specified, the PBX will match the call leg by <code>extension_number</code> and session ID, then perform the unhold on the matched call leg. If the ID parameter in the URL is set to <code>1</code>, the unhold will be performed on the last call associated with this <code>extension_number</code>. (required)</td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

**Response**

{% tabs %}
{% tab title="200" %}
```json
```
{% endtab %}
{% endtabs %}

## Blind transfer a call

<mark style="color:green;">`POST`</mark> `/sessions/{id}/refer`

Use this API to blind transfer a call by specifying the session ID. Pass the call session ID to the URL parameter ID. In the URL parameter, you can specify the `{id}` as 1 means do not need to match the call.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Body**

| Name               | Type   | Description                                                                                                                                                                                                                                                                                               |
| ------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `extension_number` | string | To transfer a call to a specific `refer_to` number, you need to specify the `extension_number,` it's can be either a PortSIP PBX extension number or a PSTN number. If the `ID` parameter in the URL is set to `1`, the last call associated with that `extension_number` will be transferred. (required) |
| `refer_to`         | string | The target number to which the call will be transferred.  (required)                                                                                                                                                                                                                                      |
|                    |        |                                                                                                                                                                                                                                                                                                           |

**Example 1:**&#x20;

1001 has established a call. Use the following body payload to blind transfer extension 1001 to the number 0033187691.

1. Pass  `1`  for the URL parameter `{id}`
2. Use the below POST body

**Note**: If the `sessionId` is specified as `1` in the URL parameter `{id}` and extension **1001** has multiple established calls, the PBX will automatically select the most recently established call and perform the transfer.

```json
{
"extension_number" : "1001",
"refer_to" : "0033187691"
}
```

**Example 2:**

Extension **1001** has multiple active calls. To blind transfer the call with session ID **111222** from extension 1001 to the number **0033187691**, use the following POST request:

* Set the URL parameter `{id}` to **111222**
* Use the following POST body:

```json
{
"extension_number" : "1001",
"refer_to" : "0033187691"
}
```

**Response**

{% tabs %}
{% tab title="200" %}
```json
```
{% endtab %}

{% tab title="400" %}
```json
```
{% endtab %}
{% endtabs %}

## Attended transfer a call

<mark style="color:green;">`POST`</mark> `/sessions/{id}/attended_refer`

Use this API to attend transfer a call by specifying the session ID. Pass the `1` as the URL parameter ID.&#x20;

Consider this scenario: Extension 1001 establishes calls with both 1002 and 1003. Now, use this API to perform an attended transfer, which will connect the call between 1002 and 1003, and extension 1001 will be disconnected from the calls.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `Bearer <token>`   |

**Body**

| Name               | Type   | Description                                                                                                                                                                                                                                            |
| ------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `extension_number` | string | To do attended transfer, you need to specify the `extension_number`for select the transffer, it's can be either a PortSIP PBX extension number or a PSTN number (required)                                                                             |
| session\_1         | string | The session ID of the call which will be transffered. The PBX will match the call leg by both `extension_number` and this specified session ID. This parameter is optional, or can be set as "0".                                                      |
| refer\_to\_1       | string | If the `session_1` is specified as 0, then the PBX will use the `extension_number` and this `refer_to_1` to match the call leg. This parameter can be set as a empty string. If the `session_1`  is "0" and `refer_to_1` is empty, this API will fail. |
| session\_2         | string | The session ID of the call which will be transffered. The PBX will match the call leg by both `extension_number` and this specified session ID. This parameter is optional, or can be set as "0".                                                      |
| refer\_to\_2       | string | If the `session_2` is specified as 0, then the PBX will use the `extension_number` and this `refer_to_2` to match the call leg. This parameter can be set as a empty string. If the `session_2`  is "0" and `refer_to_2` is empty, this API will fail. |
|                    |        |                                                                                                                                                                                                                                                        |
|                    |        |                                                                                                                                                                                                                                                        |

**Example 1:**

Extension 1001 establishes a call with 1002, the session ID is **111111**,  and establishes a call with 1003, the session ID is **222222**. Now, use this API to perform an attended transfer, which will connect the call between 1002 and 1003, and extension 1001 will be disconnected from the call.

* Pass  `1`  for the URL parameter `{id}`
* Use the following POST body:

```json
{
"extension_number" : "1001",
"refer_to_1" : "1002",
"session_1" : "111111",
"refer_to_2" : "1003",
"session_2" : "222222"
}
```

**Example 2:**

Extension 1001 establishes calls with both 1002 and 1003. Now, use this API to perform an attended transfer, which will connect the call between 1002 and 1003, and extension 1001 will be disconnected from the call.

* Pass  `1`  for the URL parameter `{id}`
* Use the following POST body:

```json
{
"extension_number" : "1001",
"refer_to_1" : "1002",
"refer_to_2" : "1003"
}
```

**Response**

{% tabs %}
{% tab title="200" %}
```json
```
{% endtab %}

{% tab title="400" %}
```json
```
{% endtab %}
{% endtabs %}

