# How to Adjust the REST API Rate Limit?

By default, the PortSIP PBX versions v16.0.x and v16.1.x limit the REST API to 1,000 requests per minute. Starting from version v16.2.0, this rate has been increased to 5,000 requests per minute.

If you wish to modify this rate, please follow the steps below:

1. Open the file `var/lib/portsip/pbx/system.ini`. For Windows, this file is located at `C:/ProgramData/PortSIP/pbx/log`.
2. In the `[apigateway]` section, locate the `access_limit` key.
3. To change the limitation, simply write the new value, for example, `2000`.
4. Save the changes.

To apply these changes, you’ll need to restart the media server:

* **Windows**: Open the Windows Service Manager and restart the PortSIP Gateway Server service.
* **Linux**: Execute the following commands:

```
cd /opt/portsip && /bin/sh pbx_ctl.sh restart -s portsip.gateway
```

