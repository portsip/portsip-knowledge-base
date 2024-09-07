# Call Control APIs

This section describes the Call Control API portion of the PortSIP PBX [REST API ](rest-apis.md)and provides guidance for developers building 3rd applications. You can use this API to write agent applications that provide a variety of call-related features, from agent state management and call control to supervisor monitoring and call recording.

## Authentication

Before calling the REST API to make the call, and control the call, we need to authenticate with the PortSIP PBX to obtain the `access_token`. Please refer to the  [REST API](rest-apis.md).

## Initiate a call directly

<mark style="color:green;">`GET`</mark> `/sessions/directly`

Use the GET method to initiate a call directly. This API does not require an `access_token`. Some hardware is not programmable and only allows pasting the URL to initiate the call. This API is designed for that purpose.

**Example**

<pre class="language-url"><code class="lang-url"><strong>/api/sessions/directly?extension_number=1001&#x26;password=A1s2d3f4&#x26;caller=1001&#x26;callee=1002&#x26;domain=test.io&#x26;send_sdp=true
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

<table><thead><tr><th>Name</th><th width="305">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>extension_number</code></td><td>string</td><td>If the <code>extension_number</code> is specified, the PBX will also check if it is an participant of the call. If not, the hold will fail. If the <code>extension_number</code> is not specified, the PBX will only check the call with the session ID parameter. If the session ID does not match any existing call, the hold will fail.</td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

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

<table><thead><tr><th>Name</th><th width="305">Type</th><th>Description</th></tr></thead><tbody><tr><td><code>extension_number</code></td><td>string</td><td>If the <code>extension_number</code> is specified, the PBX will also check if it is an participant of the call. If not, the unhold will fail. If the <code>extension_number</code> is not specified, the PBX will only check the call with the session ID parameter. If the session ID does not match any existing call, the unhold will fail.</td></tr><tr><td></td><td></td><td></td></tr></tbody></table>

**Response**

{% tabs %}
{% tab title="200" %}
```json
```
{% endtab %}
{% endtabs %}







