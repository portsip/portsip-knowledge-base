# Messaging APIs

## Overview

The PortSIP Instant Messaging (IM) service facilitates the routing and storage of messages for both peer-to-peer and group communications, utilizing a publish-subscribe model. The core components of this service include sessions, users, and topics.

* **Sessions**: These are WebSocket connections established between client applications and the server.
* **Users**: PBX users (extensions) who connect to the IM service through active sessions.
* **Topics**: Named communication channels that route content between sessions. Both peer-to-peer and group messaging operate within the framework of topics.

Each user and topic within the IM service is assigned a unique identifier for seamless communication and message management.

## Client Behavior

### Connection and Authentication

Clients, such as mobile, desktop apps, or web applications, connect to the IM service via WebSocket to establish a session. All operations require successful client authentication. Clients authenticate their sessions by sending a `{login}` message. Each user can have multiple active sessions, supporting multi-device login.

### Message Exchange

After authentication, users can send and receive messages with other users through various topic types, whether in peer-to-peer (P2P) or group settings.

#### **Topic Types**

* **me**: A special topic used to receive notifications related to other topics. Each user has their own `me` topic.
* **emc**: This topic handles the sending and receiving of external messages, such as SMS and WhatsApp. For instance, if user A's ID is `123456`, they can subscribe to `emc123456` to receive messages when contacts send them SMS/WhatsApp messages to him.
* **P2P**: A direct communication channel between two users. Each user subscribes to the other’s user ID as the topic ID. For example, other users can subscribe to the `usr123456` topic to communicate directly with user A, in case 123456 is the user ID of A.
* **Group**: A communication channel for multiple users. Group topics must be explicitly created by a user.

#### Message Operations

* Users subscribe to topics by sending `{sub}` messages. Once subscribed, they can publish messages to a topic using `{pub}` messages. The server will then push `{data}` messages to all subscribers of that topic.
* Users can query or modify topic information using `{get}` and `{set}` messages.
* When topic information changes (e.g., updates in description, or users joining/leaving), subscribers will receive `{pres}` (presence) messages.

#### Presence

When a user subscribes to their own `me` topic, the server will send a `{pres}` message to all subscribers of that user’s P2P topic, notifying them that the user is online.

### Notes

* **Timestamps**: All timestamps are represented as strings in RFC 3339 format, with millisecond precision, and the timezone set to UTC. For example: `2015-10-06T18:07:29.841Z`.
* **Base64 Encoding**: The service uses Base64 URL encoding without padding, following RFC 4648.
* **Message IDs**: In `{data}` messages, the server-defined message ID (`seq` field) is a decimal number starting from 1, incrementing with each message to ensure uniqueness within each topic.
* **Request-Response Association**: Each message sent by a client to the server must include a unique request ID. The server will return this ID as-is in the response, but it will not interpret or modify it, then client app can easy to match the response with request.

## WebSocket

Clients connect to the server via WebSocket. When establishing the connection, the path is:&#x20;

* `/im`

## Users

In the PortSIP IM service, `users` refer to PBX users (extensions) who act as both producers and consumers of messages.

* **Authentication**: After successfully establishing a WebSocket connection, the client application authenticates the user by sending a `{login}` message.
* **User ID**: Each user is assigned a unique ID, which likes `804252613548703744.`
* **Multiple Sessions**: A user can maintain multiple concurrent session connections with the server, allowing for multi-device support.
* **Logout Behavior**: The IM service does not support explicit logout by design. If the application needs to switch users, it must close the existing WebSocket connection and establish a new connection with the new user's credentials.

## Login

Users initiate a `{login}` message request through an active session, prompting the server to perform authentication.\
Upon processing the login request, the server responds with a `{ctrl}` message. A successful authentication returns a 200 status code, while a failed attempt results in a 4xx error code.

## Access Control

Access control in the PortSIP IM service is managed through Access Control Lists (ACLs), which assign individual permissions to each subscriber within a topic. Access control is primarily applied to group topics, with limited functionality for `me` and P2P topics, such as managing status notifications or restricting P2P conversations.

