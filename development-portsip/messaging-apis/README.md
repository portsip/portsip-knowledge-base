# Messaging APIs

## Overview

The PortSIP Instant Messaging (IM) service facilitates the routing and storage of messages for both peer-to-peer and group communications, utilizing a publish-subscribe model. The core components of this service include sessions, users, and topics.

* **Sessions**: These are WebSocket connections established between client applications and the server.
* **Users**: PBX users (extensions) who connect to the IM service through active sessions.
* **Topics**: Named communication channels that route content between sessions. Both peer-to-peer and group messaging operate within the framework of topics.

Each user and topic within the IM service is assigned a unique identifier for seamless communication and message management.

