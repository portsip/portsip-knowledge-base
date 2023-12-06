# Trace server - A Better Way to Debug PortSIP UC

PortSIP has been building its SIP Trace Server base on the open-source project [HOMER](https://github.com/sipcapture/homer),  which provides key information in troubleshooting SIP Trunks, SIP endpoints, and other SIP related issues. It provides a place to:

* Access singularly to retrieve SIP captures via Web UI
* Centrally store SIP capture data across many hosts
* More intuitively filter SIP capture data and correlate the data to the dialog/transactions each request/response is part of (this is immensely useful!)
* Gracefully age dated capture data you don’t want to persist for very long (though persisting longer term can be configured very easily)
* Charts! glorious charts!

##

### **Supported Linux OS**

* CentOS: 7.9
* Ubuntu: 18.04, 20.04
* Debian: 10.x
* Only supports 64bit OS

### Preparing the Linux Host Machine for Installation&#x20;

Since the Trace server usually will take large CPU and memory resources, we recommend below hardware specifications for the SIP Trace Server:

#### Maximum of 100 simultaneous calls

* CPU: 2-4 cores
* Memory: 2G - 4G

#### Maximum of 1,000 simultaneous calls

* CPU: 4 cores
* Memory: 4G

#### Maximum of 5,000 simultaneous calls

* CPU: 8 - 10 cores
* Memory: 8G

#### Maximum of 10,000 simultaneous calls

* CPU: 16 - 20 cores
* Memory: 16G

{% hint style="info" %}
**Important**: don't install the PortSIP Trace Server on the same server of PortSIP PBX, since it will consume large resources cause the PBX does not work smoothly.

We recommend installing the PortSIP Trace Server in the same LAN as the PortSIP PBX.
{% endhint %}

### Firewall Rule

The firewall and cloud platform security group must allow the ports listed below.

* Port 9061 on TCP
* Port 9060 on TCP

### Installing PortSIP Trace Server

* Ensure server date-time is synced correctly
* Must perform all Linux commands by the **root** user, please **su root** first

#### **Download the installer**

```
curl  https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/trace-server/trace-server.sh -o trace-server.sh
```

#### Install and Start Service

```
/bin/sh trace-server.sh start
```

After the PortSIP Trace Server is successfully installed, you can access the trace server Web Portal by below URL:

`http://trace-server-ip:9080`

The user name is `admin`, the password is `admin`

### Enable SIP Trace in the PortSIP PBX

Sign in the PortSIP PBX Web Portal, click the menu "**Advanced**" > "**Settings**", on the "**Advanced**" page, enter the trace server information for the "**Custom Options**" field as below:

```
{"trace_server_ip" : "192.168.1.17", "trace_server_port" : 9061}
```

Please see the below screenshot.

![](<../../.gitbook/assets/image (17).png>)



{% hint style="info" %}
Important: replace the IP `192.168.1.17` with your trance server IP.
{% endhint %}

After successfully set up the SIP trace server information in the "**Custom Options**" field and clicked the "**Apply**" button, the PBX will send all SIP messages to the trace server.

### Disable SIP Trace in the PortSIP PBX

Click the menu "**Advanced**" > "**Settings**", on the "**Advanced**" page, delete all text content in the "**Custom Options**" field, and click the "**Apply**" button, the PortSIP PBX will stop to send the SIP message to trace server.

{% hint style="danger" %}
Important: SIP Trace should be off most of the time and only enabled when troubleshooting is required.
{% endhint %}

### Troubleshooting

You can choose to display the registration messages or the call SIP messages on the SIP Trace Server Web Portal, as seen in the screenshot below; the "**Home**" is for calls, and the "**REGISTRATION**" is for the **REGISTER** messages.

![](<../../.gitbook/assets/image (2).png>)

You can view the details of a message by clicking on the method name, as shown in the screenshot below.

![](<../../.gitbook/assets/image (19).png>)

There is the way to check the call-id, X-Session-Id, and X-CID of the SIP message are as follows:

![](<../../.gitbook/assets/image (21).png>)

call-id, X-Session-Id, and X-CID can now be used to search SIP messages.

![](<../../.gitbook/assets/image (10).png>)

![](<../../.gitbook/assets/image (7).png>)



From the messages list, we can see the call flow by clicking the Session ID.

![](<../../.gitbook/assets/image (6).png>)

![](<../../.gitbook/assets/image (28).png>)



### Manage the Trace Server Server

#### Start Service

```
/bin/sh trace-server.sh start
```

#### Stop Service:

```
/bin/sh trace-server.sh stop
```

#### Remove Server

```
/bin/sh trace-server.sh remove
```

