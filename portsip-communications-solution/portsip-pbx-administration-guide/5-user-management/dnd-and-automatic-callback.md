# DND and Automatic Callback

### Do Not Disturb (DND)

Do Not Disturb (DND) allows a user to silence incoming call notifications when they need to focus or avoid interruptions.\
When DND is enabled, incoming calls are **sent directly to voicemail**.

#### Enable or Disable DND

You can activate or deactivate DND using either of the following methods:

* **Feature Access Codes (FAC)**
  * Dial **`*78`** from a client app or IP phone to **enable DND**
  * Dial **`*79`** to **disable DND**
* **PBX Web Portal**
  1. Go to **Call Manager > Users**
  2. Select the user and click **Edit**
  3. On the **Extension** page, toggle **Do Not Disturb** on or off

When DND is enabled, a **DND icon** appears next to the user in the user list to indicate the current status.

***

### Automatic Callback (ACB)

Automatic Callback (ACB) allows users to monitor a busy extension and automatically place a call when that extension becomes idle.

When a user calls another extension that is busy and a valid ACB condition is met, the caller hears a prompt asking whether they want to monitor the line and receive a callback when it becomes available.

#### Enable or Disable Automatic Callback

Users can activate or deactivate ACB using either of the following methods:

* **Feature Access Codes (FAC)**
  * Dial **`*33`** from a client app or IP phone to **enable ACB**
  * Dial **`*43`** to **disable ACB**
* **PBX Web Portal**
  1. Go to **Call Manager > Users**
  2. Select the user and click **Edit**
  3. On the **Extension** page, toggle **Automatic Callback** on or off

When ACB is enabled, an **ACB icon** appears next to the user in the user list.

***

#### Automatic Callback Call Behavior

Automatic Callback is an **outgoing call feature** that works between users within the **same tenant**.

The call flow is as follows:

1. User A places a call to User B.
2. User B is busy.
3. User A activates Automatic Callback.
4. The PBX monitors User B’s line.
5. When User B becomes idle, the PBX automatically places a **new call** to User B.

> **Important**
>
> * The caller does **not** need to redial the number.
> * The callback attempt is treated as a **new originating call**.
> * For the callback to succeed, **both users must be available** at the time the callback is initiated.

***

#### Operating Parameters

Automatic Callback behavior is controlled by tenant-level parameters.

To configure these settings:

1. Go to **Advanced Services > Automatic Callback**

The following parameters are available:

* **ACB Service Extension Number**\
  The default extension number used by the ACB service.\
  **Recommendation:** Do not change this value.
* **Monitor Minutes**\
  The maximum time (in minutes) the PBX will monitor a busy line while waiting for it to become idle.\
  &#xNAN;_&#x44;efault: 30 minutes_
* **Retry Originator Minutes**\
  The time (in minutes) the PBX waits before retrying a busy ACB originator (the user who requested the callback).\
  &#xNAN;_&#x44;efault: 5 minutes_
* **Prompt Language**\
  Specifies the language used for ACB audio prompts.

<figure><img src="../../../.gitbook/assets/ACB.png" alt=""><figcaption></figcaption></figure>



