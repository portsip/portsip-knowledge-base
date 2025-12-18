# Managing Virtual Receptionist

### **Recording a Menu Prompt**

Before creating your virtual receptionist, you must decide the menu options you wish to offer the caller and record the announcement. A sample would be, "**Welcome to XYZ. For sales, press 1. For support, press 2 or stay on the line for an operator**".

{% hint style="info" %}
It is recommended to put the number the user should press after the option, i.e. "**For sales, press 1**", rather than “**press 1 for sales**". This is because the user will wait for the desired option and then "**register**" what number to press.
{% endhint %}

{% hint style="info" %}
For the prompt file format, please refer to [What's the file format required for the PortSIP PBX prompt files?](../../faq/what-file-format-is-required-for-portsip-pbx-prompt.md)
{% endhint %}

### **Creating a Virtual Receptionist**

You can create multiple digital receptionists and link them to a particular line.

To create a virtual receptionist:

1. Navigate to the **Advanced Services > Virtual Receptionist** option in the Web Portal menu, then click the **Add** button.
2. On the **General** tab, enter the name and extension number for the virtual receptionist.
3. By default, the PBX uses a system-predefined audio file for the prompt. To use a custom file, click the **Browse** button and select a previously recorded file for the prompt menu. Additionally, you can select the prompt language for the virtual receptionist under **Virtual Receptionist Language**.
4. **Prompt When Call is Transferring**: This is the prompt file that will be played when the call is being transferred after the caller presses a DTMF key.
5. **Virtual Receptionist Language**: This is the language used for the prompt files.
6. **Gap Time Between DTMF Digits (Seconds)**: This is the time the virtual attendant will wait before searching for an account that matches the entered digits. If the account does not exist, the system will play an announcement indicating that the extension does not exist.
7. **DISA PIN**: If you want to set the DISA feature within the virtual receptionist, you can set the PIN for accessing the DISA feature. For more details, please refer to [Direct Inward System Access](direct-inward-system-access-disa.md).
8. **Verify the PIN for DISA**: This indicates whether the virtual receptionist should verify the PIN for DISA.
9. **Night Mode**: When Night Mode is enabled, all calls that reach this virtual receptionist are handled according to the **Destination for Night Mode** settings.
10. **Block Direct Extension Dialing:** Prevents callers from dialing extensions directly. You can block specific extensions individually or define extension ranges.\
    Multiple entries must be separated by semicolons. **Example:** `1001-2001;3000;7000-8000`
11. In the **Destination for Night Mode** section, you can define how incoming calls should be handled when Night Mode is active for a tenant. For detailed configuration instructions, please refer to the [**Night Mode**](../32-night-mode.md) section of this guide.
12. **Timeout (Seconds)**: Specifies how long the virtual receptionist waits for a **DTMF input** from the caller. If no input is received within the specified time, the system automatically executes the **default action**.

    This option is intended for callers who do not understand the menu options or who are using a phone that does not support DTMF input.
13. **Call Failure**: A **Call Failure** occurs when a caller enters a DTMF digit that triggers a call transfer, but the transfer attempt fails.\
    In the **Call Failure** section, you can define how the call should be handled when this situation occurs.

### Menu Options

In the **Menu Options** tab, you specify actions and the extension number or system extension number for each of the numeric keys input by the caller via DTMF. If the action is directed to a specific extension, ring group, call queue, or another virtual receptionist, please also select the target extension number you desire.

#### **User Input**

This option allows you to control when the virtual attendant will start searching for an extension that matches the user’s input. The following behavior is expected when the caller presses the DTMF key 2:

* If the current time is within office hours, the call will be forwarded to extension 2001.
* If it’s outside of office hours, the call will be hung up.
* If it’s during holidays, the call will be forwarded to the voicemail of extension 102.

<figure><img src="../../../.gitbook/assets/vr_menu_options.png" alt=""><figcaption></figcaption></figure>

The virtual attendant will wait until the caller’s digit sequence matches an existing account. Once a match is found, the virtual attendant will initiate a call to that extension. This mechanism is beneficial when accounts of varying lengths are used. However, it might be frustrating for callers who enter a non-existent number, as the virtual attendant will never start the search.

{% hint style="warning" %}
If the caller enters a DTMF digit that does not match any configured DTMF option, the Call Failure rule is not triggered.
{% endhint %}

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

