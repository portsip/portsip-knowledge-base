# Add Extended Media Server

When a PBX handles large simultaneous calls, we will need to set up some extended media servers to handle the media stream and recording.

Please follow the below steps to set up an extended media server.

## 1 Setup the PBX

Please ensure you have successfully installed the PBX v12.x on the Linux server.

## 2 Enable the Database Remote Access&#x20;

On the PBX server, edit the `/var/lib/portsip/pgsql/data/pg_hba.conf` file.

Add the following line at the end of the configuration file:

```
 host    all    all   0.0.0.0/0
```

&#x20;You can also use the media server IP instead of the `0.0.0.0`, for example, if the extended media server is `192.168.0.11`:

```
 host    all    all   192.168.0.11/0
```

If the **FirewallD** is enabled in the PBX server, the port `5432` and `8903` on `TCP` must be allowed.

```
firewall-cmd --permanent --service=portsip-pbx --add-port=5432/tcp
firewall-cmd --permanent --service=portsip-pbx --add-port=8903/tcp
```

## 3 Restart PBX Container

Perform the below command to restart the PBX.

```
docker stop -t 120 portsip-pbx 
docker start portsip-pbx
```

## 4 Add Extended Media Server

Sign in to the PBX Web Portal and select the menu: **Advanced >** Media **Server > Add,** fill out the extended media server information.

## 5 Setup the Extended Media Server

1. Login to the extended media server via SSH, and perform the below command:

```
curl https://raw.githubusercontent.com/portsip/portsip-pbx-sh/master/v12.6.x/install_extend.sh -o install_extend.sh
sh install_extend.sh -s media -i <img> -d <passwd> -a <ip>
```

### **Options:**

* \-s media: required, service (media or conference)
* \-i \<img>: required, PBX docker image
* \-d \<passwd>: required, PBX DB password (MUST be the DB password used when deploying PBX)
* \-a \<ip>: specify the  PBX private IP address, required when the PBX without public IP
* \-p \<ip>: specify the PBX public IP address, required when the PBX PBX without private IP&#x20;
* \-v \<path>: optional, host path which will be used to store data(default: /var/lib/portsip/\<service>)

### **Example**:

```
sh install_extend.sh -s media -s portsip/pbx:12.7 -d password -a 192.168.1.98
```

## 6 Checking the Extended Media Server Status

Sign in to the PBX Web Portal and select the menu: **Advanced >** **Media Server,** check the extended media server status is **Online**.