### Permission Structure

User access to topics is governed by two sets of permissions:

* **Wanted**: The permissions the user requests.
* **Given**: The permissions granted by the topic manager.

Permissions are represented as bits within a bitmap and can either be present or absent. The actual access rights are determined by performing a bitwise AND operation between the _wanted_ and _given_ permissions. These permissions are communicated as a set of ASCII characters, where each character corresponds to specific permission bits:

* **No Access (N)**: Not a permission itself but an explicit indication that no permissions are set. This typically means default permissions should not apply.
* **Join (J)**: Permission to subscribe to the topic.
* **Read (R)**: Permission to receive `{data}` messages (i.e., read messages).
* **Write (W)**: Permission to send `{pub}` messages (i.e., publish messages).
* **Presence (P)**: Permission to receive `{pres}` messages (i.e., status updates).
* **Approve (A)**: Permission to approve requests to join the topic, remove members, or ban users. This permission designates topic administrators.
* **Share (S)**: Permission to invite others to join the topic.
* **Delete (D)**: Permission to hard delete messages. Only the topic owner can fully delete a topic.
* **Owner (O)**: The user is the topic owner, with the ability to assign any other permissions to members, change the topic description, or delete the topic. Each topic can have only one owner, though some topics may have no designated owner.

When users subscribe to a topic or initiate conversations, access permissions are either explicitly set or assigned by default (`defacs`). These permissions can be modified via the `{set}` message.

### Setting Permissions

Clients can define permissions in both `{sub}` and `{set}` messages. If permissions are omitted or set to an empty string (excluding `N!`), the server will apply the previously assigned default permissions (`defacs`). If no default permissions exist, authenticated users in group topics will be granted **JRWPS** access, while those in P2P topics will receive **JRWPA** access.

### **Default Access Permissions**

Default access permissions are configured for a class of users (authenticated users). These defaults are applied as the _given_ permissions for all new subscribers. Default permissions for a topic are set during topic creation via the `{sub.desc.defacs}` field and can be updated by the topic owner through a `{set}` message.

The global default access permissions for all users are **JRWPA**. Users can modify their personal default permissions by sending a `{set}` message to their `me` topic.

## Topics

Topics are communication channels between users (P2P, group, and external message channels such as SMS, Whatsapp).

{% hint style="info" %}
Clients must actively subscribe to a topic in order to send and receive messages, as well as to receive notifications about any changes in the topic's status.
{% endhint %}

### Topic Attributes

Topic attributes can be queried using the `{get what="desc"}` message. Common attributes include:

* **created**: Timestamp indicating when the topic was created.
* **updated**: Timestamp of the last update to trusted, public, or private attributes.
* **touched**: String representing the timestamp of the most recent message received.
* **defacs**: String representing the default access mode.
* **auth**: String representing the default access mode for authenticated users.
* **anon**: String representing the default access mode for anonymous users (currently unused).
* **seq**: Numeric value representing the current latest message ID for the topic.
* **trusted**: JSON object representing authentication attributes (currently unused). Readable by anyone, but only administrators can modify it.
* **public**: JSON object representing public attributes. Any user with subscription access can view public data.

#### User-related Attributes

* **acs**: Describes the current access permissions for a given user.
* **want**: The access permissions the user requests.
* **given**: The access permissions granted to the user.
* **private**: The user's private attributes.

Typically, topics have multiple subscribers, and one subscriber can be designated as the topic owner with full access permissions (O access).\
The subscriber list can be queried using the `{get what="sub"}` message, with the list returned in the `sub` section of the `{meta}` message.

### **me Topic**

Each user has a unique `me` topic, which is used for receiving presence status notifications and updates about the status of other subscribed topics.

