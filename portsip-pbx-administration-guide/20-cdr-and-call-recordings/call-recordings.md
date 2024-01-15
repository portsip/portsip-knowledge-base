# Call Recordings

Businesses today need to be able to communicate with their clients quickly and efficiently. Your business phone system must have the call recording option to achieve this. Once call recording is activated, you can access received calls and have a copy of the conversation aside from knowing who called. The functionality of an automatic call recorder helps businesses maintain high-quality service and train new agents, investigate disputes between company personnel--and more. Discover how PorttSIP PBX can help you improve your business.

## Call Recording Management

The tenant administrator can select the menu **Call Statistics > Call Recordings** to view all recording files. Double-click the recording record, the recording file can be downloaded or played.

<figure><img src="../../.gitbook/assets/call-recording-1.png" alt=""><figcaption></figcaption></figure>

Click the **Action** button will pop-ups the menu, allowing you to navigate to the CDR of this call.

<figure><img src="../../.gitbook/assets/call-recording-2.png" alt=""><figcaption></figcaption></figure>

PortSIP PBX allows you to control the call recording in two levels.

### User Level

To enable/disable call recording for a user, follow these steps: Click the “**Call Manager**” menu, then click “**Users**”. Double-click the user you want to modify, and in the “**Extension**” tab, you can turn on/off the “**Record Audio Calls**”, and “**Record Video Calls**” options.

If “**Record Audio Calls**” is enabled, the call of this user will be recorded as a WAV file, even if the call is a video call. If “**Record Video Calls**” is enabled, the call of this user will be recorded as an MP4 file if the call is a video call.

### Tenant Level

The tenant administrator has the ability to enable/disable the call recording for all users of this tenant.

Click the left menu "**Company**", on the "**General**" page, under the "**Options**" section, you can turn on/off the  "**Enable extension audio recording**" or "**Enable extension video recording**" options.

If “**Enable extension audio recording**” is enabled, a user's call will be recorded as a WAV file, even if the call is a video call. If “**Enable extension video recording**” is enabled, a user's call will be recorded as an MP4 file if the call is a video call.

### Automatically stop recording if the call between two external numbers

In some countries, due to privacy and security concerns, the law stipulates that when a call is made between two external numbers, it should not be recorded.

Consider the following scenario: The client calls the contact center from the trunk, and the agent answers, the call is starting to record, and after a while of conversation, the agent transfers the client’s call to another mobile phone number. At this point, the call recording should stop.

PortSIP PBX provides corresponding features to support this regulation. The system administrator can click the menu **Advanced > Settings**, on the **General** page, and turn off the “**Record the call between external numbers**” option so that when the call is transferred to be made between two external numbers, the call recording will automatically stop.

### Pause/Resume Recording

PortSIP PBX allows Pause/Resume of the call recording during the call.

* Dials FAC `*48` will pause the call recording during the call
* Dials FAC `*49` will resume the call recording during the call

The client APP or IP Phone can also pause/resume the call recording by sending the out-of-dialog SIP message to the PBX, the message should be as below:

<pre><code>MESSAGE sip:102@test.io SIP/2.0
Via: SIP/2.0/UDP 192.168.0.16:5568;branch=z9hG4bK-524287-1---a67ad51a25df052f;rport
Max-Forwards: 70
To: &#x3C;sip:102@test.io>
From: &#x3C;sip:101@test.io>;tag=fa2d6f1d
Call-ID: SFNH1yZ5c4LrG70_Tlyxgg..
CSeq: 3 MESSAGE
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY, MESSAGE, REGISTER, SUBSCRIBE, INFO, PUBLISH
Content-Type: application/x-media-control+json
Proxy-Authorization: Digest username="101",realm="test.io",nonce="13323933303:c8b6b255488669affb8a3e657188569a",uri="sip:102@test.io",response="da014b313dde19ff29c82043404af10f",algorithm=MD5
User-Agent: abcd
Allow-Events: hold, talk, conference, dialog
Content-Length: 21

<strong>{ "operation" : "pause-audio-recording", "session_id" : "5984923794364"}
</strong></code></pre>

In the above out-of-dialog message:

* The message body should be in JSON format.&#x20;
* The **Content-Type** should be "**application/x-media-control+json".**
* The **operation** indicates whether to pause or resume recording
* The **session\_id** specifies the call session ID. You can obtain this ID from the **X-Session-Id** header in the INVITE or 200 OK SIP message.&#x20;

You can also use an in-dialog message to pause or resume recording. The message would look like this

```
MESSAGE sip:102@test.io SIP/2.0
Via: SIP/2.0/UDP 192.168.0.16:5568;branch=z9hG4bK-524287-1---a67ad51a25df052f;rport
Max-Forwards: 70
To: <sip:102@test.io>
From: <sip:101@test.io>;tag=fa2d6f1d
Call-ID: SFNH1yZ5c4LrG70_Tlyxgg..
CSeq: 3 MESSAGE
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, NOTIFY, MESSAGE, REGISTER, SUBSCRIBE, INFO, PUBLISH
Content-Type: application/x-media-control
Proxy-Authorization: Digest username="101",realm="test.io",nonce="13323933303:c8b6b255488669affb8a3e657188569a",uri="sip:102@test.io",response="da014b313dde19ff29c82043404af10f",algorithm=MD5
User-Agent: abcd
Allow-Events: hold, talk, conference, dialog
Content-Length: 21

pause-audio-recording
```

For the in-dialog message, the message body is plain text.

The PBX supports the following message commands:

* pause-audio-recording: pause the audio recording for the call
* pause-video-recording: pause the video recording for the call
* resume-audio-recording: resume the audio recording for the call
* resume-video-recording: resume the video recording for the call

## **Call Recording File Format**

By default, PortSIP PBX records files in the WAV format. This format offers superior performance but consumes more disk space.

PortSIP PBX also supports recording calls in MP3 and AMR formats. To change the settings, please follow these steps:

1. Open the file `var/lib/portsip/pbx/system.ini`. For Windows, this file is located at `C:/ProgramData/PortSIP/pbx/log`.
2. In the `[mediaserver]` section, find the key `recorder_file_format`. The default value  `1` corresponds to recording in WAV format.
3. If you wish to record the call in AMR format, change this value to `2`. For recording in MP3 format, change the value to `3`.
4. Save the changes.

To apply these changes, you'll need to restart the media server:

* **Windows**: Open the Windows Service Manager and restart the **PortSIP Media Server** service.
* **Linux**: Execute the following commands:

```
cd /opt/portsip && /bin/sh pbx_ctl.sh restart -s portsip.mediaserver
```

That’s it! Your changes should now be in effect.

{% hint style="danger" %}
Recording the calls in AMR or MP3 format will reduce the disk space but will cause poor performance.
{% endhint %}

Here’s a revised version of your text:

## **Recording File Size Calculation**

You can calculate the size of the recording file based on the following conditions:

* **Wave**: This format is uncompressed and typically results in a file size of approximately 1MB per minute.
* **AMR**: This format results in a file size of about 100KB per minute. Please note that AMR files cannot be played in a browser.
* **MP3**: This format results in a file size of approximately 256KB per minute. MP3 files can be played in a browser.

Please keep these conditions in mind when choosing a format for your recording files.

