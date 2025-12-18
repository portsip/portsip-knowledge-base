# Managing Virtual Receptionist

### **Recording a Menu Prompt**

Before creating a virtual receptionist, decide which **menu options** you want to offer callers and record the corresponding **announcement prompt**.

**Example prompt:**

> “Welcome to XYZ. For Sales, press 1. For Support, press 2, or stay on the line for an operator.”

#### Best Practice for Prompt Design

It is recommended to state the **menu option first**, followed by the **DTMF digit**, for example:

> “For Sales, press 1”

rather than:

> “Press 1 for Sales”

This approach allows callers to listen for their desired option first and then register the digit to press, resulting in a better caller experience.

{% hint style="info" %}
For the prompt file format, please refer to [What's the file format required for the PortSIP PBX prompt files?](../../faq/what-file-format-is-required-for-portsip-pbx-prompt.md)
{% endhint %}

***

### Creating a Virtual Receptionist

You can create multiple virtual receptionists and assign them to specific extensions or inbound numbers.

#### To create a virtual receptionist:

1. Navigate to **Advanced Services > Virtual Receptionist** in the Web Portal.
2. Click **Add**.
3. On the **General** section, enter the **Name** and **Extension Number** for the virtual receptionist.

***

### General Settings

#### Prompt File

By default, the PBX uses a **system-predefined audio prompt**.\
To use a custom prompt, click **Browse** and select a previously recorded audio file.

#### Prompt When Call Is Transferring

The audio file played when a call is being transferred after the caller presses a DTMF key.

#### Virtual Receptionist Language

Specifies the language used for the prompt files.

#### Gap Time Between DTMF Digits (Seconds)

Defines how long the virtual receptionist waits before attempting to match the entered digits to an extension or account.\
If no matching account is found, the system plays an announcement indicating that the extension does not exist.

#### DISA PIN

Configures the [**Direct Inward System Access (DISA)**](direct-inward-system-access-disa.md) PIN if DISA is enabled for this virtual receptionist.\
For more information, refer to **Direct Inward System Access**.

#### Verify PIN for DISA

Specifies whether the virtual receptionist should verify the DISA PIN before allowing access.

***

### Call Handling Options

#### Night Mode

When [**Night Mode** ](../32-night-mode.md)is enabled, all calls reaching this virtual receptionist are handled according to the **Destination for Night Mode** settings.

#### Block Direct Extension Dialing

Prevents callers from dialing extensions directly.

* You can block individual extensions or define extension ranges.
* Multiple entries must be separated by semicolons.

**Example:** `1001-2001;3000;7000-8000`

#### Destination for Night Mode

Defines how incoming calls are handled when **Night Mode** is active for the tenant.\
For detailed configuration instructions, refer to the **Night Mode** section of this guide.

***

### Timeout and Failure Handling

#### Timeout (Seconds)

Specifies how long the virtual receptionist waits for a **DTMF input** from the caller.\
If no input is received within the specified time, the system automatically performs the **default action**.

This option is intended for callers who do not understand the menu or who are using phones that do not support DTMF input.

#### Call Failure

A **Call Failure** occurs when a caller enters a DTMF digit that triggers a call transfer, but the transfer attempt fails.

In the **Call Failure** section, you can define how the call should be handled in this situation.

***

### Menu Options

In the **Menu Options** tab, you configure the actions associated with each **DTMF digit** entered by the caller.

For each numeric key, you can define an action such as:

* Forwarding the call to an extension
* Ring group
* Call queue
* Another virtual receptionist
* System extension

If the action targets a specific destination, select the desired extension or system extension number.

***

### User Input and Time-Based Routing

The **User Input** option controls when the virtual receptionist starts searching for a destination that matches the caller’s input.

#### Example Behavior (DTMF = 2)

* **During office hours:**\
  The call is forwarded to extension 2001.
* **Outside office hours:**\
  The call is disconnected.
* **During holidays:**\
  The call is forwarded to the voicemail of extension 102.

This allows flexible, time-based call routing using a single virtual receptionist.

> If the caller enters a DTMF digit that does not match any configured DTMF option, the Call Failure rule is not triggered.

<figure><img src="../../../.gitbook/assets/vr_menu_options.png" alt=""><figcaption></figcaption></figure>

***

#### **Office Hours**

You can define the **office hours** for this DTMF input by clicking the **Office Hours** button. Two options are available:

* **Use Global Office Hours**\
  Applies the default office hours defined at the **tenant level**.
* **Use Specific Office Hours**\
  Allows you to define **custom office hours** for this DTMF input.

By default, each DTMF input follows the global office hours configured for the tenant.

<figure><img src="../../../.gitbook/assets/vr_menu_options_1.png" alt=""><figcaption></figcaption></figure>

#### **Holidays**

Set the holidays for this DTMF input by clicking the **Holidays** button. You can select a holiday list from the tenant scope by clicking the **Select Holidays** button.

### **Direct Destinations**

The Direct Destinations feature is somewhat like a built-in version of the IVR system.

To direct inbound calls to specified extensions, you can use the pre-configured destination fields and link them to pre-recorded announcements and user input options.

Using the sample shown below, the virtual receptionist’s welcome message will be as follows: "**For Sales, press 1. For Support, press 2. For all other inquiries, press 0**." (The user input options are linked to extensions 8003, 8000, and 8001.)

<figure><img src="../../../.gitbook/assets/vr_menu_options_3.png" alt=""><figcaption></figcaption></figure>

***

### Direct Destination vs. IVR Node

For **simple virtual receptionist configurations**, using a **direct destination** is often the most straightforward and effective option.\
For virtual receptionists that require **advanced IVR logic, complex call flows, or integrations**, it is recommended to use an **IVR node**.

***

### Direct Destination Behavior

Once direct destination mappings are configured, the system automatically dials the associated destination when the caller enters the corresponding DTMF input.

**Example:**\
As shown in the screenshot above, when a caller presses **2**, the call is routed to **extension 8001**.

***

### Using the Pound Sign (#) Delay

By appending a **pound sign (#)** to a direct destination (for example, `2#`), the system waits **3 seconds** before dialing the destination.

This delay is especially useful when extension numbers consist of **multiple digits**, such as extensions in the **100 range** (101, 102, etc.).\
The 3-second pause ensures the system collects the **entire user input** (for example, `101`) instead of immediately acting on the first digit (`1`).

***

### User Input Considerations

The **User Input** value can consist of **one or multiple digits**. By default, the system dials a direct destination **immediately after the caller finishes entering the keypad input**.

This behavior can cause conflicts when:

* A direct destination uses a digit that overlaps with the beginning of an extension number
* For example, a direct destination of **1** conflicts with extensions such as **101**, **102**, etc.

In this scenario, the system cannot distinguish whether the caller intends to reach the direct destination or continue dialing an extension.

***

### Best Practices to Avoid Conflicts

To prevent overlap issues:

* Use extension ranges that **do not conflict** with direct destinations, mailbox prefixes, or outbound dialing prefixes
* Extension ranges such as **4xx to 7xx** are commonly recommended

If changing extension assignments is not practical (for example, extension numbers are already printed on business cards), you can use the **timeout mechanism** instead.

By appending a **pound sign (#)** to the direct destination (for example, `1#`), the system waits **3 seconds** before dialing, allowing callers enough time to complete multi-digit extension input.

***

### Destination Extension

The **Destination Extension** can be any valid internal destination, including:

* An extension
* A conference room
* Other internal system numbers

***

#### Why This Version Is Better







For simple virtual receptionist configurations, a direct destination is an excellent choice. However, for virtual receptionists that require advanced IVR development and functionality, the use of an IVR node is advised.

Once the direct destination links are set up, the system will dial the destination number whenever a caller inputs the number associated with it. For instance as in the above screenshot, if a caller presses **2**, the call will be routed to extension **8001**.

By appending a pound sign to the direct destination (e.g., "2#"), the system will pause for 3 seconds before dialing the direct destination. This feature is particularly useful for extension numbers in the 100 range (101, 102, etc.). The 3-second delay ensures that the system processes the caller’s complete input (e.g., 101) rather than just the first digit.

* **User Input:** This number can be a single digit or multiple digits. However, the system dials direct destinations immediately after a user has entered their keypad input. This can cause overlap issues between a direct destination and an extension number. For instance, extensions beginning with '1' would conflict with a direct destination of '1', as the system would be unable to dial the extension number. To avoid this, choose extension numbers that do not overlap with direct destinations or mailbox and outbound call prefixes. The extension range 4xx through 7xx is a good choice.&#x20;
  * If it’s challenging to change the extension assignments (e.g., business cards with extension numbers have already been distributed), a timeout mechanism can be employed. By appending a pound sign to the direct destination (e.g., '1#'), the system will pause for 3 seconds before dialing the destination.
* **Destination Extension**: This number can be either an internal number (e.g., an extension or conference room).

### Allow Callers to Dial a Known Extension Directly

While the virtual receptionist prompt is playing, callers can enter a known **extension number** to be connected immediately. This allows callers who already know their party’s extension to bypass the receptionist menu and reach the intended extension more quickly.

This option is **enabled by default**. To make effective use of this feature, include clear instructions in the voice prompt.

**Example prompt:**

> “Welcome to Company XYZ. If you know your party’s extension number, please enter it now. For Sales, press 1. For Support, press 2.”

### **Sending HTTP Request to WebHook**

When creating a virtual receptionist, the user has three tabs: **Virtual Receptionist**, **Action URL**, and **Outbound Caller ID**. In the **Virtual Receptionist** tab, the user can configure a common Virtual Receptionist and define WebHook and relevant actions in the **Action URL**.

**Action URL** is applied as in the scenario below:

When users dial the pre-configured DTMF key, the Virtual Receptionist will send an HTTP request to a third-party server as defined by the URL and parse the destination extension number in the response message from the third-party server to forward the call to the target extension.

* **Name:** Enter a user-friendly name for the Action URL. This field is mandatory.
* **Type:** Choose the method to trigger the Action URL. PortSIP PBX allows triggering it with the user-inputted DTMF key or caller number. Depending on his request, the user may choose **DTMF** or **Caller Number**. Once **DTMF** is chosen, if the DTMF entered is a replica of the DTMF specified in the **Virtual Receptionis**t tab, PBX will always invalidate settings in **Virtual Receptionist** and handle the call as defined in the **Action URL**.
*   **DTMF match list/ Caller number match list**: Depending on the selection in **Action Type**, the user may specify the **DTMF match number** or **Caller number match list**. Users may enter a semicolon-separated list of numbers at one time, e.g. “101;102;103”. The entered number must be unique and must not be duplicated.

    The match list can also specify a number range, for example, 860000-880000. It’s used for the following scenario: someone calls the virtual receptionist and enters their bank card number. If the number falls in the matched DTMF range, the virtual receptionist will call the action URL to return some values to indicate the next actions.

    The match list also specifies a serial  `*` number, for example, `*****` It’s used to match any 5-digit DTMF that the caller input.

    Once an item of the Action URL is triggered, an HTTP request will be sent to the third-party server. Users may specify the username and password for authentication in the **Credentials for HTTP Basic authentication** section (not mandatory), and choose the method for sending HTTP requests in the POST or GET. Fields **Connection timeout** and **Timeout for waiting for response** are filled to set up the timeout value for communication between the Virtual Receptionist and WebHook server.
* **Request URL:** The WebHook URL to be executed will be entered here when the preset action is triggered. The virtual Receptionist will send an HTTP request to this URL and process the call depending on the HTTP response.&#x20;
* **Additional Headers**: Allows setting the additional HTTP headers when sending requests to the WebHook. For example, if we want to add the key1:value, and key2:value2 headers, enter them as the `key1:value1&key2:value2`.

### HTTP Request Message

PortSIP defines the following parameters for constructing the **HTTP request message** sent to a WebHook.\
The request payload is formatted in **JSON** (for POST requests) or as URL query parameters (for GET requests).

#### Parameters

* **from:** The caller’s number (the extension or number calling the Virtual Receptionist).
* **to:** The callee’s number (the extension number assigned to the Virtual Receptionist).
* **input:** The DTMF digits entered by the caller.
* **from\_name:** The display name of the caller. This field is empty if no display name is available.
* **account\_name:** The name of the Virtual Receptionist.

***

#### Example Scenario

Assume a Virtual Receptionist is configured with the following settings:

* **Number:** 888
* **Name:** Sales
* **Action Name:** Action1
* **Action Type:** DTMF
* **DTMF Match List:** `22;33`
* **HTTP Method:** GET
* **Request URL:**\
  `http://www.appserver.com/dest.php`

***

#### GET Request Example

When **extension 101** (display name **Jason**) calls **888**, the Virtual Receptionist automatically answers the call and plays the configured prompt.

If the caller enters **DTMF 22**, the Virtual Receptionist sends the following **HTTP GET request** to the WebHook endpoint:

```
http://www.appserver.com/dest.php?from=101&to=888&input=22&from_name=Jason&account_name=Sales
```

***

#### POST Request Example

If **POST** is selected as the HTTP method, the Virtual Receptionist sends the request in **JSON format** in the HTTP request body:

```json
{
  "from": "101",
  "to": "888",
  "input": "22",
  "from_name": "Jason",
  "account_name": "Sales"
}
```

### HTTP Response Message

PortSIP PBX defines the following **HTTP response format** for WebHook requests sent by the **Virtual Receptionist**.\
The response is expected in **JSON format** and determines how the Virtual Receptionist should handle the call.

#### Response Parameters

* **status\_code**\
  The HTTP status code returned by the WebHook endpoint.\
  A value of **200** indicates that the request was processed successfully. Any other value is treated as a failure.
* **action**\
  Specifies the action the Virtual Receptionist should take. Supported values are:
  * **call** – Forward the call to the number specified in `destination`
  * **hangup** – Immediately terminate the call
  * **repeat** – Replay the prompt message
* **destination**\
  The target callee number. This parameter is **only valid when `action` is set to `call`**.\
  It is ignored for all other actions.

***

#### Example Response

```json
{
  "status_code": 200,
  "action": "call",
  "destination": "222"
}
```

***

#### Call Handling Behavior

When the Virtual Receptionist receives the response above, it forwards the call to **extension 222**, as specified in the `destination` field of the JSON response.