* The `me` topic has no owner and cannot be deleted or unsubscribed from.
* Users can leave the `me` topic, halting related communications and signaling that the user is offline (though the user may still be logged in and interacting with other topics).
* Joining or leaving the `me` topic triggers `{pres}` status updates, sent to the user and all subscribers with P permissions in P2P topics.
* The `me` topic is read-only, and `{pub}` messages sent to it will be rejected.
* Queries using `{get what="sub"}` on the `me` topic return the list of topics the user is subscribed to, not subscriptions to the `me` topic itself.
* Specific status-related fields for `me` topics include:
  * **recv**: The message ID (seq) that the current user reports as received.
  * **read**: The message ID (seq) that the current user reports as read.
  * **seen**: For P2P subscriptions, reports the timestamp and User Agent of the user's last presence.
  * **when**: The timestamp of the user’s last online status.

Messages sent to `me` using `{get what="data"}` are rejected.

#### Subscribing to the me Topic

After successful client authentication, users must actively subscribe to their `me` topic. Subscriptions are initiated by sending a `{sub}` message, and the server responds with a `{ctrl}` message.

### **emc Topic (External Message Channel)**

The `emc` topic is used for sending and receiving external messages (SMS, MMS). The format for `emc` topics is: `emc[user ID, ring group ID, queue ID]`.

* After login, clients must subscribe to the `emc[current user ID]` topic. If the user is an agent in a queue or ring group (associated with `emc`), they should also subscribe to `emc[ring group ID, queue ID]`.
* When the PBX receives an external message, it notifies the IM service, which determines the target topic and broadcasts the message.
* Users can leave the `emc` topic, which will stop all external message transmission and reception.
* Clients send `{pub}` messages to the `emc` topic, and the server forwards the content to the PBX’s message queue. The message is also synchronized across the user's other sessions (multi-device synchronization).
* All operations within the `emc` topic follow the same behavior as in group topics.

### P2P Topics

P2P topics represent a direct communication channel between two users and do not have a designated owner. Each participant views the other user’s ID as the P2P topic ID. For example, if the two users are `usr804252613548703744` and `usr804252613548777777`, user 1 will see the topic ID as `usr804252613548777777`, while user 2 will see it as `usr804252613548703744`.

* **Public Parameter**: The public parameter in a P2P topic is user-specific. For instance, in a P2P topic between user A and user B, user A’s public data will be visible to user B, and vice versa. When a user updates their public data, all P2P topics involving that user will reflect the update.
* **Private Parameter**: Like other topic types, the private parameter in a P2P topic is defined individually by each participant.

#### Creating a P2P Topic

A P2P topic is automatically created when one user subscribes to another user's ID. The topic ID corresponds to the other user’s ID. For example, user `usr804252613548703744` can create a P2P topic with user `usr804252613548777777` by sending `{sub topic="usr804252613548777777"}`.

* The server responds with a `{ctrl}` message containing the topic ID, as described above.
* The target user will receive a `{pres}` message on their `me` topic, which includes access permissions.
* To engage in the conversation or receive notifications, the other user must actively subscribe to the P2P topic.

#### Subscribing to a P2P Topic

Clients subscribe to a P2P topic by sending the `{sub topic="usr804252613548777777" what="desc data"}` message. The server will respond with a `{ctrl}` message confirming the subscription.

### Group Topics

Group topics are communication channels designed for multiple users. The topic ID format is a string consisting of the fixed prefix `grp` followed by 11 pseudo-random characters.

* **Subscriber Limit**: The number of subscribers is limited and controlled by the `im.max_subscriber_count` configuration in the settings file, with a default limit of 1,000 subscribers.
* **Access Permissions**: Each subscriber’s access permissions in a group topic are managed individually.
* **Ownership**: Ownership of a group topic can be transferred to another user using a `{set}` message. However, there must always be one designated owner of the topic.

#### Creating a Group Topic

A new group topic is created by sending a `{sub}` message with the topic field set to the string `"new"` (any additional characters following `"new"` are ignored, e.g., `new` or `newAbC123` are treated the same).

