# Configuring Private RPS Account

PortSIP PBX allows customers to configure their own private RPS (Remote Provisioning Server) accounts for auto-provisioning IP phones. This feature simplifies device management and enhances security by giving customers direct control over their IP phone configurations.

Follow these steps to configure your private RPS account:

1. **Locate the Configuration File**
   * **Linux**: Open the file located at `/var/lib/portsip/pbx/system.ini`.
   * **Windows**: Open the file located at `C:\ProgramData\PortSIP\PBX\system.ini`.
2. **Modify the Configuration**\
   Append the following sections to the file, corresponding to the IP phone brands you wish to configure and save the changes.

```ini
[yealink]
accesskey_id = xxxx
accesskey_secret = xxxx

[ale]
accesskey_id = xxxx
accesskey_secret = xxxx

[grandstream]
username = xxxx
password = xxxx 
# The value for the password key should be: sha256(md5(password))
api_id = xxxx
api_secret = xxxx
site_id = xxxx

[fanvil]
username = xxxx
password = xxxx
#The value for the password key should be: md5(md5(password))

[htek]
username = xxxx
password = xxxx

[snom]
username = xxxx
password = xxxx
```

3.  **Restarting the Service**

    To apply the changes, you need to restart the service. Follow the instructions below based on your operating system:

    #### **For Linux**

    1.  Open a terminal and navigate to the PortSIP directory:

        ```bash
        cd /opt/portsip
        ```
    2.  Restart the provisioning service with the following command:

        ```bash
        sudo /sh/bin/pbx_ctl.sh restart -s portsip.provision
        ```

    #### **For Windows**

    1. Open the **Windows Services Manager**.
    2. Locate the service named **PortSIP Auto Provisioning**.
    3. Restart the service by right-clicking on it and selecting **Restart**.
