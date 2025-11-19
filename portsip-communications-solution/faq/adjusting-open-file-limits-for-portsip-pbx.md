# Adjusting Open File Limits for PortSIP PBX

Sometimes, depending on the default Linux OS configuration, the maximum number of open files (`nofile`) may be set too low. When this happens, PortSIP PBX may not be able to support a large number of users, especially when many clients connect to the PBX using TCP.

**Note:**\
In most deployments, you do not need to change this. The default Linux and Docker settings already allow a sufficiently high number of open files.\
Only follow this guide if you encounter system warnings or limitations such as **“too many open files.”**

To ensure stable performance under heavy workloads, it is recommended to increase the maximum open file limit (`nofile`) at both the **Linux system level** and the **Docker engine level**.

This guide provides detailed and safe steps to update these limits.

***

### **1. Check the Current File Limit for Docker**

Perform the command below to check whether the Docker service already has a file limit applied:

```shellscript
systemctl show docker | grep LimitNOFILE
```

If the value is low (for example, **1024**, meaning the PBX can open only up to 1024 files/TCP connections), continue with the steps below.

***

### **2. Update the Docker Systemd Service Limit**

Perform the command below to open the override configuration for the Docker service:

```shellscript
systemctl edit docker.service
```

Add the following content:

```shellscript
[Service]
LimitNOFILE=1065535
```

Save and close the editor.

***

### **3. Apply the Systemd Changes**

Perform the commands below to reload Systemd and restart Docker:

```shellscript
systemctl daemon-reload
systemctl restart docker
```

***

### **4. Configure Docker Daemon File Descriptor Limits**

Perform the command below to open the Docker daemon configuration file (ensure you have installed the nano editor):

```shellscript
nano /etc/docker/daemon.json
```

If the file does not exist, **create it**.

Add the following configuration (or merge it with existing settings):

```json
{
  "default-ulimits": {
    "nofile": {
      "name": "nofile",
      "soft": 565535,
      "hard": 565535
    }
  }
}
```

Save the file and exit the editor.

***

### **5. Restart Docker to Apply the New Limits**

Perform the command below:

```shellscript
systemctl restart docker
```

***

### **6. Verify the Limits Inside the PortSIP PBX Container**

Perform the command below to enter the PortSIP Call Manager container:

```shellscript
docker exec -it portsip.callmanager /bin/bash
```

Perform the commands below to check the soft and hard limits:

```shellscript
ulimit -Sn
ulimit -Hn
```

**Expected output:**

```shellscript
565535
565535
```

If both values match, the configuration has been successfully applied.