* The server will respond with a `{ctrl}` message containing the newly generated topic ID. For example, `{sub topic="new"}` might return `{ctrl topic="grpmiKBkQVXnm3P"}`.
* If the topic creation fails, an error message will be reported on the original topic (`new` or `newAbC123`).
* The user who initiates the topic creation automatically becomes the topic owner.

#### Subscribing to a Group Topic

Clients can subscribe to a group topic by sending a `{sub topic="grp***" what="desc sub data"}` message. The server will then respond with a `{ctrl}` message confirming the subscription.

#### Leaving a Group Topic

Clients can leave a group topic by sending a `{leave}` message, or they may be removed by an administrator via a `{del}` message.

## Server-Issued Message IDs

The server assigns message IDs (`seq` field) to support clients in caching `{data}` messages.

* **Retrieving the Latest Message ID**: Clients can request the latest message ID for a topic by sending a `{get what="desc"}` message.
  * If the returned message ID is higher than the last message ID received by the client, the client will recognize that there are unread messages in the topic and can determine their count.
* **Retrieving Unread Messages**: Clients can retrieve unread messages by sending a `{get what="data"}` message.
* **Paginating Through Historical Messages**: Message IDs can also be used by clients to paginate through historical (offline) messages for a topic.

## User Agent and Presence Notifications

When one or more sessions are connected to a user's `me` topic, the user is reported as online through a `{pres}` message.

## Trusted, Public, and Private Attributes

Both topics and subscriptions contain **Trusted**, **Public**, and **Private** fields.

* The server does not validate or enforce these fields; instead, they are managed and interpreted by the client software using a consistent format.

### Trusted (Not Currently Used)

The `trusted` field in topic attributes is optional and formatted as a set of key-value pairs.

### Public

Public attributes:

```js
{
  "fn": "John Doe", // String, topic name
  "photo": {
    "type": "jpeg", // String, MIME type
    "data": "Rt53jUU...iVBORw0KGgoA==", // String, base64 (icon file data) (in message body)
    "ref": "file id", // String, icon file ID (outside message body)
    "width": 512, // Number, icon width
    "height": 512, // Number, icon height
    "size": 123456 // Number, icon file size
  },
  "note": "Some notes" // String, topic description
}
```

### Private (Not Currently Used)

Private attributes are formatted as a set of key-value pairs.

## Messages

All messages are in UTF-8 encoding and formatted as JSON.

* **Request ID**: All client-to-server messages may include an `id` field, referred to as the request ID. This ID is set by the client to track the receipt and processing of messages by the server. The request ID must be a unique string within the session, formatted as a UUID string (e.g., `"00000000-0000-0000-0000-000000000000"`, 36 characters long). When the server replies to the client message, it returns the request ID unchanged.
* **Strict JSON Compliance**: The server requires strictly valid JSON format, including double quotes around field names.
* **Clearing Fields**: To update fields such as `private` or `public` in `{set}` messages, use a string containing a single Unicode DEL character `"␡"` (`\u2421`) to clear data. Sending `"public": null` will not clear the field, but sending `"public": "␡"` will.
* **Unrecognized Fields**: Any unrecognized fields in the message will be silently ignored by the server.

### Content Format

The `content` field in `{pub}` and `{data}` messages support the following formats:

* **Plain text**

All messages can be sent as plain text. If you need to include formatting information for rich text, you can describe the formats using JSON, HTML, or XML, based on your preference. The specific format should be negotiated between the communicating applications. The receiving application is responsible for parsing the format information and then displaying the message as either rich text or plain text.

### Message Storage

The server stores the content of `{pub}` messages, including both the `head` and `content` fields.

### Message Routing

After receiving and storing a `{pub}` message, the server distributes it to all other active sessions within the topic via `{data}` messages, ensuring message distribution and multi-device synchronization.

The specific process is as follows:

1. **Receive and Store**: The server receives the `{pub}` message and stores the `head` and `content` fields.
2. **Message Distribution**: The server identifies all active sessions subscribed to the topic.
3. **Multi-Device Synchronization**: The server sends `{data}` messages to all relevant sessions, ensuring that users receive the message across multiple devices if they have more than one session active.
4. **Notification**: Depending on access permissions and topic settings, users may also receive additional notifications such as presence updates.

