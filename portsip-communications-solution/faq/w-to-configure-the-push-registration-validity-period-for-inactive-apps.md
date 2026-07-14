# w to Configure the Push Registration Validity Period for Inactive Apps

You can configure how long the push registration information for an app remains valid when the app has not logged in to PortSIP PBX.

If an app does not log in for longer than the configured number of days, its push registration information is considered expired and will no longer be used for push notifications.

### Steps

1.  Open the following configuration file:

    ```bash
    /var/lib/portsip/pbx/system.ini
    ```
2.  In the `[global]` section, add the following setting:

    ```ini
    push_valid_days = 7
    ```

    In this example, if an app has not logged in to PortSIP PBX for more than **7 days**, its push registration information expires.

    The supported value range is **1 to 365 days**.
3. Save the configuration file.
4.  Restart the PortSIP Call Manager container for the change to take effect:

    ```bash
    sudo docker restart portsip.callmanager
    ```

### Expected Result

After the Call Manager restarts, PortSIP PBX uses the configured `push_valid_days` value to determine when the push registration information for an inactive app expires.

