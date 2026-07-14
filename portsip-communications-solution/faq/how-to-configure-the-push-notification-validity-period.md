# How to Configure the Push Notification Validity Period

By default, PortSIP PBX uses a predefined validity period for push notifications. You can customize this period by updating the PBX system configuration file.

### Steps

1.  Open the following configuration file:

    ```bash
    /var/lib/portsip/pbx/system.ini
    ```
2.  In the `[global]` section, add the following setting:

    ```ini
    push_valid_days = 7
    ```

    In this example, the value `7` means that a push notification remains valid for **7 days**.

    The supported value range is **1 to 365 days**.
3. Save the configuration file.
4.  Restart the PortSIP Call Manager container for the change to take effect:

    ```bash
    sudo docker restart portsip.callmanager
    ```

### Expected Result

After the Call Manager restarts, PortSIP PBX uses the configured `push_valid_days` value as the validity period for push notifications.