<figure><img src="../.gitbook/assets/portsip-im-msg-routing.png" alt=""><figcaption></figcaption></figure>

## Examples

In this section, we will provide examples in Python to demonstrate how the client application interacts with the PortSIP PBX IM service. For these examples, we assume the PBX server is hosted at `https://pbx.portsip.com:8887`.

### Connecting IM Server

The client application needs to use WebSocket to connect to the URL `wss://pbx.portsip.com:8887/im`.

```python
ssl_context = ssl.create_default_context()  
ssl_context.check_hostname = False  
ssl_context.verify_mode = ssl.CERT_NONE
websocket = await websockets.connect(uri, ssl = ssl_context)
```

### Authentication

Once the connection to the IM server is successfully established, the client must authenticate with the server in order to send and receive messages.

#### Obtain Access Token

The IM service authenticates the client application using the PortSIP PBX access token. Please follow the instructions in the article "[PortSIP PBX REST API Authentication](rest-apis/authentication.md)" to obtain the token.

#### Authenticate with IM Service using the Access Token

Once the access token is obtained, you can authenticate with the IM service by sending the `login` command to the server.

```python
auth = str(user) + "@" + domain + ":" + access_token
secret = base64.b64encode(auth.encode())
uuid4 = uuid.uuid4()
message = {
    "login": {
    "id": str(uuid4),
    "scheme": "access_token",
    "secret": secret.decode()
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("client_auth  response : " + response)
ret = json.loads(response)
if ret["ctrl"]["code"] == 200 :
    return ret["ctrl"]["params"]["user"]
else :
    logger.error("client_auth [" + str(user) + "] retcode not 200  response : " + response) 
return ""
```

If authentication is successful, the server will respond as shown below.

{% code overflow="wrap" %}
```json
{
    "ctrl": {
        "id": "fef15fa2-3e27-49ec-9286-50279d67cde3",
        "params": {
            "authlvl": "auth",
            "token": "AAAAhnJBgwwAAAAvbEGDDB7R+mgUAAAAAQAC9wKOmcHTClrJc8wtV5MD7AIx0LTN5iy5E901p+EBgg==",
            "user": "usr804252613548777777"
        },
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T01:06:38.924626Z"
    }
}
```
{% endcode %}

* `["ctrl"]["params"]["user"]`: Represents the user ID.
* `["ctrl"]["params"]["token"]`: Represents the user's authentication token, which can be used for verifying permissions in future HTTP file uploads to the IM server.

### Subscribe to P2P Topic

P2P topics represent a communication channel between two users, and these topics do not have an owner.

The topic IDs for the two participating users are different. Each user views the other user’s ID as the P2P topic ID. For example, if the two users in the topic are `usr804252613548703744` and `usr804252613548777777`, the first user will see the topic ID as `usr804252613548777777`, while the second user will see the topic ID as `usr804252613548703744`.

```python
uuid4 = uuid.uuid4()
message = {
    "sub": {
        "id": str(uuid4),
        "topic": "usr804252613548703744"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("p2p_sub_message to : usr804252613548703744 response : " + response)
```

### Sending Message to P2P Topic

You can send a message to a P2P topic for another user using the following code snippet. The `["pub"]["id"]` is a UUID that the client application must generate. The server will return this ID in its response, allowing the client to track the message by this ID.

```python
uuid4 = uuid.uuid4()
message = {
    "pub": {
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "noecho": False,
        "content": "test test test"
    }
}
try:
    await websocket.send(json.dumps(message))
    response = await websocket.recv()
except ConnectionClosedError as e:
    logger.error("Connection closed unexpectedly : " + str(e))
    break
logger.debug("pub_message to : usr804252613548703744 response : " + response)
ret = json.loads(response)
if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
    logger.error("pub_message  retcode error  response : " + response)
```

