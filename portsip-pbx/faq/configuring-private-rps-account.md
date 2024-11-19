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

