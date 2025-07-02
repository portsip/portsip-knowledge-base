# Configure Files Vacuum Time

PortSIP PBX supports configuring the time when the system automatically vacuums temporary and expired files (such as old recording files).

Starting from **v16.4.5** and the upcoming **v22.3** (not yet released), the default vacuum time is set to **2:00 AM** for a new installation.

If you upgrade to **v16.4.5** or **v22.2.3**, please follow the steps below to customize the vacuum time:

**For v16.4.5**

1. Open the file:\
   `/var/lib/portsip/pbx/system.ini`
2. Locate the `vacuum_rate` setting.
3.  Modify the line to set your desired time under `apigateway` section. For example, to run at 2:00 AM. Note, please keep the space in the `0 2 * * *`

    ```
    vacuum_rate = 0 2 * * *
    ```
4. Save the changes.