The response will look like the following:

```json
{
    "ctrl": {
        "id": "99c5075a-cf52-493c-8572-9e30437a02ce",
        "topic": "usr804252613548703744",
        "params": {
            "seq": 377
        },
        "code": 202,
        "text": "accepted",
        "ts": "2024-10-24T08:24:41.913558Z"
    }
}

```

The user with the ID `804252613548777777` will receive the message as shown below:

```json
{
    "data": {
        "topic": "usr804252613548703744",
        "from": "usr804252613548777777",
        "ts": "2024-10-24T08:24:41.913558Z",
        "seq": 377,
        "content": "test test test",
        "reqid": "99c5075a-cf52-493c-8572-9e30437a02ce"
    }
}
```

### Get the Topics I Have Subscribed To

You can use the following code to retrieve the topics you have subscribed to.

```python
uuid4 = uuid.uuid4()
message = {
    "get": {
        "sub": {
            "ims": "2024-01-03T07:38:56.304Z"
         },
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "what": "sub"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("p2p_sub_message to : usr804252613548703744 response : " + response)
 
```

The response will look like the following:

```json
{
    "meta": {
        "id": "362c603e-4343-40a6-ab32-4e4248171d89",
        "topic": "usr804252613548703744",
        "ts": "2024-10-24T01:34:48.960092Z",
        "sub": [
            {
                "updated": "2024-10-24T01:04:20.272656Z",
                "acs": {
                    "want": "JRWPA",
                    "given": "JRWPAS",
                    "mode": "JRWPA"
                },
                "user": "usr804252613548703744"
            },
            {
                "updated": "2024-10-24T01:30:49.077521Z",
                "acs": {
                    "want": "JRWPA",
                    "given": "JRWPA",
                    "mode": "JRWPA"
                },
                "read": 367,
                "recv": 367,
                "user": "usr804252613548777777"
            }
        ]
    }
}
```

### Query Topic Details

You can use the following message `{get what="desc"}` to query the details of a topic. For example, group-related information can be found within the details of a group topic.

```
uuid4 = uuid.uuid4()
message = {
    "get": {
        "desc": {
            "ims": "2024-01-03T07:38:56.304Z"
         },
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "what": "desc"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("p2p_sub_message to : usr804252613548703744 response : " + response)
```

The response will look like the following:

```json
{
    "meta": {
        "id": "926c7203-0ff7-4241-b9d2-0b46a5e625c6",
        "topic": "usr804252613548703744",
        "ts": "2024-10-24T01:30:45.144013Z",
        "desc": {
            "created": "2024-10-24T01:04:20.286287Z",
            "updated": "2024-10-24T01:26:51.198007Z",
            "touched": "2024-10-24T01:26:51.197597Z",
            "acs": {
                "want": "JRWPA",
                "given": "JRWPA",
                "mode": "JRWPA"
            },
            "seq": 305,
            "read": 305,
            "recv": 305
        }
    }
}
```

### Obtain Historical Messages

By using the `["meta"]["desc"]["seq"]` field returned from the `{get what="desc"}` message, you can retrieve the number of messages in the subscribed topic. You can then use `{get what="data"}` to fetch historical messages.

```python
uuid4 = uuid.uuid4()
message = {
    "get": {
        "data": {
            "before": 10000,
            "limit": 1000,
            "since": 0
        },
        "id": str(uuid4),
        "topic": "usr804252613548703744",
        "what": "data"
    }
}
try:
    await websocket.send(json.dumps(message))
    response = await websocket.recv()
except ConnectionClosedError as e:
    logger.error("Connection closed unexpectedly : " + str(e))
    break
logger.debug("sub_message to usr804252613548703744 response : " + response)
ret = json.loads(response)
if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
    logger.error("sub_message  retcode error  response : " + response)
while IS_RUNNING:
    try:
        message = await asyncio.wait_for(websocket.recv(), timeout=SENDER_SLEEP)
        logger.debug("sub_message  : " + message)
        ret = json.loads(message)
        if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
            logger.error("sub_message  retcode error  response : " + message)
        if 'data' in ret:
            //recived msg
    except asyncio.TimeoutError:
        break
    except ConnectionClosedError as e:
        logger.error("Connection closed unexpectedly : " + str(e))
        break

```

