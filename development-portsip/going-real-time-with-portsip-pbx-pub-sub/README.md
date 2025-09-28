# WSI: Pub/Sub

PortSIP PBX offers a Pub/Sub mechanism using WebSocket (PortSIP WSI). This allows users to subscribe to PBX events in any programming language. When subscribed events occur, PortSIP PBX automatically pushes event messages to subscribers in JSON format.

Support version: v16.0 or higher

## Service Port

The PortSIP PBX/UCaaS provides WSI on port **8887** over **WSS**, the server must be allowed this port on the firewall for TCP, which requires WSS(TLS).

## Server URL

The WSI Server URL varies between PortSIP PBX versions:

* **v22.0 or higher**: `wss://pbx.portsip.com:8887/wsi`
* **v16.x**: `wss://pbx.portsip.com:8885`

The WebSocket client application needs to connect to the appropriate URL. Please replace `pbx.portsip.com` it with your actual PBX domain.



