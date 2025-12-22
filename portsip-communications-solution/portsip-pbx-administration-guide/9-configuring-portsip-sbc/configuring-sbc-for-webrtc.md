# Configuring PortSIP SBC for WebRTC

After successfully installing the **PortSIP SBC**, you can now configure it to enable the **WebRTC** feature.

Choose the appropriate configuration method based on your deployment model.

***

### Configure PortSIP SBC on the Same Server as PortSIP PBX

#### Step 1: Prepare the SSL Certificate

Prepare the SSL certificate as described in the guide [Certificates for TLS / HTTPS / WebRTC](../certificates-for-tls-https-webrtc/).

You should have the following files available:

* `portsip.pem`
* `portsip.key`

***

#### Step 2: Sign In to the SBC Web Portal

1. Open the following URL in your browser: https://66.175.221.120:8883
2. Log in using the default credentials:
   * **Username:** `admin`
   * **Password:** `admin`
3. Ignore the browser SSL warning and continue.

***

#### Step 3: Upload the TLS Certificate

1. Navigate to **Settings > TLS Certificates**.
2. Click **Add**.
3. Fill in the fields:
   * **Description:** `SBC Host Name` (example)
   * **TLS Domain:** `uc.portsip.cc`
4. Open `portsip.pem` in a text editor and paste its contents into **Certificate Context**.
5. Open `portsip.key` and paste its contents into **Private Key** Context.
6. Enable **This is SBC Web Domain Certificate**.
7. Click **OK** to save.

***

#### Step 4: Configure Network Settings

1. Navigate to **Settings > Network**.
2. Configure the following values:
   * **Web Domain:** `uc.portsip.cc`
   * **Private IPv4:** `192.168.1.72`
   * **Public IPv4:** `66.175.221.120`
3. Keep **Create default transports automatically** enabled.

The SBC will automatically create the following transports:

* **TCP 5069** – Communication with PBX
* **TLS 5067** – Microsoft Teams Direct Routing
* **WSS 5065** – WebRTC services
* **UDP 5066** – Standard SIP services

> Disabling automatic transport creation is not recommended unless you are performing a custom configuration.

4. Click **OK**.\
   The SBC will restart automatically and log you out.

***

#### Step 5: Restart the PBX

Execute the command below to restart the SBC:

```bash
cd /opt/portsip
sudo /bin/sh sbc_ctl.sh restart
```

***

#### Step 6: Generate the SBC Access Token in PBX

1. Sign in to the PBX Web Portal: https://uc.portsip.cc:8887
2. Navigate to **Servers** > **SBC**.
3. Click **Generate** to create an access token.
4. Click **Copy** to copy the token.

***

#### Step 7: Configure PBX Connection in SBC

1. Sign in to the SBC Web Portal: https://uc.portsip.cc:8883
2. Navigate to **Settings > PBX**.
3. Configure the following:
   * **PBX Access Token:** Paste the copied token
   * **PBX IPv4 Address:** `192.168.1.72`
     * _(If PBX runs in HA mode, enter the PBX Virtual IP)_
   * **Prefer Transport:** `TCP`
   * **PBX Port:** `5063`
4. Click **OK** to save.

***

#### Step 8: Access the WebRTC Client

Open the following URL in your browser: https://uc.portsip.cc:10443/webrtc, the WebRTC client will launch.\
Scan the user’s QR code to register with the PBX and place or receive calls.

***

### Configure PortSIP SBC on a Separate Server

Follow these steps if the PortSIP SBC is installed on a separate server from the PortSIP PBX.

#### Step 1: Prepare the SSL Certificate

Prepare the SSL certificate as described in the guide [Certificates for TLS / HTTPS / WebRTC](../certificates-for-tls-https-webrtc/).

You should have the following files ready:

* `portsip.pem`
* `portsip.key`

A **trusted wildcard SSL certificate** (not self-signed) must be installed for the domain: `portsip.cc`

***

#### Step 2: Sign In to the SBC Web Portal

1. Open the following URL in your browser: https://66.175.221.120:8883
2. Log in using the default credentials:
   * **Username:** `admin`
   * **Password:** `admin`
3. Ignore the browser SSL certificate warning and continue.

***

#### Step 3: Upload the TLS Certificate

1. Navigate to **Settings > TLS Certificates**.
2. Click **Add**.
3. Enter the following values:
   * **Description:** `SBC Host Name` (example)
   * **TLS Domain:** `sbc.portsip.cc`
4. Open the `portsip.pem` file in a text editor and copy its contents into the **Certificate Context** field.
5. Open the `portsip.key` file and copy its contents into the **Private Key Context** field.
6. Enable **This is SBC Web Domain Certificate**.
7. Click **OK** to save the certificate.

***

#### Step 4: Configure Network Settings

1. Navigate to **Settings > Network**.
2. Configure the following fields:
   * **Web Domain:** `sbc.portsip.cc`
   * **Private IPv4:** `192.168.1.73`
   * **Public IPv4:** `66.175.221.120`
3. Ensure **Create default transports automatically** is enabled.

When this option is enabled, the SBC automatically creates the following transports:

* **TCP 5069** – Communication with the PBX
* **TLS 5067** – Microsoft Teams Direct Routing
* **WSS 5065** – WebRTC services
* **UDP 5066** – Standard SIP services

> Turning off automatic transport creation is **not recommended** unless you are performing an advanced custom configuration.

4. Click **OK**.

The SBC will **restart automatically** and log you out.

***

#### Step 5: Restart the PBX

After the SBC restarts, restart the PBX to ensure proper connectivity.

```bash
cd /opt/portsip
sudo /bin/sh sbc_ctl.sh restart
```

***

#### Step 6: Generate the SBC Access Token in the PBX

1. Sign in to the PBX Web Portal: https://uc.portsip.cc:8887
2. Navigate to **Servers > SBC**.
3. Click **Generate** to create an SBC access token.
4. Click **Copy** to copy the generated token.

***

#### Step 7: Configure PBX Connection in the SBC

1. Sign in to the SBC Web Portal: https://sbc.portsip.cc:8883
2. Navigate to **Settings > PBX**.
3. Configure the following fields:
   * **PBX Access Token:** Paste the copied token
   * **PBX IPv4 Address:** `192.168.1.72`
     * _(If the PBX is deployed in HA mode, enter the PBX **Virtual IP** instead)_
   * **Prefer Transport:** `TCP`
   * **PBX Port:** `5063`
4. Click **OK** to save the configuration.

***

#### Step 8: Access the WebRTC Client

Open the following URL in your browser: https://sbc.portsip.cc:10443/webrtc, the WebRTC client will launch.\
Scan the user’s QR code to register with the PBX and make or receive calls.

***

#### Step 9: Add the SBC IP Address to the PBX Whitelist

To prevent the PBX from rate-limiting requests from the SBC, you must whitelist the SBC IP address.

<figure><img src="../../../.gitbook/assets/sbc_whitelist.png" alt=""><figcaption></figcaption></figure>



1. Sign in to the **PBX Web Portal** as a **System Administrator**.
2. Navigate to **IP Blacklist**.
3. Click **Add**.
4. Enter the **SBC IP address** (`192.168.1.73` or the public IP if applicable).
5. Set a **long expiration date**.
6. Save the configuration.

***

### Check Open Firewall Ports

The following commands can be used to verify which firewall ports are currently open on the server hosting the PortSIP SBC.

```sh
sudo firewall-cmd --info-service=portsip-sbc
```