### Creating a Group Chat

You can use the following message to create a group chat.

```python
uuid4 = uuid.uuid4()
message = {
    "sub": {
        "id": str(uuid4),
        "topic": "new"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("group_create  response : " + response)
ret = json.loads(response)
if ret["ctrl"]["code"] == 200 :
    return ret["ctrl"]["topic"]
else :
    logger.error("group_create retcode not 200  response : " + response) 
return ""
```

The response will look like the following, the `["ctrl"]["topic"]` represents the group topic. Then other users can subscribe to group messages by sending `{sub topic="grpf0NlkdDEQHs"}`.

```json
{
    "ctrl": {
        "id": "06679cbc-b1d8-4821-a71f-44c1229e350e",
        "topic": "grpf0NlkdDEQHs",
        "params": {
            "acs": {
                "want": "JRWPASDO",
                "given": "JRWPASDO",
                "mode": "JRWPASDO"
            },
            "tmpname": "new"
        },
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T01:12:11.153147Z"
    }
}

```

### Join a Group

The user can send the following message to subscribe to the group topic, allowing them to join the group and send or receive messages within the group.

```
uuid4 = uuid.uuid4()
message = {
    "sub": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("sub to : grpf0NlkdDEQHs response : " + response)
```

The response will look like the following:

```json
{
    "ctrl": {
        "id": "9c1c98b5-5123-41be-a6b9-2fe03877b946",
        "topic": "grpf0NlkdDEQHs",
        "params": {
            "seq": 1
        },
        "code": 202,
        "text": "accepted",
        "ts": "2024-10-24T07:04:01.052963Z"
    }
}

```

### Sending a Message to a Group

You can use the following code to send a message to a group.

```python
uuid4 = uuid.uuid4()
message = {
    "pub": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs",
        "noecho": False,
        "content": "test test test"
    }
}
try:
    await websocket.send(json.dumps(message))
    response = await websocket.recv()
except ConnectionClosedError as e:
    logger.error("Connection closed unexpectedly : " + str(e))
    break
logger.debug("pub_message to : grpf0NlkdDEQHs response : " + response)
ret = json.loads(response)
if 'ctrl' in ret and ret["ctrl"]["code"] >= 300:
    logger.error("pub_message  retcode error  response : " + response)

```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "c1dd7a45-93db-4af9-870d-10643222b7e7",
        "topic": "grpf0NlkdDEQHs",
        "params": {
            "seq": 2
        },
        "code": 202,
        "text": "accepted",
        "ts": "2024-10-24T07:04:01.120034Z"
    }
}
```

The following message will be sent to all users who have subscribed to the group topic.

```json
{
    "data": {
        "topic": "grpf0NlkdDEQHs",
        "from": "usr901636310534455296",
        "ts": "2024-10-24T07:04:01.120034Z",
        "seq": 2,
        "content": "test test test",
        "reqid": "c1dd7a45-93db-4af9-870d-10643222b7e7"
    }
}
```

### Leave From a Group

Users can unsubscribe from a group by sending a `{leave}` message. This is the opposite function of the `{sub}` message. The `{leave}` message supports two options:

* **Leave without unsubscribing** (`unsub=false`)
* **Unsubscribe and leave** (`unsub=true`)

The server will respond to a `{leave}` message with a `{ctrl}` packet.

* **Leave without unsubscribing** (`unsub=false`) only affects the current WebSocket session. You will still receive messages if you establish a new WebSocket session with the IM server, and if the user is connected to the IM server from another app, they will also continue receiving messages.
* **Unsubscribe and leave** (`unsub=true`) affects all sessions for that user. The user will no longer receive messages from this group across any session unless he subscribes to this group topic again.

```python
uuid4 = uuid.uuid4()
message = {
    "leave": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs"
        "unsub": True 
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("leave to : grpf0NlkdDEQHs response : " + response)
```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "b154ccc9-4ac8-4a71-bf74-288771d818e5",
        "topic": "grp5XUxk__lpVY",
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T07:35:51.253545Z"
    }
}
```

### Remove a Member from the Group

The group administrator can remove a member from the group by sending the following message:

```python
uuid4 = uuid.uuid4()
message = {
    "del": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs",
        "user": "usr804252613548777777",
        "what": "sub"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("del usr804252613548777777 from grpf0NlkdDEQHs response : " + response)

```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "b154ccc9-4ac8-4a71-bf74-288771d818e5",
        "topic": "grpf0NlkdDEQHs",
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T07:35:51.253545Z"
    }
}
```

### Remove a Member from the Group

The group administrator can delete a group by sending the following message:

```python
uuid4 = uuid.uuid4()
message = {
    "del": {
        "id": str(uuid4),
        "topic": "grpf0NlkdDEQHs",
        "what": "topic"
    }
}
await websocket.send(json.dumps(message))
response = await websocket.recv()
logger.debug("del grpf0NlkdDEQHs response : " + response)

