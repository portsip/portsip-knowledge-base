# Managing PortSIP Docker Instances

Run the commands in this guide from `/opt/portsip` on the server hosting the relevant service:

```bash
cd /opt/portsip
```

***

### Managing the PortSIP PBX Instance

After installing PortSIP PBX, use `pbx_ctl.sh` to manage its Docker container.

#### Show PBX status

```bash
sudo /bin/sh pbx_ctl.sh status
```

#### Start the PBX

```bash
sudo /bin/sh pbx_ctl.sh start
```

#### Stop the PBX

```bash
sudo /bin/sh pbx_ctl.sh stop
```

#### Restart the PBX

```bash
sudo /bin/sh pbx_ctl.sh restart
```

#### Remove the PBX container

```bash
sudo /bin/sh pbx_ctl.sh rm
```

**Removing the PBX container does not delete PBX data.**

***

### Managing the PortSIP IM Service Instance

After installing PortSIP IM Service, use `im_ctl.sh` to manage its Docker container.

#### Show IM Service status

```bash
sudo /bin/sh im_ctl.sh status
```

#### Start the IM Service

```bash
sudo /bin/sh im_ctl.sh start
```

#### Stop the IM Service

```bash
sudo /bin/sh im_ctl.sh stop
```

#### Restart the IM Service

```bash
sudo /bin/sh im_ctl.sh restart
```

#### Remove the IM Service container

```bash
sudo /bin/sh im_ctl.sh rm
```

Removing the IM Service container does not delete PBX data.

***

### Managing a PortSIP Data Flow Service Instance

Use `dataflow_ctl.sh` on the Data Flow server. The commands depend on the installed Data Flow version.

#### Versions earlier than v22.8.0

**Show Data Flow Service status**

```bash
sudo /bin/sh dataflow_ctl.sh status
```

**Start the Data Flow Service**

```bash
sudo /bin/sh dataflow_ctl.sh start
```

**Stop the Data Flow Service**

```bash
sudo /bin/sh dataflow_ctl.sh stop
```

**Restart the Data Flow Service**

```bash
sudo /bin/sh dataflow_ctl.sh restart
```

**Remove the Data Flow Service container**

```bash
sudo /bin/sh dataflow_ctl.sh rm
```

#### v22.8.0 and later

Data Flow v22.8.0 and later can host separate instances for multiple PBXs. Specify the target PBX with `-x`. Use **the same PBX IP address that you entered when installing that Data Flow instance**. Replace `<PBX_IP>` in the commands below with that address.

**Show Data Flow Service status**

```bash
sudo /bin/sh dataflow_ctl.sh status -x <PBX_IP>
```

**Start the Data Flow Service**

```bash
sudo /bin/sh dataflow_ctl.sh start -x <PBX_IP>
```

**Stop the Data Flow Service**

```bash
sudo /bin/sh dataflow_ctl.sh stop -x <PBX_IP>
```

**Restart the Data Flow Service**

```bash
sudo /bin/sh dataflow_ctl.sh restart -x <PBX_IP>
```

**Remove the Data Flow Service Instance**

```bash
sudo /bin/sh dataflow_ctl.sh rm -x <PBX_IP>
```

For example, if the Data Flow server hosts instances for three PBXs, a command with `-x` targets only the instance associated with the specified PBX IP address. Removing a Data Flow container does not delete PBX data.

> **Note:** `<PBX_IP>` is a placeholder. Substitute the actual IP address before running a command; do not type the angle brackets.

***

### Deleting Data Flow Data

The following file deletions are permanent. Confirm the PBX IP address, `instance_id`, and storage paths before running them.

#### Delete one PBX's Data Flow instance data (v22.8.0 and later)

1. On the **PBX server**, open `/var/lib/portsip/pbx/system.ini` and copy the `instance_id` value from the `[global]` section.
2.  On the **Data Flow server**, remove the container for that PBX. Replace `<PBX_IP>` with the PBX IP address used when the Data Flow instance was installed:

    ```bash
    cd /opt/portsip
    sudo /bin/sh dataflow_ctl.sh rm -x <PBX_IP>
    ```
3.  On the **Data Flow server**, delete that instance's Data Flow directory. Replace `<PBX_INSTANCE_ID>` with the exact `instance_id` copied in step 1:

    ```bash
    sudo rm -rf -- "/var/lib/portsip/dataflow/<PBX_INSTANCE_ID>"
    ```

This deletes the specified instance directory. Do not delete the shared ClickHouse directory when removing only one PBX's Data Flow instance.

#### Delete all Data Flow data

After removing all Data Flow instances on the server, run the following commands **on the Data Flow server** to delete the default Data Flow and ClickHouse data directories:

```bash
sudo rm -rf -- /var/lib/portsip/dataflow
sudo rm -rf -- /var/lib/portsip/clickhouse
```

**Note:** If you specified custom data storage paths during the Data Flow Service installation, replace `/var/lib/portsip/dataflow` and `/var/lib/portsip/clickhouse` in the commands above with the corresponding paths you specified.
