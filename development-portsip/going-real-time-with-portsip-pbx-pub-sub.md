# WSI: Pub/Sub

PortSIP PBX provides a Publish/Subscribe (Pub/Sub) mechanism over WebSocket, known as PortSIP WSI. This mechanism allows applications written in any programming language to subscribe to PBX events.

When a subscribed event occurs, PortSIP PBX automatically pushes event notifications to the client in JSON format, enabling real-time, event-driven integrations without polling.

***

### Supported Versions

* PortSIP PBX v16.0 or later

***

### Service Port

PortSIP PBX (including UCaaS deployments) exposes the WSI service using **secure WebSocket (WSS)**:

* **Port:** `8887`
* **Protocol:** WSS (WebSocket over TLS)
* **Firewall Requirement:**\
  The server firewall must allow **TCP traffic on port 8887**.

***

### Server URL

The WSI server URL depends on the PortSIP PBX version:

*   **v22.0 or later**

    ```
    wss://pbx.portsip.com:8887/wsi
    ```
*   **v16.x**

    ```
    wss://pbx.portsip.com:8885
    ```

> **Note:** Replace `pbx.portsip.com` with your **actual PBX domain or IP address** when configuring your WebSocket client.