```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "b154ccc9-4ac8-4a71-bf74-288771d818e5",
        "topic": "grpf0NlkdDEQHs",
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T07:35:51.253545Z"
    }
}
```

### Uploading a File to the IM Server

To send a file in a chat, you first need to upload the file to the IM server using an HTTP POST form request to `https://pbx.portsip.com:8887/im/file`. An example code is provided below:

```python
//uuid4 = uuid.uuid4()
filename = str(uuid4)
with open(TEMP_DIR + '/'+ filename, 'w') as f:
    f.write(str(uuid4) * REPEAT_SIZE)

form = FormData()  
form.add_field('id', str(uuid4))  
form.add_field('file', open(TEMP_DIR + '/'+ filename, 'rb'), filename=filename)  

header = {  
    'Authorization': 'token ' + token
}  
try:
    if 'https' in upload_url:
        async with aiohttp.ClientSession(connector=TCPConnector(ssl=False)) as session:  
            async with session.post(upload_url, data=form, headers=header) as response:  
                if response.status == 200:  
                    text = await response.text()  
                    logger.debug(text)
                    count_upload()
                else:
                    text = await response.text()   
                    logger.error("http  post error status :" +  str(response.status) + " response : " + text)
    else:
        async with aiohttp.ClientSession() as session:  
            async with session.post(upload_url, data=form, headers=header) as response:  
                if response.status == 200:  
                    text = await response.text()  
                    logger.debug(text)
                    count_upload()
                else:
                    text = await response.text()   
                    logger.error("http  post error status :" +  str(response.status) + " response : " + text)
except aiohttp.ClientError as e:
    logger.error("http post error : " + str(e))
    continue
os.remove(TEMP_DIR + '/'+ filename)
await asyncio.sleep(UPLOAD_SLEEP)  
```

The response will look like the following if succeeds:

```json
{
    "ctrl": {
        "id": "93db2d24-6663-4071-a373-586b184be576",
        "params": {
            "url": "NDW4_NYseeH20iuC3rih_wAAwItyRIMM"
        },
        "code": 200,
        "text": "ok",
        "ts": "2024-10-24T01:13:54.464796Z"
    }
}
```

`["ctrl"]["params"]["url"]` is the file download URL. The sender can send this URL in a message to the recipient. After receiving the message, the recipient can download the file and display it in the chat according to the file type.

### Download the Chat File

Once a user receives a message containing the file URL, they can assemble the URL as shown below and download the file using an HTTP GET request.

[https://pbx.portsip.com:8887/im/file/NDW4\_NYseeH20iuC3rih\_wAAwItyRIMM](https://pbx.portsip.com:8887/im/file/NDW4\_NYseeH20iuC3rih\_wAAwItyRIMM)





