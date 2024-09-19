# Messaging APIs

## Overview

The PortSIP Instant Messaging (IM) service is designed to route and store messages (both peer-to-peer and group-based), following the publish-subscribe model. The service consists of sessions, users, and topics.

* **Sessions**: Represent WebSocket connections between client applications and the server.
* **Users**: Refer to PBX users(extensions) who connect to the service via sessions.
* **Topics**: Named communication channels used to route content between sessions. Both peer-to-peer and group communications are unified under the concept of topics.

Within the IM service, both users and topics have unique identifiers.

## Client Behavior

* **Connection and Authentication**: Clients, such as mobile apps or web applications, connect to the IM service via WebSocket to establish a session. All operations require successful client authentication. Clients authenticate their sessions by sending a {login} message. A single user can have multiple active sessions (supporting multi-device login).
* **Message Exchange**: Once authenticated, users can send and receive messages with other users through topics (P2P or group).
* **Topic Types**:
  * `me`: A special topic for receiving notifications for other topics. Every user has a me topic.
  * `emc`: Used for receiving and sending external messages like SMS and WhatsApp. For example, if user A’s user ID is 123456, once they subscribe to `emc123456`, they will receive messages when contacts send SMS/WhatsApp messages to them.
  * `P2P`: A direct communication channel between two users. Each participant subscribes to the other user’s ID as the topic ID. For example, if user A’s user ID is 123456, other users can subscribe to the `usr123456` topic.
  * `Group`: A communication channel for multiple users. Group topics must be explicitly created.
* **Message Operations:**
  * Users subscribe to topics using `{sub}` messages. Once subscribed, they can publish messages to the topic using `{pub}` messages. The server then pushes `{data}` messages to the subscribers of that topic.
  * Users can query or modify topic information by using `{get}` and `{set}` messages.
  * When a topic is changed (e.g., updating description, user joining/leaving), the subscribers will receive `{pres}` messages.
  * Presence: When a user subscribes to their own me topic, the server will push a `{pres}` message to the subscribers who have subscribed to this user’s P2P topic, informing them that the user is online.

## Notes

* Timestamps are always represented as strings in RFC 3339 format, with millisecond precision and the timezone always set to UTC, e.g., 2015-10-06T18:07:29.841Z.
* Base64 encoding is Base64 URL encoding with padding characters removed, as per RFC 4648.
* In `{data}` messages, the server-defined message ID (seq field) is a decimal number starting from 1, incrementing by 1 for each message, ensuring uniqueness within each topic.
* To associate request messages with response messages, each message sent from the client to the server must define a unique request ID. The server will not parse this ID and will return it to the client as-is.

## WebSocket

Clients connect to the server via WebSocket. When establishing the connection, the path is:&#x20;

* `/im`

## Users

Users refer to PBX users, who are both producers and consumers of messages.

* **Authentication**: After successfully establishing a WebSocket connection, the client application sends a `{login}` message to authenticate the user.
* **User ID**: Each user is assigned a unique ID. The user ID format is a string composed of a fixed prefix usr followed by the PBX user’s ID number, for example, `usr804252613548703744`.
* A user can maintain multiple session connections with the server simultaneously.
* Logging out is not supported by design. If the application needs to switch the user, it should close the current WebSocket connction and open a new connection and authenticate it with the new user’s credentials.

## Login

Users initiate a `{login}` message request through a session, and the server performs authentication.

The response to any login attempt is a `{ctrl}` message containing either a `200` code for success or a `4xx` error code for failure.

## Access Control

Access control manages user access to topics through **Access Control Lists** (ACLs), assigning permissions individually to each subscriber within a topic.

Access control is primarily used for **group** topics. Its availability for `me` and P2P topics is limited to managing status notifications and preventing users from initiating or continuing P2P conversations.

User access to topics is defined by two sets of permissions: the permissions the user wants (`want`), and the permissions granted to the user by the topic manager (`given`).

Each permission is represented by a bit in a bitmap. It can either be present or absent. Actual access rights are determined by the bitwise AND of the wanted and given permissions. Permissions are communicated in messages as a set of ASCII characters, where the presence of a character indicates a set of permission bits:

* No Access: `N`, not a permission itself, but an explicit indication of cleared/unset permissions. It typically means default permissions should not be applied.
* Join: `J`, permission to subscribe to the topic.
* Read: `R`, permission to receive messages (`{data}` messages).
* Write: `W`, permission to send messages (`{pub}` messages).
* Presence: `P`, permission to receive status updates (`{pres}` messages).
* Approve: `A`, permission to approve requests to join the topic, remove, and ban members; users with this permission are topic administrators.
* Share: `S`, permission to invite others to join the topic.
* Delete: `D`, permission to hard delete messages; only the topic owner can completely delete the topic.
* Owner: `O`, the user is the topic owner; owners can assign any other permissions to any topic member, change the topic description, and delete the topic. A topic can have only one owner; some topics may have no owner.

