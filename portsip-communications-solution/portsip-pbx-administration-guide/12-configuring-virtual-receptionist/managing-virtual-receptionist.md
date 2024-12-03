# Managing Virtual Receptionist

## **Recording a Menu Prompt**

Before creating your virtual receptionist, you must decide the menu options you wish to offer the caller and record the announcement. A sample would be, "**Welcome to XYZ. For sales, press 1. For support, press 2 or stay on the line for an operator**".

{% hint style="info" %}
It is recommended to put the number the user should press after the option, i.e. "**For sales, press 1**", rather than “**press 1 for sales**". This is because the user will wait for the desired option and then "**register**" what number to press.
{% endhint %}

{% hint style="info" %}
For the prompt file format, please refer to [What's the file format required for the PortSIP PBX prompt files?](../../faq/what-file-format-is-required-for-portsip-pbx-prompt.md)
{% endhint %}

## **Creating a Virtual Receptionist**

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
9. **Timeout (Seconds)**: This allows you to specify how long the virtual receptionist should wait for a DTMF input. If no input is received, it will automatically perform the default action. This is for callers who do not understand the menu or who do not have a DTMF-capable phone.
10. **Call Failure**: If the caller enters a DTMF value or key that IVR will refers the call to it, if the refer fails then action fails. In this Call Failure section, you can define how the call should be handled in such cases.

## Menu Options

In the **Menu Options** tab, you specify actions and the extension number or system extension number for each of the numeric keys input by the caller via DTMF. If the action is directed to a specific extension, ring group, call queue, or another virtual receptionist, please also select the target extension number you desire.

### **User Input**

This option allows you to control when the virtual attendant will start searching for an extension that matches the user’s input. The following behavior is expected when the caller presses the DTMF key 2:

* If the current time is within office hours, the call will be forwarded to extension 2001.
* If it’s outside of office hours, the call will be hung up.
* If it’s during holidays, the call will be forwarded to the voicemail of extension 102.

<figure><img src="../../../.gitbook/assets/vr_menu_options.png" alt=""><figcaption></figcaption></figure>

The virtual attendant will wait until the caller’s digit sequence matches an existing account. Once a match is found, the virtual attendant will initiate a call to that extension. This mechanism is beneficial when accounts of varying lengths are used. However, it might be frustrating for callers who enter a non-existing number, as the virtual attendant will never start the search.

{% hint style="warning" %}
If the caller presses the DTMF tones that do not match the single pre-configured DTMF tone, it will not to triggered the failure rule.
{% endhint %}

### **Office Hours**

Define the office hours for this DTMF input by clicking the **Office Hours** button, you have two options:

* **Use the default Global Office Hours** from the tenant scope.
* Specify custom office hours by selecting the **Use specific Office Hours** option.

By default, a DTMF input will follow the office hours set at the tenant level.

<figure><img src="../../../.gitbook/assets/vr_menu_options_1.png" alt=""><figcaption></figcaption></figure>

### **Holidays**

Set the holidays for this DTMF input by clicking the **Holidays** button. You can select a holiday list from the tenant scope by clicking the **Select Holidays** button.

## **Direct Destinations**

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

## Allowing Callers to Dial a Known Extension Directly

Whilst a virtual receptionist prompt is playing, a caller can enter the extension number directly to be connected to an extension immediately. This allows callers who know their party’s extension to avoid going through a receptionist. This option is enabled by default. If you wish to make use of this feature, simply instruct your callers by explaining this in the voice prompt.

For example, _“W**elcome to Company XYZ. If you know your party's extension number, you may enter it now, otherwise, for sales press 1. For support press 2**”_.

## **Sending HTTP Request to WebHook**

When creating a virtual receptionist, the user has three tabs: **Virtual Receptionist**, **Action URL**, and **Outbound Caller ID**. In the **Virtual Receptionist** tab, the user can configure a common Virtual Receptionist and define WebHook and relevant actions in the **Action URL**.

**Action URL** is applied as in the below scenario:

When users dial the pre-configured DTMF key, the Virtual Receptionist will send an HTTP request to a third-party server as defined by the URL and parse the destination extension number in the response message from the third-party server to forward the call to the target extension.

* **Name:** Enter a user-friendly name for the Action URL. This field is mandatory.
* **Type:** Choose the method to trigger the Action URL. PortSIP PBX allows triggering it with the user-inputted DTMF key or caller number. Depending on his request, the user may choose **DTMF** or **Caller Number**. Once **DTMF** is chosen, if the DTMF entered is a replica of the DTMF specified in the **Virtual Receptionis**t tab, PBX will always invalidate settings in **Virtual Receptionist** and handle the call as defined in the **Action URL**.
*   **DTMF match list/ Caller number match list**: Depending on the selection in **Action Type**, the user may specify the **DTMF match number** or **Caller number match list**. Users may enter a semicolon-separated list of numbers at one time, e.g. “101;102;103”. The entered number must be unique and must not be duplicated.

    The match list also can specify a number range, for example, 860000-880000, it’s used for the below scenario: someone calls the virtual receptionist and enters his bank card number. If the number falls in the matched DTMF range, the virtual receptionist will call the action URL to return some values to indicate the next actions.

    Once an item of the Action URL is triggered, an HTTP request will be sent to the third-party server. Users may specify the username and password for authentication in the **Credentials for HTTP Basic authentication** section (not mandatory), and choose the method for sending HTTP requests in the POST or GET. Fields **Connection timeout** and **Timeout for waiting for response** are filled to set up the timeout value for communication between the Virtual Receptionist and WebHook server.
* **Request URL:** The WebHook URL to be executed will be entered here when the preset action is triggered. The virtual Receptionist will send an HTTP request to this URL and process the call depending on the HTTP response.&#x20;
* **Additional Headers**: Allows to set the additional HTTP headers when sending requests to the WebHook. For example, if we want to add the key1:value, and key2:value2 headers, enter them as the `key1:value1&key2:value2`.

## **HTTP Request Message**

PortSIP has defined the following parameters to form the HTTP request message to WebHook in JSON format:

* **from**: Caller’s number, i.e. the caller number who’s calling to Virtual Receptionist
* **to**: Callee’s number, i.e. the extension number for the Virtual Receptionist
* **input**: DTMF inputted by the user
* **from\_name**: The display name of the caller. It will be empty if no value is provided
* **account\_name**: Name of the Virtual Receptionist

Assuming that we have created a Virtual Receptionist with the number **888** and named **Sales**, and its **Action URL** is defined as follows:

* **Name**: Action1
* **Action Type**: DTMF
* **DTMF match list**: 22;33
* **HTTP method**: GET
* **Request URL**: http://www.appserver.com/dest.php

When extension 101 (display name Jason) calls 888, Virtual Receptionist 888 will auto-answer the call and play a prompt to the caller. As extension 101 dials DTMF 22, the Virtual Receptionist will send the below HTTP request in the GET method to: `http://www.appserver.com/dest.php?from=101&to=888&input=22&from_name=Jason&account_name=Sales`

If POST is chosen for the HTTP method, the Virtual Receptionist will send the below HTTP request in JSON format by means of POST:

```json
{
"from" : "101",
"to" : "888",
"input": "22",
"from_name" : "Jason",
"account_name" : "Sales"
}
```

## **HTTP Response Message**

PortSIP PBX has defined the following response to HTTP requests sent by Virtual Receptionist as follows:

* **status\_code:** 200 or other possible status code, of which 200 represents a successful request and others refers to failure.
* **action**: Values including "**call**", "**hangup**"**,** and "**repeat**" indicate the action to be taken by Virtual Receptionist.
  * call – To forward the call to the number as defined in "**destination**"
  * hangup – To hang up the call directly
  * repeat – To repeat the prompt message
* **destination**: The destination callee number. It’s valid only if the value for "**action**" is set as "**call**"; otherwise it will be ignored.

```json
{
"status_code" : 200,
"action" : "call",
"destination" : "222"
}
```

Once the Virtual Receptionist has received the response as above, it will forward the call to extension **222** which is indicated in the JSON response.

