# Upgrading High Availability Installation

{% hint style="info" %}
Before upgrading the PBX HA, please consult with PortSIP support to ensure the versions are compatible.
{% endhint %}

Please follow the below steps to upgrade your current PBX HA.

## Back up data

Before upgrading, please back up the PBX data.

1. Stop the PBX service

Perform the below command only on the **pbx01.**

```sh
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh stop -s pbx
```

2. Check the current master node

Perform the below command only on the **pbx01.**

```sh
cd /opt/portsip-pbx-ha-guide && sudo /bin/bash ha_ctl.sh master
```

For example, the below output indicates the current master node is **pbx01**.&#x20;

<figure><img src="../../../.gitbook/assets/ubuntu-ha-27.png" alt=""><figcaption></figcaption></figure>

3. Back up data

Log in to the current master node, and back up the PBX data directory: `/var/lib/portsip`

## Download and update resources

Perform the below command only on the **pbx01.**

```sh
  cd /opt && rm -rf portsip-pbx-ha-guide-16-online.tar.gz && \
  sudo wget -N https://www.portsip.com/downloads/ha/v16/portsip-pbx-ha-guide-16-online.tar.gz \
  && sudo tar xf portsip-pbx-ha-guide-16-online.tar.gz
```

## **Update**

Use the new version image of PortSIP PBX to update the PBX.

{% hint style="danger" %}
Please contact PortSIP support to obtain the **\<PortSIP PBX new version image>** before upgrading.
{% endhint %}

Perform the below command only on the **pbx01.**

<pre class="language-sh"><code class="lang-sh"><strong>cd /opt/portsip-pbx-ha-guide/ &#x26;&#x26; /bin/bash update.sh &#x3C;PortSIP PBX new version image>
</strong></code></pre>

For example:

```sh
cd /opt/portsip-pbx-ha-guide/ && /bin/bash update.sh portsip/pbx:16.4.4.2814-release
```