When users subscribe to a topic or start chatting with other users, access permissions are either explicitly set or assigned by default (`defacs`). Permissions can be modified by sending a `{set}` message.

Clients can set permissions in `{sub}` and `{set}` messages. If permissions are missing or set to an empty string (not `N`!), the server will use the previously assigned default permissions (`defacs`). If no default permissions are found, authenticated users in group topics will receive `JRWPS` access, and authenticated users in P2P topics will receive `JRWPA`.

Default access permissions are defined for a class of users: authenticated users. The value of default access permissions will be used as the `given` permissions for all new subscribers.

The default access permissions for a topic are established at the time of topic creation via `{sub.desc.defacs}`, and the owner can modify them by sending a `{set}` message.

The default access permissions for all users are `JRWPA`, and users can modify them by sending a `{set}` message to the `me` topic.

## Topics

Topics are communication channels between users (P2P, group).

{% hint style="info" %}
Clients must actively subscribe to a topic to send and receive messages, as well as receive notifications about topic status changes.
{% endhint %}

Topic attributes can be queried using the `{get what="desc"}` message.

Common attributes:

* `created`: Timestamp indicating when the topic was created.
* `updated`: Timestamp indicating the last update to `trusted`, `public`, or `private`.
* `touched`: String representing the timestamp of the latest received message.
* `defacs`: String representing the default access mode.
* `auth`: String representing the default access mode for authenticated users.
* `anon`: String representing the default access mode for anonymous users (currently not used).
* `seq`: Numeric value representing the current latest message ID for the topic.
* `trusted`: JSON object representing authentication attributes (currently not used). It can be read by anyone but only changed by administrators.
* `public`: JSON object representing public attributes. Anyone who can subscribe to the topic can receive `public` data.

User-related attributes:

* `acs`: Describes the current access permissions for a given user.
* `want`: The access permissions requested by the user.
* `given`: The access permissions granted to the user.
* `private`: The user’s private attributes.

Topics typically have subscribers. One of the subscribers can be designated as the topic owner with full access permissions (`O` access).

The subscriber list can be queried using the `{get what="sub"}` message, and the subscriber list is returned in the `sub` section of the `{meta}` message.

### `me` Topic

Every user has a `me` topic. It is used for receiving presence status notifications and notifications about the status of subscribed topics.

The `me` topic has no owner, and it cannot be deleted or unsubscribed from.

Users can `leave` the `me` topic, which will stop all related communications and indicate that the user is offline (although the user may still be logged in and continue using other topics).

Joining or leaving the `me` topic generates `{pres}` status updates, which are sent to the given user and all users with `P` permissions in P2P topics.

The `me` topic is read-only, and `{pub}` messages sent to `me` are rejected.

Messages sent to `me` with `{get what="sub"}` are different from any other topic because they return the list of topics the current user is subscribed to, which is not the expected subscription to `me`.

* `recv`: The message ID (`seq` value) that the current user reports as received.
* `read`: The message ID (`seq` value) that the current user reports as read.
* `seen`: For P2P subscriptions, reports the timestamp and `User Agent` of the user’s last presence.
* `when`: The timestamp of the user’s last online status.

Messages sent to `me` with `{get what="data"}` are rejected.

After successful client authentication, the `me` topic must be actively subscribed to.

### **Subscribing to the `me` Topic**

Clients initiate a subscription by sending a `sub` message, and the server responds with a `ctrl` message.

#### `emc` Topic (External Message Channel)

* The `emc` topic is used for sending or receiving external messages (SMS, MMS). The topic name format is: “`emc`\[user id, ring group id, queue id]”.

After a client successfully logs in, they must subscribe to the `emc[current user id]` topic. If the current user is an agent in a queue or ring group (associated with EMC), they also need to subscribe to these topics: `emc[ring group id, queue id]`.

When the PBX  receives an external message, it sends a notification to the IM service, which then determines the target topic and broadcasts the message.

Users can `leave` the `emc` topic, which will stop all external message sending and receiving.

Clients send `{pub}` messages to the `emc` topic, and the server forwards the message content to the PBX’s message queue. The message is also forwarded to other sessions of the same user (multi-device synchronization).

All operations within the `emc` topic are the same as those in group topics.

### P2P Topics

