# Configuring the Automatic Vacuum Schedule

PortSIP PBX allows you to configure the time at which the system automatically **cleans up temporary and expired files**, such as old call recording files. This automated cleanup process helps maintain disk health and system performance.

### Default Vacuum Schedule

* Starting from **PortSIP PBX v16.4.5** and **v22.3**, the default vacuum time for **new installations** is **2:00 AM** (local server time).

### Customizing the Vacuum Time After an Upgrade

If you **upgrade** an existing system to **v16.4.5 or v22.3.x**, you can manually customize the vacuum schedule by following the steps below.

#### Step-by-Step Instructions

1.  Open the configuration file with a text editor such as vi, nano

    ```bash
    /var/lib/portsip/pbx/system.ini
    ```
2. Locate the `vacuum_rate` setting\
   Find the `vacuum_rate` parameter under the **`[apigateway]`** section.
3.  Modify the vacuum schedule\
    Set `vacuum_rate` using a cron-style expression to define the desired execution time.

    Example: Run the vacuum task daily at 2:00 AM

    ```ini
    vacuum_rate = 0 2 * * *
    ```

    > **Important:**\
    > Ensure that spaces in the cron expression are preserved exactly as shown (`0 2 * * *`).\
    > Removing spaces or altering the format may prevent the task from running correctly.
4. Save the file and exit the editor

### Expected Outcome

* The PortSIP PBX system will automatically vacuum temporary and expired files at the configured time each day.
* No service restart is required unless otherwise specified in future releases.

### Notes & Best Practices

* Schedule the vacuum task during **off-peak hours** to minimize disk I/O impact on active calls and recordings.
* Ensure sufficient disk permissions and available storage space to avoid cleanup failures.



