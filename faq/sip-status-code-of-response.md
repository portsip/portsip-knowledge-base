# SIP Status Code of Response

When you troubleshoot [PortSIP PBX](https://www.portsip.com/portsip-pbx) issues, the specific cause of a call/request failure can be identified through the SIP response codes.

## What are SIP response codes?

SIP Codes are pre-defined three-digit codes that convey critical status information when making a call.&#x20;

The SIP response codes are defined in [RFC 3261](https://www.rfc-editor.org/rfc/rfc3261#page-182).

When making a phone call or establishing a communication session over SIP, a series of exchanges occur between the user agent sending the call request (called User Agent Clients or UAC) and the recipient’s server (called User Agent Servers or UAS). These exchanges are governed by a set of codes known as SIP response codes.

SIP codes are three-digit numbers that provide information about the status and progress of a SIP session. They serve as a common language that allows the various components of a communication system to communicate effectively and handle calls appropriately.

## What do SIP codes look like?

SIP response codes are grouped into different categories, each with its own significance. They’re grouped by the first integer of the three-digit code.&#x20;

### 1. Informational responses (1xx):

These codes indicate that the recipient server has received the request and is processing it. Examples include “100 – Trying” and “180 – Ringing.”

### 2. Success responses (2xx):

Success codes indicate that the request was successfully processed, and the desired action has been completed. Examples include “200 – OK” and “202 – Accepted.”

### 3. Redirection responses (3xx):

Redirection codes indicate that further action is needed to complete the request. The user agent may need to take additional steps to reach the desired endpoint. Examples include “301 – Moved Permanently” and “302 – Moved Temporarily.”

### 4. Client Error responses (4xx):

Client error codes indicate that there was an issue with the request, usually due to a mistake made by the user agent or client. Examples include “400 – Bad Request” and “404 – Not Found.”

### 5. Server Error responses (5xx):

Server error codes indicate that there was an issue on the recipient server’s end. These codes denote that the server encountered an unexpected condition or error while processing the request. Examples include “500 – Server Internal Error” and “503 – Service Unavailable.”

### 6. Global Failure responses

The 6xx response codes indicate that the response will fail, irrespective of the location where it’s tried. Examples include “604 – Does Not Exist Anywhere” and “607 – Unwanted”.

## Why do SIP codes matter?

SIP response codes play a crucial role in troubleshooting and identifying the status of a communication session. By understanding these codes, businesses can proactively address issues, ensure smoother call operations, and provide a better experience for their customers.

Moreover, the insights gained from analyzing SIP response codes can help service providers optimize their systems, identify performance bottlenecks, and improve call routing strategies. These codes provide invaluable diagnostic information that can be used to enhance the overall quality and reliability of communication services.

## A list of all SIP codes

Need to make a quick check of what a 3-digit SIP Response Code means? We’ve got you. Refer to the table below:

#### 1xx = Informational SIP responses

| **SIP Response Code** | **SIP Equivalent**          | **Description**                                                                                             |
| --------------------- | --------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **100**               | **Trying**                  | Extended search is being performed so a forking proxy must send a 100 Trying response.                      |
| **180**               | **Ringing**                 | The Destination User Agent has received the INVITE message and is alerting the user of call.                |
| **181**               | **Call is being forwarded** | Optional, send by Server to indicate a call is being forwarded.                                             |
| **182**               | **Queued**                  | Destination was temporarily unavailable, the server has queued the call until the destination is available. |
| **183**               | **Session progress**        | This response may be used to send extra information for a call which is still being set up.                 |
| **199**               | **Early dialog terminated** | Send by the User Agent Server to indicate that an early dialogue has been terminated.                       |

#### 2xx = Success responses

| **SIP Response Code** | **SIP Equivalent**  | **Description**                                                                         |
| --------------------- | ------------------- | --------------------------------------------------------------------------------------- |
| **200**               | **OK**              | Shows that the request was successful.                                                  |
| **202**               | **Accepted**        | Indicates that the request has been accepted for processing, mainly used for referrals. |
| **204**               | **No notification** | Indicates that the request was successful but no response will be received.             |

#### 3xx = Redirection responses

| **SIP Response Code** | **SIP Equivalent**      | **Description**                                                                              |
| --------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| **300**               | **Multiple choices**    | The address resolved to one of several options for the user or client to choose between.     |
| **301**               | **Moved permanently**   | The original Request URI is no longer valid, the new address is given in the Contact header. |
| **302**               | **Moved temporarily**   | The client should try at the address in the Contact field.                                   |
| **305**               | **Use proxy**           | The Contact field details a proxy that must be used to access the requested destination.     |
| **380**               | **Alternative service** | The call failed, but alternatives are detailed in the message body.                          |

#### 4xx = Request failures

Some of the most common 4xx SIP codes are below. See the complete list in the [appendix](https://docs.google.com/document/d/1qrYBmmDCVljVLvYTjr5YnCTm7TYkTEkoqnpbNuJKOtw/edit#heading=h.wv3cc3g6j8px).

| **SIP Response Code** | **SIP Equivalent**                | **Description**                                                                                 |
| --------------------- | --------------------------------- | ----------------------------------------------------------------------------------------------- |
| **400**               | **Bad Request**                   | The request could not be understood due to malformed syntax.                                    |
| **401**               | **Unauthorized**                  | The request requires user authentication. This response is issued by a UAS or a registrar.      |
| **404**               | **Not found**                     | The server has definitive information that the user does not exist at the (User not found)      |
| **407**               | **Proxy authentication required** | The request requires user authentication.                                                       |
| **408**               | **Request timeout**               | Couldn’t find the user in time.                                                                 |
| **409**               | **Conflict**                      | User already registered (deprecated).                                                           |
| **411**               | **Length required**               | The server will not accept the request without a valid content length (deprecated).             |
| **412**               | **Conditional request failed**    | The given precondition has not been met.                                                        |
| **415**               | **Unsupported media type**        | Request body is in a non-supported format.                                                      |
| **424**               | **Bad location information**      | The request’s location content was malformed or otherwise unsatisfactory.                       |
| **436**               | **Identity info**                 | The request has an Identity-Info header and the   URI scheme contained cannot be de-referenced. |
| **470**               | **Consent needed**                | The source of the request did not have the permission of the recipient to make such a request.  |
| **480**               | **Temporarily unavailable**       | Callee currently unavailable.                                                                   |
| **489**               | **Bad event**                     | The server did not understand an event package specified in an Event header field.              |

#### 5xx = Server errors

| **SIP Response Code** | **SIP Equivalent**                          | **Description**                                                                                           |
| --------------------- | ------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **500**               | **Server internal error**                   | The server could not fulfill the request due to some unexpected condition.                                |
| **501**               | **Not implemented**                         | The SIP request method is not implemented here.                                                           |
| **502**               | **Bad gateway**                             | The server, received an invalid response from a downstream server while trying to fulfill a request.      |
| **503**               | **Service unavailable**                     | The server is in maintenance or is temporarily overloaded and cannot process the request.                 |
| **504**               | **Server time-out**                         | The server tried to access another server while trying to process a request, no timely response.          |
| **505**               | **Version not supported**                   | The SIP protocol version in the request is not supported by the server.                                   |
| **513**               | **Message too large**                       | The request message length is longer than the server can process.                                         |
| **555**               | **Push notification service not supported** | The server does not support the push notification service specified in the pn-provider SIP URI parameter. |
| **580**               | **Precondition failure**                    | The server is unable or unwilling to meet some constraints specified in the offer.                        |

#### 6xx = Global failures

| **SIP Response Code** | **SIP Equivalent**          | **Description**                                                                                                                            |
| --------------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **600**               | **Busy everywhere**         | All possible destinations are busy.                                                                                                        |
| **603**               | **Decline**                 | Destination cannot/doesn’t wish to participate in the call,  no alternative destinations.                                                  |
| **604**               | **Does not exist anywhere** | The server has authoritative information that the requested user does not exist anywhere.                                                  |
| **606**               | **Not acceptable**          | The server has authoritative information that the requested user does not exist anywhere.                                                  |
| **607**               | **Unwanted**                | The called party did not want his call from the calling party. Future attempts from the calling party are likely to be similarly rejected. |
| **608**               | **Rejected**                | An intermediary machine or process rejected the call attempt.                                                                              |

## Appendix: Full list of 4xx SIP codes

| **SIP Response Code** | **SIP Equivalent**                   | **Description**                                                                                                                                                                                                                                                                                                                                                  |
| --------------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **400**               | **Bad Request**                      | The request could not be understood due to malformed syntax.                                                                                                                                                                                                                                                                                                     |
| **401**               | **Unauthorized**                     | The request requires user authentication. This response is issued by UASs and registrars.                                                                                                                                                                                                                                                                        |
| **402**               | **Payment required**                 | Reserved for future use.                                                                                                                                                                                                                                                                                                                                         |
| **403**               | **Forbidden**                        | The server understood the request, but is refusing to fulfill it.                                                                                                                                                                                                                                                                                                |
| **404**               | **Not found**                        | The server has definitive information that the user does not exist at the (User not found)                                                                                                                                                                                                                                                                       |
| **405**               | **Method not allowed**               | The method specified in the Request-Line is understood, but not allowed.                                                                                                                                                                                                                                                                                         |
| **406**               | **Not acceptable**                   | The resource is only capable of generating responses with unacceptable content.                                                                                                                                                                                                                                                                                  |
| **407**               | **Proxy authentication required**    | The request requires user authentication.                                                                                                                                                                                                                                                                                                                        |
| **408**               | **Request timeout**                  | Couldn’t find the user in time.                                                                                                                                                                                                                                                                                                                                  |
| **409**               | **Conflict**                         | User already registered (deprecated).                                                                                                                                                                                                                                                                                                                            |
| **410**               | **Gone**                             | The user existed once but is not available here anymore.                                                                                                                                                                                                                                                                                                         |
| **411**               | **Length required**                  | The server will not accept the request without a valid content length (deprecated).                                                                                                                                                                                                                                                                              |
| **412**               | **Conditional request failed**       | The given precondition has not been met.                                                                                                                                                                                                                                                                                                                         |
| **413**               | **Request entity too large**         | Request body too large.                                                                                                                                                                                                                                                                                                                                          |
| **414**               | **Request URI too long**             | Server refuses to service the request, the Req-URI is longer than the server can interpret.                                                                                                                                                                                                                                                                      |
| **415**               | **Unsupported media type**           | Request body is in a non-supported format.                                                                                                                                                                                                                                                                                                                       |
| **416**               | **Unsupported URI scheme**           | Request-URI is unknown to the server.                                                                                                                                                                                                                                                                                                                            |
| **417**               | **Unknown Resource-Priority**        | There was a resource-priority option tag, but no Resource-Priority header.                                                                                                                                                                                                                                                                                       |
| **420**               | **Bad extension**                    | Bad SIP Protocol Extension used, not understood by the server.                                                                                                                                                                                                                                                                                                   |
| **421**               | **Extension required**               | The server needs a specific extension not listed in the Supported header.                                                                                                                                                                                                                                                                                        |
| **422**               | **Session interval too small**       | The request contains a Session-Expires header field with a duration below the minimum.                                                                                                                                                                                                                                                                           |
| **423**               | **Interval too brief**               | Expiration time of the resource is too short.                                                                                                                                                                                                                                                                                                                    |
| **424**               | **Bad location information**         | The request’s location content was malformed or otherwise unsatisfactory.                                                                                                                                                                                                                                                                                        |
| **428**               | **Use identity header**              | The server policy requires an Identity header, and one has not been provided.                                                                                                                                                                                                                                                                                    |
| **429**               | **Provide referrer identity**        | The server did not receive a valid Referred-By token on the request.                                                                                                                                                                                                                                                                                             |
| **430**               | **Flow failed**                      | A specific flow to a user agent has failed, although other flows may succeed.                                                                                                                                                                                                                                                                                    |
| **433**               | **Anonymity disallowed**             | The request has been rejected because it was anonymous.                                                                                                                                                                                                                                                                                                          |
| **436**               | **Identity info**                    | The request has an Identity-Info header and the   URI scheme contained cannot be de-referenced.                                                                                                                                                                                                                                                                  |
| **437**               | **Unsupported certificate**          | The server was unable to validate a certificate for the domain that signed the request.                                                                                                                                                                                                                                                                          |
| **438**               | **Invalid identity header**          | Server obtained a valid certificate used to sign a request, was unable to verify the signature.                                                                                                                                                                                                                                                                  |
| **439**               | **First hop lacks outbound support** | The first outbound proxy doesn’t support the “outbound” feature.                                                                                                                                                                                                                                                                                                 |
| **440**               | **Max-Breadth Exceeded**             | If a SIP proxy determined a response context had insufficient Incoming Max-Breadth to carry out a desired parallel fork, and the proxy is unwilling/unable to compensate by forking serially or sending a redirect, that proxy MUST return a 440 response. A client receiving a 440 response can infer that its request did not reach all possible destinations. |
| **469**               | **Bad info package**                 | If a SIP UA receives an INFO request associated with an Info Package that the UA has not indicated willingness to receive, the UA MUST send a 469 response, which contains a Recv-Info header field with Info Packages for which UA is willing to receive INFO requests.                                                                                         |
| **470**               | **Consent needed**                   | The source of the request did not have the permission of the recipient to make such a request.                                                                                                                                                                                                                                                                   |
| **480**               | **Temporarily unavailable**          | Callee currently unavailable.                                                                                                                                                                                                                                                                                                                                    |
| **481**               | **Call/Transaction does not exist**  | Server received a request that does not match any dialogue or transaction.                                                                                                                                                                                                                                                                                       |
| **482**               | **Loop detected**                    | Server has detected a loop.                                                                                                                                                                                                                                                                                                                                      |
| **483**               | **Too many hops**                    | Max-Forwards header has reached the value ‘0’.                                                                                                                                                                                                                                                                                                                   |
| **484**               | **Address incomplete**               | Request-URI incomplete.                                                                                                                                                                                                                                                                                                                                          |
| **485**               | **Ambiguous**                        | Request-URI is ambiguous.                                                                                                                                                                                                                                                                                                                                        |
| **486**               | **Busy here**                        | Callee is busy.                                                                                                                                                                                                                                                                                                                                                  |
| **487**               | **Request terminated**               | Request has terminated by bye or cancel.                                                                                                                                                                                                                                                                                                                         |
| **488**               | **Not acceptable here**              | Some aspects of the session description of the Request-URI are not acceptable.                                                                                                                                                                                                                                                                                   |
| **489**               | **Bad event**                        | The server did not understand an event package specified in an Event header field.                                                                                                                                                                                                                                                                               |
| **491**               | **Request pending**                  | Server has some pending request from the same dialogue.                                                                                                                                                                                                                                                                                                          |
| **493**               | **Undecipherable**                   | Request contains an encrypted MIME body, which recipient can not decrypt.                                                                                                                                                                                                                                                                                        |
| **494**               | **Security agreement required**      | <p></p><p></p><p>The server has received a request that requires a negotiated security mechanism.</p>                                                                                                                                                                                                                                                            |

## Speaking of SIP

SIP response codes are the language of communication systems, enabling efficient and effective call management. By familiarizing yourself with these codes, you can gain deeper insights into the status of your communication sessions and troubleshoot any issues that may arise. Additionally, service providers can leverage SIP response codes to deliver reliable and high-quality communication experiences to their customers.

