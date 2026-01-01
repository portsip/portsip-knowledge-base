# Click to Call

**Click-to-Call** is a feature that allows users to initiate a phone call with a single click. It leverages **Voice over Internet Protocol (VoIP)** technology to establish a call between two parties and is also commonly known as **click-to-dial** or **one-click calling**.

Originally designed for call centers, click-to-call helped streamline outbound calling, reduce dialing errors, and improve agent efficiency. Over time, it has evolved to support modern technologies such as **WebRTC** and **browser-based integrations**, making it widely accessible for everyday business communications.

***

### Setting Up Click-to-Call

#### Desktop-Based Calling

Follow these steps to enable click-to-call on your desktop:

1. **Download and install** the **PortSIP ONE** app on your desktop.
2. **Sign in** to the PortSIP ONE app.
3. While browsing the web, click a **telephone number** (for example, in Google Chrome, Microsoft Edge, or Mozilla Firefox).
4.  When prompted, choose an application to place the call.

    You may select the option:\
    &#xNAN;**“Always allow this site to open links of this type in the associated app.”**

    This ensures that phone numbers clicked on that website (for example, [_www.portsip.com_](http://www.portsip.com)) automatically open in your selected calling app.

<figure><img src="../../.gitbook/assets/portsip-one-tel-call-1.png" alt=""><figcaption></figcaption></figure>

***

#### Setting PortSIP ONE as the Default Calling App (Windows)

After clicking **Open** or **Pick an app** in the previous step, the **Windows Default Apps** settings window will appear.

To set PortSIP ONE as the default app for telephone numbers:

1. Select **PortSIP ONE** from the list of available applications.
2. Check **Always use this app**.
3. Click **OK** to confirm.

Once completed, Windows will assign **PortSIP ONE** as the default application for dialing telephone numbers.

<figure><img src="../../.gitbook/assets/portsip-one-tel-call-2.png" alt="" width="295"><figcaption></figcaption></figure>

***

#### Expected Behavior

* Clicking a phone number on a web page automatically launches **PortSIP ONE** and places the call.
* If PortSIP ONE is already running and signed in, the call is placed immediately.
* If PortSIP ONE is not running, it will launch automatically. After you sign in, the phone number will be dialed without further action.

***

> **Note**\
> Click-to-call behavior depends on your operating system’s default app settings and browser permissions. Administrator policies may also affect call routing and outbound caller ID behavior.

***

### Set PortSIP ONE as the Default App for Phone Numbers (Windows)

If PortSIP ONE is not currently set as your default phone application, follow the steps below to configure it.

#### To set the default app for phone numbers:

1. Click the **Start** menu and open **Settings**.
2. Select **Apps**, then click **Default apps**.
3. Scroll down and select **Default apps by protocol**.
4. Locate **TEL (URL: Tel Protocol)**.
5. Click **Choose a default**.
6. Select **PortSIP ONE** from the list of available applications.

***

> **Expected Result**\
> Phone numbers (tel: links) clicked in web browsers or other applications will automatically open in **PortSIP ONE** and initiate a call.

<figure><img src="../../.gitbook/assets/portsip-one-tel-call-3.png" alt="" width="563"><figcaption></figcaption></figure>



