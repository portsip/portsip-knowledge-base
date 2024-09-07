# User Manual for Windows

## **FAQ**

### **Where can I download the PortSIP VoIP SDK for test?**

The PortSIP VoIP SDK with Sample project at [PortSIP Website](https://www.portsip.com/download-portsip-voip-sdk/).

### **How can I compile the sample project?**

1. Download the sample project from [PortSIP Website](https://www.portsip.com/download-portsip-voip-sdk/).
2. Uncompress the .zip file.
3. Open the project with your IDE. and&#x20;
4. Compile the sample project directly and run it to test.
5. If the SDK connects to the PortSIP PBX, it's no limitations.&#x20;
6. The trial SDK works with any 3rd PBX and SIP server, which only allows a 2-3 minute conversation.

### **How can I create a new project with PortSIP VoIP SDK?**

#### **C#/VB.NET:**

1. Download the Sample project and uncompress it.&#x20;
2. Create a new "Windows application" project in Visual Studio.
3. Copy the `portsip_sdk.dll` and `portsip_media.dll` from the sample project directory to your project output directory: `bin\release` and `bin\debug`.
4. Copy the **PortSIP** folder from the sample project directory to your project folder and add it to Solution.
5. Inherit the interface **SIPCallbackEvents** to process the callback events.
6. Right-click the project, and choose Properties. Click the **Build** tab, and then check the **Allow unsafe code** checkbox.

For more details please read the Sample project source code.

#### **VC++:**

1. Download and uncompress the sample project.&#x20;
2. Create a new "MFC Application" project.
3. Copy the `portsip_sdk.dll` and `portsip_media.dll` from the sample project directory to your project output directories.
4. Copy the `include/PortSIPLib` folder to your project folder and add the `.hxx` files from the `PortSIPLib` folder to your project.
5. Copy the `lib` folder to your project folder and link `portsip_sdk.lib` into your project.&#x20;

For more details please read the Sample project source code.

### **How can I test the P2P call (without an SIP server PBX )?**

4. Download and extract the SDK sample project ZIP file, compile and run the "P2PSample" project.
5. Run the P2Psample on two devices. For example, run it on device A and device B, and IP address for A is 192.168.1.10, IP address for B is 192.168.1.11.
6. Enter a user name and password on A. For example, user name 111, and password aaa (you can enter anything for the password as the SDK will ignore it). Enter a user name and password on B, for example: user name 222, password aaa.
7. Click the "Initialize" button on A and B. If the default port 5060 is already in use, the P2PSample will prompt "Initialize failure". In case of this, please click the "Uninitialize" button and change the local port, and click the "Initialize" button again.
8. The log box will appear "Initialized." if the SDK is successfully initialized.
9. To make call from A to B, enter: sip[:222@192.168.1.11 ](mailto:222@192.168.1.11)and click "Dial" button; while to make call from B to A, enter: sip[:111@192.168.1.10.](mailto:111@192.168.1.10)

### **Is the SDK thread safe?**

Yes, the SDK is thread safe. You can call any of the API functions without the need to consider the multiple threads. Note: the SDK allows to call of API functions in callback events directly - except for the "onAudioRawCallback", "onVideoRawCallback", and "onRTPPacketCallback" callbacks.

### **Does the SDK support native 64-bit?**&#x20;

Yes, both 32-bit and 64-bit are supported for SDK.

## **Initialize and register functions**

```csharp
Int32 PortSIP.PortSIPLib.initialize (TRANSPORT_TYPE transportType, 
                                String localIp, 
                                Int32 localSIPPort,
                                PORTSIP_LOG_LEVEL logLevel, 
                                String logFilePath, 
                                Int32 maxCallSessions, 
                                String sipAgentString, 
                                Int32 audioDeviceLayer, 
                                Int32 videoDeviceLayer, 
                                String TLSCertificatesRootPath, 
                                String TLSCipherList, 
                                Boolean verifyTLSCertificate)
```

Initialize the SDK.&#x20;

#### **Parameters**

| _transport_               | Transport for SIP signaling. TRANSPORT\_PERS is the PortSIP private transport for anti SIP blocking. It must be used with PERS.                                                                                                                                                                                                                                   |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _localIP_                 | <p>The local computer IP address to be bound (for example: 192.168.1.108). It will be used for sending and receiving SIP messages and RTP packets. If this IP is passed in IPv6 format, the SDK will be using IPv6.</p><p>If you want the SDK to choose correct network interface (IP) automatically, please pass the "0.0.0.0"(for IPv4) or "::" (for IPv6).</p> |
| _localSIPPort_            | The SIP message transport listener port, for example: 5060.                                                                                                                                                                                                                                                                                                       |
| _logLevel_                | Set the application log level. The SDK will generate "PortSIP\_Log\_datatime.log" file if the log enabled.                                                                                                                                                                                                                                                        |
| _logFilePath_             | The log file path. The path (folder) MUST be existent.                                                                                                                                                                                                                                                                                                            |
| _maxCallLines_            | Theoretically, unlimited lines could be supported depending on the device capability. For SIP client recommended value ranges 1 - 100;                                                                                                                                                                                                                            |
| _sipAgent_                | The User-Agent header to be inserted into SIP messages.                                                                                                                                                                                                                                                                                                           |
| _audioDeviceLayer_        | <p>Specify the audio device layer to be used: </p><p>0 = Use the OS default device.</p><p>1 = Virtual device, usually use this for the device which has no sound device installed.</p>                                                                                                                                                                            |
| _videoDeviceLayer_        | <p>Specifies the video device layer that should be used:</p><p>0 = Use the OS default device.</p><p>1 = Use a Virtual device. Usually use this for the device that has no camera installed.</p>                                                                                                                                                                   |
| _TLSCertificatesRootPath_ | Specify the TLS certificate path, from which the SDK will load the certificates automatically. Note: On Windows, this path will be ignored, and SDK will read the certificates from the Windows certificates stored area instead.                                                                                                                                 |
| _TLSCipherList_           | Specify the TLS cipher list. This parameter is usually passed as empty so that the SDK will offer all available ciphers.                                                                                                                                                                                                                                          |
| _verifyTLSCertificat_     | Indicate if SDK will verify the TLS certificate. By setting it to false, the SDK will not verify the validity of the TLS certificate.                                                                                                                                                                                                                             |
|                           |                                                                                                                                                                                                                                                                                                                                                                   |
|                           |                                                                                                                                                                                                                                                                                                                                                                   |

#### **Returns**

If the function succeeds, it will return a value of 0. If the function fails, it will return a specific error code

```csharp
String PortSIP.PortSIPLib.getVersion ()
```

Get the current version number of the SDK.

**Returns**

Return a current version number MAJOR.MINOR.PATCH of the SDK.

**Int32 PortSIP.PortSIPLib.setLicenseKey (String **_**key**_**)**

Set the license key. It must be called before setUser function.

**Parameters**

| _key_ | The SDK license key, please purchase from PortSIP. |
| ----- | -------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

12

**NIC and local IP functions**

**Functions**

* Int32 PortSIP.PortSIPLib.getNICNums () _Get the Network Interface Card numbers._
* Int32 PortSIP.PortSIPLib.getLocalIpAddress (Int32 index, StringBuilder ip, Int32 ipSize) _Get the local IP address by Network Interface Card index._
*   Int32 PortSIP.PortSIPLib.setUser (String userName, String displayName, String authName, String password, String sipDomain, String sipServerAddr, Int32 sipServerPort, String stunServerAddr, Int32 stunServerPort, String outboundServerAddr, Int32 outboundServerPort)

    _Set user account info._
*   void PortSIP.PortSIPLib.removeUser ()

    _Remove the user. It will un-register from SIP server given that the user is already registered._
* Int32 PortSIP.PortSIPLib.setDisplayName (String displayName) _Set the display name of user._
*   Int32 PortSIP.PortSIPLib.setInstanceId (String uuid)

    _Set outbound (RFC5626) instanceId to be used in contact headers._
* Int32 PortSIP.PortSIPLib.registerServer (Int32 expires, Int32 retryTimes) _Register to SIP proxy server (login to server)._
* Int32 PortSIP.PortSIPLib.unRegisterServer (Int32 waitMS) _Un-register from the SIP proxy server._
* Int32 PortSIP.PortSIPLib.enableRport (Boolean enable) _Enable/disable rport(RFC3581)._
* Int32 PortSIP.PortSIPLib.enableEarlyMedia (Boolean enable) _Enable/disable Early Media._
*   Int32 PortSIP.PortSIPLib.enablePriorityIPv6Domain (Boolean enable)

    _Enable/disable which allows specifying the preferred protocol when a domain supports both IPV4 and IPV6 simultaneously._
* Int32 PortSIP.PortSIPLib.setUriUserEncoding (String character, Boolean enable) _Modifies the default URI user character needs to be escaped._
* Int32 PortSIP.PortSIPLib.setReliableProvisional (Int32 mode) _Enable/disable PRACK._
*   Int32 PortSIP.PortSIPLib.enable3GppTags (Boolean enable)

    _Enable/disable the 3Gpp tags, including "ims.icsi.mmtel" and "g.3gpp.smsip"._

13

*   void PortSIP.PortSIPLib.enableCallbackSignaling (Boolean enableSending, Boolean enableReceived)

    _Enable/disable to callback the SIP messages._
*   Int32 PortSIP.PortSIPLib.enableRtpCallback (Int32 sessionId, Int32 mediaType, Int32 directionMode)

    _Set the RTP callbacks to allow access to the sent and received RTP packets. The onRTPPacketCallback events will be triggered._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.getNICNums ()**

Get the Network Interface Card numbers. **Returns**

If the function succeeds, it will return the NIC numbers which is greater than or equal to 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.getLocalIpAddress (Int32 **_**index**_**, StringBuilder **_**ip**_**, Int32 **_**ipSize**_**)**

Get the local IP address by Network Interface Card index. **Parameters**

| _index_  | <p>The IP address index. For example, if the PC has two NICs, and we wish to</p><p>obtain the second NIC IP. Set this parameter to 1 and the first NIC IP index is 0.</p> |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _ip_     | The buffer that is used to receive the IP.                                                                                                                                |
| _ipSize_ | The IP buffer size, which cannot be less than 32 bytes.                                                                                                                   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setUser (String **_**userName**_**, String **_**displayName**_**, String **_**authName**_**, String **_**password**_**, String **_**sipDomain**_**, String **_**sipServerAddr**_**, Int32 **_**sipServerPort**_**, String **_**stunServerAddr**_**, Int32 **_**stunServerPort**_**, String **_**outboundServerAddr**_**, Int32 **_**outboundServerPort**_**)**

Set user account info.

14 **Parameters**

| _userName_            | Account (User name) of the SIP. Usually provided by an IP-Telephony service provider.                                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _displayName_         | The display name of user. You can set it as your like, such as "James Kend". It's optional.                                                                                                             |
| _authName_            | Authorization user name (usually equal to the username).                                                                                                                                                |
| _password_            | The password of user. It's optional.                                                                                                                                                                    |
| _localIp_             | The local computer IP address to be bound. For example: 192.168.1.108. It will be used for sending and receiving SIP message and RTP packet. If pass this IP as the IPv6 format, the SDK will use IPv6. |
| _localSipPort_        | The SIP message transport listener port. For example: 5060.                                                                                                                                             |
| _userDomain_          | User domain; this parameter is optional that allows to pass an empty string if you are not using the domain.                                                                                            |
| _sipServer_           | SIP proxy server IP or domain. For example: xx.xxx.xx.x or sip.xxx.com.                                                                                                                                 |
| _sipServerPort_       | Port of the SIP proxy server. For example: 5060.                                                                                                                                                        |
| _stunServer_          | Stun server used for NAT traversal. It's optional and can pass empty string to disable STUN.                                                                                                            |
| _stunServerPort_      | STUN server port. It will be ignored if the outboundServer is empty.                                                                                                                                    |
| _outboundServer_      | Outbound proxy server. For example: sip.domain.com. It's optional and allows to pass a empty string if not using outbound server.                                                                       |
| _outboundServerPo rt_ | Outbound proxy server port, it will be ignored if the outboundServer is empty.                                                                                                                          |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setDisplayName (String **_**displayName**_**)**

Set the display name of user.

**Parameters**

| _displayName_ | that will appear in the From/To Header. |
| ------------- | --------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setInstanceId (String **_**uuid**_**)**

Set outbound (RFC5626) instanceId to be used in contact headers. **Parameters**

| _uuid_ | The ID that will appear in the contact header. Please make sure it's a unique ID. |
| ------ | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.registerServer (Int32 **_**expires**_**, Int32 **_**retryTimes**_**)**

Register to SIP proxy server (login to server).

15 **Parameters**

| _expires_    | Registration refresh Interval in seconds with maximum 3600. It will be inserted into SIP REGISTER message headers.                                                                       |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _retryTimes_ | The retry times if failed to refresh the registration. If it's set to be less than or equal to 0, the retry will be disabled and onRegisterFailure callback triggered when retry failed. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code. If registration to server succeeded, onRegisterSuccess will be triggered, otherwise onRegisterFailure triggered.

**Int32 PortSIP.PortSIPLib.unRegisterServer (Int32 **_**waitMS**_**)**

Un-register from the SIP proxy server.

**Parameters**

| _waitMS_ | Wait for the server to reply that the un-registration is successful, waitMS is the longest waiting milliseconds, 0 means not waiting. |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.enableRport (Boolean **_**enable**_**)**

Enable/disable rport(RFC3581).

**Parameters**

| _enable_ | Set to true to enable the SDK to support rport. By default it is enabled. |
| -------- | ------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.enableEarlyMedia (Boolean **_**enable**_**)**

Enable/disable Early Media.

**Parameters**

| _enable_ | Set to true to enable the SDK to support Early Media. By default Early Media is disabled. |
| -------- | ----------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.enablePriorityIPv6Domain (Boolean **_**enable**_**)**

Enable/disable which allows specifying the preferred protocol when a domain supports both IPV4 and IPV6 simultaneously.

16 **Parameters**

| _enable_ | Set to true to enable priority IPv6 Domain. with the default priority being IPV4. |
| -------- | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setUriUserEncoding (String **_**character**_**, Boolean **_**enable**_**)**

Modifies the default URI user character needs to be escaped.

**Parameters**

| _character_ | The character to be modified, set one at a time.                                        |
| ----------- | --------------------------------------------------------------------------------------- |
| _enable_    | Whether escaping is required, true for allowing escaping, false for disabling escaping. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setReliableProvisional (Int32 **_**mode**_**)**

Enable/disable PRACK.

**Parameters**

| _mode_ | Modes work as follows: 0 - Never, Disable PRACK,By default the PRACK is disabled. 1 - SupportedEssential, Only send reliable provisionals if sending a body and far end supports. 2 - Supported, Always send reliable provisionals if far end supports. 3 - Required Always send reliable provisionals. |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.enable3GppTags (Boolean **_**enable**_**)**

Enable/disable the 3Gpp tags, including "ims.icsi.mmtel" and "g.3gpp.smsip".

**Parameters**

| _enable_ | Set to true to enable SDK to support 3Gpp tags. |
| -------- | ----------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**void PortSIP.PortSIPLib.enableCallbackSignaling (Boolean **_**enableSending**_**, Boolean **_**enableReceived**_**)**

Enable/disable to callback the SIP messages.

17 **Parameters**

| _enableSending_  | Set as true to enable to callback the sent SIP messages, or false to disable. Once enabled, the "onSendingSignaling" event will be triggered when the SDK sends a SIP message.         |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _enableReceived_ | Set as true to enable to callback the received SIP messages, or false to disable. Once enabled, the "onReceivedSignaling" event will be triggered when the SDK receives a SIP message. |

**Int32 PortSIP.PortSIPLib.enableRtpCallback (Int32 **_**sessionId**_**, Int32 **_**mediaType**_**, Int32 **_**directionMode**_**)**

Set the RTP callbacks to allow access to the sent and received RTP packets. The onRTPPacketCallback events will be triggered.

**Parameters**

| _sessionId_           | The session ID of call.                                                        |   |   |
| --------------------- | ------------------------------------------------------------------------------ | - | - |
| _mediaType_           | 0 -audo 1-video 2-screen.                                                      |   |   |
| _directionMode_       | The RTP stream callback mode.                                                  |   |   |
| Type                  | Description                                                                    |   |   |
| DIRECTION\_SEND       | Callback the send RTP stream for one channel based on the given sessionId.     |   |   |
| DIRECTION\_RECV       | Callback the received RTP stream for one channel based on the given sessionId. |   |   |
| DIRECTION\_SEND\_RECV | Callback both local and remote RTWP stream on the given sessionId.             |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

18

**Audio and video codecs functions**

**Functions**

* Int32 PortSIP.PortSIPLib.addAudioCodec (AUDIOCODEC\_TYPE codecType) _Enable an audio codec. It will be appears in SDP._
* Int32 PortSIP.PortSIPLib.addVideoCodec (VIDEOCODEC\_TYPE codecType) _Enable a video codec. It will appear in SDP._
* Boolean PortSIP.PortSIPLib.isAudioCodecEmpty () _Detect if enabled audio codecs is empty or not._
* Boolean PortSIP.PortSIPLib.isVideoCodecEmpty () _Detect if enabled video codecs is empty or not._
*   Int32 PortSIP.PortSIPLib.setAudioCodecPayloadType (AUDIOCODEC\_TYPE codecType, Int32 payloadType)

    _Set the RTP payload type for dynamic audio codec._
*   Int32 PortSIP.PortSIPLib.setVideoCodecPayloadType (VIDEOCODEC\_TYPE codecType, Int32 payloadType)

    _Set the RTP payload type for dynamic Video codec._
* void PortSIP.PortSIPLib.clearAudioCodec () _Remove all enabled audio codecs._
* void PortSIP.PortSIPLib.clearVideoCodec () _Remove all enabled video codecs._
*   Int32 PortSIP.PortSIPLib.setAudioCodecParameter (AUDIOCODEC\_TYPE codecType, String parameter)

    _Set the codec parameter for audio codec._
*   Int32 PortSIP.PortSIPLib.setVideoCodecParameter (VIDEOCODEC\_TYPE codecType, String parameter)

    _Set the codec parameter for video codec._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.addAudioCodec (AUDIOCODEC\_TYPE **_**codecType**_**)**

19

Enable an audio codec. It will be appears in SDP. **Parameters**

| _codecType_ | Audio codec type. |
| ----------- | ----------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.addVideoCodec (VIDEOCODEC\_TYPE **_**codecType**_**)**

Enable a video codec. It will appear in SDP.

**Parameters**

| _codecType_ | Video codec type. |
| ----------- | ----------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Boolean PortSIP.PortSIPLib.isAudioCodecEmpty ()**

Detect if enabled audio codecs is empty or not.

**Returns**

If no audio codec is enabled, it will return value true, otherwise false.

**Boolean PortSIP.PortSIPLib.isVideoCodecEmpty ()**

Detect if enabled video codecs is empty or not.

**Returns**

If no video codec is enabled, it will return value true, otherwise false.

**Int32 PortSIP.PortSIPLib.setAudioCodecPayloadType (AUDIOCODEC\_TYPE **_**codecType**_**, Int32 **_**payloadType**_**)**

Set the RTP payload type for dynamic audio codec. **Parameters**

| _codecType_   | Audio codec type, which is defined in the PortSIPTypes file. |
| ------------- | ------------------------------------------------------------ |
| _payloadType_ | The new RTP payload type that you want to set.               |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

20

**Int32 PortSIP.PortSIPLib.setVideoCodecPayloadType (VIDEOCODEC\_TYPE **_**codecType**_**, Int32 **_**payloadType**_**)**

Set the RTP payload type for dynamic Video codec. **Parameters**

| _codecType_   | Video codec type, which is defined in the PortSIPTypes file. |
| ------------- | ------------------------------------------------------------ |
| _payloadType_ | The new RTP payload type that you want to set.               |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setAudioCodecParameter (AUDIOCODEC\_TYPE **_**codecType**_**, String **_**parameter**_**)**

Set the codec parameter for audio codec. **Parameters**

| _codecType_ | Audio codec type, defined in the PortSIPTypes file. |
| ----------- | --------------------------------------------------- |
| _parameter_ | The parameter in string format.                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Example:

setAudioCodecParameter(AUDIOCODEC\_AMR, "mode-set=0; octet-align=1; robust-sorting=0");

**Int32 PortSIP.PortSIPLib.setVideoCodecParameter (VIDEOCODEC\_TYPE **_**codecType**_**, String **_**parameter**_**)**

Set the codec parameter for video codec. **Parameters**

| _codecType_ | Video codec type, defined in the PortSIPTypes file. |
| ----------- | --------------------------------------------------- |
| _parameter_ | The parameter in string format.                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

**Remarks**

Example:

setVideoCodecParameter(VIDEO\_CODEC\_H264, "profile-level-id=420033; packetization-mode=0");

21

**Additional setting functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.setSrtpPolicy (SRTP\_POLICY srtpPolicy, Boolean allowSrtpOverUnsecureTransport)

    _Set the SRTP policy._
* Int32 PortSIP.PortSIPLib.setRtpPortRange (Int32 minimumRtpPort, Int32 maximumRtpPort) _Set the RTP ports range for RTP streaming._
* Int32 PortSIP.PortSIPLib.enableCallForward (Boolean forBusyOnly, String forwardTo) _Enable call forward._
*   Int32 PortSIP.PortSIPLib.disableCallForward ()

    _Disable the call forwarding. The SDK is not forwarding any incoming call after this function is called._
*   Int32 PortSIP.PortSIPLib.enableSessionTimer (Int32 timerSeconds, SESSION\_REFRESH\_MODE refreshMode)

    _Allows to periodically refresh Session Initiation Protocol (SIP) sessions by sending INVITE requests repeatedly._
* Int32 PortSIP.PortSIPLib.disableSessionTimer () _Disable the session timer._
* void PortSIP.PortSIPLib.setDoNotDisturb (Boolean state) _Enable the "Do not disturb" to enable/disable._
*   Int32 PortSIP.PortSIPLib.enableAutoCheckMwi (Boolean state)

    _Allows to enable/disable the check MWI (Message Waiting Indication) automatically._
*   Int32 PortSIP.PortSIPLib.setRtpKeepAlive (Boolean state, Int32 keepAlivePayloadType, Int32 deltaTransmitTimeMS)

    _Enable or disable to send RTP keep-alive packet when the call is established._
* Int32 PortSIP.PortSIPLib.setKeepAliveTime (Int32 keepAliveTime) _Enable or disable to send SIP keep-alive packet._
*   Int32 PortSIP.PortSIPLib.getSipMessageHeaderValue (String sipMessage, String headerName, StringBuilder headerValue, Int32 headerValueLength)

    _Access the SIP header of SIP message._
*   Int32 PortSIP.PortSIPLib.addSipMessageHeader (Int32 sessionId, String methodName, Int32 msgType, String headerName, String headerValue)

    _Add the SIP Message header into the specified outgoing SIP message._
* Int32 PortSIP.PortSIPLib.removeAddedSipMessageHeader (Int32 sipMessageHeaderId) _Remove the headers (custom header) added by addSipMessageHeader._

22

* Int32 PortSIP.PortSIPLib.clearAddedSipMessageHeaders () _Clear the added extension headers (custom headers)_
*   Int32 PortSIP.PortSIPLib.modifySipMessageHeader (Int32 sessionId, String methodName, Int32 msgType, String headerName, String headerValue)

    _Modify the special SIP header value for every outgoing SIP message._
* Int32 PortSIP.PortSIPLib.removeModifiedSipMessageHeader (Int32 sipMessageHeaderId) _Remove the extension header (custom header) from every outgoing SIP message._
*   Int32 PortSIP.PortSIPLib.clearModifiedSipMessageHeaders ()

    _Clear the modified headers value, and do not modify every outgoing SIP message header values any longer._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.setSrtpPolicy (SRTP\_POLICY **_**srtpPolicy**_**, Boolean **_**allowSrtpOverUnsecureTransport**_**)**

Set the SRTP policy. **Parameters**

| _srtpPolicy_ | The SRTP policy. |
| ------------ | ---------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

**Int32 PortSIP.PortSIPLib.setRtpPortRange (Int32 **_**minimumRtpPort**_**, Int32 **_**maximumRtpPort**_**)**

Set the RTP ports range for RTP streaming. **Parameters**

| _minimumRtpPort_ | The minimum RTP port. |
| ---------------- | --------------------- |
| _maximumRtpPort_ | The maximum RTP port. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The port range ((max - min) % maxCallLines) should be greater than 4. **Int32 PortSIP.PortSIPLib.enableCallForward (Boolean **_**forBusyOnly**_**, String **_**forwardTo**_**)**

Enable call forward. **Parameters**

| _forBusyOnly_ | If this parameter is set as true, the SDK will forward all incoming calls when currently it's busy. If it's set as false, the SDK forward all incoming calls anyway. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _forwardTo_   | The call forward target. It must be like sip[:xxxx@sip.portsip.com.](mailto:xxxx@sip.portsip.com)                                                                    |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.disableCallForward ()**

Disable the call forwarding. The SDK is not forwarding any incoming call after this function is called.

**Returns**

If the function succeeds, it will return value 0. If the function fails, the return value is a specific error code.

**Int32 PortSIP.PortSIPLib.enableSessionTimer (Int32 **_**timerSeconds**_**, SESSION\_REFRESH\_MODE **_**refreshMode**_**)**

Allows to periodically refresh Session Initiation Protocol (SIP) sessions by sending INVITE requests repeatedly.

**Parameters**

| _timerSeconds_ | The value of the refreshment interval in seconds. Minimum value of 90 seconds required.           |
| -------------- | ------------------------------------------------------------------------------------------------- |
| _refreshMode_  | Allow to set the session refresh by UAC or UAS: SESSION\_REFERESH\_UAC or SESSION\_REFERESH\_UAS; |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The repeated INVITE requests, or re-INVITEs, are sent during an active call log to allow user agents (UA) or proxies to determine the status of a SIP session. Without this keepalive mechanism, proxies that remember incoming and outgoing requests (stateful proxies) may continue to retain call state in vain. If a UA fails to send a BYE message at the end of a session or if the BYE message is lost because of network problems, a stateful proxy will not know that the session has ended. The re-INVITES ensure that active sessions stay active and completed sessions are terminated.

**Int32 PortSIP.PortSIPLib.disableSessionTimer ()**

Disable the session timer.

24

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**void PortSIP.PortSIPLib.setDoNotDisturb (Boolean **_**state**_**)**

Enable the "Do not disturb" to enable/disable.

**Parameters**

| _state_ | If it is set to true, the SDK will reject all incoming calls anyway. |
| ------- | -------------------------------------------------------------------- |

**Int32 PortSIP.PortSIPLib.enableAutoCheckMwi (Boolean **_**state**_**)**

Allows to enable/disable the check MWI (Message Waiting Indication) automatically. **Parameters**

| _state_ | If it is set as true, MWI will be checked automatically once successfully registered to a SIP proxy server. |
| ------- | ----------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setRtpKeepAlive (Boolean **_**state**_**, Int32 **_**keepAlivePayloadType**_**, Int32 **_**deltaTransmitTimeMS**_**)**

Enable or disable to send RTP keep-alive packet when the call is established. **Parameters**

| _state_                 | Set to true to allow to send the keep-alive packet during the conversation.                        |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| _keepAlivePayload Type_ | The payload type of the keep-alive RTP packet. It's usually set to 126.                            |
| _deltaTransmitTime MS_  | The keep-alive RTP packet sending interval, in millisecond. Recommend value ranges 15000 - 300000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setKeepAliveTime (Int32 **_**keepAliveTime**_**)**

Enable or disable to send SIP keep-alive packet.

**Parameters**

| _keepAliveTime_ | This is the SIP keep alive time interval in seconds. Set it to 0 to disable the SIP keep alive. Recommend to set as 30 or 50. |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

25

**Int32 PortSIP.PortSIPLib.getSipMessageHeaderValue (String **_**sipMessage**_**, String **_**headerName**_**, StringBuilder **_**headerValue**_**, Int32 **_**headerValueLength**_**)**

Access the SIP header of SIP message. **Parameters**

| _sipMessage_         | The SIP message.                                                                 |
| -------------------- | -------------------------------------------------------------------------------- |
| _headerName_         | The header which wishes to access the SIP message.                               |
| _headerValue_        | The buffer to receive header value.                                              |
| _headerValueLengt h_ | The headerValue buffer size. Usually we recommend to set it more than 512 bytes. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

When receiving a SIP message in the onReceivedSignaling callback event, and wishes to get SIP message header value, please use getSipMessageHeaderValue:

StringBuilder value = new StringBuilder(); value.Length = 512; getSipMessageHeaderValue(message, name, value);

**Int32 PortSIP.PortSIPLib.addSipMessageHeader (Int32 **_**sessionId**_**, String **_**methodName**_**, Int32 **_**msgType**_**, String **_**headerName**_**, String **_**headerValue**_**)**

Add the SIP Message header into the specified outgoing SIP message. **Parameters**

| _sessionId_   | Add the header to the SIP message with the specified session Id only. By setting to -1, it will be added to all messages.                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_  | Just add the header to the SIP message with specified method name. For example: "INVITE", "REGISTER", "INFO" etc. If "ALL" specified, it will add all SIP messages. |
| _msgType_     | 1 refers to apply to the request message, 2 refers to apply to the response message, 3 refers to apply to both request and response.                                |
| _headerName_  | The custom header name that will appears in every outgoing SIP message.                                                                                             |
| _headerValue_ | The custom header value.                                                                                                                                            |

**Returns**

If the function succeeds, it will return addedSipMessageId, which is greater than 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.removeAddedSipMessageHeader (Int32 **_**sipMessageHeaderId**_**)**

Remove the headers (custom header) added by addSipMessageHeader. **Parameters**

| _addedSipMessageI d_ | The addedSipMessageId return by addSipMessageHeader. |
| -------------------- | ---------------------------------------------------- |

26

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.clearAddedSipMessageHeaders ()**

Clear the added extension headers (custom headers)

**Remarks**

For example, we have added two custom headers into every outgoing SIP message and wish to remove them.

addExtensionHeader(-1, "ALL", 3, "Blling", "usd100.00"); addExtensionHeader(-1, "ALL", 3, "ServiceId", "8873456"); clearAddedSipMessageHeaders();

**Int32 PortSIP.PortSIPLib.modifySipMessageHeader (Int32 **_**sessionId**_**, String **_**methodName**_**, Int32 **_**msgType**_**, String **_**headerName**_**, String **_**headerValue**_**)**

Modify the special SIP header value for every outgoing SIP message. **Parameters**

| _sessionId_  | The header to the SIP message with the specified session Id. By setting to -1, it will be added to all messages.                                                       |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_ | Modify the header to the SIP message with specified method name only. For example: "INVITE", "REGISTER", "INFO" etc. If "ALL" specified, it will add all SIP messages. |
| _msgType_    | 1 refers to apply to the request message, 2 refers to apply to the response message, 3 refers to apply to both request and response.                                   |

**Returns**

If the function succeeds, it will return modifiedSipMessageId, which is greater than 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.removeModifiedSipMessageHeader (Int32 **_**sipMessageHeaderId**_**)**

Remove the extension header (custom header) from every outgoing SIP message. **Parameters**

| _modifiedSipMessa geId_ | The modifiedSipMessageId is returned by modifySipMessageHeader. |
| ----------------------- | --------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.clearModifiedSipMessageHeaders ()**

Clear the modified headers value, and do not modify every outgoing SIP message header values any longer.

For example, to modify two headers' value for every outgoing SIP message and wish to clear it:

modifySipMessageHeader(-1, "ALL", 3, "Expires", "1000"); modifySipMessageHeader(-1, "ALL", 3, "User-Agent", "MyTest Softphone 1.0""); clearModifiedSipMessageHeaders();

**Audio and video functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.addSupportedMimeType (String methodName, String mimeType, String subMimeType)

    _Set the SDK to receive the SIP messages that include special mime type._
* Int32 PortSIP.PortSIPLib.setAudioSamples (Int32 ptime, Int32 maxPtime) _Set the audio capture sample._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.addSupportedMimeType (String **_**methodName**_**, String **_**mimeType**_**, String **_**subMimeType**_**)**

Set the SDK to receive the SIP messages that include special mime type. **Parameters**

| _methodName_  | Method name of the SIP message, such as INVITE, OPTION, INFO, MESSAGE, UPDATE, ACK etc. For more details please read the RFC3261. |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| _mimeType_    | The mime type of SIP message.                                                                                                     |
| _subMimeType_ | The sub mime type of SIP message.                                                                                                 |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

By default, PortSIP VoIP SDK supports these media types (mime types) below for incoming SIP messages:

"message/sipfrag" in NOTIFY message. "application/simple-message-summary" in NOTIFY message. "text/plain" in MESSAGE message. "application/dtmf-relay" in INFO message. "application/media\_control+xml" in INFO message.

The SDK allows to receive SIP messages that include above mime types. Now if remote side send an INFO SIP message with its "Content-Type" header value "text/plain". SDK will reject this INFO message, as "text/plain" of INFO message is not included in the default support list. How should we enable the SDK to receive the SIP INFO message that includes "text/plain" mime type? The answer is addSupportedMimyType:

addSupportedMimeType("INFO", "text", "plain");&#x20;

If we want to receive the NOTIFY message with "application/media\_control+xml", please:

addSupportedMimeType("NOTIFY", "application", "media\_control+xml");

For more details about the mime type, please visit this website: [http://www.iana.org/assignments/media-types/](http://www.iana.org/assignments/media-types/)

29

**Int32 PortSIP.PortSIPLib.setAudioSamples (Int32 **_**ptime**_**, Int32 **_**maxPtime**_**)**

Set the audio capture sample.

**Parameters**

| _ptime_    | It should be a multiple of 10 between 10 - 60 (with 10 and 60 inclusive).                                                               |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| _maxPtime_ | For the "maxptime" attribute, it should be a multiple of 10 between 10 - 60 (with 10 and 60 inclusive). It cannot be less than "ptime". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

It will appear in the SDP of INVITE and 200 OK message as "ptime and "maxptime" attribute.

30

**Access SIP message header functions**

**Functions**

* Int32 PortSIP.PortSIPLib.setAudioDeviceId (Int32 recordingDeviceId, Int32 playoutDeviceId) _Set the audio device that will be used for audio call._
* Int32 PortSIP.PortSIPLib.setVideoOrientation (Int32 rotation) _Set the video device that will be used for video call._
* Int32 PortSIP.PortSIPLib.setVideoDeviceId (Int32 deviceId) _Set the video device that will be used for video call._
* Int32 PortSIP.PortSIPLib.setVideoResolution (Int32 width, Int32 height) _Set the video capturing resolution._
*   Int32 PortSIP.PortSIPLib.setAudioBitrate (Int32 sessionId, AUDIOCODEC\_TYPE audioCodecType, Int32 bitrateKbps)

    _Set the audio bitrate._
* Int32 PortSIP.PortSIPLib.setVideoBitrate (Int32 sessionId, Int32 bitrateKbps) _Set the video bitrate._
* Int32 PortSIP.PortSIPLib.setVideoFrameRate (Int32 sessionId, Int32 frameRate) _Set the video frame rate._
* Int32 PortSIP.PortSIPLib.sendVideo (Int32 sessionId, Boolean sendState) _Send the video to remote side._
*   void PortSIP.PortSIPLib.muteMicrophone (Boolean mute)

    _Mute the device microphone. It's unavailable for Android and iOS._
*   void PortSIP.PortSIPLib.muteSpeaker (Boolean mute)

    _Mute the device speaker. It's unavailable for Android and iOS._
* void PortSIP.PortSIPLib.setChannelOutputVolumeScaling (Int32 sessionId, Int32 scaling)
* void PortSIP.PortSIPLib.setChannelInputVolumeScaling (Int32 sessionId, Int32 scaling)
*   Int32 PortSIP.PortSIPLib.setRemoteVideoWindow (Int32 sessionId, IntPtr remoteVideoWindow)

    _Set the window for a session that is used to display the received remote video image._
*   Int32 PortSIP.PortSIPLib.displayLocalVideo (Boolean state, Boolean mirror, IntPtr localVideoWindow)

    _Start/stop displaying the local video image._
*   Int32 PortSIP.PortSIPLib.setVideoNackStatus (Boolean state)

    _Enable/disable the NACK feature (rfc6642) that helps to improve the video quality._&#x20;

31

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.setAudioDeviceId (Int32 **_**recordingDeviceId**_**, Int32 **_**playoutDeviceId**_**)**

Set the audio device that will be used for audio call. **Parameters**

| _recordingDeviceId_ | Device ID (index) for audio recording. (Microphone). |
| ------------------- | ---------------------------------------------------- |
| _playoutDeviceId_   | Device ID (index) for audio playback (Speaker).      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoOrientation (Int32 **_**rotation**_**)**

Set the video device that will be used for video call.

**Parameters**

| _rotation_ | for video |
| ---------- | --------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoDeviceId (Int32 **_**deviceId**_**)**

Set the video device that will be used for video call.

**Parameters**

| _deviceId_ | Device ID (index) for video device (camera). |
| ---------- | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoResolution (Int32 **_**width**_**, Int32 **_**height**_**)**

Set the video capturing resolution.

**Parameters**

| _width_  | Video width.  |
| -------- | ------------- |
| _height_ | Video height. |

32 **Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setAudioBitrate (Int32 **_**sessionId**_**, AUDIOCODEC\_TYPE **_**audioCodecType**_**, Int32 **_**bitrateKbps**_**)**

Set the audio bitrate. **Parameters**

| _bitrateKbps_ | The audio bitrate in KBPS. |
| ------------- | -------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoBitrate (Int32 **_**sessionId**_**, Int32 **_**bitrateKbps**_**)**

Set the video bitrate.

**Parameters**

| _bitrateKbps_ | The video bitrate in KBPS. |
| ------------- | -------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoFrameRate (Int32 **_**sessionId**_**, Int32 **_**frameRate**_**)**

Set the video frame rate.

**Parameters**

| _frameRate_ | The frame rate value with minimum value 5, and maximum value 30. A greater value will enable you better video quality but requires more bandwidth. |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Usually you do not need to call this function to set the frame rate. The SDK uses default frame rate.

**Int32 PortSIP.PortSIPLib.sendVideo (Int32 **_**sessionId**_**, Boolean **_**sendState**_**)**

Send the video to remote side.

**Parameters**

| _sessionId_ | The session ID of the call.                              |
| ----------- | -------------------------------------------------------- |
| _sendState_ | Set to true to send the video, or false to stop sending. |

33 **Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**void PortSIP.PortSIPLib.muteMicrophone (Boolean **_**mute**_**)**

Mute the device microphone. It's unavailable for Android and iOS.

**Parameters**

| _mute_ | If the value is set to true, the microphone will be muted. You may also set it to false to un-mute it. |
| ------ | ------------------------------------------------------------------------------------------------------ |

**void PortSIP.PortSIPLib.muteSpeaker (Boolean **_**mute**_**)**

Mute the device speaker. It's unavailable for Android and iOS. **Parameters**

| _mute_ | If the value is set to true, the speaker is muted. You may also set it to false to un-mute it. |
| ------ | ---------------------------------------------------------------------------------------------- |

**void PortSIP.PortSIPLib.setChannelOutputVolumeScaling (Int32 **_**sessionId**_**, Int32 **_**scaling**_**)**

Set a volume |scaling| to be applied to the outgoing signal of a specific audio channel. **Parameters**

| _sessionId_ | The session ID of the call.                    |
| ----------- | ---------------------------------------------- |
| _scaling_   | Valid scale ranges \[0, 1000]. Default is 100. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**void PortSIP.PortSIPLib.setChannelInputVolumeScaling (Int32 **_**sessionId**_**, Int32 **_**scaling**_**)**

Set a volume |scaling| to be applied to the microphone signal of a specific audio channel. **Parameters**

| _sessionId_ | The session ID of the call.                    |
| ----------- | ---------------------------------------------- |
| _scaling_   | Valid scale ranges \[0, 1000]. Default is 100. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setRemoteVideoWindow (Int32 **_**sessionId**_**, IntPtr **_**remoteVideoWindow**_**)**

Set the window for a session that is used to display the received remote video image. **Parameters**

| _sessionId_        | The session ID of the call.                        |
| ------------------ | -------------------------------------------------- |
| _remoteVideoWindo_ | The window to display received remote video image. |

34

| _w_         |   |
| ----------- | - |
| **Returns** |   |

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.displayLocalVideo (Boolean **_**state**_**, Boolean **_**mirror**_**, IntPtr **_**localVideoWindow**_**)**

Start/stop displaying the local video image. **Parameters**

| _state_            | Set to true to display local video image.                                |
| ------------------ | ------------------------------------------------------------------------ |
| _mirror_           | Set to true to display the mirror image of local video.                  |
| _localVideoWindow_ | The window on which the local video image from camera will be displayed. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoNackStatus (Boolean **_**state**_**)**

Enable/disable the NACK feature (rfc6642) that helps to improve the video quality.

**Parameters**

| _state_ | Set to true to enable. |
| ------- | ---------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

35

**Call functions**

**Functions**

* Int32 PortSIP.PortSIPLib.call (String callee, Boolean sendSdp, Boolean videoCall) _Make a call._
* Int32 PortSIP.PortSIPLib.rejectCall (Int32 sessionId, int code) _rejectCall Reject the incoming call._
* Int32 PortSIP.PortSIPLib.hangUp (Int32 sessionId) _hangUp Hang up the call._
* Int32 PortSIP.PortSIPLib.answerCall (Int32 sessionId, Boolean videoCall) _answerCall Answer the incoming call._
*   Int32 PortSIP.PortSIPLib.updateCall (Int32 sessionId, bool enableAudio, bool enableVideo, bool enableScreen)

    _Use the re-INVITE to update the established call._
* Int32 PortSIP.PortSIPLib.hold (Int32 sessionId) _To place a call on hold._
* Int32 PortSIP.PortSIPLib.unHold (Int32 sessionId) _Take off hold._
*   Int32 PortSIP.PortSIPLib.muteSession (Int32 sessionId, Boolean muteIncomingAudio, Boolean muteOutgoingAudio, Boolean muteIncomingVideo, Boolean muteOutgoingVideo)

    _Mute the specified session audio or video._
* Int32 PortSIP.PortSIPLib.forwardCall (Int32 sessionId, String forwardTo) _Forward call to another one when receiving the incoming call._
* Int32 PortSIP.PortSIPLib.pickupBLFCall (String replaceDialogId, Boolean videoCall) _This function will be used for picking up a call based on the BLF (Busy Lamp Field) status._
*   Int32 PortSIP.PortSIPLib.sendDtmf (Int32 sessionId, DTMF\_METHOD dtmfMethod, int code, int dtmfDuration, bool playDtmfTone)

    _Send DTMF tone._&#x20;

**Detailed Description**&#x20;

36

**Function Documentation**

**Int32 PortSIP.PortSIPLib.call (String **_**callee**_**, Boolean **_**sendSdp**_**, Boolean **_**videoCall**_**)**

Make a call.

**Parameters**

| _callee_    | The callee. It can be either name or full SIP URI. For example: user001, sip[:user001@sip.iptel.org ](mailto:user001@sip.iptel.org)or sip[:user002@sip.yourdomain.com:](mailto:user002@sip.yourdomain.com)5068 |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _sendSdp_   | If it's set to false, the outgoing call doesn't include the SDP in INVITE message.                                                                                                                             |
| _videoCall_ | If it's set to true with at least one video codecs added, the outgoing call will include the video codec into SDP.                                                                                             |

**Returns**

If the function succeeds, it will return the session ID of the call that is greater than 0. If the function fails, it will return a specific error code. Note: the function success just means the outgoing call is being processed. You need to detect the call final state in onInviteTrying, onInviteRinging, onInviteFailure callback events.

**Int32 PortSIP.PortSIPLib.rejectCall (Int32 **_**sessionId**_**, int **_**code**_**)**

rejectCall Reject the incoming call.

**Parameters**

| _sessionId_ | The sessionId of the call.              |
| ----------- | --------------------------------------- |
| _code_      | Reject code. For example, 486, 480 etc. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.hangUp (Int32 **_**sessionId**_**)**

hangUp Hang up the call.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.answerCall (Int32 **_**sessionId**_**, Boolean **_**videoCall**_**)**

answerCall Answer the incoming call.

**Parameters**

| _sessionId_ | The session ID of call.                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| _videoCall_ | If the incoming call is a video call and the video codec is matched, set it to true to answer the video call. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.updateCall (Int32 **_**sessionId**_**, bool **_**enableAudio**_**, bool **_**enableVideo**_**, bool **_**enableScreen**_**)**

Use the re-INVITE to update the established call. **Parameters**

| _sessionId_   | The session ID of call.                                                                    |
| ------------- | ------------------------------------------------------------------------------------------ |
| _enableAudio_ | Set to true to allow the audio in updated call, or false to disable audio in updated call. |
| _enableVideo_ | Set to true to allow the video in updated call, or false to disable video in updated call. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

**Remarks**

Example usage:

Example 1: A called B with the audio only, B answered A, there has an audio conversation between A, B. Now A wants to see B visually, A could use these functions to do it.

clearVideoCodec(); addVideoCodec(VIDEOCODEC\_H264); updateCall(sessionId, true, true);

Example 2: Remove video stream from current conversation.

updateCall(sessionId, true, false);&#x20;

**Int32 PortSIP.PortSIPLib.hold (Int32 **_**sessionId**_**)**

To place a call on hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.unHold (Int32 **_**sessionId**_**)**

Take off hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

38

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.muteSession (Int32 **_**sessionId**_**, Boolean **_**muteIncomingAudio**_**, Boolean **_**muteOutgoingAudio**_**, Boolean **_**muteIncomingVideo**_**, Boolean **_**muteOutgoingVideo**_**)**

Mute the specified session audio or video. **Parameters**

| _sessionId_          | The session ID of the call.                                                                |
| -------------------- | ------------------------------------------------------------------------------------------ |
| _muteIncomingAudi o_ | Set it to true to mute incoming audio stream, and remote side audio cannot be heard.       |
| _muteOutgoingAudi o_ | Set it to true to mute outgoing audio stream, and the remote side can't hear the audio.    |
| _muteIncomingVide o_ | Set it to true to mute incoming video stream, and the remote side video will be invisible. |
| _muteOutgoingVide o_ | Set it to true to mute outgoing video stream, and the remote side can't see the video.     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.forwardCall (Int32 **_**sessionId**_**, String **_**forwardTo**_**)**

Forward call to another one when receiving the incoming call.

**Parameters**

| _sessionId_ | The session ID of the call.                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| _forwardTo_ | Target of the forwarding. It can be "sip:number@sipserver.com" or "number" only. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

**Int32 PortSIP.PortSIPLib.pickupBLFCall (String **_**replaceDialogId**_**, Boolean **_**videoCall**_**)**

This function will be used for picking up a call based on the BLF (Busy Lamp Field) status. **Parameters**

| _replaceDialogId_ | The session ID of the call.                                                      |
| ----------------- | -------------------------------------------------------------------------------- |
| _videoCall_       | Target of the forwarding. It can be "sip:number@sipserver.com" or "number" only. |

**Returns**

If the function succeeds, it will return a session ID that is greater than 0 to the new call, otherwise returns a specific error code that is less than 0.

**Remarks**

The scenario is:

39

1. User 101 subscribed the user 100's call status: sendSubscription("100", "dialog");
2. When 100 hold a call or 100 is ringing, onDialogStateUpdated callback will be triggered, and 101 will receive this callback. Now 101 can use pickupBLFCall function to pick the call rather than 100 to talk with caller.

**Int32 PortSIP.PortSIPLib.sendDtmf (Int32 **_**sessionId**_**, DTMF\_METHOD **_**dtmfMethod**_**, int **_**code**_**, int **_**dtmfDuration**_**, bool **_**playDtmfTone**_**)**

Send DTMF tone. **Parameters**

| _sessionId_  | The session ID of the call.                                                                                  |   |   |
| ------------ | ------------------------------------------------------------------------------------------------------------ | - | - |
| _dtmfMethod_ | DTMF tone could be sent with two methods: DTMF\_RFC2833 and DTMF\_INFO, of which DTMF\_RFC2833 is recommend. |   |   |
| _code_       | The DTMF tone (0-16).                                                                                        |   |   |
| code         | Description                                                                                                  |   |   |
| 0            | The DTMF tone 0.                                                                                             |   |   |
| 1            | The DTMF tone 1.                                                                                             |   |   |
| 2            | The DTMF tone 2.                                                                                             |   |   |
| 3            | The DTMF tone 3.                                                                                             |   |   |
| 4            | The DTMF tone 4.                                                                                             |   |   |
| 5            | The DTMF tone 5.                                                                                             |   |   |
| 6            | The DTMF tone 6.                                                                                             |   |   |
| 7            | The DTMF tone 7.                                                                                             |   |   |
| 8            | The DTMF tone 8.                                                                                             |   |   |
| 9            | The DTMF tone 9.                                                                                             |   |   |
| 10           | The DTMF tone \*.                                                                                            |   |   |
| 11           | The DTMF tone #.                                                                                             |   |   |
| 12           | The DTMF tone A.                                                                                             |   |   |
| 13           | The DTMF tone B.                                                                                             |   |   |
| 14           | The DTMF tone C.                                                                                             |   |   |
| 15           | The DTMF tone D.                                                                                             |   |   |
| 16           | The DTMF tone FLASH.                                                                                         |   |   |

**Parameters**

| _dtmfDuration_ | The DTMF tone samples. Recommended value 160.                                |
| -------------- | ---------------------------------------------------------------------------- |
| _playDtmfTone_ | If it is set to true, the SDK plays local DTMF tone sound when sending DTMF. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

40

**Refer functions**

**Functions**

* Int32 PortSIP.PortSIPLib.refer (Int32 sessionId, String referTo) _Refer the current call to another one._
* Int32 PortSIP.PortSIPLib.attendedRefer (Int32 sessionId, Int32 replaceSessionId, String referTo)
*   Int32 PortSIP.PortSIPLib.attendedRefer2 (IntPtr libSDK, Int32 sessionId, Int32 replaceSessionId, String replaceMethod, String target, String referTo)

    _Make an attended refer with specified request line and specified method embedded into the "Refer-To" header._
*   Int32 PortSIP.PortSIPLib.outOfDialogRefer (Int32 replaceSessionId, String replaceMethod, String target, String referTo)

    _Send an out of dialog REFER to replace the specified call._
*   Int32 PortSIP.PortSIPLib.acceptRefer (Int32 referId, String referSignalingMessage)

    _Accept the REFER request, and a new call will be made if called this function. The function is usually called after onReceivedRefer callback event._
* Int32 PortSIP.PortSIPLib.rejectRefer (Int32 referId) _Reject the REFER request._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.refer (Int32 **_**sessionId**_**, String **_**referTo**_**)**

Refer the current call to another one.

**Parameters**

| _sessionId_ | The session ID of the call.                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| _referTo_   | Target of the refer, which can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

refer(sessionId, "sip:testuser12@sip.portsip.com");&#x20;

41

You can watch the video on YouTube at[ https://www.youtube.com/watch?v=\_2w9EGgr3FY.](https://www.youtube.com/watch?v=\_2w9EGgr3FY) It will demonstrate the transfer.

**Int32 PortSIP.PortSIPLib.attendedRefer (Int32 **_**sessionId**_**, Int32 **_**replaceSessionId**_**, String **_**referTo**_**)**

@brief Make an attended refer.&#x20;

@param sessionId The session ID of the call.

@param replaceSessionId Session ID of the repferred call.

@param referTo Target of the refer, which can be either "sip:number@sipserver.com" or "number".&#x20;

@return If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

@remark

Please read the sample project source code for more details, or you can watch the video on YouTube at https://www.youtube.com/watch?v=NezhIZW4lV4,

which will demonstrate the transfer.

**Int32 PortSIP.PortSIPLib.attendedRefer2 (IntPtr **_**libSDK**_**, Int32 **_**sessionId**_**, Int32 **_**replaceSessionId**_**, String **_**replaceMethod**_**, String **_**target**_**, String **_**referTo**_**)**

Make an attended refer with specified request line and specified method embedded into the "Refer-To" header.

**Parameters**

| _sessionId_        | Session ID of the call.                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------------------- |
| _replaceSessionId_ | Session ID of the replaced call.                                                                                    |
| _replaceMethod_    | The SIP method name which will be embeded in the "Refer-To" header, usually INVITE or BYE.                          |
| _target_           | The target to which the REFER message will be sent. It appears in the "Request Line" of REFER message.              |
| _referTo_          | Target of the refer that appears in the "Refer-To" header. It can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Please read the sample project source code for more details, or you can watch the video on YouTube at[ https://www.youtube.com/watch?v=NezhIZW4lV4,](https://www.youtube.com/watch?v=NezhIZW4lV4) which will demonstrate the transfer.

**Int32 PortSIP.PortSIPLib.outOfDialogRefer (Int32 **_**replaceSessionId**_**, String **_**replaceMethod**_**, String **_**target**_**, String **_**referTo**_**)**

Send an out of dialog REFER to replace the specified call. **Parameters**

| _replaceSessionId_ | The session ID of the session which will be replaced.                                    |
| ------------------ | ---------------------------------------------------------------------------------------- |
| _replaceMethod_    | The SIP method name which will be added in the "Refer-To" header, usually INVITE or BYE. |
| _target_           | The target to which the REFER message will be sent.                                      |
| _referTo_          | The URI to be added into the "Refer-To" header.                                          |

42

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.acceptRefer (Int32 **_**referId**_**, String **_**referSignalingMessage**_**)**

Accept the REFER request, and a new call will be made if called this function. The function is usually called after onReceivedRefer callback event.

**Parameters**

| _referId_                | The ID of REFER request that comes from onReceivedRefer callback event.          |
| ------------------------ | -------------------------------------------------------------------------------- |
| _referSignalingMes sage_ | The SIP message of REFER request that comes from onReceivedRefer callback event. |

**Returns**

If the function succeeds, it will return a session ID greater than 0 to the new call for REFER; otherwise a specific error code less than 0.

**Int32 PortSIP.PortSIPLib.rejectRefer (Int32 **_**referId**_**)**

Reject the REFER request.

**Parameters**

| _referId_ | The ID of REFER request that comes from onReceivedRefer callback event. |
| --------- | ----------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

43

**Send audio and video stream functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.enableSendPcmStreamToRemote (Int32 sessionId, Boolean state, Int32 streamSamplesPerSec)

    _Enable the SDK to send PCM stream data to remote side from another source instead of microphone._
*   Int32 PortSIP.PortSIPLib.sendPcmStreamToRemote (Int32 sessionId, byte\[] data, Int32 dataLength)

    _Send the audio stream in PCM format from another source instead of audio device capturing (microphone)._
* Int32 PortSIP.PortSIPLib.enableSendVideoStreamToRemote (Int32 sessionId, Boolean state) _Enable the SDK send video stream data to remote side from another source instead of camera._
*   Int32 PortSIP.PortSIPLib.sendVideoStreamToRemote (Int32 sessionId, byte\[] data, Int32 dataLength, Int32 width, Int32 height)

    _Send the video stream to remote side._
* Int32 PortSIP.PortSIPLib.enableSendScreenStreamToRemote (Int32 sessionId, Boolean state) _Enable the SDK send Screen stream data to remote side from selected screen source instead of camera._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.enableSendPcmStreamToRemote (Int32 **_**sessionId**_**, Boolean **_**state**_**, Int32 **_**streamSamplesPerSec**_**)**

Enable the SDK to send PCM stream data to remote side from another source instead of microphone.

**Parameters**

| _sessionId_            | The session ID of call.                                            |
| ---------------------- | ------------------------------------------------------------------ |
| _state_                | Set to true to enable the send stream, or false to disable.        |
| _streamSamplesPer Sec_ | The PCM stream data sample in seconds. For example: 8000 or 16000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

This function MUST be called first to send the PCM stream data to another side.

44

**Int32 PortSIP.PortSIPLib.sendPcmStreamToRemote (Int32 **_**sessionId**_**, byte\[] **_**data**_**, Int32 **_**dataLength**_**)**

Send the audio stream in PCM format from another source instead of audio device capturing (microphone).

**Parameters**

| _sessionId_  | Session ID of the call conversation.               |
| ------------ | -------------------------------------------------- |
| _data_       | The PCM audio stream data. It must be 16bit, mono. |
| _dataLength_ | The size of data.                                  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Usually it should be used as below:

enableSendPcmStreamToRemote(sessionId, true, 16000); sendPcmStreamToRemote(sessionId, data, dataSize);

You can't have too much audio data at one time as we have 100ms audio buffer only. Once you put too much, data will be lost. It is recommended to send 20ms audio data every 20ms.

**Int32 PortSIP.PortSIPLib.enableSendVideoStreamToRemote (Int32 **_**sessionId**_**, Boolean **_**state**_**)**

Enable the SDK send video stream data to remote side from another source instead of camera. **Parameters**

| _sessionId_ | The session ID of call.                                        |
| ----------- | -------------------------------------------------------------- |
| _state_     | Set to true to enable the sending stream, or false to disable. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.sendVideoStreamToRemote (Int32 **_**sessionId**_**, byte\[] **_**data**_**, Int32 **_**dataLength**_**, Int32 **_**width**_**, Int32 **_**height**_**)**

Send the video stream to remote side. **Parameters**

| _sessionId_  | Session ID of the call conversation.              |
| ------------ | ------------------------------------------------- |
| _data_       | The video stream data. It must be in i420 format. |
| _dataLength_ | The size of data.                                 |
| _width_      | The video image width.                            |
| _height_     | The video image height.                           |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Send the video stream in i420 from another source instead of video device capturing (camera).

Before calling this function, you MUST call the enableSendVideoStreamToRemote function.

45

Usually it should be used as below:

enableSendVideoStreamToRemote(sessionId, true); sendVideoStreamToRemote(sessionId, data, dataSize, 352, 288);

**Int32 PortSIP.PortSIPLib.enableSendScreenStreamToRemote (Int32 **_**sessionId**_**, Boolean **_**state**_**)**

Enable the SDK send Screen stream data to remote side from selected screen source instead of camera.

**Parameters**

| _sessionId_ | The session ID of call.                                        |
| ----------- | -------------------------------------------------------------- |
| _state_     | Set to true to enable the sending stream, or false to disable. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

46

**RTP packets, Audio stream and video stream callback functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.enableAudioStreamCallback (Int32 sessionId, Boolean enable, DIRECTION\_MODE direction)

    _Enable/disable the audio stream callback._
*   Int32 PortSIP.PortSIPLib.enableVideoStreamCallback (Int32 sessionId, DIRECTION\_MODE direction)

    _Enable/disable the video stream callback._
*   Int32 PortSIP.PortSIPLib.enableScreenStreamCallback (Int32 sessionId, DIRECTION\_MODE direction)

    _Enable/disable the video stream callback._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.enableAudioStreamCallback (Int32 **_**sessionId**_**, Boolean **_**enable**_**, DIRECTION\_MODE **_**direction**_**)**

Enable/disable the audio stream callback. **Parameters**

| _sessionId_           | The session ID of call.                                                                  |   |   |
| --------------------- | ---------------------------------------------------------------------------------------- | - | - |
| _enable_              | Set to true to enable audio stream callback, or false to stop the callback.              |   |   |
| _direction_           | The audio stream callback direction.                                                     |   |   |
| Type                  | Description                                                                              |   |   |
| DIRECTION\_SEND       | Callback the send audio stream for one channel based on the given sessionId.             |   |   |
| DIRECTION\_RECV       | Callback the received audio stream for one channel based on the given sessionId.         |   |   |
| DIRECTION\_SEND\_RECV | Callback both send & received audio stream for one channel based on the given sessionId. |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onAudioRawCallback event will be triggered if the callback is enabled.

47

**Int32 PortSIP.PortSIPLib.enableVideoStreamCallback (Int32 **_**sessionId**_**, DIRECTION\_MODE **_**direction**_**)**

Enable/disable the video stream callback. **Parameters**

| _callbackObject_      | The callback object that you passed in can be accessed once callback function triggered. |   |   |
| --------------------- | ---------------------------------------------------------------------------------------- | - | - |
| _sessionId_           | The session ID of call.                                                                  |   |   |
| _direction_           | The video stream callback direction.                                                     |   |   |
| Type                  | Description                                                                              |   |   |
| DIRECTION\_SEND       | Callback the send video stream for one channel based on the given sessionId.             |   |   |
| DIRECTION\_RECV       | Callback the received video stream for one channel based on the given sessionId.         |   |   |
| DIRECTION\_SEND\_RECV | Callback both send & received video stream for one channel based on the given sessionId. |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onVideoRawCallback event will be triggered if the callback is enabled.

**Int32 PortSIP.PortSIPLib.enableScreenStreamCallback (Int32 **_**sessionId**_**, DIRECTION\_MODE **_**direction**_**)**

Enable/disable the video stream callback. **Parameters**

| _callbackObject_      | The callback object that you passed in can be accessed once callback function triggered. |   |   |
| --------------------- | ---------------------------------------------------------------------------------------- | - | - |
| _sessionId_           | The session ID of call.                                                                  |   |   |
| _direction_           | The video stream callback direction.                                                     |   |   |
| Type                  | Description                                                                              |   |   |
| DIRECTION\_SEND       | Callback the send video stream for one channel based on the given sessionId.             |   |   |
| DIRECTION\_RECV       | Callback the received video stream for one channel based on the given sessionId.         |   |   |
| DIRECTION\_SEND\_RECV | Callback both send & received video stream for one channel based on the given sessionId. |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onVideoRawCallback event will be triggered if the callback is enabled.

48

**Record functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.startRecord (Int32 sessionId, String recordFilePath, String recordFileName, Boolean appendTimestamp, Int32 channels, FILE\_FORMAT recordFileFormat, RECORD\_MODE audioRecordMode, RECORD\_MODE videoRecordMode)

    _Start recording the call._
* Int32 PortSIP.PortSIPLib.stopRecord (Int32 sessionId) _Stop record._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.startRecord (Int32 **_**sessionId**_**, String **_**recordFilePath**_**, String **_**recordFileName**_**, Boolean **_**appendTimestamp**_**, Int32 **_**channels**_**, FILE\_FORMAT **_**recordFileFormat**_**, RECORD\_MODE **_**audioRecordMode**_**, RECORD\_MODE **_**videoRecordMode**_**)**

Start recording the call. **Parameters**

| _sessionId_        | The session ID of call conversation.                                             |
| ------------------ | -------------------------------------------------------------------------------- |
| _recordFilePath_   | The file path to which the record file will be saved. It must be existent.       |
| _recordFileName_   | The file name of record file. For example: audiorecord.wav or videorecord.avi.   |
| _appendTimestamp_  | Set to true to append the timestamp to the recording file name.                  |
| _channels_         | Set to record file audio channels, 1 - mono 2 - stereo.                          |
| _recordFileFormat_ | The file format for the recording.                                               |
| _audioRecordMode_  | Allow to set audio recording mode. Support to record received and/or sent audio. |
| _videoRecordMode_  | Allow to set video recording mode. Support to record received and/or sent video. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.stopRecord (Int32 **_**sessionId**_**)**

Stop record.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

49

**Play audio and video file to remote functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.startPlayingFileToRemote (Int32 sessionId, String fileName, Boolean loop, Int32 playAudio)

    _Play a file to remote party._
* Int32 PortSIP.PortSIPLib.stopPlayingFileToRemote (Int32 sessionId) _Stop playing file to remote party._
*   Int32 PortSIP.PortSIPLib.startPlayingFileLocally (String fileUrl, Boolean loop, IntPtr playVideoWindow)

    _Play a file to remote party._
* Int32 PortSIP.PortSIPLib.stopPlayingFileLocally () _Stop playing file to locally._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.startPlayingFileToRemote (Int32 **_**sessionId**_**, String **_**fileName**_**, Boolean **_**loop**_**, Int32 **_**playAudio**_**)**

Play a file to remote party. **Parameters**

| _sessionId_ | Session ID of the call.                                                                  |
| ----------- | ---------------------------------------------------------------------------------------- |
| _fileUrl_   | url or file name, such as "/test.mp4","/test.wav".                                       |
| _loop_      | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _playAudio_ | 0 - Not play file audio. 1 - Play file audio, 2 - Play file audio mix with Mic.          |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.stopPlayingFileToRemote (Int32 **_**sessionId**_**)**

Stop playing file to remote party.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

51

**Int32 PortSIP.PortSIPLib.startPlayingFileLocally (String **_**fileUrl**_**, Boolean **_**loop**_**, IntPtr **_**playVideoWindow**_**)**

Play a file to remote party. **Parameters**

| _sessionId_       | Session ID of the call.                                                                  |
| ----------------- | ---------------------------------------------------------------------------------------- |
| _fileUrl_         | url or file name, such as "/test.mp4","/test.wav".                                       |
| _loop_            | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _playVideoWindow_ | The PortSIPVideoRenderView used for displaying the video.                                |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.stopPlayingFileLocally ()**

Stop playing file to locally.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Conference functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.createAudioConference ()

    _Create an audio conference. It will be failed if the existent conference is not ended yet._
*   Int32 PortSIP.PortSIPLib.createVideoConference (IntPtr conferenceVideoWindow, Int32 width, Int32 height, Int32 layout)

    _Create a video conference. It will be failed if the existent conference is not ended yet._
* void PortSIP.PortSIPLib.destroyConference () _End the existent conference._
*   Int32 PortSIP.PortSIPLib.setConferenceVideoWindow (IntPtr videoWindow)

    _Set the window for a conference that is used to display the received remote video image._
*   Int32 PortSIP.PortSIPLib.joinToConference (Int32 sessionId)

    _Join a session into existent conference. If the call is in hold, it will be un-hold automatically._
* Int32 PortSIP.PortSIPLib.removeFromConference (Int32 sessionId) _Remove a session from an existent conference._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.createAudioConference ()**

Create an audio conference. It will be failed if the existent conference is not ended yet. **Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.createVideoConference (IntPtr **_**conferenceVideoWindow**_**, Int32 **_**width**_**, Int32 **_**height**_**, Int32 **_**layout**_**)**

Create a video conference. It will be failed if the existent conference is not ended yet. **Parameters**

| _conferenceVideoW indow_ | The UIView used to display the conference video. |
| ------------------------ | ------------------------------------------------ |

53

| _videoResolution_ | The conference video resolution.                                                                                                                                                                            |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _layout_          | Conference Video layout, default is 0 - Adaptive. 0 - Adaptive(1,3,5,6) 1 - Only Local Video 2 - 2 video,PIP 3 - 2 video, Left and right 4 - 2 video, Up and Down 5 - 3 video 6 - 4 split video 7 - 5 video |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setConferenceVideoWindow (IntPtr **_**videoWindow**_**)**

Set the window for a conference that is used to display the received remote video image.

**Parameters**

| _videoWindow_ | The UIView used to display the conference video. |
| ------------- | ------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.joinToConference (Int32 **_**sessionId**_**)**

Join a session into existent conference. If the call is in hold, it will be un-hold automatically. **Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.removeFromConference (Int32 **_**sessionId**_**)**

Remove a session from an existent conference.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

54

**RTP and RTCP QOS functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.setAudioRtcpBandwidth (Int32 sessionId, Int32 BitsRR, Int32 BitsRS, Int32 KBitsAS)

    _Set the audio RTCP bandwidth parameters to the RFC3556._
*   Int32 PortSIP.PortSIPLib.setVideoRtcpBandwidth (Int32 sessionId, Int32 BitsRR, Int32 BitsRS, Int32 KBitsAS)

    _Set the video RTCP bandwidth parameters as the RFC3556._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.setAudioRtcpBandwidth (Int32 **_**sessionId**_**, Int32 **_**BitsRR**_**, Int32 **_**BitsRS**_**, Int32 **_**KBitsAS**_**)**

Set the audio RTCP bandwidth parameters to the RFC3556. **Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |
| _BitsRR_    | The bits for the RR parameter.       |
| _BitsRS_    | The bits for the RS parameter.       |
| _KBitsAS_   | The Kbits for the AS parameter.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoRtcpBandwidth (Int32 **_**sessionId**_**, Int32 **_**BitsRR**_**, Int32 **_**BitsRS**_**, Int32 **_**KBitsAS**_**)**

Set the video RTCP bandwidth parameters as the RFC3556. **Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |
| _BitsRR_    | The bits for the RR parameter.       |
| _BitsRS_    | The bits for the RS parameter.       |
| _KBitsAS_   | The Kbits for the AS parameter.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

55

**RTP statistics functions**

**Functions**

*   Int32 PortSIP.PortSIPLib.getStatistics (Int32 sessionId)

    _Obtain the statistics of channel. the event onStatistics will be triggered._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.getStatistics (Int32 **_**sessionId**_**)**

Obtain the statistics of channel. the event onStatistics will be triggered.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

56

**Audio effect functions**

**Functions**

* void PortSIP.PortSIPLib.enableVAD (Boolean state) _Enable/disable Voice Activity Detection (VAD)._
* void PortSIP.PortSIPLib.enableAEC (Boolean state) _Enable/disable AEC (Acoustic Echo Cancellation)._
* void PortSIP.PortSIPLib.enableCNG (Boolean state) _Enable/disable Comfort Noise Generator (CNG)._
* void PortSIP.PortSIPLib.enableAGC (Boolean state) _Enable/disable Automatic Gain Control (AGC)._
* void PortSIP.PortSIPLib.enableANS (Boolean state) _Enable/disable Audio Noise Suppression (ANS)._
*   Int32 PortSIP.PortSIPLib.enableAudioQos (Boolean state)

    _Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for audio channel._
*   Int32 PortSIP.PortSIPLib.enableVideoQos (Boolean state)

    _Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for video channel._
* Int32 PortSIP.PortSIPLib.setVideoMTU (Int32 mtu) _Set the MTU size for video RTP packet._&#x20;

**Detailed Description Function Documentation**&#x20;

**void PortSIP.PortSIPLib.enableVAD (Boolean **_**state**_**)**

Enable/disable Voice Activity Detection (VAD). **Parameters**

| _state_ | Set to true to enable VAD, or false to disable. |
| ------- | ----------------------------------------------- |

**void PortSIP.PortSIPLib.enableAEC (Boolean **_**state**_**)**

Enable/disable AEC (Acoustic Echo Cancellation).

57

**Parameters**

| _state_ | Set it to true to enable AEC, or false to disable. |
| ------- | -------------------------------------------------- |

**void PortSIP.PortSIPLib.enableCNG (Boolean **_**state**_**)**

Enable/disable Comfort Noise Generator (CNG). **Parameters**

| _state_ | Set it to true to enable CNG, or false to disable. |
| ------- | -------------------------------------------------- |

**void PortSIP.PortSIPLib.enableAGC (Boolean **_**state**_**)**

Enable/disable Automatic Gain Control (AGC). **Parameters**

| _state_ | Set it to true to enable AGC, or false to disable. |
| ------- | -------------------------------------------------- |

**void PortSIP.PortSIPLib.enableANS (Boolean **_**state**_**)**

Enable/disable Audio Noise Suppression (ANS). **Parameters**

| _state_ | Set it to true to enable ANS, or false to disable. |
| ------- | -------------------------------------------------- |

**Int32 PortSIP.PortSIPLib.enableAudioQos (Boolean **_**state**_**)**

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for audio channel.

**Parameters**

| _state_ | Set to true to enable audio QoS, and DSCP value will be 46; or false to disable audio QoS, and DSCP value will be 0. |
| ------- | -------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.enableVideoQos (Boolean **_**state**_**)**

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for video channel.

**Parameters**

| _state_ | Set as true to enable video QoS and DSCP value will be 34; or false to disable Video Qos , and DSCP value will be 0. |
| ------- | -------------------------------------------------------------------------------------------------------------------- |

58

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setVideoMTU (Int32 **_**mtu**_**)**

Set the MTU size for video RTP packet.

**Parameters**

| _mtu_ | Set MTU value. Allow value ranges (512-65507). Other value will be modified to the default 1450. |
| ----- | ------------------------------------------------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

59

**Send OPTIONS/INFO/MESSAGE functions**

**Functions**

* Int32 PortSIP.PortSIPLib.sendOptions (String to, String sdp) _Send OPTIONS message._
*   Int32 PortSIP.PortSIPLib.sendInfo (Int32 sessionId, String mimeType, String subMimeType, String infoContents)

    _Send a INFO message to remote side in a call._
* Int32 PortSIP.PortSIPLib.sendSubscription (String to, String eventName) _Send a SUBSCRIBE message to subscribe an event._
* Int32 PortSIP.PortSIPLib.terminateSubscription (Int32 subscribeId) _Terminate the given subscription._
*   Int32 PortSIP.PortSIPLib.sendMessage (Int32 sessionId, String mimeType, String subMimeType, byte\[] message, Int32 messageLength)

    _Send a MESSAGE message to remote side in dialog._
*   Int32 PortSIP.PortSIPLib.sendOutOfDialogMessage (String to, String mimeType, String subMimeType, Boolean isSMS, byte\[] message, Int32 messageLength)

    _Send an out of dialog MESSAGE message to remote side._
*   Int32 PortSIP.PortSIPLib.setDefaultSubscriptionTime (Int32 secs)

    _Set the default expiration time to be used when creating a subscription._
*   Int32 PortSIP.PortSIPLib.setDefaultPublicationTime (Int32 secs)

    _Set the default expiration time to be used when creating a publication._
*   Int32 PortSIP.PortSIPLib.setPresenceMode (Int32 mode)

    _Indicate the SDK uses the P2P mode for presence or presence agent mode._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.sendOptions (String **_**to**_**, String **_**sdp**_**)**

Send OPTIONS message.

**Parameters**

| _to_ | The recipient of OPTIONS message. |
| ---- | --------------------------------- |

60

| _sdp_ | The SDP of OPTIONS message. It's optional if user does not wish to send the SDP with OPTIONS message. |
| ----- | ----------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.sendInfo (Int32 **_**sessionId**_**, String **_**mimeType**_**, String **_**subMimeType**_**, String **_**infoContents**_**)**

Send a INFO message to remote side in a call. **Parameters**

| _sessionId_    | The session ID of call.                      |
| -------------- | -------------------------------------------- |
| _mimeType_     | The mime type of INFO message.               |
| _subMimeType_  | The sub mime type of INFO message.           |
| _infoContents_ | The contents that is sent with INFO message. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.sendSubscription (String **_**to**_**, String **_**eventName**_**)**

Send a SUBSCRIBE message to subscribe an event.

**Parameters**

| _to_        | The user/extension to be subscribed. |
| ----------- | ------------------------------------ |
| _eventName_ | The event name to be subscribed.     |

**Returns**

If the function succeeds, it will return the ID of SUBSCRIBE which is greater than 0. If the function fails, it will return a specific error code which is less than 0.

**Remarks**

Example 1, below code indicates that user/extension 101 is subscribed to MWI (Message Waiting notifications) for checking his voicemail: int32 mwiSubId = sendSubscription("sip:101@test.com", "message-summary");

Example 2, to monitor a user/extension call status, You can use code: sendSubscription("100", "dialog"); Extension 100 refers to the user/extension to be monitored. Once being monitored, when extension 100 hold a call or is ringing, the

onDialogStateUpdated callback will be triggered.

**Int32 PortSIP.PortSIPLib.terminateSubscription (Int32 **_**subscribeId**_**)**

Terminate the given subscription. **Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

61

**Remarks**

For example, if you want stop check the MWI, use below code:

terminateSubscription(mwiSubId);&#x20;

**Int32 PortSIP.PortSIPLib.sendMessage (Int32 **_**sessionId**_**, String **_**mimeType**_**, String **_**subMimeType**_**, byte\[] **_**message**_**, Int32 **_**messageLength**_**)**

Send a MESSAGE message to remote side in dialog. **Parameters**

| _sessionId_     | The session ID of the call.                                           |
| --------------- | --------------------------------------------------------------------- |
| _mimeType_      | The mime type of MESSAGE message.                                     |
| _subMimeType_   | The sub mime type of MESSAGE message.                                 |
| _message_       | The contents which is sent with MESSAGE message. Binary data allowed. |
| _messageLength_ | The message size.                                                     |

**Returns**

If the function succeeds, it will return a message ID that allows to track the message sending state in onSendMessageSuccess and onSendMessageFailure. If the function fails, it will return a specific error code less than 0.

**Remarks**

Example 1: send a plain text message. Note: to send text in other languages, please use the UTF-8 to encode the message before sending.

sendMessage(sessionId, "text", "plain", "hello",6);&#x20;

Example 2: send a binary message.

sendMessage(sessionId, "application", "vnd.3gpp.sms", binData, binDataSize);

**Int32 PortSIP.PortSIPLib.sendOutOfDialogMessage (String **_**to**_**, String **_**mimeType**_**, String **_**subMimeType**_**, Boolean **_**isSMS**_**, byte\[] **_**message**_**, Int32 **_**messageLength**_**)**

Send an out of dialog MESSAGE message to remote side. **Parameters**

| _to_            | The message recipient, such as sip[:receiver@portsip.com](mailto:receiver@portsip.com)                                       |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| _mimeType_      | The mime type of MESSAGE message.                                                                                            |
| _subMimeType_   | The sub mime type of MESSAGE message. @isSMS isSMS Set to YES to specify "messagetype=SMS" in the To line, or NO to disable. |
| _message_       | The contents which is sent with MESSAGE message. Binary data allowed.                                                        |
| _messageLength_ | The message size.                                                                                                            |

**Returns**

If the function succeeds, it will return a message ID that allows to track the message sending state in onSendOutOfMessageSuccess and onSendOutOfMessageFailure. If the function fails, it will return a specific error code less than 0.

**Remarks**

Example 1: send a plain text message. Note: to send text in other languages, please use the UTF-8 to encode the message before sending.

sendOutOfDialogMessage("sip:user1@sip.portsip.com", "text", "plain", false, "hello", 6);

Example 2: send a binary message.

62

sendOutOfDialogMessage("sip:user1@sip.portsip.com","application", "vnd.3gpp.sms", false, binData, binDataSize);

**Int32 PortSIP.PortSIPLib.setDefaultSubscriptionTime (Int32 **_**secs**_**)**

Set the default expiration time to be used when creating a subscription. **Parameters**

| _secs_ | The default expiration time of subscription. |
| ------ | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setDefaultPublicationTime (Int32 **_**secs**_**)**

Set the default expiration time to be used when creating a publication.

**Parameters**

| _secs_ | The default expiration time of publication. |
| ------ | ------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setPresenceMode (Int32 **_**mode**_**)**

Indicate the SDK uses the P2P mode for presence or presence agent mode. **Parameters**

| _mode_ | 0 - P2P mode; 1 - Presence Agent mode, default is P2P mode. |
| ------ | ----------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Since presence agent mode requires the PBX/Server support the PUBLISH, please ensure you have your and PortSIP PBX support this feature. For more details please visit: [https://www.portsip.com/portsip-pbx](https://www.portsip.com/portsip-pbx)

63

**Presence functions**

**Functions**

* Int32 PortSIP.PortSIPLib.presenceSubscribe (String to, String subject) _Send a SUBSCRIBE message for subscribing the contact's presence status._
* Int32 PortSIP.PortSIPLib.presenceTerminateSubscribe (Int32 subscribeId) _Terminate the given presence subscription._
* Int32 PortSIP.PortSIPLib.presenceRejectSubscribe (Int32 subscribeId) _Reject a presence SUBSCRIBE request which is received from contact._
* Int32 PortSIP.PortSIPLib.presenceAcceptSubscribe (Int32 subscribeId) _Accept the presence SUBSCRIBE request which is received from contact._
* Int32 PortSIP.PortSIPLib.setPresenceStatus (Int32 subscribeId, String stateText) _Set the presence status._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.presenceSubscribe (String **_**to**_**, String **_**subject**_**)**

Send a SUBSCRIBE message for subscribing the contact's presence status. **Parameters**

| _to_      | The target contact. It must be like sip[:contact001@sip.portsip.com.](mailto:contact001@sip.portsip.com)                                                                           |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _subject_ | <p>This subject text will be inserted into the SUBSCRIBE message. For example: "Hello, I'm Jason".</p><p>The subject maybe in UTF-8 format. You should use UTF-8 to decode it.</p> |

**Returns**

If the function succeeds, it will return value subscribeId. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.presenceTerminateSubscribe (Int32 **_**subscribeId**_**)**

Terminate the given presence subscription.

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

64

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.presenceRejectSubscribe (Int32 **_**subscribeId**_**)**

Reject a presence SUBSCRIBE request which is received from contact.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event includes the subscription ID. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

If the P2P presence mode is enabled, when someone subscribe your presence status, you will receive the subscribe request in the callback, and you can use this function to accept it.

**Int32 PortSIP.PortSIPLib.presenceAcceptSubscribe (Int32 **_**subscribeId**_**)**

Accept the presence SUBSCRIBE request which is received from contact. **Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event will include the subscription ID. |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

If the P2P presence mode is enabled, when someone subscribes your presence status, you will receive the subscription request in the callback, and you can use this function to reject it.

**Int32 PortSIP.PortSIPLib.setPresenceStatus (Int32 **_**subscribeId**_**, String **_**stateText**_**)**

Set the presence status.

**Parameters**

| _subscribeId_ | Subscription ID.                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _stateText_   | The state text of presence online. For example: "I'm here". If you want to appear as offline to others, please pass the "Offline" to "statusText" parameter. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

With P2P presence mode, when receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event includes the subscription ID. This

65

function will cause the SDK sending a NOTIFY message to update your presence status, and you must pass the correct subscribeId.

With presence agent mode, this function will cause the SDK to send a PUBLISH message to update your presence status, and you must pass 0 to the "subscribeId" parameter.

66

**Device Manage functions.**

**Functions**

* Int32 PortSIP.PortSIPLib.getNumOfRecordingDevices () _Gets the count of audio devices available for audio recording._
*   Int32 PortSIP.PortSIPLib.getNumOfPlayoutDevices ()

    _Gets the number of audio devices available for audio playout._
*   Int32 PortSIP.PortSIPLib.getRecordingDeviceName (Int32 deviceIndex, StringBuilder nameUTF8, Int32 nameUTF8Length)

    _Gets the name of a specific recording device given by an index._
*   Int32 PortSIP.PortSIPLib.getPlayoutDeviceName (Int32 deviceIndex, StringBuilder nameUTF8, Int32 nameUTF8Length)

    _Get the name of a specific playout device given by an index._
* Int32 PortSIP.PortSIPLib.setSpeakerVolume (Int32 volume) _Set the speaker volume level._
* Int32 PortSIP.PortSIPLib.getSpeakerVolume () _Gets the speaker volume level._
* Int32 PortSIP.PortSIPLib.setMicVolume (Int32 volume) _Sets the microphone volume level._
* Int32 PortSIP.PortSIPLib.getMicVolume () _Retrieves the current microphone volume._
* Int32 PortSIP.PortSIPLib.getScreenSourceCount () _Retrieves the current number of screen._
*   Int32 PortSIP.PortSIPLib.getScreenSourceTitle (Int32 deviceIndex, StringBuilder nameUTF8, Int32 nameUTF8Length)

    _Retrieves the current screen title ._
* Int32 PortSIP.PortSIPLib.selectScreenSource (Int32 nDeviceIndex) _Sets the Screen to share ._
* Int32 PortSIP.PortSIPLib.SetScreenFrameRate (Int32 nFrameRate) _Sets the Screen video framerate ._
* Int32 PortSIP.PortSIPLib.setScreenVideoWindow (Int32 sessionId, IntPtr screenVideoWindow) _Set the window for a session that is used to display the received screen video ._
* void PortSIP.PortSIPLib.audioPlayLoopbackTest (Boolean enable) _Use it for the audio device loop back test._

67

* Int32 PortSIP.PortSIPLib.getNumOfVideoCaptureDevices () _Get the number of available capturing devices._
*   Int32 PortSIP.PortSIPLib.getVideoCaptureDeviceName (Int32 deviceIndex, StringBuilder uniqueIdUTF8, Int32 uniqueIdUTF8Length, StringBuilder deviceNameUTF8, Int32 deviceNameUTF8Length)

    _Get the name of a specific video capture device given by an index._
*   Int32 PortSIP.PortSIPLib.showVideoCaptureSettingsDialogBox (String uniqueIdUTF8, Int32 uniqueIdUTF8Length, String dialogTitle, IntPtr parentWindow, Int32 x, Int32 y)

    _Display the capture device property dialog box for the specified capture device._&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.PortSIPLib.getNumOfRecordingDevices ()**

Gets the count of audio devices available for audio recording. **Returns**

It will return the count of recording devices. If the function fails, it will return a specific error code less than 0.

**Int32 PortSIP.PortSIPLib.getNumOfPlayoutDevices ()**

Gets the number of audio devices available for audio playout. **Returns**

It will return the count of playout devices. If the function fails, it will return a specific error code less than 0.

**Int32 PortSIP.PortSIPLib.getRecordingDeviceName (Int32 **_**deviceIndex**_**, StringBuilder **_**nameUTF8**_**, Int32 **_**nameUTF8Length**_**)**

Gets the name of a specific recording device given by an index. **Parameters**

| _deviceIndex_ | Device index (0, 1, 2, ..., N-1), where N is given by getNumOfRecordingDevices (). Also -1 is a valid value and will return the name of the default recording device. |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _nameUTF8_    | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                               |

68

| _nameUTF8Length_ | The size of nameUTF8 buffer. It cannot be less than 128. |
| ---------------- | -------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.getPlayoutDeviceName (Int32 **_**deviceIndex**_**, StringBuilder **_**nameUTF8**_**, Int32 **_**nameUTF8Length**_**)**

Get the name of a specific playout device given by an index. **Parameters**

| _deviceIndex_    |                                                                                                                                                                       |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _deviceIndex_    | Device index (0, 1, 2, ..., N-1), where N is given by getNumOfRecordingDevices (). Also -1 is a valid value and will return the name of the default recording device. |
| _nameUTF8_       | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                               |
| _nameUTF8Length_ | The size of nameUTF8 buffer. It cannot be less than 128.                                                                                                              |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setSpeakerVolume (Int32 **_**volume**_**)**

Set the speaker volume level.

**Parameters**

| _volume_ | Volume level of speaker. Valid value ranges 0 - 255. |
| -------- | ---------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.getSpeakerVolume ()**

Gets the speaker volume level.

**Returns**

If the function succeeds, it will return the speaker volume with valid range 0 - 255. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.setMicVolume (Int32 **_**volume**_**)**

Sets the microphone volume level.

**Parameters**

| _volume_ | The microphone volume level. Valid value ranges 0 - 255. |
| -------- | -------------------------------------------------------- |

69

**Returns**

If the function succeeds, the return value is 0. If the function fails, the return value is a specific error code.

**Int32 PortSIP.PortSIPLib.getMicVolume ()**

Retrieves the current microphone volume. **Returns**

If the function succeeds, it will return the microphone volume. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.getScreenSourceCount ()**

Retrieves the current number of screen.

**Returns**

If the function succeeds, it will return the screen number. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.getScreenSourceTitle (Int32 **_**deviceIndex**_**, StringBuilder **_**nameUTF8**_**, Int32 **_**nameUTF8Length**_**)**

Retrieves the current screen title . **Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.selectScreenSource (Int32 **_**nDeviceIndex**_**)**

Sets the Screen to share .

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

**Int32 PortSIP.PortSIPLib.SetScreenFrameRate (Int32 **_**nFrameRate**_**)**

Sets the Screen video framerate .

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

70

**Int32 PortSIP.PortSIPLib.setScreenVideoWindow (Int32 **_**sessionId**_**, IntPtr **_**screenVideoWindow**_**)**

Set the window for a session that is used to display the received screen video . **Parameters**

| _sessionId_          | The session ID of the call.                        |
| -------------------- | -------------------------------------------------- |
| _remoteVideoWindo w_ | The window to display received remote video image. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**void PortSIP.PortSIPLib.audioPlayLoopbackTest (Boolean **_**enable**_**)**

Use it for the audio device loop back test.

**Parameters**

| _enable_ | Set to true to start audio look back test; or fase to stop. |
| -------- | ----------------------------------------------------------- |

**Int32 PortSIP.PortSIPLib.getNumOfVideoCaptureDevices ()**

Get the number of available capturing devices.

**Returns**

It will return the count of video capturing devices. If it fails, it will return a specific error code less than 0.

**Int32 PortSIP.PortSIPLib.getVideoCaptureDeviceName (Int32 **_**deviceIndex**_**, StringBuilder **_**uniqueIdUTF8**_**, Int32 **_**uniqueIdUTF8Length**_**, StringBuilder **_**deviceNameUTF8**_**, Int32 **_**deviceNameUTF8Length**_**)**

Get the name of a specific video capture device given by an index. **Parameters**

| _deviceIndex_           | Device index (0, 1, 2, ..., N-1), where N is given by getNumOfVideoCaptureDevices (). Also -1 is a valid value and will return the name of the default capturing device. |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _uniqueIdUTF8_          | Unique identifier of the capturing device.                                                                                                                               |
| _uniqueIdUTF8Len gth_   | Size in bytes of uniqueIdUTF8.                                                                                                                                           |
| _deviceNameUTF8_        | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                                  |
| _deviceNameUTF8 Length_ | The size of nameUTF8 buffer. It cannot be less than 128.                                                                                                                 |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

71

**Int32 PortSIP.PortSIPLib.showVideoCaptureSettingsDialogBox (String **_**uniqueIdUTF8**_**, Int32 **_**uniqueIdUTF8Length**_**, String **_**dialogTitle**_**, IntPtr **_**parentWindow**_**, Int32 **_**x**_**, Int32 **_**y**_**)**

Display the capture device property dialog box for the specified capture device. **Parameters**

| _uniqueIdUTF8_        | Unique identifier of the capture device.                                     |
| --------------------- | ---------------------------------------------------------------------------- |
| _uniqueIdUTF8Len gth_ | Size in bytes of uniqueIdUTF8.                                               |
| _dialogTitle_         | The title of the video settings dialog.                                      |
| _parentWindow_        | Parent window used for the dialog box. It should originally be a HWND.       |
| _x_                   | Horizontal position for the dialog relative to the parent window, in pixels. |
| _y_                   | Vertical position for the dialog relative to the parent window, in pixels.   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

72

**SDK Callback events**

**Modules**

* Register events
* Call events
* Refer events
* Signaling events
* MWI events
* DTMF events
* INFO/OPTIONS message events
* Presence events
* Play audio and video file finished events
* RTP callback events
* Audio and video stream callback events&#x20;

**Detailed Description** SDK Callback events

73

**Register events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onRegisterSuccess (String statusText, Int32 statusCode, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onRegisterFailure (String statusText, Int32 statusCode, StringBuilder sipMessage)&#x20;

**Detailed Description** Register events&#x20;

**Function Documentation**

**Int32 PortSIP.SIPCallbackEvents.onRegisterSuccess (String **_**statusText**_**, Int32 **_**statusCode**_**, StringBuilder **_**sipMessage**_**)**

When successfully registered to server, this event will be triggered. **Parameters**

| _callbackIndex_  | This is a callback index passed in when creating the SDK library.  |
| ---------------- | ------------------------------------------------------------------ |
| _callbackObject_ | This is a callback object passed in when creating the SDK library. |
| _statusText_     | The status text.                                                   |
| _statusCode_     | The status code.                                                   |
| _sipMessage_     | The SIP message received.                                          |

**Int32 PortSIP.SIPCallbackEvents.onRegisterFailure (String **_**statusText**_**, Int32 **_**statusCode**_**, StringBuilder **_**sipMessage**_**)**

If registration to SIP server fails, this event will be triggered. **Parameters**

| _statusText_ | The status text.          |
| ------------ | ------------------------- |
| _statusCode_ | The status code.          |
| _sipMessage_ | The SIP message received. |

74

**Call events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onInviteIncoming (Int32 sessionId, String callerDisplayName, String caller, String calleeDisplayName, String callee, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onInviteTrying (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onInviteSessionProgress (Int32 sessionId, String audioCodecNames, String videoCodecNames, Boolean existsEarlyMedia, Boolean existsAudio, Boolean existsVideo, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onInviteRinging (Int32 sessionId, String statusText, Int32 statusCode, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onInviteAnswered (Int32 sessionId, String callerDisplayName, String caller, String calleeDisplayName, String callee, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onInviteFailure (Int32 sessionId, String callerDisplayName, String caller, String calleeDisplayName, String callee, String reason, Int32 code, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onInviteUpdated (Int32 sessionId, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo, Boolean existsScreen, StringBuilder sipMessage)
* Int32 PortSIP.SIPCallbackEvents.onInviteConnected (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onInviteBeginingForward (String forwardTo)
* Int32 PortSIP.SIPCallbackEvents.onInviteClosed (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onDialogStateUpdated (String BLFMonitoredUri, String BLFDialogState, String BLFDialogId, String BLFDialogDirection)
* Int32 PortSIP.SIPCallbackEvents.onRemoteHold (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onRemoteUnHold (Int32 sessionId, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onInviteIncoming (Int32 **_**sessionId**_**, String **_**callerDisplayName**_**, String **_**caller**_**, String **_**calleeDisplayName**_**, String **_**callee**_**, String **_**audioCodecNames**_**, String **_**videoCodecNames**_**, Boolean **_**existsAudio**_**, Boolean **_**existsVideo**_**, StringBuilder **_**sipMessage**_**)**

When the call is coming, this event will be triggered. **Parameters**

| _sessionId_          | The session ID of the call.                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | The display name of caller                                                                      |
| _caller_             | The caller.                                                                                     |
| _calleeDisplayNam e_ | The display name of callee.                                                                     |
| _callee_             | The callee.                                                                                     |
| _audioCodecNames_    | <p>The matched audio codecs. It's separated by "#" if there are more than one</p><p>codecs.</p> |
| _videoCodecNames_    | The matched video codecs. It's separated by "#" if there are more than one codecs.              |

75

| _existsAudio_ | If it's true, it indicates that this call includes the audio. |
| ------------- | ------------------------------------------------------------- |
| _existsVideo_ | If it's true, it indicates that this call includes the video. |
| _sipMessage_  | The SIP message received.                                     |

**Int32 PortSIP.SIPCallbackEvents.onInviteTrying (Int32 **_**sessionId**_**)**

If the outgoing call is being processed, this event will be triggered. **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onInviteSessionProgress (Int32 **_**sessionId**_**, String **_**audioCodecNames**_**, String **_**videoCodecNames**_**, Boolean **_**existsEarlyMedia**_**, Boolean **_**existsAudio**_**, Boolean **_**existsVideo**_**, StringBuilder **_**sipMessage**_**)**

Once the caller received the "183 session progress" message, this event will be triggered. **Parameters**

| _sessionId_        | The session ID of the call.                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| _audioCodecNames_  | <p>The matched audio codecs. It's separated by "#" if there are more than one</p><p>codecs.</p> |
| _videoCodecNames_  | The matched video codecs. It's separated by "#" if there are more than one codecs.              |
| _existsEarlyMedia_ | If it's true, it indicates that the call has early media.                                       |
| _existsAudio_      | If it's true, it indicates that this call includes the audio.                                   |
| _existsVideo_      | If it's true, it indicates that this call includes the video.                                   |
| _sipMessage_       | The SIP message received.                                                                       |

**Int32 PortSIP.SIPCallbackEvents.onInviteRinging (Int32 **_**sessionId**_**, String **_**statusText**_**, Int32 **_**statusCode**_**, StringBuilder **_**sipMessage**_**)**

If the outgoing call was ringing, this event would be triggered. **Parameters**

| _sessionId_  | The session ID of the call. |
| ------------ | --------------------------- |
| _statusText_ | The status text.            |
| _statusCode_ | The status code.            |
| _sipMessage_ | The SIP message received.   |

**Int32 PortSIP.SIPCallbackEvents.onInviteAnswered (Int32 **_**sessionId**_**, String **_**callerDisplayName**_**, String **_**caller**_**, String **_**calleeDisplayName**_**, String **_**callee**_**, String **_**audioCodecNames**_**, String **_**videoCodecNames**_**, Boolean **_**existsAudio**_**, Boolean **_**existsVideo**_**, StringBuilder **_**sipMessage**_**)**

If the remote party answered the call, this event would be triggered. **Parameters**

| _sessionId_          | The session ID of the call.                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | The display name of caller                                                                      |
| _caller_             | The caller.                                                                                     |
| _calleeDisplayNam e_ | The display name of callee.                                                                     |
| _callee_             | The callee.                                                                                     |
| _audioCodecNames_    | <p>The matched audio codecs. It's separated by "#" if there are more than one</p><p>codecs.</p> |
| _videoCodecNames_    | The matched video codecs. It's separated by "#" if there are more than one codecs.              |
| _existsAudio_        | If it's true, it indicates that this call includes the audio.                                   |
| _existsVideo_        | If it's true, it indicates that this call includes the video.                                   |

76

| _sipMessage_ | The SIP message received. |
| ------------ | ------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onInviteFailure (Int32 **_**sessionId**_**, String **_**callerDisplayName**_**, String **_**caller**_**, String **_**calleeDisplayName**_**, String **_**callee**_**, String **_**reason**_**, Int32 **_**code**_**, StringBuilder **_**sipMessage**_**)**

If the outgoing call fails, this event will be triggered. **Parameters**

| _sessionId_          | The session ID of the call. |
| -------------------- | --------------------------- |
| _callerDisplayNam e_ | The display name of caller  |
| _caller_             | The caller.                 |
| _calleeDisplayNam e_ | The display name of callee. |
| _callee_             | The callee.                 |
| _reason_             | The failure reason.         |
| _code_               | The failure code.           |
| _sipMessage_         | The SIP message received.   |

**Int32 PortSIP.SIPCallbackEvents.onInviteUpdated (Int32 **_**sessionId**_**, String **_**audioCodecNames**_**, String **_**videoCodecNames**_**, Boolean **_**existsAudio**_**, Boolean **_**existsVideo**_**, Boolean **_**existsScreen**_**, StringBuilder **_**sipMessage**_**)**

This event will be triggered when remote party updates this call. **Parameters**

| _sessionId_       | The session ID of the call.                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------- |
| _audioCodecNames_ | <p>The matched audio codecs. It's separated by "#" if there are more than one</p><p>codecs.</p> |
| _videoCodecNames_ | The matched video codecs. It's separated by "#" if there are more than one codecs.              |
| _existsAudio_     | If it's true, it indicates that this call includes the audio.                                   |
| _existsVideo_     | If it's true, it indicates that this call includes the video.                                   |
| _sipMessage_      | The SIP message received.                                                                       |

**Int32 PortSIP.SIPCallbackEvents.onInviteConnected (Int32 **_**sessionId**_**)**

This event would be triggered when UAC sent/UAS received ACK(the call is connected). Some functions (hold, updateCall etc...) can be called only after the call connected, otherwise these functions will return error.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onInviteBeginingForward (String **_**forwardTo**_**)**

If the enableCallForward method is called and a call is incoming, the call will be forwarded automatically and this event will be triggered.

**Parameters**

| _forwardTo_ | The forwarding target SIP URI. |
| ----------- | ------------------------------ |

**Int32 PortSIP.SIPCallbackEvents.onInviteClosed (Int32 **_**sessionId**_**)**

This event is triggered once remote side closes the call.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

77

**Int32 PortSIP.SIPCallbackEvents.onDialogStateUpdated (String **_**BLFMonitoredUri**_**, String **_**BLFDialogState**_**, String **_**BLFDialogId**_**, String **_**BLFDialogDirection**_**)**

If a user subscribed and his dialog status monitored, when the monitored user is holding a call or being rang, this event will be triggered

**Parameters**

| _BLFMonitoredUri_     | the monitored user's URI    |
| --------------------- | --------------------------- |
| _BLFDialogState_      | - the status of the call    |
| _BLFDialogId_         | - the id of the call        |
| _BLFDialogDirecti on_ | - the direction of the call |

**Int32 PortSIP.SIPCallbackEvents.onRemoteHold (Int32 **_**sessionId**_**)**

If the remote side placed the call on hold, this event would be triggered. **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onRemoteUnHold (Int32 **_**sessionId**_**, String **_**audioCodecNames**_**, String **_**videoCodecNames**_**, Boolean **_**existsAudio**_**, Boolean **_**existsVideo**_**)**

If the remote side un-hold the call, this event would be triggered. **Parameters**

| _sessionId_       | The session ID of the call.                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------- |
| _audioCodecNames_ | <p>The matched audio codecs. It's separated by "#" if there are more than one</p><p>codecs.</p> |
| _videoCodecNames_ | The matched video codecs. It's separated by "#" if there are more than one codecs.              |
| _existsAudio_     | If it's true, it indicates that this call includes the audio.                                   |
| _existsVideo_     | If it's true, it indicates that this call includes the video.                                   |

78

**Refer events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onReceivedRefer (Int32 sessionId, Int32 referId, String to, String from, StringBuilder referSipMessage)
* Int32 PortSIP.SIPCallbackEvents.onReferAccepted (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onReferRejected (Int32 sessionId, String reason, Int32 code)
* Int32 PortSIP.SIPCallbackEvents.onTransferTrying (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onTransferRinging (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onACTVTransferSuccess (Int32 sessionId)
* Int32 PortSIP.SIPCallbackEvents.onACTVTransferFailure (Int32 sessionId, String reason, Int32 code)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onReceivedRefer (Int32 **_**sessionId**_**, Int32 **_**referId**_**, String **_**to**_**, String **_**from**_**, StringBuilder **_**referSipMessage**_**)**

This event will be triggered once receiving a REFER message. **Parameters**

| _sessionId_       | The session ID of the call.                                        |
| ----------------- | ------------------------------------------------------------------ |
| _referId_         | The ID of the REFER message. Pass it to acceptRefer or rejectRefer |
| _to_              | The refer target.                                                  |
| _from_            | The sender of REFER message.                                       |
| _referSipMessage_ | The SIP message of "REFER". Pass it to "acceptRefer" function.     |

**Int32 PortSIP.SIPCallbackEvents.onReferAccepted (Int32 **_**sessionId**_**)**

This callback will be triggered once remote side called "acceptRefer" to accept the REFER.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onReferRejected (Int32 **_**sessionId**_**, String **_**reason**_**, Int32 **_**code**_**)**

This callback will be triggered once remote side called "rejectRefer" to reject the REFER. **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _reason_    | Reject reason.              |
| _code_      | Reject code.                |

**Int32 PortSIP.SIPCallbackEvents.onTransferTrying (Int32 **_**sessionId**_**)** When the refer call is being processed, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

79

**Int32 PortSIP.SIPCallbackEvents.onTransferRinging (Int32 **_**sessionId**_**)**

When the refer call is ringing, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onACTVTransferSuccess (Int32 **_**sessionId**_**)**

When the refer call succeeds, this event will be triggered. The ACTV means Active. For example: A established the call with B, and A transferred B to C. When C accepts the refer call, A will receive this event.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onACTVTransferFailure (Int32 **_**sessionId**_**, String **_**reason**_**, Int32 **_**code**_**)**

When the refer call fails, this event will be triggered. The ACTV means Active. For example: A established the call with B, and A transfered B to C. When C rejects the refer call, A will receive this event.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _reason_    | The error reason.           |
| _code_      | The error code.             |

80

**Signaling events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onReceivedSignaling (Int32 sessionId, StringBuilder signaling)
* Int32 PortSIP.SIPCallbackEvents.onSendingSignaling (Int32 sessionId, StringBuilder signaling)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onReceivedSignaling (Int32 **_**sessionId**_**, StringBuilder **_**signaling**_**)**

This event will be triggered when receiving an SIP message. **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _signaling_ | The SIP message received.   |

**Int32 PortSIP.SIPCallbackEvents.onSendingSignaling (Int32 **_**sessionId**_**, StringBuilder **_**signaling**_**)**

This event will be triggered when a SIP message sent. **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _signaling_ | The SIP message sent.       |

81

**MWI events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onWaitingVoiceMessage (String messageAccount, Int32 urgentNewMessageCount, Int32 urgentOldMessageCount, Int32 newMessageCount, Int32 oldMessageCount)
* Int32 PortSIP.SIPCallbackEvents.onWaitingFaxMessage (String messageAccount, Int32 urgentNewMessageCount, Int32 urgentOldMessageCount, Int32 newMessageCount, Int32 oldMessageCount)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onWaitingVoiceMessage (String **_**messageAccount**_**, Int32 **_**urgentNewMessageCount**_**, Int32 **_**urgentOldMessageCount**_**, Int32 **_**newMessageCount**_**, Int32 **_**oldMessageCount**_**)**

If there is voice message (MWI) waiting, this event will be triggered. **Parameters**

| _messageAccount_         | Voice message account     |
| ------------------------ | ------------------------- |
| _urgentNewMessag eCount_ | Urgent new message count. |
| _urgentOldMessage Count_ | Urgent old message count. |
| _newMessageCount_        | New message count.        |
| _oldMessageCount_        | Old message count.        |

**Int32 PortSIP.SIPCallbackEvents.onWaitingFaxMessage (String **_**messageAccount**_**, Int32 **_**urgentNewMessageCount**_**, Int32 **_**urgentOldMessageCount**_**, Int32 **_**newMessageCount**_**, Int32 **_**oldMessageCount**_**)**

If there is fax message (MWI) waiting, this event will be triggered. **Parameters**

| _messageAccount_         | Fax message account       |
| ------------------------ | ------------------------- |
| _urgentNewMessag eCount_ | Urgent new message count. |
| _urgentOldMessage Count_ | Urgent old message count. |
| _newMessageCount_        | New message count.        |
| _oldMessageCount_        | Old message count.        |

82

**DTMF events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onRecvDtmfTone (Int32 sessionId, Int32 tone)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onRecvDtmfTone (Int32 **_**sessionId**_**, Int32 **_**tone**_**)**

This event will be triggered when receiving a DTMF tone from remote side.

**Parameters**

| _sessionId_ | The session ID of the call. |   |   |
| ----------- | --------------------------- | - | - |
| _tone_      | DTMF tone.                  |   |   |
| code        | Description                 |   |   |
| 0           | The DTMF tone 0.            |   |   |
| 1           | The DTMF tone 1.            |   |   |
| 2           | The DTMF tone 2.            |   |   |
| 3           | The DTMF tone 3.            |   |   |
| 4           | The DTMF tone 4.            |   |   |
| 5           | The DTMF tone 5.            |   |   |
| 6           | The DTMF tone 6.            |   |   |
| 7           | The DTMF tone 7.            |   |   |
| 8           | The DTMF tone 8.            |   |   |
| 9           | The DTMF tone 9.            |   |   |
| 10          | The DTMF tone \*.           |   |   |
| 11          | The DTMF tone #.            |   |   |
| 12          | The DTMF tone A.            |   |   |
| 13          | The DTMF tone B.            |   |   |
| 14          | The DTMF tone C.            |   |   |
| 15          | The DTMF tone D.            |   |   |
| 16          | The DTMF tone FLASH.        |   |   |

83

**INFO/OPTIONS message events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onRecvOptions (StringBuilder optionsMessage)
* Int32 PortSIP.SIPCallbackEvents.onRecvInfo (StringBuilder infoMessage)
* Int32 PortSIP.SIPCallbackEvents.onRecvNotifyOfSubscription (Int32 subscribeId, StringBuilder notifyMsg, byte\[] contentData, Int32 contentLenght)
* Int32 PortSIP.SIPCallbackEvents.onSubscriptionFailure (Int32 subscribeId, Int32 statusCode)
* Int32 PortSIP.SIPCallbackEvents.onSubscriptionTerminated (Int32 subscribeId)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onRecvOptions (StringBuilder **_**optionsMessage**_**)**

This event will be triggered when receiving the OPTIONS message.

**Parameters**

| _optionsMessage_ | The received whole OPTIONS message in text format. |
| ---------------- | -------------------------------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onRecvInfo (StringBuilder **_**infoMessage**_**)**

This event will be triggered when receiving the INFO message. **Parameters**

| _infoMessage_ | The received whole INFO message in text format. |
| ------------- | ----------------------------------------------- |

**Int32 PortSIP.SIPCallbackEvents.onRecvNotifyOfSubscription (Int32 **_**subscribeId**_**, StringBuilder **_**notifyMsg**_**, byte\[] **_**contentData**_**, Int32 **_**contentLenght**_**)**

This event will be triggered when receiving a NOTIFY message of the subscription. **Parameters**

| _subscribeId_   | The ID of SUBSCRIBE request.                                       |
| --------------- | ------------------------------------------------------------------ |
| _notifyMessage_ | The received INFO message in text format.                          |
| _contentData_   | The received message body. It's can be either text or binary data. |
| _contentLenght_ | The length of "messageData".                                       |

**Int32 PortSIP.SIPCallbackEvents.onSubscriptionFailure (Int32 **_**subscribeId**_**, Int32 **_**statusCode**_**)**

This event will be triggered on sending SUBSCRIBE failure. **Parameters**

| _subscribeId_ | The ID of SUBSCRIBE request. |
| ------------- | ---------------------------- |
| _statusCode_  | The status code.             |

**Int32 PortSIP.SIPCallbackEvents.onSubscriptionTerminated (Int32 **_**subscribeId**_**)**

This event will be triggered when a SUBSCRIPTION is terminated or expired. **Parameters**

| _subscribeId_ | The ID of SUBSCRIBE request. |
| ------------- | ---------------------------- |

84

**Presence events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onPresenceRecvSubscribe (Int32 subscribeId, String fromDisplayName, String from, String subject)
* Int32 PortSIP.SIPCallbackEvents.onPresenceOnline (String fromDisplayName, String from, String stateText)
* Int32 PortSIP.SIPCallbackEvents.onPresenceOffline (String fromDisplayName, String from)
* Int32 PortSIP.SIPCallbackEvents.onRecvMessage (Int32 sessionId, String mimeType, String subMimeType, byte\[] messageData, Int32 messageDataLength)
* Int32 PortSIP.SIPCallbackEvents.onRecvOutOfDialogMessage (String fromDisplayName, String from, String toDisplayName, String to, String mimeType, String subMimeType, byte\[] messageData, Int32 messageDataLength)
* Int32 PortSIP.SIPCallbackEvents.onSendMessageSuccess (Int32 sessionId, Int32 messageId)
* Int32 PortSIP.SIPCallbackEvents.onSendMessageFailure (Int32 sessionId, Int32 messageId, String reason, Int32 code)
* Int32 PortSIP.SIPCallbackEvents.onSendOutOfDialogMessageSuccess (Int32 messageId, String fromDisplayName, String from, String toDisplayName, String to)
* Int32 PortSIP.SIPCallbackEvents.onSendOutOfDialogMessageFailure (Int32 messageId, String fromDisplayName, String from, String toDisplayName, String to, String reason, Int32 code)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onPresenceRecvSubscribe (Int32 **_**subscribeId**_**, String **_**fromDisplayName**_**, String **_**from**_**, String **_**subject**_**)**

This event will be triggered when receiving the SUBSCRIBE request from a contact. **Parameters**

| _subscribeId_     | The ID of SUBSCRIBE request.                 |
| ----------------- | -------------------------------------------- |
| _fromDisplayName_ | The display name of contact.                 |
| _from_            | The contact who sends the SUBSCRIBE request. |
| _subject_         | The subject of the SUBSCRIBE request.        |

**Int32 PortSIP.SIPCallbackEvents.onPresenceOnline (String **_**fromDisplayName**_**, String **_**from**_**, String **_**stateText**_**)**

When the contact is online or changes presence status, this event will be triggered. **Parameters**

| _fromDisplayName_ | The display name of contact.                 |
| ----------------- | -------------------------------------------- |
| _from_            | The contact who sends the SUBSCRIBE request. |
| _stateText_       | The presence status text.                    |

**Int32 PortSIP.SIPCallbackEvents.onPresenceOffline (String **_**fromDisplayName**_**, String **_**from**_**)**

When the contact goes offline, this event will be triggered. **Parameters**

| _fromDisplayName_ | The display name of contact.                |
| ----------------- | ------------------------------------------- |
| _from_            | The contact who sends the SUBSCRIBE request |

85

**Int32 PortSIP.SIPCallbackEvents.onRecvMessage (Int32 **_**sessionId**_**, String **_**mimeType**_**, String **_**subMimeType**_**, byte\[] **_**messageData**_**, Int32 **_**messageDataLength**_**)**

This event will be triggered when receiving a MESSAGE message in dialog. **Parameters**

| _sessionId_          | The session ID of the call.                               |
| -------------------- | --------------------------------------------------------- |
| _mimeType_           | The message mime type.                                    |
| _subMimeType_        | The message sub mime type.                                |
| _messageData_        | The received message body. It can be text or binary data. |
| _messageDataLengt h_ | The length of "messageData".                              |

**Int32 PortSIP.SIPCallbackEvents.onRecvOutOfDialogMessage (String **_**fromDisplayName**_**, String **_**from**_**, String **_**toDisplayName**_**, String **_**to**_**, String **_**mimeType**_**, String **_**subMimeType**_**, byte\[] **_**messageData**_**, Int32 **_**messageDataLength**_**)**

This event will be triggered when receiving a MESSAGE message out of dialog. For example: pager message.

**Parameters**

| _fromDisplayName_    | The display name of sender.                               |
| -------------------- | --------------------------------------------------------- |
| _from_               | The message sender.                                       |
| _toDisplayName_      | The display name of recipient.                            |
| _to_                 | The recipient.                                            |
| _mimeType_           | The message mime type.                                    |
| _subMimeType_        | The message sub mime type.                                |
| _messageData_        | The received message body. It can be text or binary data. |
| _messageDataLengt h_ | The length of "messageData".                              |

**Int32 PortSIP.SIPCallbackEvents.onSendMessageSuccess (Int32 **_**sessionId**_**, Int32 **_**messageId**_**)**

If the message is sent successfully in dialog, this event will be triggered. **Parameters**

| _sessionId_ | The session ID of the call.                                             |
| ----------- | ----------------------------------------------------------------------- |
| _messageId_ | The message ID. It's equal to the return value of sendMessage function. |

**Int32 PortSIP.SIPCallbackEvents.onSendMessageFailure (Int32 **_**sessionId**_**, Int32 **_**messageId**_**, String **_**reason**_**, Int32 **_**code**_**)**

If the message fails to be sent out of dialog, this event will be triggered. **Parameters**

| _sessionId_ | The session ID of the call.                                             |
| ----------- | ----------------------------------------------------------------------- |
| _messageId_ | The message ID. It's equal to the return value of sendMessage function. |
| _reason_    | The failure reason.                                                     |
| _code_      | Failure code.                                                           |

**Int32 PortSIP.SIPCallbackEvents.onSendOutOfDialogMessageSuccess (Int32 **_**messageId**_**, String **_**fromDisplayName**_**, String **_**from**_**, String **_**toDisplayName**_**, String **_**to**_**)**

If the message is sent succeeded out of dialog, this event will be triggered. **Parameters**

| _messageId_       | The message ID. It's equal to the return value of SendOutOfDialogMessage function. |
| ----------------- | ---------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of message sender.                                                |
| _from_            | The message sender.                                                                |

86

| _toDisplayName_ | The display name of message recipient. |
| --------------- | -------------------------------------- |
| _to_            | The message receiver.                  |

**Int32 PortSIP.SIPCallbackEvents.onSendOutOfDialogMessageFailure (Int32 **_**messageId**_**, String **_**fromDisplayName**_**, String **_**from**_**, String **_**toDisplayName**_**, String **_**to**_**, String **_**reason**_**, Int32 **_**code**_**)**

If the message was sent failure out of dialog, this event will be triggered. **Parameters**

| _messageId_       | The message ID. It's equal to the return value of SendOutOfDialogMessage function. |
| ----------------- | ---------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of message sender                                                 |
| _from_            | The message sender.                                                                |
| _toDisplayName_   | The display name of message recipient.                                             |
| _to_              | The message recipient.                                                             |
| _reason_          | The failure reason.                                                                |
| _code_            | The failure code.                                                                  |

87

**Play audio and video file finished events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onPlayFileFinished (Int32 sessionId, String fileName)
* Int32 PortSIP.SIPCallbackEvents.onStatistics (Int32 sessionId, String stat)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onPlayFileFinished (Int32 **_**sessionId**_**, String **_**fileName**_**)**

If startPlayingFileToRemote function is called with no loop mode, this event will be triggered once the file play finished.

**Parameters**

| _sessionId_ | The session ID of the call.  |
| ----------- | ---------------------------- |
| _fileName_  | The name of the file played. |

**Int32 PortSIP.SIPCallbackEvents.onStatistics (Int32 **_**sessionId**_**, String **_**stat**_**)**

If getStatistics function is called, this event will be triggered once receiving a RTP statistics .

**Parameters**

| _sessionId_ | The session ID of the call.      |
| ----------- | -------------------------------- |
| _stat_      | RTP statistics of a json format. |

88

**RTP callback events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onRTPPacketCallback (IntPtr callbackObject, Int32 sessionId, Int32 mediaType, Int32 direction, byte\[] RTPPacket, Int32 packetSize)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onRTPPacketCallback (IntPtr **_**callbackObject**_**, Int32 **_**sessionId**_**, Int32 **_**mediaType**_**, Int32 **_**direction**_**, byte\[] **_**RTPPacket**_**, Int32 **_**packetSize**_**)**

If enableRtpCallback function is called to enable the RTP callback, this event will be triggered once receiving a RTP packet or a RTP packet sent.

**Parameters**

| _sessionId_     | The session ID of the call.                                                                                  |   |   |
| --------------- | ------------------------------------------------------------------------------------------------------------ | - | - |
| _mediaType_     | If the received RTP packet is audio this parameter is 0 video this parameter is 1 screen this parameter is 2 |   |   |
| _direction_     | The RTP stream callback direction.                                                                           |   |   |
| Type            | Description                                                                                                  |   |   |
| DIRECTION\_SEND | Callback the send RTP stream for one channel based on the given sessionId.                                   |   |   |
| DIRECTION\_RECV | Callback the received RTP stream for one channel based on the given sessionId.                               |   |   |

**Parameters**

| _RTPPacket_  | The memory of whole RTP packet.  |
| ------------ | -------------------------------- |
| _packetSize_ | The size of received RTP Packet. |

**Note**

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

89

**Audio and video stream callback events**

**Functions**

* Int32 PortSIP.SIPCallbackEvents.onAudioRawCallback (IntPtr callbackObject, Int32 sessionId, Int32 callbackType, byte\[] data, Int32 dataLength, Int32 samplingFreqHz)
* Int32 PortSIP.SIPCallbackEvents.onVideoRawCallback (IntPtr callbackObject, Int32 sessionId, Int32 callbackType, Int32 width, Int32 height, byte\[] data, Int32 dataLength)&#x20;

**Detailed Description Function Documentation**&#x20;

**Int32 PortSIP.SIPCallbackEvents.onAudioRawCallback (IntPtr **_**callbackObject**_**, Int32 **_**sessionId**_**, Int32 **_**callbackType**_**, byte\[] **_**data**_**, Int32 **_**dataLength**_**, Int32 **_**samplingFreqHz**_**)**

This event will be triggered once receiving the audio packets if called enableAudioStreamCallback function.

**Parameters**

| _sessionId_          | The session ID of the call.                                     |
| -------------------- | --------------------------------------------------------------- |
| _audioCallbackMod e_ | The type that is passed in enableAudioStreamCallback function.  |
| _data_               | The memory of audio stream. It's in PCM format.                 |
| _dataLength_         | The data size.                                                  |
| _samplingFreqHz_     | The audio stream sample in HZ. For example, it's 8000 or 16000. |

**Note**

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

**Int32 PortSIP.SIPCallbackEvents.onVideoRawCallback (IntPtr **_**callbackObject**_**, Int32 **_**sessionId**_**, Int32 **_**callbackType**_**, Int32 **_**width**_**, Int32 **_**height**_**, byte\[] **_**data**_**, Int32 **_**dataLength**_**)**

This event will be triggered once receiving the video packets if called enableVideoStreamCallback function.

**Parameters**

| _sessionId_          | The session ID of the call.                                     |
| -------------------- | --------------------------------------------------------------- |
| _videoCallbackMod e_ | The type which is passed in enableVideoStreamCallback function. |
| _width_              | The width of video image.                                       |
| _height_             | The height of video image.                                      |
| _data_               | The memory of video stream. It's in YUV420 format, YV12.        |
| _dataLength_         | The data size.                                                  |

**Note**

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