P2P topics represent a communication channel between two users and do not have an owner.

The topic IDs for the two participating users are different, with each user viewing the other user’s ID as the P2P topic ID. For example, if the two users in the topic are `usr804252613548703744` and `usr804252613548777777`, the first user will see the topic ID as `usr804252613548777777`, and the second user will see the topic ID as `usr804252613548703744`.

The `public` parameter of a P2P topic depends on the user. For example, a P2P topic between user A and user B will show user A’s `public` to user B, and vice versa. If a user updates their `public`, all P2P topics involving that user will automatically update to reflect the new `public`.

Like any other topic type, the `private` parameter of a P2P topic is defined individually by each participant.

### **Creating a P2P Topic**

A P2P topic is automatically created when one user subscribes to the other user, with the topic ID being the same as the other user’s ID. For example, user `usr804252613548703744` can establish a P2P topic with user `usr804252613548777777` by sending `{sub topic="usr804252613548777777"}`. The server will respond with a `{ctrl}` message containing the newly created topic ID, as described above. The target user will receive a `{pres}` message on their `me` topic, which includes access permissions. The other user must actively initiate a subscription to receive messages or notifications from this P2P topic.

### **Subscribing to a P2P Topic**

Clients initiate a subscription using the `{sub topic="usr804252613548777777" what="desc data"}` message, and the server will respond with a `{ctrl}` message.

### Group Topics

Group topics represent communication channels among multiple users.

The topic ID format is a string composed of a fixed `grp` prefix followed by 11 pseudo-random characters.

A limited number of subscribers are supported, controlled by the `im.max_subscriber_count` configuration in the configuration file, with a default of 1000. Access permissions for each subscriber in a group topic are managed individually.

Ownership can be transferred to another user via a `{set}` message, but there must always be one user designated as the owner of the topic.

### **Creating a Group Topic**

A topic is created by sending a `{sub}` message with the `topic` field set to the string `new` (it can be followed by any characters, e.g., `new` or `newAbC123` are equivalent). The server will respond with a `{ctrl}` message containing the newly created topic ID, e.g., `{sub topic="new"}` will respond with `{ctrl topic="grpmiKBkQVXnm3P"}`.

If the topic creation fails, an error will be reported on the original `topic`, i.e., `new` or `newAbC123`.

The user who creates the topic becomes the topic owner.

### **Subscribing to a Group Topic**

Clients initiate a subscription using the `{sub topic="grp***" what="desc sub data"}` message, and the server will respond with a `{ctrl}` message.

!Subscribing to a Group Topic

### Leaving a Group Topic

Clients can leave a group topic by sending a `{leave}` message or be removed by an administrator sending a `{del}` message.

## Server-Issued Message IDs

Server-issued message IDs (`seq` field) provide support for clients to cache `{data}` messages.

* Clients can obtain the latest message ID within a topic by sending a `{get what="desc"}` message.
* If the returned ID is greater than the last message ID received by the client, the client considers the topic to have unread messages and their count.
* Clients can retrieve these messages using the `{get what="data"}` message.
* Clients can also use message IDs to paginate through historical messages (offline messages).

### User Agent and Presence Notifications

For the same user, when one or more sessions are attached to the `me` topic, the user will be reported as online (`pres` message).

## Trusted, Public, and Private Attributes

Topics and subscriptions have `Trusted`, `Public`, and `Private` fields.

The server does not validate these fields; client software uses the same format.

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

All messages are text in UTF-8 encoding and JSON format.

All client-to-server messages may have an `id` field, known as the request ID. It is set by the client to acknowledge receipt and processing of the message by the server. The request ID is a unique string within the session, formatted as a UUID string (format: “00000000-0000-0000-0000-000000000000”, 36 characters long). When the server replies to the client message, it returns the request ID unchanged.

The server requires strictly valid JSON, including double quotes around field names.

For messages that update data, such as the `private` or `public` fields in `{set}` messages, use a string with a single Unicode DEL character "␡" (`\u2421`) to clear the data. Sending `"public": null` will not clear the field, but sending `"public": "␡"` will.

The server will silently ignore any unrecognized fields.

### Content Format

The `content` field in `{pub}` and `{data}` messages supports the following formats:

* Plain text
* Rich text (Drafty)

If rich text messages are used, the message header must be set to `"head": {"mime": "text/x-drafty"}`.

### Message Storage

The server stores the content of `pub` messages (`head` and `content` fields).

### Message Routing

After receiving and storing a `pub` message, the server distributes it to other sessions within the topic via `data` messages (message distribution, multi-device synchronization, etc.).

The specific process is as follows:







