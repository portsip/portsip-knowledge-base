# User Manual for Windows

## **FAQ**

### Where can I download the PortSIP VoIP SDK for testing?

You can download the PortSIP VoIP SDK along with the sample project from the [PortSIP Website](https://www.portsip.com/download-portsip-voip-sdk/).

### How can I compile the sample project?

1. Download the sample project from the PortSIP Website.
2. Uncompress the .zip file.
3. Open the project with Visual Studio.
4. Compile the sample project directly and run it to test.

If the SDK connects to the PortSIP PBX, there are no limitations. The trial SDK works with any third-party PBX and SIP server, but it only allows for a 2-3 minute conversation.

### **What are operating systems supported?**

PortSIP VoIP SDK supports development on:

* **Windows 10**&#x20;
* **Windows 11**
* **Windows Server 2016, 2019, and 2022**&#x20;

### **What are development tools supported?**

*   **Microsoft Visual Studio versions 2017, 2019, and 2023 are supported**.

    Ensure you have the appropriate build tools installed for your desired languages: C#, VB.NET, and VC++.

### How can I create a new project with PortSIP VoIP SDK?

#### **C#/VB.NET:**

1. Download and uncompress the sample project.
2. Create a new “Windows Application” project in Visual Studio.
3. Copy `portsip_sdk.dll` and `portsip_media.dll` from the sample project directory to your project’s output directories: `bin\release` and `bin\debug`.
4. Copy the `PortSIP` folder from the sample project directory to your project folder and add it to the Solution.
5. Implement the `SIPCallbackEvents` interface to process callback events.
6. Right-click the project, choose **Properties**, click the **Build** tab, and check the **Allow unsafe code** checkbox.

For more details, please refer to the sample project source code.

#### VC++:

1. Download and uncompress the sample project.
2. Create a new “MFC Application” project.
3. Copy `portsip_sdk.dll` and `portsip_media.dll` from the sample project directory to your project’s output directories.
4. Copy the `include/PortSIPLib` folder to your project folder and add the `.hxx` files from the `PortSIPLib` folder to your project.
5. Copy the `lib` folder to your project folder and link `portsip_sdk.lib` into your project.

For more details, please refer to the sample project source code.

### How can I test a P2P call (without a SIP server PBX)?

1. Uncompress the SDK sample project ZIP file and compile the `P2PSample` project.
2. Run the `P2PSample` on device A and device B. For example, IP address for A is `192.168.1.10`, and IP address for B is `192.168.1.11`.
3. Enter a username and password on A (e.g., username: `111`, password: `aaa`). You can enter anything for the password as the SDK will ignore it. Do the same for B (e.g., username: `222`, password: `aaa`).
4. Click the **Initialize** button on both A and B. If the default port `5060` is already in use by another application, the `P2PSample` will prompt “Initialize failure”. In this case, click the **Uninitialize** button, change the local port, and click the **Initialize** button again.
5. The log box will display “Initialized.” if the SDK is successfully initialized.
6. To make a call from A to B, enter `sip:222@192.168.1.11` and click the **Dial** button. To make a call from B to A, enter `sip:111@192.168.1.10`. If A used `5066` as the local port, for example, dial to `sip:111@192.168.1.10:5066`, and vice versa for B.

### Is the SDK thread-safe?

Yes, the SDK is thread-safe. You can call any of the API functions without worrying about multiple threads. Note: The SDK allows calling API functions in callback events directly, except for the `onAudioRawCallback`, `onVideoRawCallback`, and `onRTPPacketCallback` callbacks.

### Does the SDK support native 64-bit?

Yes, the SDK supports both 32-bit and 64-bit architectures.

***

## SDK API Functions

### **Initialize and register functions**

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

| _transport_               | <p></p><p>The transport for SIP signaling can be one of the following values:</p><ul><li><code>TRANSPORT_UDP</code></li><li><code>TRANSPORT_TLS</code></li><li><code>TRANSPORT_TCP</code></li></ul>                                                                                                                                                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _localIP_                 | <p>The local computer IP address to be bound (e.g., <code>192.168.1.108</code>) will be used for sending and receiving SIP messages and RTP packets. If this IP is provided in IPv6 format, the SDK will use IPv6.</p><p>To allow the SDK to automatically choose the correct network interface (IP), use <code>"0.0.0.0"</code> for IPv4 or <code>"::"</code> for IPv6.</p>                                                                      |
| _localSIPPort_            | The SIP message transport listener port (e.g., `5060`) is used for SIP signaling. Please ensure this port is not being used by another application.                                                                                                                                                                                                                                                                                               |
| _logLevel_                | <p></p><p>Set the application log level to enable logging. When logging is enabled, the SDK will generate a log file named <code>PortSIP_Log_&#x3C;datetime>.log</code>.</p><p>The supported log levels are:</p><ul><li><code>PORTSIP_LOG_NONE = -1</code></li><li><code>PORTSIP_LOG_ERROR = 1</code></li><li><code>PORTSIP_LOG_WARNING = 2</code></li><li><code>PORTSIP_LOG_INFO = 3</code></li><li><code>PORTSIP_LOG_DEBUG = 4</code></li></ul> |
| _logFilePath_             | Specify the log file path. The path (folder) must already exist.                                                                                                                                                                                                                                                                                                                                                                                  |
| _maxCallLines_            | Theoretically, unlimited lines could be supported depending on the device’s capability. For a client app, the recommended value ranges from 1 to 100.                                                                                                                                                                                                                                                                                             |
| _sipAgent_                | The User-Agent header to be inserted into SIP messages.                                                                                                                                                                                                                                                                                                                                                                                           |
| _audioDeviceLayer_        | <p></p><p>Specify the audio device layer to be used:</p><ul><li><code>0</code> = Use the OS default device.</li><li><code>1</code> = Virtual device, typically used for devices without a sound device installed.</li></ul>                                                                                                                                                                                                                       |
| _videoDeviceLayer_        | <p></p><p>Specify the video device layer to be used:</p><ul><li><code>0</code> = Use the OS default device.</li><li><code>1</code> = Use a virtual device, typically used for devices without a camera installed.</li></ul>                                                                                                                                                                                                                       |
| _TLSCertificatesRootPath_ | Specify the TLS certificate path from which the SDK will automatically load the certificates. Note: On Windows, this path will be ignored, and the SDK will read the certificates from the Windows certificate store instead.                                                                                                                                                                                                                     |
| _TLSCipherList_           | Specify the TLS cipher list. This parameter is usually passed as empty so that the SDK will offer all available ciphers. It can be passed empty string if not use the TLS transport.                                                                                                                                                                                                                                                              |
| _verifyTLSCertificat_     | Specify the TLS cipher list. This parameter is usually left empty so that the SDK will offer all available ciphers.                                                                                                                                                                                                                                                                                                                               |
|                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

#### **Returns**

If the function succeeds, it will return a value of 0. If the function fails, it will return a specific error code.

***

```csharp
String PortSIP.PortSIPLib.getVersion ()
```

Get the current version number of the SDK.

**Returns**

Return a current version number MAJOR.MINOR.PATCH of the SDK.

***

```csharp
Int32 PortSIP.PortSIPLib.setLicenseKey (String key)
```

Set the license key. It must be called before the `setUser` function.

**Parameters**

| _key_ | The SDK license key, please purchase from PortSIP. |
| ----- | -------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setUser (String userName, 
                                String displayName, 
                                String authName, 
                                String password, 
                                String sipDomain, 
                                String sipServerAddr,
                                Int32 sipServerPort, 
                                String stunServerAddr, 
                                Int32 stunServerPort, 
                                String outboundServerAddr,
                                Int32 outboundServerPort)
```

Set user account info.

**Parameters**

| _userName_            | The SIP account (username) is usually provided by an IP-Telephony service provider. For PortSIP PBX, this is the extension number.                                                                   |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _displayName_         | The display name of the user. You can set it to anything you like, such as “James Kend”. This field is optional and can be an empty string.                                                          |
| _authName_            | The authorization username is usually the same as the SIP account (username).                                                                                                                        |
| _password_            | The password of user. It's optional and can be an empty string.                                                                                                                                      |
| _localIp_             | The local computer IP address to be bound (e.g., `192.168.1.108`) will be used for sending and receiving SIP messages and RTP packets. If this IP is provided in IPv6 format, the SDK will use IPv6. |
| _localSipPort_        | The SIP message transport listener port (e.g., `5060`) is used for SIP signaling.                                                                                                                    |
| _userDomain_          | The user domain is an optional parameter. You can pass an empty string if you are not using a domain. To connect to the PortSIP PBX, this is the tenant SIP domain.                                  |
| _sipServer_           | Specify the IP address or domain of the SIP server or PBX.                                                                                                                                           |
| _sipServerPort_       | Specify the SIP message port that the SIP server or PBX is listening on.                                                                                                                             |
| _stunServer_          | Specify the STUN server used for NAT traversal. This parameter is optional, and you can pass an empty string to disable STUN.                                                                        |
| _stunServerPort_      | Specify the STUN server port. This parameter will be ignored if the `outboundServer` is empty.                                                                                                       |
| _outboundServer_      | Specify the outbound proxy server IP address or domain. This parameter is optional, and you can pass an empty string if you are not using an outbound server.                                        |
| _outboundServerPo rt_ | Specify the outbound proxy server port. This parameter will be ignored if the `outboundServer` is empty.                                                                                             |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setDisplayName (String displayName)
```

Set the display name for the user.

**Parameters**

| _displayName_ | that will appear in the From/To Header. |
| ------------- | --------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```
Int32 PortSIP.PortSIPLib.setInstanceId (String uuid)
```

Set outbound (RFC5626) instanceId to be used in contact headers.&#x20;

**Parameters**

| _uuid_ | The ID that will appear in the contact header. Please make sure it's a unique ID. |
| ------ | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```
Int32 PortSIP.PortSIPLib.registerServer (Int32 expires, Int32 retryTimes)
```

Register to the SIP server or PBX server(login to the server).

15 **Parameters**

| _expires_    | Specify the registration refresh interval in seconds, with a maximum of 3600 seconds. This value will be inserted into the SIP REGISTER message headers.                        |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _retryTimes_ | The number of retry attempts if registration refresh fails. If set to 0 or less, retries will be disabled, and the `onRegisterFailure` callback will be triggered upon failure. |

**Returns**

If the function succeeds, it will return 0. If it fails, it will return a specific error code. Upon successful registration to the server, the `onRegisterSuccess` callback will be triggered; otherwise, the `onRegisterFailure` callback will be triggered.

***

```csharp
Int32 PortSIP.PortSIPLib.unRegisterServer (Int32 waitMS)
```

Unregister from the SIP server/PBX.

**Parameters**

| _waitMS_ | Wait for the server to confirm successful un-registration. `waitMS` specifies the maximum wait time in milliseconds; a value of 0 means no waiting |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.enableRport (Boolean enable)
```

Enable/disable rport(RFC3581).

**Parameters**

| _enable_ | Set to true to enable the SDK to support rport. By default it is enabled. |
| -------- | ------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.enableEarlyMedia (Boolean enable)
```

Enable/disable Early Media.

**Parameters**

| _enable_ | Set to true to enable the SDK to support Early Media. By default Early Media is disabled. |
| -------- | ----------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.enablePriorityIPv6Domain (Boolean enable)
```

Enable or disable the option to specify the preferred protocol when a domain supports both IPv4 and IPv6 simultaneously.

16 **Parameters**

| _enable_ | Set to `true` to prioritize IPv6 domains. By default, IPv4 is prioritized. |
| -------- | -------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setUriUserEncoding (String character, Boolean enable)
```

Indicate to the SDK that the user part of the URI should be encoded for escaping.

**Parameters**

| _character_ | Specify the character to be encoded, setting one at a time.                                       |
| ----------- | ------------------------------------------------------------------------------------------------- |
| _enable_    | Indicate whether escaping is required: set to `true` to allow escaping, or `false` to disable it. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setReliableProvisional (Int32 mode)
```

Enable/disable PRACK.

**Parameters**

| _mode_ | <p></p><p>This mode parameter can be set to one of the following values:</p><ul><li><code>0 - Never</code>: Disable PRACK. By default, PRACK is disabled.</li><li><code>1 - SupportedEssential</code>: Only send reliable provisionals if sending a body and the far end supports it.</li><li><code>2 - Supported</code>: Always send reliable provisionals if the far end supports it.</li><li><code>3 - Required</code>: Always send reliable provisionals.</li></ul> |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```
Int32 PortSIP.PortSIPLib.enable3GppTags (Boolean enable)
```

Enable/disable the 3Gpp tags, including "ims.icsi.mmtel" and "g.3gpp.smsip".

**Parameters**

| _enable_ | Set to true to enable SDK to support 3Gpp tags. |
| -------- | ----------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
void PortSIP.PortSIPLib.enableCallbackSignaling (Boolean enableSending, 
                                Boolean enableReceived)
```

This function is used to enable or disable the callback for SIP messages.

**Parameters**

| _enableSending_  | Set to `true` to enable the callback for sent SIP messages, or `false` to disable it. Once enabled, the `onSendingSignaling` event will be triggered when the SDK sends a SIP message.         |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _enableReceived_ | Set to `true` to enable the callback for received SIP messages, or `false` to disable it. Once enabled, the `onReceivedSignaling` event will be triggered when the SDK receives a SIP message. |
|                  |                                                                                                                                                                                                |

***

```csharp
Int32 PortSIP.PortSIPLib.enableRtpCallback (Int32 sessionId, 
                                Int32 mediaType, 
                                Int32 directionMode)
```

Enable RTP callbacks to access sent and received RTP packets. The `onRTPPacketCallback` events will be triggered.

**Parameters**

| _sessionId_     | The session ID of call.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _mediaType_     | 0 -audo 1-video 2-screen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| _directionMode_ | <p></p><p>Specify the RTP stream callback mode. The available options are:</p><ul><li><strong>DIRECTION_SEND</strong>: Callback for the sending RTP stream of a single channel, based on the provided <code>sessionId</code>.</li><li><strong>DIRECTION_RECV</strong>: Callback for the receiving RTP stream of a single channel, based on the provided <code>sessionId</code>.</li><li><strong>DIRECTION_SEND_RECV</strong>: Callback for both the local and remote RTP streams on the provided <code>sessionId</code>.</li></ul> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **NIC and local IP functions**

```csharp
Int32 PortSIP.PortSIPLib.getNICNums ()
```

Retrieve the number of Network Interface Cards (NICs) available on the device.

**Returns**

If the function succeeds, it will return the number of Network Interface Cards (NICs), which will be greater than or equal to 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.getLocalIpAddress (Int32 index, 
                                StringBuilder ip, 
                                Int32 ipSize)
```

Get the local IP address by Network Interface Card index.&#x20;

**Parameters**

| _index_  | Specify the IP address index. For example, if the PC has two NICs and you wish to obtain the IP address of the second NIC, set this parameter to `1`. The first NIC IP index is `0`. |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _ip_     | Specify the `StringBuilder` buffer that is used to receive the IP address.                                                                                                           |
| _ipSize_ | Specify the `StringBuilder` buffer size, which must be at least 32 bytes.                                                                                                            |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Audio and video codec functions**

```csharp
Int32 PortSIP.PortSIPLib.addAudioCodec (AUDIOCODEC_TYPE codecType)
```

Enables an audio codec, making it appear in the Session Description Protocol (SDP).

**Parameters**

| _codecType_ | <p>This parameter specifies the type of audio codec you want to enable. It should be of the <code>AUDIOCODEC_TYPE</code> enum. The enum values are following: </p><ul><li><strong>AUDIOCODEC_NONE</strong>: Undefined video codec..</li><li><strong>AUDIOCODEC_G729</strong>: G729 codec, 8kHz, 8kbps.</li><li><strong>AUDIOCODEC_PCMA</strong>: PCMA/G711 A-law codec, 8kHz, 64kbps.</li><li><strong>AUDIOCODEC_PCMU</strong>: PCMU/G711 μ-law codec, 8kHz, 64kbps.</li><li><strong>AUDIOCODEC_GSM</strong>: GSM codec, 8kHz, 13kbps.</li><li><strong>AUDIOCODEC_G722</strong>: G722 codec, 16kHz, 64kbps.</li><li><strong>AUDIOCODEC_ILBC</strong>: iLBC codec, 8kHz, 30ms-13kbps or 20ms-15kbps.</li><li><strong>AUDIOCODEC_AMR</strong>: Adaptive Multi-Rate (AMR) codec, 8kHz, various bitrates (4.75-12.20kbps).</li><li><strong>AUDIOCODEC_AMRWB</strong>: Adaptive Multi-Rate Wideband (AMR-WB) codec, 16kHz, various bitrates (6.60-23.85kbps).</li><li><strong>AUDIOCODEC_SPEEX</strong>: SPEEX codec, 8kHz, various bitrates (2-24kbps).</li><li><strong>AUDIOCODEC_SPEEXWB</strong>: SPEEX Wideband codec, 16kHz, various bitrates (4-42kbps).</li><li><strong>AUDIOCODEC_ISACWB</strong>: iSAC Wideband codec, 16kHz, various bitrates (32-54kbps).</li><li><strong>AUDIOCODEC_ISACSWB</strong>: iSAC Super Wideband codec, 16kHz, various bitrates (32-160kbps).</li><li><strong>AUDIOCODEC_G7221</strong>: G722.1 codec, 16kHz, various bitrates (16, 24, 32kbps).</li><li><strong>AUDIOCODEC_OPUS</strong>: OPUS codec, 48kHz, 32kbps.</li><li><strong>AUDIOCODEC_DTMF</strong>: DTMF codec, RFC 2833.</li></ul> |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.addVideoCodec (VIDEOCODEC_TYPE codecType)
```

Enables a video codec, making it appear in the Session Description Protocol (SDP).

**Parameters**

| _codecType_ | <p></p><p>This parameter specifies the type of video codec you want to enable. It should be of the VIDEO<code>CODEC_TYPE</code> enum. The enum values are following: </p><ul><li><strong>VIDEO_CODEC_NONE</strong>: Undefined video codec..</li><li><strong>VIDEO_CODEC_I420</strong></li><li><strong>VIDEO_CODEC_H263</strong></li><li><strong>VIDEO_CODEC_H263_1998</strong></li><li><strong>VIDEO_CODEC_H264</strong></li><li><strong>VIDEO_CODEC_VP8</strong></li><li><strong>VIDEO_CODEC_VP9</strong></li></ul> |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```
Boolean PortSIP.PortSIPLib.isAudioCodecEmpty ()
```

This function checks whether any audio codecs are enabled.

**Returns**

If no audio codec is enabled, it will return value true, otherwise false.

***

```csharp
Boolean PortSIP.PortSIPLib.isVideoCodecEmpty ()
```

This function checks whether any video codecs are enabled.

**Returns**

If no video codec is enabled, it will return value true, otherwise false.

***

```csharp
Int32 PortSIP.PortSIPLib.setAudioCodecPayloadType (AUDIOCODEC_TYPE codecType, 
                                                   Int32 payloadType)
```

Set the RTP payload type for a dynamic audio codec.

**Parameters**

| _codecType_   | Audio codec type, which is defined in the PortSIPTypes file. |
| ------------- | ------------------------------------------------------------ |
| _payloadType_ | The new RTP payload type that you want to set.               |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoCodecPayloadType (VIDEOCODEC_TYPE codecType, 
                                                   Int32 payloadType)
```

Set the RTP payload type for a dynamic Video codec.

**Parameters**

| _codecType_   | Video codec type, which is defined in the PortSIPTypes file. |
| ------------- | ------------------------------------------------------------ |
| _payloadType_ | The new RTP payload type that you want to set.               |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setAudioCodecParameter (AUDIOCODEC_TYPE codecType, 
                                                 String parameter)
```

Set the codec parameters for an audio codec.

**Parameters**

| _codecType_ | Audio codec type, defined in the PortSIPTypes file. |
| ----------- | --------------------------------------------------- |
| _parameter_ | The code parameter in string format.                |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

**Remarks**

Here is an example:

`setAudioCodecParameter(AUDIOCODEC_AMR, "mode-set=0; octet-align=1; robust-sorting=0");`

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoCodecParameter (VIDEOCODEC_TYPE codecType, 
                                                 String parameter)
```

Set the codec parameter for a video codec.&#x20;

**Parameters**

| _codecType_ | Video codec type, defined in the PortSIPTypes file. |
| ----------- | --------------------------------------------------- |
| _parameter_ | The parameter in string format.                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

**Remarks**

Here is an example:

`setVideoCodecParameter(VIDEO_CODEC_H264, "profile-level-id=420033; packetization-mode=0");`

***

### **Additional setting functions**

```csharp
Int32 PortSIP.PortSIPLib.setSrtpPolicy (SRTP_POLICY srtpPolicy, 
                                        Boolean allowSrtpOverUnsecureTransport)
```

Set the SRTP policy.&#x20;

**Parameters**

| _srtpPolicy_                   | <p>The SRTP policy can be one of the following enum values:</p><ul><li><strong>SRTP_POLICY_NONE = 0</strong>: Do not use SRTP. The SDK can receive both encrypted (SRTP) and unencrypted calls, but cannot place outgoing encrypted calls.</li><li><strong>SRTP_POLICY_FORCE</strong>: All calls must use SRTP. The SDK allows receiving encrypted calls and placing outgoing encrypted calls only.</li><li><strong>SRTP_POLICY_PREFER</strong>: Prefer using SRTP. The SDK allows receiving both encrypted and unencrypted calls, and placing both outgoing encrypted and unencrypted calls.</li></ul> |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| allowSrtpOverUnsecureTransport | The `allowSrtpOverUnsecureTransport` parameter specifies whether SRTP is allowed over unsecured transport protocols such as UDP and TCP. Set to `true` to allow, and `false` to disallow.                                                                                                                                                                                                                                                                                                                                                                                                               |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setRtpPortRange (Int32 minimumRtpPort, Int32 maximumRtpPort)
```

Set the RTP port range for RTP traffic.

**Parameters**

| _minimumRtpPort_ | The minimum RTP port. |
| ---------------- | --------------------- |
| _maximumRtpPort_ | The maximum RTP port. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

**Remarks**

The port range ((max - min) % maxCallLines) should be greater than 4.

```csharp
Int32 PortSIP.PortSIPLib.enableCallForward (Boolean forBusyOnly, String forwardTo)
```

Enable call forwarding.&#x20;

**Parameters**

| _forBusyOnly_ |  If set to `true`, the SDK will forward all incoming calls when it is busy. If set to `false`, the SDK will forward all incoming calls regardless. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| _forwardTo_   | The call forward target. It must be in the format `sip:xxxx@sip.portsip.com`                                                                       |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.disableCallForward ()
```

Disable call forwarding. The SDK will not forward any incoming calls after this function is called.

**Returns**

If the function succeeds, it will return value 0. If the function fails, the return value is a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.enableSessionTimer (Int32 timerSeconds, 
                                             SESSION_REFRESH_MODE refreshMode)
```

Allows periodic refreshing of Session Initiation Protocol (SIP) sessions by repeatedly sending INVITE requests.

**Parameters**

| _timerSeconds_ | The refresh interval value in seconds. A minimum value of 90 seconds is required.                                                                                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _refreshMode_  | <p></p><p>Allows setting the session refresh by either the User Agent Client (UAC) or the User Agent Server (UAS):</p><ul><li><code>SESSION_REFRESH_UAC</code></li><li><code>SESSION_REFRESH_UAS</code></li></ul> |
|                |                                                                                                                                                                                                                   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Repeated INVITE requests, or re-INVITEs, are sent during an active call to allow user agents (UAs) or proxies to determine the status of a SIP session. Without this keepalive mechanism, stateful proxies that remember incoming and outgoing requests may continue to retain call state unnecessarily. If a UA fails to send a BYE message at the end of a session, or if the BYE message is lost due to network issues, a stateful proxy will not know that the session has ended. Re-INVITEs ensure that active sessions remain active and completed sessions are terminated.

***

```csharp
Int32 PortSIP.PortSIPLib.disableSessionTimer ()
```

Disable the session timer.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
void PortSIP.PortSIPLib.setDoNotDisturb (Boolean state)
```

Enable or disable the "Do Not Disturb" feature.

**Parameters**

| _state_ | If set to `true`, the SDK will reject all incoming calls automatically. |
| ------- | ----------------------------------------------------------------------- |
|         |                                                                         |

***

```csharp
Int32 PortSIP.PortSIPLib.enableAutoCheckMwi (Boolean state)
```

Allows enabling or disabling the automatic check for Message Waiting Indication (MWI).

**Parameters**

| _state_ | If set to `true`, MWI will be checked automatically once successfully registered to a SIP server/PBX. |
| ------- | ----------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setRtpKeepAlive (Boolean state, 
                                Int32 keepAlivePayloadType, 
                                Int32 deltaTransmitTimeMS)
```

Enable or disable to send RTP keep-alive packet when the call is established.&#x20;

**Parameters**

| _state_                 | Set to true to allow to send the keep-alive packet during the call.                                               |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| _keepAlivePayload Type_ | The payload type of the keep-alive RTP packet, usually set to 126.                                                |
| _deltaTransmitTime MS_  | The keep-alive RTP packet sending interval, in milliseconds. The recommended value ranges from 15,000 to 300,000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setKeepAliveTime (Int32 keepAliveTime)
```

Enable or disable the sending of SIP keep-alive packets.

**Parameters**

| _keepAliveTime_ | This is the SIP keep-alive time interval in seconds. Set it to 0 to disable SIP keep-alive. It is recommended to set it to 30 or 50 seconds. |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.addSupportedMimeType (String methodName, String mimeType, String subMimeType)
```

Set the SDK to receive SIP messages that include a special MIME type.

**Parameters**

| _methodName_  | The method name of the SIP message, such as `INVITE`, `OPTION`, `INFO`, `MESSAGE`, `UPDATE`, `ACK`, etc. For more details, please refer to RFC3261. |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| _mimeType_    | The mime type of SIP message.                                                                                                                       |
| _subMimeType_ | The sub mime type of SIP message.                                                                                                                   |

**Returns**

If the function succeeds, it will return the value `0`. If the function fails, it will return a specific error code.

**Remarks**

By default, the PortSIP VoIP SDK supports the following media types (MIME types) for incoming SIP messages:

* `"message/sipfrag"` in NOTIFY messages.
* `"application/simple-message-summary"` in NOTIFY messages.
* `"text/plain"` in MESSAGE messages.
* `"application/dtmf-relay"` in INFO messages.
* `"application/media_control+xml"` in INFO messages.

The SDK allows receiving SIP messages that include the above MIME types. If the remote side sends an INFO SIP message with its “Content-Type” header value set to `"text/plain"`, the SDK will reject this INFO message, as `"text/plain"` for INFO messages is not included in the default support list.

To enable the SDK to receive SIP INFO messages that include the `"text/plain"` MIME type, use the following command:

```cpp
addSupportedMimeType("INFO", "text", "plain");
```

If you want to receive NOTIFY messages with `"application/media_control+xml"`, use the following command:

```cpp
addSupportedMimeType("NOTIFY", "application", "media_control+xml");
```

For more details about MIME types, please visit the IANA Media Types website:[http://www.iana.org/assignments/media-types/](http://www.iana.org/assignments/media-types/)

### **Access SIP message header functions**

<pre class="language-csharp"><code class="lang-csharp">Int32 PortSIP.PortSIPLib.getSipMessageHeaderValue (String sipMessage, 
<strong>                                String headerName, 
</strong>                                StringBuilder headerValue, 
                                Int32 headerValueLength)
</code></pre>

Access the SIP header of a SIP message.

**Parameters**

| _sipMessage_         | The SIP message.                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------ |
| _headerName_         | The header to access in the SIP message.                                                   |
| _headerValue_        | The buffer to receive header value.                                                        |
| _headerValueLengt h_ | The `headerValue` buffer size. It is usually recommended to set it to more than 512 bytes. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

When receiving a SIP message in the `onReceivedSignaling` callback event and you wish to get the SIP message header value, please use `getSipMessageHeaderValue.` Example:

```csharp
StringBuilder value = new StringBuilder(); 
value.Length = 512; 
getSipMessageHeaderValue(message, name, value);
```

***

```csharp

Int32 PortSIP.PortSIPLib.addSipMessageHeader (Int32 sessionId, 
                                String methodName, 
                                Int32 msgType, 
                                String headerName, 
                                String headerValue)
```

Add a custom SIP message header to the specified outgoing SIP message.

**Parameters**

| _sessionId_   | To add a header to the SIP message with a specified session ID, use the following instructions. If you set the session ID to `-1`, the header will be added to all messages.                                                    |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_  | To add a header to the SIP message with a specified method name, such as “INVITE”, “REGISTER”, or “INFO”, follow these instructions. If you specify “ALL”, the header will be added to all SIP messages.                        |
| _msgType_     | <ul><li><code>msgType: 1</code> - Applies to the request message.</li><li><code>msgType: 2</code> - Applies to the response message.</li><li><code>msgType: 3</code> - Applies to both request and response messages.</li></ul> |
| _headerName_  | The custom header name that will appear in every outgoing SIP message.                                                                                                                                                          |
| _headerValue_ | The custom header value.                                                                                                                                                                                                        |

**Returns**

If the function succeeds, it will return addedSipMessageId, which is greater than 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.removeAddedSipMessageHeader (Int32 sipMessageHeaderId)
```

Remove the custom headers that were added using `addSipMessageHeader`.

**Parameters**

| _addedSipMessageId_ | The `addedSipMessageId` is returned by the `addSipMessageHeader` function. |
| ------------------- | -------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.clearAddedSipMessageHeaders ()
```

Clear the added extension headers (custom headers).

**Remarks**

**Example: Adding and Removing Custom Headers**

For example, if you have added two custom headers to every outgoing SIP message and wish to remove them, you can use the following commands:

```csharp
addExtensionHeader(-1, "ALL", 3, "Billing", "usd100.00");
addExtensionHeader(-1, "ALL", 3, "ServiceId", "8873456");
clearAddedSipMessageHeaders();
```

***

```csharp
Int32 PortSIP.PortSIPLib.modifySipMessageHeader (Int32 sessionId, 
                                String methodName, 
                                Int32 msgType, 
                                String headerName, 
                                String headerValue)
```

Modify the special SIP header value for every outgoing SIP message.

**Parameters**

| _sessionId_  | The header of the SIP message with the specified session ID. By setting it to `-1`, the header will be modified to all messages.                                                                     |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_ | Modify the header of the SIP message with the specified method name only. For example, “INVITE”, “REGISTER”, or “INFO”. If “ALL” is specified, the header will be added to all SIP messages.         |
| _msgType_    | <ul><li><code>1</code> - Applies to the request message.</li><li><code>2</code> - Applies to the response message.</li><li><code>3</code> - Applies to both request and response messages.</li></ul> |

**Returns**

If the function succeeds, it will return modifiedSipMessageId, which is greater than 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.removeModifiedSipMessageHeader (Int32 sipMessageHeaderId)
```

Remove the modified headers (custom headers) from every outgoing SIP message.

**Parameters**

| _modifiedSipMessageId_ | The `modifiedSipMessageId` is returned by the `modifySipMessageHeader` function. |
| ---------------------- | -------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.clearModifiedSipMessageHeaders ()
```

Clear the modified header values, and stop modifying the header values of every outgoing SIP message.

For example, to modify the values of two headers for every outgoing SIP message and then clear them, use the following commands:

```cpp
modifySipMessageHeader(-1, "ALL", 3, "Expires", "1000");
modifySipMessageHeader(-1, "ALL", 3, "User-Agent", "MyTest Softphone 1.0");
clearModifiedSipMessageHeaders();

```

***

### **Audio and video functions**

```csharp
Int32 PortSIP.PortSIPLib.setAudioSamples (Int32 ptime, Int32 maxPtime)
```

Set the audio capture sample for the SDK.

**Parameters**

| _ptime_    | It should be a multiple of 10 between 10 - 60 (with 10 and 60 inclusive).                                                             |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| _maxPtime_ | The `maxptime` attribute should be a multiple of 10, ranging from 10 to 60 (inclusive). It cannot be less than the `ptime` attribute. |

**Returns**

If the function succeeds, it will return the value `0`. If the function fails, it will return a specific error code.

**Remarks**

The `ptime` and `maxptime` attributes will appear in the SDP of INVITE and 200 OK messages.

***

```csharp
Int32 PortSIP.PortSIPLib.setAudioDeviceId (Int32 recordingDeviceId, 
                                           Int32 playoutDeviceId)
```

This function specifies the audio devices to be used for recording and playback during voice calls.

**Parameters**

| _recordingDeviceId_ | The ID(index) of the audio device to use for recording(microphone). |
| ------------------- | ------------------------------------------------------------------- |
| _playoutDeviceId_   | The ID(index) of the audio device to use for playback(speaker).     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoOrientation (Int32 rotation)
```

This function sets the rotation angle for the video captured by the camera.

**Parameters**

| _rotation_ | The rotation angle in degrees. Valid values are 0 (no rotation), 90 (clockwise rotation), 180 (180-degree rotation), and 270 (counterclockwise rotation). |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoDeviceId (Int32 deviceId)
```

This function specifies the video device to be used for capturing and sending video during video calls.

**Parameters**

| _deviceId_ | The ID(index) of the video device(camera) to use. |
| ---------- | ------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoResolution (Int32 width, Int32 height)
```

This function sets the resolution (width and height) of the video captured by the camera.

**Parameters**

| _width_  | The desired width of the video in pixels.  |
| -------- | ------------------------------------------ |
| _height_ | The desired height of the video in pixels. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setAudioBitrate (Int32 sessionId, AUDIOCODEC_TYPE audioCodecType, Int32 bitrateKbps)
```

This function sets the audio bitrate for a specific audio codec in a given session.

**Parameters**

| _sessionId_    | The ID of the session for which to set the audio bitrate. |
| -------------- | --------------------------------------------------------- |
| audioCodecType | The type of audio codec to configure.                     |
| bitrateKbps    | The desired audio bitrate in kilobits per second (kbps).  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoBitrate (Int32 sessionId, Int32 bitrateKbps)
```

This function sets the video bitrate for a given session.

**Parameters**

| _sessionId_    | The ID of the session for which to set the video bitrate. |
| -------------- | --------------------------------------------------------- |
| videoCodecType | The type of video codec to configure.                     |
| bitrateKbps    | The desired video bitrate in kilobits per second (kbps).  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoFrameRate (Int32 sessionId, Int32 frameRate)
```

This function sets the video frame rate for a given session.

**Parameters**

| _sessionId_ | The ID of the session for which to set the video frame rate.                                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| frameRate   | The desired video frame rate in frames per second (fps). The minimum allowed frame rate is 5 fps, and the maximum is 30 fps. Increasing the frame rate can improve video smoothness but will also require more bandwidth. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

By default, the SDK uses a reasonable frame rate for video calls. You generally do not need to explicitly set the frame rate using this function unless you require a specific value. However, if you want to fine-tune the video quality-bandwidth trade-off, you can adjust the frame rate as needed within the supported range.

***

```csharp
Int32 PortSIP.PortSIPLib.sendVideo (Int32 sessionId, Boolean sendState)
```

This function controls whether video is sent to the remote party in a given session.

**Parameters**

| _sessionId_ | The ID of the session for which to control video sending.                                       |
| ----------- | ----------------------------------------------------------------------------------------------- |
| _sendState_ | A Boolean value indicating whether to start sending video (true) or stop sending video (false). |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
void PortSIP.PortSIPLib.muteMicrophone (Boolean mute)
```

This function controls whether the microphone is muted for audio input. This function is not available on Android and iOS platforms.

**Parameters**

| _mute_ | A Boolean value indicating whether to mute the microphone (true) or unmute it (false). |
| ------ | -------------------------------------------------------------------------------------- |

***

```csharp
void PortSIP.PortSIPLib.muteSpeaker (Boolean mute)
```

This function controls whether the speaker is muted for audio output. This function is not available on Android and iOS platforms.

**Parameters**

| _mute_ | A Boolean value indicating whether to mute the speaker (true) or unmute it (false). |
| ------ | ----------------------------------------------------------------------------------- |

***

```csharp
void PortSIP.PortSIPLib.setChannelOutputVolumeScaling (Int32 sessionId, 
                                                       Int32 scaling)
```

This function adjusts the volume scaling for a specific audio channel in a given session.

**Parameters**

| _sessionId_ | The ID of the session for which to adjust the volume scaling. |
| ----------- | ------------------------------------------------------------- |
| _scaling_   | Scale ranges \[0, 1000]. Default is 100.                      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
void PortSIP.PortSIPLib.setChannelInputVolumeScaling (Int32 sessionId, Int32 scaling)
```

This function adjusts the volume scaling for the microphone signal of a specific audio channel in a given session.

**Parameters**

| _sessionId_ | The ID of the session for which to adjust the volume scaling. |
| ----------- | ------------------------------------------------------------- |
| _scaling_   | Ccale ranges \[0, 1000]. Default is 100.                      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setRemoteVideoWindow (Int32 sessionId, 
                                               IntPtr remoteVideoWindow)
```

This function specifies the window handle where the received remote video will be displayed for a given session.

**Parameters**

| _sessionId_        | The ID of the session for which to set the remote video window. |
| ------------------ | --------------------------------------------------------------- |
| _remoteVideoWindo_ | The window handle where the remote video will be rendered.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.displayLocalVideo (Boolean state, 
                                      Boolean mirror, 
                                      IntPtr localVideoWindow)
```

This function controls whether the local video is displayed in a specified window.

**Parameters**

| _state_            | A Boolean value indicating whether to start displaying local video (true) or stop displaying it (false). |
| ------------------ | -------------------------------------------------------------------------------------------------------- |
| _mirror_           | A Boolean value indicating whether to mirror the local video horizontally.                               |
| _localVideoWindow_ | The window handle where the local video will be rendered.                                                |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoNackStatus (Boolean state)
```

This function controls whether the NACK (Negative ACKnowledgement) feature is enabled for video transmission in a session. NACK helps to improve video quality by requesting retransmission of lost packets.

**Parameters**

| _state_ | A Boolean value indicating whether to enable NACK (true) or disable it (false). |
| ------- | ------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Call functions**

```csharp
Int32 PortSIP.PortSIPLib.call (String callee, Boolean sendSdp, Boolean videoCall)
```

Make a call.

**Parameters**

| _callee_    | The callee. It can be either name or full SIP URI. For example: user001, sip[:user001@sip.iptel.org ](mailto:user001@sip.iptel.org)or sip[:user002@sip.yourdomain.com:](mailto:user002@sip.yourdomain.com)5068 |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _sendSdp_   | If it's set to false, the outgoing call doesn't include the SDP in INVITE message.                                                                                                                             |
| _videoCall_ | If it's set to true with at least one video codecs added, the outgoing call will include the video codec into SDP.                                                                                             |

**Returns**

If the function succeeds, it will return the session ID of the call that is greater than 0. If the function fails, it will return a specific error code. Note: the function success just means the outgoing call is being processed. You need to detect the call final state in onInviteTrying, onInviteRinging, onInviteFailure callback events.

***

```csharp
Int32 PortSIP.PortSIPLib.rejectCall (Int32 sessionId, int code)
```

rejectCall Reject the incoming call.

**Parameters**

| _sessionId_ | The sessionId of the call.              |
| ----------- | --------------------------------------- |
| _code_      | Reject code. For example, 486, 480 etc. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.hangUp (Int32 sessionId)
```

hangUp Hang up the call.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.answerCall (Int32 sessionId, Boolean videoCall)
```

answerCall Answer the incoming call.

**Parameters**

| _sessionId_ | The session ID of call.                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------- |
| _videoCall_ | If the incoming call is a video call and the video codec is matched, set it to true to answer the video call. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.updateCall (Int32 sessionId, bool enableAudio, bool enableVideo, bool enableScreen)
```
{% endcode %}

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

***

```csharp
Int32 PortSIP.PortSIPLib.hold (Int32 sessionId)
```

To place a call on hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.unHold (Int32 sessionId)
```

Take off hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.muteSession (Int32 sessionId, Boolean muteIncomingAudio, Boolean muteOutgoingAudio, Boolean muteIncomingVideo, Boolean muteOutgoingVideo)
```
{% endcode %}

Mute the specified session audio or video. **Parameters**

| _sessionId_          | The session ID of the call.                                                                |
| -------------------- | ------------------------------------------------------------------------------------------ |
| _muteIncomingAudi o_ | Set it to true to mute incoming audio stream, and remote side audio cannot be heard.       |
| _muteOutgoingAudi o_ | Set it to true to mute outgoing audio stream, and the remote side can't hear the audio.    |
| _muteIncomingVide o_ | Set it to true to mute incoming video stream, and the remote side video will be invisible. |
| _muteOutgoingVide o_ | Set it to true to mute outgoing video stream, and the remote side can't see the video.     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.forwardCall (Int32 sessionId, String forwardTo)
```

Forward call to another one when receiving the incoming call.

**Parameters**

| _sessionId_ | The session ID of the call.                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| _forwardTo_ | Target of the forwarding. It can be "sip:number@sipserver.com" or "number" only. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.pickupBLFCall (String replaceDialogId, Boolean videoCall)
```

This function will be used for picking up a call based on the BLF (Busy Lamp Field) status. **Parameters**

| _replaceDialogId_ | The session ID of the call.                                                      |
| ----------------- | -------------------------------------------------------------------------------- |
| _videoCall_       | Target of the forwarding. It can be "sip:number@sipserver.com" or "number" only. |

**Returns**

If the function succeeds, it will return a session ID that is greater than 0 to the new call, otherwise returns a specific error code that is less than 0.

**Remarks**

The scenario is:

1. User 101 subscribed the user 100's call status: sendSubscription("100", "dialog");
2. When 100 hold a call or 100 is ringing, onDialogStateUpdated callback will be triggered, and 101 will receive this callback. Now 101 can use pickupBLFCall function to pick the call rather than 100 to talk with caller.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.sendDtmf (Int32 sessionId, DTMF_METHOD dtmfMethod, int code, int dtmfDuration, bool playDtmfTone)
```
{% endcode %}

Send DTMF tone. **Parameters**

| _sessionId_  | The session ID of the call.                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------ |
| _dtmfMethod_ | DTMF tone could be sent with two methods: DTMF\_RFC2833 and DTMF\_INFO, of which DTMF\_RFC2833 is recommend. |
| _code_       | The DTMF tone (0-16).                                                                                        |
| code         | Description                                                                                                  |
| 0            | The DTMF tone 0.                                                                                             |
| 1            | The DTMF tone 1.                                                                                             |
| 2            | The DTMF tone 2.                                                                                             |
| 3            | The DTMF tone 3.                                                                                             |
| 4            | The DTMF tone 4.                                                                                             |
| 5            | The DTMF tone 5.                                                                                             |
| 6            | The DTMF tone 6.                                                                                             |
| 7            | The DTMF tone 7.                                                                                             |
| 8            | The DTMF tone 8.                                                                                             |
| 9            | The DTMF tone 9.                                                                                             |
| 10           | The DTMF tone \*.                                                                                            |
| 11           | The DTMF tone #.                                                                                             |
| 12           | The DTMF tone A.                                                                                             |
| 13           | The DTMF tone B.                                                                                             |
| 14           | The DTMF tone C.                                                                                             |
| 15           | The DTMF tone D.                                                                                             |
| 16           | The DTMF tone FLASH.                                                                                         |

**Parameters**

| _dtmfDuration_ | The DTMF tone samples. Recommended value 160.                                |
| -------------- | ---------------------------------------------------------------------------- |
| _playDtmfTone_ | If it is set to true, the SDK plays local DTMF tone sound when sending DTMF. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Refer functions**

```csharp
Int32 PortSIP.PortSIPLib.refer (Int32 sessionId, String referTo)
```

Refer the current call to another one.

**Parameters**

| _sessionId_ | The session ID of the call.                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| _referTo_   | Target of the refer, which can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

refer(sessionId, "sip:testuser12@sip.portsip.com");&#x20;

You can watch the video on YouTube at[ https://www.youtube.com/watch?v=\_2w9EGgr3FY.](https://www.youtube.com/watch?v=\_2w9EGgr3FY) It will demonstrate the transfer.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.attendedRefer (Int32 sessionId, Int32 replaceSessionId, String referTo)
```
{% endcode %}

**Parameters**

| _sessionId_       | The session ID of the call.                                                                    |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| replaceSessionId  | Session ID of the repferred call.                                                              |
| _referTo_         | <p>Target of the refer, which can be either </p><p>"sip:number@sipserver.com" or "number".</p> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

Please read the sample project source code for more details, or you can watch the video on YouTube at https://www.youtube.com/watch?v=NezhIZW4lV4,

which will demonstrate the transfer.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.attendedRefer2 (IntPtr libSDK, Int32 sessionId, Int32 replaceSessionId, String replaceMethod, String target, String referTo)
```
{% endcode %}

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

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.outOfDialogRefer (Int32 replaceSessionId, String replaceMethod, String target, String referTo)
```
{% endcode %}

Send an out of dialog REFER to replace the specified call.&#x20;

**Parameters**

| _replaceSessionId_ | The session ID of the session which will be replaced.                                    |
| ------------------ | ---------------------------------------------------------------------------------------- |
| _replaceMethod_    | The SIP method name which will be added in the "Refer-To" header, usually INVITE or BYE. |
| _target_           | The target to which the REFER message will be sent.                                      |
| _referTo_          | The URI to be added into the "Refer-To" header.                                          |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.acceptRefer (Int32 referId, String referSignalingMessage)
```

Accept the REFER request, and a new call will be made if called this function. The function is usually called after onReceivedRefer callback event.

**Parameters**

| _referId_                | The ID of REFER request that comes from onReceivedRefer callback event.          |
| ------------------------ | -------------------------------------------------------------------------------- |
| _referSignalingMes sage_ | The SIP message of REFER request that comes from onReceivedRefer callback event. |

**Returns**

If the function succeeds, it will return a session ID greater than 0 to the new call for REFER; otherwise a specific error code less than 0.

***

```csharp
Int32 PortSIP.PortSIPLib.rejectRefer (Int32 referId)
```

Reject the REFER request.

**Parameters**

| _referId_ | The ID of REFER request that comes from onReceivedRefer callback event. |
| --------- | ----------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Send audio and video stream functions**&#x20;

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.enableSendPcmStreamToRemote (Int32 sessionId, Boolean state, Int32 streamSamplesPerSec)
```
{% endcode %}

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

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.sendPcmStreamToRemote (Int32 sessionId, byte[] data, Int32 dataLength)
```
{% endcode %}

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

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.enableSendVideoStreamToRemote (Int32 sessionId, Boolean state)
```
{% endcode %}

Enable the SDK send video stream data to remote side from another source instead of camera. **Parameters**

| _sessionId_ | The session ID of call.                                        |
| ----------- | -------------------------------------------------------------- |
| _state_     | Set to true to enable the sending stream, or false to disable. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.sendVideoStreamToRemote (Int32 sessionId, byte[] data, Int32 dataLength, Int32 width, Int32 height)
```
{% endcode %}

Send the video stream to remote side.

&#x20;**Parameters**

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

Usually it should be used as below:

enableSendVideoStreamToRemote(sessionId, true); sendVideoStreamToRemote(sessionId, data, dataSize, 352, 288);

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.enableSendScreenStreamToRemote (Int32 sessionId, Boolean state)
```
{% endcode %}

Enable the SDK send Screen stream data to remote side from selected screen source instead of camera.

**Parameters**

| _sessionId_ | The session ID of call.                                        |
| ----------- | -------------------------------------------------------------- |
| _state_     | Set to true to enable the sending stream, or false to disable. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **RTP packets, Audio stream and video stream callback functions**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.enableAudioStreamCallback (Int32 sessionId, Boolean enable, DIRECTION_MODE direction)
```
{% endcode %}

Enable/disable the audio stream callback.&#x20;

**Parameters**

| _sessionId_           | The session ID of call.                                                                  |
| --------------------- | ---------------------------------------------------------------------------------------- |
| _enable_              | Set to true to enable audio stream callback, or false to stop the callback.              |
| _direction_           | The audio stream callback direction.                                                     |
| Type                  | Description                                                                              |
| DIRECTION\_SEND       | Callback the send audio stream for one channel based on the given sessionId.             |
| DIRECTION\_RECV       | Callback the received audio stream for one channel based on the given sessionId.         |
| DIRECTION\_SEND\_RECV | Callback both send & received audio stream for one channel based on the given sessionId. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onAudioRawCallback event will be triggered if the callback is enabled.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.enableVideoStreamCallback (Int32 sessionId, DIRECTION_MODE direction)
```
{% endcode %}

Enable/disable the video stream callback.&#x20;

**Parameters**

| _callbackObject_      | The callback object that you passed in can be accessed once callback function triggered. |
| --------------------- | ---------------------------------------------------------------------------------------- |
| _sessionId_           | The session ID of call.                                                                  |
| _direction_           | The video stream callback direction.                                                     |
| Type                  | Description                                                                              |
| DIRECTION\_SEND       | Callback the send video stream for one channel based on the given sessionId.             |
| DIRECTION\_RECV       | Callback the received video stream for one channel based on the given sessionId.         |
| DIRECTION\_SEND\_RECV | Callback both send & received video stream for one channel based on the given sessionId. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onVideoRawCallback event will be triggered if the callback is enabled.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.enableScreenStreamCallback (Int32 sessionId, DIRECTION_MODE direction)
```
{% endcode %}

Enable/disable the video stream callback.&#x20;

**Parameters**

| _callbackObject_      | The callback object that you passed in can be accessed once callback function triggered. |
| --------------------- | ---------------------------------------------------------------------------------------- |
| _sessionId_           | The session ID of call.                                                                  |
| _direction_           | The video stream callback direction.                                                     |
| Type                  | Description                                                                              |
| DIRECTION\_SEND       | Callback the send video stream for one channel based on the given sessionId.             |
| DIRECTION\_RECV       | Callback the received video stream for one channel based on the given sessionId.         |
| DIRECTION\_SEND\_RECV | Callback both send & received video stream for one channel based on the given sessionId. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onVideoRawCallback event will be triggered if the callback is enabled.

***

### **Record functions**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.startRecord (Int32 sessionId, String recordFilePath, String recordFileName, Boolean appendTimestamp, Int32 channels, FILE_FORMAT recordFileFormat, RECORD_MODE audioRecordMode, RECORD_MODE videoRecordMode)
```
{% endcode %}

Start recording the call.

&#x20;**Parameters**

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

***

```csharp
Int32 PortSIP.PortSIPLib.stopRecord (Int32 sessionId)
```

Stop record.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

***

### **Play audio and video file to remote functions**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.startPlayingFileToRemote (Int32 sessionId, String fileName, Boolean loop, Int32 playAudio)
```
{% endcode %}

Play a file to remote party.&#x20;

**Parameters**

| _sessionId_ | Session ID of the call.                                                                  |
| ----------- | ---------------------------------------------------------------------------------------- |
| _fileUrl_   | url or file name, such as "/test.mp4","/test.wav".                                       |
| _loop_      | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _playAudio_ | 0 - Not play file audio. 1 - Play file audio, 2 - Play file audio mix with Mic.          |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.stopPlayingFileToRemote (Int32 sessionId)
```

Stop playing file to remote party.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.startPlayingFileLocally (String fileUrl, Boolean loop, IntPtr playVideoWindow)
```
{% endcode %}

Play a file to remote party.&#x20;

**Parameters**

| _sessionId_       | Session ID of the call.                                                                  |
| ----------------- | ---------------------------------------------------------------------------------------- |
| _fileUrl_         | url or file name, such as "/test.mp4","/test.wav".                                       |
| _loop_            | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _playVideoWindow_ | The PortSIPVideoRenderView used for displaying the video.                                |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.stopPlayingFileLocally ()
```

Stop playing file to locally.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Conference functions**

```csharp
Int32 PortSIP.PortSIPLib.createAudioConference ()
```

Create an audio conference. It will be failed if the existent conference is not ended yet.&#x20;

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.createVideoConference (IntPtr conferenceVideoWindow, Int32 width, Int32 height, Int32 layout)
```
{% endcode %}

Create a video conference. It will be failed if the existent conference is not ended yet.&#x20;

**Parameters**

| _conferenceVideoWindow_ | The UIView used to display the conference video.                                                                                                                                                            |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _videoResolution_       | The conference video resolution.                                                                                                                                                                            |
| _layout_                | Conference Video layout, default is 0 - Adaptive. 0 - Adaptive(1,3,5,6) 1 - Only Local Video 2 - 2 video,PIP 3 - 2 video, Left and right 4 - 2 video, Up and Down 5 - 3 video 6 - 4 split video 7 - 5 video |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setConferenceVideoWindow (IntPtr videoWindow)
```

Set the window for a conference that is used to display the received remote video image.

**Parameters**

| _videoWindow_ | The UIView used to display the conference video. |
| ------------- | ------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.joinToConference (Int32 sessionId)
```

Join a session into existent conference. If the call is in hold, it will be un-hold automatically.&#x20;

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.removeFromConference (Int32 sessionId)
```

Remove a session from an existent conference.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **RTP and RTCP QOS functions**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.setAudioRtcpBandwidth (Int32 sessionId, Int32 BitsRR, Int32 BitsRS, Int32 KBitsAS)
```
{% endcode %}

Set the audio RTCP bandwidth parameters to the RFC3556.&#x20;

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |
| _BitsRR_    | The bits for the RR parameter.       |
| _BitsRS_    | The bits for the RS parameter.       |
| _KBitsAS_   | The Kbits for the AS parameter.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.setVideoRtcpBandwidth (Int32 sessionId, Int32 BitsRR, Int32 BitsRS, Int32 KBitsAS)
```
{% endcode %}

Set the video RTCP bandwidth parameters as the RFC3556.&#x20;

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |
| _BitsRR_    | The bits for the RR parameter.       |
| _BitsRS_    | The bits for the RS parameter.       |
| _KBitsAS_   | The Kbits for the AS parameter.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **RTP statistics functions**

```csharp
Int32 PortSIP.PortSIPLib.getStatistics (Int32 sessionId)
```

Obtain the statistics of channel. the event onStatistics will be triggered.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Audio effect functions**

```csharp
void PortSIP.PortSIPLib.enableVAD (Boolean state)
```

Enable/disable Voice Activity Detection (VAD).&#x20;

**Parameters**

| _state_ | Set to true to enable VAD, or false to disable. |
| ------- | ----------------------------------------------- |

***

```csharp
void PortSIP.PortSIPLib.enableAEC (Boolean state)
```

Enable/disable AEC (Acoustic Echo Cancellation).

**Parameters**

| _state_ | Set it to true to enable AEC, or false to disable. |
| ------- | -------------------------------------------------- |

***

```csharp
void PortSIP.PortSIPLib.enableCNG (Boolean state)
```

Enable/disable Comfort Noise Generator (CNG).&#x20;

**Parameters**

| _state_ | Set it to true to enable CNG, or false to disable. |
| ------- | -------------------------------------------------- |

***

```csharp
void PortSIP.PortSIPLib.enableAGC (Boolean state)
```

Enable/disable Automatic Gain Control (AGC).&#x20;

**Parameters**

| _state_ | Set it to true to enable AGC, or false to disable. |
| ------- | -------------------------------------------------- |

***

```csharp
void PortSIP.PortSIPLib.enableANS (Boolean state)
```

Enable/disable Audio Noise Suppression (ANS).&#x20;

**Parameters**

| _state_ | Set it to true to enable ANS, or false to disable. |
| ------- | -------------------------------------------------- |

***

```csharp
Int32 PortSIP.PortSIPLib.enableAudioQos (Boolean state)
```

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for audio channel.

**Parameters**

| _state_ | Set to true to enable audio QoS, and DSCP value will be 46; or false to disable audio QoS, and DSCP value will be 0. |
| ------- | -------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.enableVideoQos (Boolean state)
```

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for video channel.

**Parameters**

| _state_ | Set as true to enable video QoS and DSCP value will be 34; or false to disable Video Qos , and DSCP value will be 0. |
| ------- | -------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setVideoMTU (Int32 mtu)
```

Set the MTU size for video RTP packet.

**Parameters**

| _mtu_ | Set MTU value. Allow value ranges (512-65507). Other value will be modified to the default 1450. |
| ----- | ------------------------------------------------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

### **Send OPTIONS/INFO/MESSAGE functions**

```csharp
Int32 PortSIP.PortSIPLib.sendOptions (String to, String sdp)
```

Send OPTIONS message.

**Parameters**

| _to_ | The recipient of OPTIONS message.                                                                     |
| ---- | ----------------------------------------------------------------------------------------------------- |
| sdp  | The SDP of OPTIONS message. It's optional if user does not wish to send the SDP with OPTIONS message. |
|      |                                                                                                       |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.sendInfo (Int32 sessionId, String mimeType, String subMimeType, String infoContents)
```
{% endcode %}

Send a INFO message to remote side in a call. **Parameters**

| _sessionId_    | The session ID of call.                      |
| -------------- | -------------------------------------------- |
| _mimeType_     | The mime type of INFO message.               |
| _subMimeType_  | The sub mime type of INFO message.           |
| _infoContents_ | The contents that is sent with INFO message. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.sendSubscription (String to, String eventName)
```

Send a SUBSCRIBE message to subscribe an event.

**Parameters**

| _to_        | The user/extension to be subscribed. |
| ----------- | ------------------------------------ |
| _eventName_ | The event name to be subscribed.     |

**Returns**

If the function succeeds, it will return the ID of SUBSCRIBE which is greater than 0. If the function fails, it will return a specific error code which is less than 0.

**Remarks**

Example 1, below code indicates that user/extension 101 is subscribed to MWI (Message Waiting notifications) for checking his voicemail: int32 mwiSubId = sendSubscription("sip:101@test.com", "message-summary");

Example 2, to monitor a user/extension call status, You can use code: sendSubscription("100", "dialog"); Extension 100 refers to the user/extension to be monitored. Once being monitored, when extension 100 hold a call or is ringing, the onDialogStateUpdated callback will be triggered.

***

```csharp
Int32 PortSIP.PortSIPLib.terminateSubscription (Int32 subscribeId)
```

Terminate the given subscription.&#x20;

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

For example, if you want stop check the MWI, use below code:

`terminateSubscription(mwiSubId);`&#x20;

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.sendMessage (Int32 sessionId, String mimeType, String subMimeType, byte[] message, Int32 messageLength)
```
{% endcode %}

Send a MESSAGE message to remote side in dialog.&#x20;

**Parameters**

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

`sendMessage(sessionId, "text", "plain", "hello",6);`&#x20;

Example 2: send a binary message.

`sendMessage(sessionId, "application", "vnd.3gpp.sms", binData, binDataSize);`

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.sendOutOfDialogMessage (String to, String mimeType, String subMimeType, Boolean isSMS, byte[] message, Int32 messageLength)
```
{% endcode %}

Send an out of dialog MESSAGE message to remote side.&#x20;

**Parameters**

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

`sendOutOfDialogMessage("sip:user1@sip.portsip.com", "text", "plain", false, "hello", 6);`

Example 2: send a binary message.

`sendOutOfDialogMessage("sip:user1@sip.portsip.com","application", "vnd.3gpp.sms", false, binData, binDataSize);`

***

```csharp
Int32 PortSIP.PortSIPLib.setDefaultSubscriptionTime (Int32 secs)
```

Set the default expiration time to be used when creating a subscription.&#x20;

**Parameters**

| _secs_ | The default expiration time of subscription. |
| ------ | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setDefaultPublicationTime (Int32 secs)
```

Set the default expiration time to be used when creating a publication.

**Parameters**

| _secs_ | The default expiration time of publication. |
| ------ | ------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setPresenceMode (Int32 mode)
```

Indicate the SDK uses the P2P mode for presence or presence agent mode.&#x20;

**Parameters**

| _mode_ | 0 - P2P mode; 1 - Presence Agent mode, default is P2P mode. |
| ------ | ----------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Since presence agent mode requires the PBX/Server support the PUBLISH, please ensure you have your and PortSIP PBX support this feature. For more details please visit: [https://www.portsip.com/portsip-pbx](https://www.portsip.com/portsip-pbx)

***

### **Presence functions**&#x20;

```csharp
Int32 PortSIP.PortSIPLib.presenceSubscribe (String to, String subject)
```

Send a SUBSCRIBE message for subscribing the contact's presence status.

**Parameters**

| _to_      | The target contact. It must be like sip[:contact001@sip.portsip.com.](mailto:contact001@sip.portsip.com)                                                                           |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _subject_ | <p>This subject text will be inserted into the SUBSCRIBE message. For example: "Hello, I'm Jason".</p><p>The subject maybe in UTF-8 format. You should use UTF-8 to decode it.</p> |

**Returns**

If the function succeeds, it will return value subscribeId. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.presenceTerminateSubscribe (Int32 subscribeId)
```

Terminate the given presence subscription.

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.presenceRejectSubscribe (Int32 subscribeId)
```

Reject a presence SUBSCRIBE request which is received from contact.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event includes the subscription ID. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

If the P2P presence mode is enabled, when someone subscribe your presence status, you will receive the subscribe request in the callback, and you can use this function to accept it.

***

```csharp
Int32 PortSIP.PortSIPLib.presenceAcceptSubscribe (Int32 subscribeId)
```

Accept the presence SUBSCRIBE request which is received from contact.&#x20;

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event will include the subscription ID. |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

If the P2P presence mode is enabled, when someone subscribes your presence status, you will receive the subscription request in the callback, and you can use this function to reject it.

***

```csharp
Int32 PortSIP.PortSIPLib.setPresenceStatus (Int32 subscribeId, String stateText)
```

Set the presence status.

**Parameters**

| _subscribeId_ | Subscription ID.                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _stateText_   | The state text of presence online. For example: "I'm here". If you want to appear as offline to others, please pass the "Offline" to "statusText" parameter. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

With P2P presence mode, when receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event includes the subscription ID. This

function will cause the SDK sending a NOTIFY message to update your presence status, and you must pass the correct subscribeId.

With presence agent mode, this function will cause the SDK to send a PUBLISH message to update your presence status, and you must pass 0 to the "subscribeId" parameter.

***

### **Device Manage functions.**&#x20;

```csharp
Int32 PortSIP.PortSIPLib.getNumOfRecordingDevices ()
```

Gets the count of audio devices available for audio recording.&#x20;

**Returns**

It will return the count of recording devices. If the function fails, it will return a specific error code less than 0.

***

```csharp
Int32 PortSIP.PortSIPLib.getNumOfPlayoutDevices ()
```

Gets the number of audio devices available for audio playout.&#x20;

**Returns**

It will return the count of playout devices. If the function fails, it will return a specific error code less than 0.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.getRecordingDeviceName (Int32 deviceIndex, StringBuilder nameUTF8, Int32 nameUTF8Length)
```
{% endcode %}

Gets the name of a specific recording device given by an index.&#x20;

**Parameters**

| _deviceIndex_    | Device index (0, 1, 2, ..., N-1), where N is given by getNumOfRecordingDevices (). Also -1 is a valid value and will return the name of the default recording device. |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _nameUTF8_       | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                               |
| _nameUTF8Length_ | The size of nameUTF8 buffer. It cannot be less than 128.                                                                                                              |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.getPlayoutDeviceName (Int32 deviceIndex, StringBuilder nameUTF8, Int32 nameUTF8Length)
```
{% endcode %}

Get the name of a specific playout device given by an index.&#x20;

**Parameters**

| _deviceIndex_    |                                                                                                                                                                       |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _deviceIndex_    | Device index (0, 1, 2, ..., N-1), where N is given by getNumOfRecordingDevices (). Also -1 is a valid value and will return the name of the default recording device. |
| _nameUTF8_       | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                               |
| _nameUTF8Length_ | The size of nameUTF8 buffer. It cannot be less than 128.                                                                                                              |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setSpeakerVolume (Int32 volume)
```

Set the speaker volume level.

**Parameters**

| _volume_ | Volume level of speaker. Valid value ranges 0 - 255. |
| -------- | ---------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.getSpeakerVolume ()
```

Gets the speaker volume level.

**Returns**

If the function succeeds, it will return the speaker volume with valid range 0 - 255. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.setMicVolume (Int32 volume)
```

Sets the microphone volume level.

**Parameters**

| _volume_ | The microphone volume level. Valid value ranges 0 - 255. |
| -------- | -------------------------------------------------------- |

**Returns**

If the function succeeds, the return value is 0. If the function fails, the return value is a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.getMicVolume ()
```

Retrieves the current microphone volume.&#x20;

**Returns**

If the function succeeds, it will return the microphone volume. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.getScreenSourceCount ()
```

Retrieves the current number of screen.

**Returns**

If the function succeeds, it will return the screen number. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.getScreenSourceTitle (Int32 deviceIndex, StringBuilder nameUTF8, Int32 nameUTF8Length)
```
{% endcode %}

Retrieves the current screen title .&#x20;

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.selectScreenSource (Int32 nDeviceIndex)
```

Sets the Screen to share .

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.SetScreenFrameRate (Int32 nFrameRate)
```

Sets the Screen video framerate .

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

***

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.PortSIPLib.setScreenVideoWindow (Int32 sessionId, IntPtr screenVideoWindow)
```
{% endcode %}

Set the window for a session that is used to display the received screen video .&#x20;

**Parameters**

| _sessionId_          | The session ID of the call.                        |
| -------------------- | -------------------------------------------------- |
| _remoteVideoWindo w_ | The window to display received remote video image. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
void PortSIP.PortSIPLib.audioPlayLoopbackTest (Boolean enable)
```

Use it for the audio device loop back test.

**Parameters**

| _enable_ | Set to true to start audio look back test; or fase to stop. |
| -------- | ----------------------------------------------------------- |

***

```csharp
Int32 PortSIP.PortSIPLib.getNumOfVideoCaptureDevices ()
```

Get the number of available capturing devices.

**Returns**

It will return the count of video capturing devices. If it fails, it will return a specific error code less than 0.

***

```
Int32 PortSIP.PortSIPLib.getVideoCaptureDeviceName (Int32 deviceIndex, 
StringBuilder uniqueIdUTF8, 
Int32 uniqueIdUTF8Length, 
StringBuilder deviceNameUTF8,
 Int32 deviceNameUTF8Length)
```

Get the name of a specific video capture device given by an index.&#x20;

**Parameters**

| _deviceIndex_           | Device index (0, 1, 2, ..., N-1), where N is given by getNumOfVideoCaptureDevices (). Also -1 is a valid value and will return the name of the default capturing device. |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _uniqueIdUTF8_          | Unique identifier of the capturing device.                                                                                                                               |
| _uniqueIdUTF8Len gth_   | Size in bytes of uniqueIdUTF8.                                                                                                                                           |
| _deviceNameUTF8_        | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                                  |
| _deviceNameUTF8 Length_ | The size of nameUTF8 buffer. It cannot be less than 128.                                                                                                                 |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

```csharp
Int32 PortSIP.PortSIPLib.showVideoCaptureSettingsDialogBox (String uniqueIdUTF8, 
                                Int32 uniqueIdUTF8Length, 
                                String dialogTitle, 
                                IntPtr parentWindow, 
                                Int32 x, 
                                Int32 y)
```

Display the capture device property dialog box for the specified capture device.&#x20;

**Parameters**

| _uniqueIdUTF8_        | Unique identifier of the capture device.                                     |
| --------------------- | ---------------------------------------------------------------------------- |
| _uniqueIdUTF8Len gth_ | Size in bytes of uniqueIdUTF8.                                               |
| _dialogTitle_         | The title of the video settings dialog.                                      |
| _parentWindow_        | Parent window used for the dialog box. It should originally be a HWND.       |
| _x_                   | Horizontal position for the dialog relative to the parent window, in pixels. |
| _y_                   | Vertical position for the dialog relative to the parent window, in pixels.   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

***

## **SDK Callback events**

### **Register events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRegisterSuccess (String statusText, Int32 statusCode, StringBuilder sipMessage)
```
{% endcode %}

When successfully registered to server, this event will be triggered.&#x20;

**Parameters**

| _callbackIndex_  | This is a unique identifier or index associated with a specific callback function that is registered with the SDK library during initialization. When the SDK encounters an event or condition that triggers the callback, it uses the callback index to locate and execute the corresponding function.                   |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _callbackObject_ | A callback object is a user-defined object or structure that contains pointers to callback functions. When creating the SDK library, you pass this callback object to the SDK. The SDK then stores the callback object and uses it to invoke the appropriate callback functions when specific events or conditions occur. |
| _statusText_     | A human-readable description of the status of the operation callback event.                                                                                                                                                                                                                                               |
| _statusCode_     | A numerical code representing the status of the operation callbck event. The specific codes and their meanings are defined in the SIP protocol.                                                                                                                                                                           |
| _sipMessage_     | The complete SIP message that was received as part of the operation.                                                                                                                                                                                                                                                      |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRegisterFailure (String statusText, Int32 statusCode, StringBuilder sipMessage)
```
{% endcode %}

This event will be triggered if the SDK fails to register with the SIP server. This can occur due to various reasons, such as network connectivity issues, incorrect SIP credentials, or server errors.

**Parameters**

| _statusText_ | A human-readable description of the status of the register failure reason. This provides additional information about why the registration attempt failed, such as network errors, authentication issues, or server-specific reasons.                                    |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _statusCode_ | A numerical code representing the status of the register failure. This code corresponds to a specific error condition defined in the SIP protocol. By examining this code, you can determine the exact reason for the registration failure and take appropriate actions. |
| _sipMessage_ | The complete SIP message that was received as part of the operation.                                                                                                                                                                                                     |

### **Call events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteIncoming (Int32 sessionId, String callerDisplayName, String caller, String calleeDisplayName, String callee, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo, StringBuilder sipMessage)
```
{% endcode %}

This callback function is invoked when an incoming call is received.

**Parameters**

<table data-header-hidden><thead><tr><th width="367"></th><th></th></tr></thead><tbody><tr><td><em>sessionId</em></td><td>The unique identifier for the incoming call.</td></tr><tr><td><em>callerDisplayNam e</em></td><td>The display name of the calling party as provided in the SIP INVITE message.</td></tr><tr><td><em>caller</em></td><td>he SIP URI of the calling party.</td></tr><tr><td><em>calleeDisplayNam e</em></td><td>The display name of the receiving party as specified in the SIP INVITE message.</td></tr><tr><td><em>callee</em></td><td>The SIP URI of the receiving party.</td></tr><tr><td><em>audioCodecNames</em></td><td>A string containing the names of the matched audio codecs for the call, separated by "#" if there are multiple codecs.</td></tr><tr><td><em>videoCodecNames</em></td><td>A string containing the names of the matched video codecs for the call, separated by "#" if there are multiple codecs.</td></tr><tr><td><em>existsAudio</em></td><td>Boolean value indicating whether the call includes audio.</td></tr><tr><td><em>existsVideo</em></td><td>A Boolean value indicating whether the call includes video.</td></tr><tr><td><em>sipMessage</em></td><td>A string object containing the complete SIP INVITE message received.</td></tr></tbody></table>

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteTrying (Int32 sessionId)
```
{% endcode %}

If the outgoing call is being processed, this event will be triggered.&#x20;

**Parameters**

| _sessionId_ | The unique identifier for the incoming call. |
| ----------- | -------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteSessionProgress (Int32 sessionId, String audioCodecNames, String videoCodecNames, Boolean existsEarlyMedia, Boolean existsAudio, Boolean existsVideo, StringBuilder sipMessage)
```
{% endcode %}

This callback function is invoked when the SDK receives a 183 Session Progress response from the SIP server during an incoming call. This indicates that the call is progressing and that early media may be available.

**Parameters**

| _sessionId_        | The unique identifier for the incoming call.                                                                                                               |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _audioCodecNames_  | A string containing the names of the matched audio codecs for the call, separated by "#" if there are multiple codecs.                                     |
| _videoCodecNames_  | A string containing the names of the matched video codecs for the call, separated by "#" if there are multiple codecs.                                     |
| _existsEarlyMedia_ | A Boolean value indicating whether early media is available. Early media allows for audio or video to be transmitted before the call is fully established. |
| _existsAudio_      | A Boolean value indicating whether the call includes audio.                                                                                                |
| _existsVideo_      | A Boolean value indicating whether the call includes video.                                                                                                |
| _sipMessage_       | A string containing the complete 183 Session Progress SIP message received.                                                                                |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteRinging (Int32 sessionId, String statusText, Int32 statusCode, StringBuilder sipMessage)
```
{% endcode %}

This callback function is invoked when an outgoing call starts ringing. This indicates that the call has been initiated and is waiting for the remote party to answer.

**Parameters**

| _sessionId_  | The unique identifier for the incoming call.                                                                           |
| ------------ | ---------------------------------------------------------------------------------------------------------------------- |
| _statusText_ | A human-readable description of the call status.                                                                       |
| _statusCode_ | A numerical code representing the call status.                                                                         |
| _sipMessage_ | A string object containing the complete SIP response received from the SIP server indicating that the call is ringing. |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteAnswered (Int32 sessionId, String callerDisplayName, String caller, String calleeDisplayName, String callee, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo, StringBuilder sipMessage)
```
{% endcode %}

This callback function is invoked when the remote party answers an incoming or outgoing call.

**Parameters**

| _sessionId_          | The unique identifier for the call.                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | he display name of the calling party.                                                                                  |
| _caller_             | he SIP URI of the calling party.                                                                                       |
| _calleeDisplayNam e_ | The display name of the receiving party.                                                                               |
| _callee_             | The SIP URI of the receiving party.                                                                                    |
| _audioCodecNames_    | A string containing the names of the matched audio codecs for the call, separated by "#" if there are multiple codecs. |
| _videoCodecNames_    | A string containing the names of the matched video codecs for the call, separated by "#" if there are multiple codecs. |
| _existsAudio_        | A Boolean value indicating whether the call includes audio.                                                            |
| _existsVideo_        | A Boolean value indicating whether the call includes video.                                                            |
| _sipMessage_         | A string object containing the complete SIP INVITE message received.                                                   |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteFailure (Int32 sessionId, String callerDisplayName, String caller, String calleeDisplayName, String callee, String reason, Int32 code, StringBuilder sipMessage)
```
{% endcode %}

This callback function is invoked when an outgoing call fails.

**Parameters**

| _sessionId_          | The unique identifier for the call.                                                                            |
| -------------------- | -------------------------------------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | The display name of the calling party.                                                                         |
| _caller_             | The SIP URI of the calling party.                                                                              |
| _calleeDisplayNam e_ | The display name of callee.The display name of the receiving party.                                            |
| _callee_             | The SIP URI of the receiving party.                                                                            |
| _reason_             | A human-readable description of the reason for the call failure.                                               |
| _code_               | A numerical code representing the reason for the call failure.                                                 |
| _sipMessage_         | A string object containing the complete SIP response received from the SIP server indicating the call failure. |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteUpdated (Int32 sessionId, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo, Boolean existsScreen, StringBuilder sipMessage)
```
{% endcode %}

This callback function is invoked when the remote party updates the parameters of an existing call. This can occur, for example, when the remote party changes the audio or video codecs being used, or when additional media streams are added or removed.

**Parameters**

| _sessionId_       | The unique identifier for the call.                                                                                    |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------- |
| _audioCodecNames_ | A string containing the names of the matched audio codecs for the call, separated by "#" if there are multiple codecs. |
| _videoCodecNames_ | A string containing the names of the matched video codecs for the call, separated by "#" if there are multiple codecs. |
| _existsAudio_     | A Boolean value indicating whether the call includes audio.                                                            |
| _existsVideo_     | A Boolean value indicating whether the call includes video.                                                            |
| _sipMessage_      | A string object containing the complete SIP UPDATE message received.                                                   |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteConnected (Int32 sessionId)
```
{% endcode %}

This callback function is invoked when a call is successfully established. This occurs after both parties have exchanged the necessary SIP messages and agreed to the terms of the call (Received the ACK).

**Parameters**

| _sessionId_ | The unique identifier for the call. |
| ----------- | ----------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteBeginingForward (String forwardTo)
```
{% endcode %}

This callback function is invoked when an incoming call is automatically forwarded to another destination. This occurs if call forwarding is enabled and an incoming call is received.

**Parameters**

| _forwardTo_ | The SIP URI of the destination to which the call is being forwarded. |
| ----------- | -------------------------------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onInviteClosed (Int32 sessionId)
```
{% endcode %}

This callback function is invoked when the remote party ends a call.

**Parameters**

| _sessionId_ | The unique identifier for the call that has been closed. |
| ----------- | -------------------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onDialogStateUpdated (String BLFMonitoredUri, String BLFDialogState, String BLFDialogId, String BLFDialogDirection)
```
{% endcode %}

This callback function is invoked when the status of a monitored user's call changes. This is typically used in BLF (Busy Lamp Field) scenarios where a user subscribes to the status of another user's calls.

**Parameters**

| _BLFMonitoredUri_     | The SIP URI of the user being monitored.                               |
| --------------------- | ---------------------------------------------------------------------- |
| _BLFDialogState_      | A string representing the current state of the monitored user's call.  |
| _BLFDialogId_         | A unique identifier for the monitored user's call.                     |
| _BLFDialogDirecti on_ | A string indicating the direction of the monitored user's call.        |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRemoteHold (Int32 sessionId)
```
{% endcode %}

This callback function is invoked when the remote party places a call on hold.

**Parameters**

| _sessionId_ | The unique identifier for the call that has been placed on hold. |
| ----------- | ---------------------------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRemoteUnHold (Int32 sessionId, String audioCodecNames, String videoCodecNames, Boolean existsAudio, Boolean existsVideo)
```
{% endcode %}

This callback function is invoked when the remote party resumes a call that was previously placed on hold.

**Parameters**

| _sessionId_       | The unique identifier for the call that has been resumed.                                                              |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------- |
| _audioCodecNames_ | A string containing the names of the matched audio codecs for the call, separated by "#" if there are multiple codecs. |
| _videoCodecNames_ | A string containing the names of the matched video codecs for the call, separated by "#" if there are multiple codecs. |
| _existsAudio_     | A Boolean value indicating whether the call includes audio.                                                            |
| _existsVideo_     | A Boolean value indicating whether the call includes video.                                                            |

### **Refer events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onReceivedRefer (Int32 sessionId, Int32 referId, String to, String from, StringBuilder referSipMessage)
```
{% endcode %}

This callback function is invoked when the SDK receives a REFER message during an active call. A REFER message is used to transfer a call to a different party.

**Parameters**

| _sessionId_       | The unique identifier for the call that received the REFER message.                                                                                                                                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _referId_         | A unique identifier for the REFER message itself. This ID is used to accept or reject the transfer.                                                                                                                                                                                                           |
| _to_              | The SIP URI of the target party to which the call is being transferred.                                                                                                                                                                                                                                       |
| _from_            | The SIP URI of the party who sent the REFER message.                                                                                                                                                                                                                                                          |
| _referSipMessage_ | A string object containing the complete REFER SIP message received. This message contains the details of the call transfer request, including the target party, reason for the transfer, and other relevant information. You can pass this message to the `acceptRefer` function to accept the call transfer. |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onReferAccepted (Int32 sessionId)
```
{% endcode %}

This callback function is invoked when the remote party accepts a call transfer that was previously requested using a REFER message.

**Parameters**

| _sessionId_ | The unique identifier for the call that was transferred. |
| ----------- | -------------------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onReferRejected (Int32 sessionId, String reason, Int32 code)
```
{% endcode %}

This callback function is invoked when the remote party rejects a call transfer.

**Parameters**

| _sessionId_ | The unique identifier for the call that was not transferred. This parameter provides a way to track and manage the call even though the transfer was rejected. |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _reason_    | A human-readable description of the reason for the call transfer rejection.                                                                                    |
| _code_      | A numerical code representing the reason for the call transfer rejection.                                                                                      |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onTransferTrying (Int32 sessionId) 
```
{% endcode %}

This callback function is invoked when a call transfer is in progress. This indicates that the SIP SERVER/PBX is processing the REFER message and attempting to transfer the call to the specified destination.

**Parameters**

| _sessionId_ | The unique identifier for the call that is being transferred. |
| ----------- | ------------------------------------------------------------- |

<pre class="language-csharp" data-overflow="wrap"><code class="lang-csharp"><strong>Int32 PortSIP.SIPCallbackEvents.onTransferRinging (Int32 sessionId)
</strong></code></pre>

This callback function is invoked when a call transfer is ringing at the destination. This indicates that the transferred call has reached the target party and is waiting for them to answer.

**Parameters**

| _sessionId_ | The unique identifier for the transferred call. |
| ----------- | ----------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onACTVTransferSuccess (Int32 sessionId)
```
{% endcode %}

This callback function is invoked when an active call transfer is successful. This occurs when the target party accepts a call that was transferred using a REFER message.

**Parameters**

| _sessionId_ | The unique identifier for the transferred call. |
| ----------- | ----------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onACTVTransferFailure (Int32 sessionId, String reason, Int32 code)
```
{% endcode %}

This callback function is invoked when an active call transfer fails. This occurs when the target party rejects a call that was transferred using a REFER message.

**Parameters**

| _sessionId_ | The unique identifier for the transferred call.                           |
| ----------- | ------------------------------------------------------------------------- |
| _reason_    | A human-readable description of the reason for the call transfer failure. |
| _code_      | A numerical code representing the reason for the call transfer failure.   |

### **Signaling events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onReceivedSignaling (Int32 sessionId, StringBuilder signaling)
```
{% endcode %}

This callback function is invoked when the SDK receives any SIP message related to a call. This includes INVITE, ACK, BYE, CANCEL, HOLD, UNHOLD, REFER, and other SIP messages.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _signaling_ | The SIP message received.   |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSendingSignaling (Int32 sessionId, StringBuilder signaling)
```
{% endcode %}

This event will be triggered when a SIP message sent. **Parameters**

| _sessionId_ | The unique identifier for the call associated with the received SIP message. |
| ----------- | ---------------------------------------------------------------------------- |
| _signaling_ | A string object containing the complete SIP message received.                |

### **MWI events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onWaitingVoiceMessage (String messageAccount, Int32 urgentNewMessageCount, Int32 urgentOldMessageCount, Int32 newMessageCount, Int32 oldMessageCount)
```
{% endcode %}

This callback function is invoked when a new voice message (MWI) is waiting for the user.

**Parameters**

| _messageAccount_         | The voice message account associated with the waiting messages. |
| ------------------------ | --------------------------------------------------------------- |
| _urgentNewMessag eCount_ | The number of urgent new voice messages.                        |
| _urgentOldMessage Count_ | The number of urgent old voice messages.                        |
| _newMessageCount_        | The number of new voice messages (non-urgent).                  |
| _oldMessageCount_        | The number of old voice messages (non-urgent).                  |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onWaitingFaxMessage (String messageAccount, Int32 urgentNewMessageCount, Int32 urgentOldMessageCount, Int32 newMessageCount, Int32 oldMessageCount)
```
{% endcode %}

This callback function is invoked when a new fax message (MWI) is waiting for the user.

**Parameters**

| _messageAccount_         | The fax message account associated with the waiting messages. |
| ------------------------ | ------------------------------------------------------------- |
| _urgentNewMessag eCount_ | The number of urgent new fax messages.                        |
| _urgentOldMessage Count_ | The number of urgent old fax messages.                        |
| _newMessageCount_        | The number of new fax messages.                               |
| _oldMessageCount_        | The number of old fax messages.                               |

### **DTMF events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRecvDtmfTone (Int32 sessionId, Int32 tone)
```
{% endcode %}

This callback function is invoked when a DTMF tone is received from the remote party during a call.

**Parameters**

<table data-header-hidden><thead><tr><th width="365"></th><th></th></tr></thead><tbody><tr><td><em>sessionId</em></td><td>The unique identifier for the call.</td></tr><tr><td><em>tone</em></td><td><p>The DTMF tone that was received. The possible values for <code>tone</code> are:</p><ul><li>0: DTMF tone 0</li><li>1: DTMF tone 1</li><li>2: DTMF tone 2</li><li>3: DTMF tone 3</li><li>4: DTMF tone 4</li><li>5: DTMF tone 5</li><li>6: DTMF tone 6</li><li>7: DTMF tone 7</li><li>8: DTMF tone 8</li><li>9: DTMF tone 9</li><li>10: DTMF tone *</li><li>11: DTMF tone #</li><li>12: DTMF tone A</li><li>13: DTMF tone B</li><li>14: DTMF tone C</li><li>15: DTMF tone D</li><li>16: DTMF tone FLASH</li></ul></td></tr></tbody></table>

### **INFO/OPTIONS message events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRecvOptions (StringBuilder optionsMessage)
```
{% endcode %}

This callback function is invoked when the SDK receives an OPTIONS SIP message.

**Parameters**

| _optionsMessage_ | A string object containing the complete OPTIONS SIP message received. |
| ---------------- | --------------------------------------------------------------------- |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRecvInfo (StringBuilder infoMessage)
```
{% endcode %}

This callback function is invoked when the SDK receives an INFO SIP message.&#x20;

**Parameters**

| _infoMessage_ | A string object containing the complete INFO SIP message received. |
| ------------- | ------------------------------------------------------------------ |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRecvNotifyOfSubscription (Int32 subscribeId, StringBuilder notifyMsg, byte[] contentData, Int32 contentLenght)
```
{% endcode %}

This callback function is invoked when the SDK receives a NOTIFY message related to a previously established subscription. A subscription is used to receive updates or notifications from a SIP server or another endpoint.

**Parameters**

| _subscribeId_   | The unique identifier for the subscription associated with the received NOTIFY message.                 |
| --------------- | ------------------------------------------------------------------------------------------------------- |
| _notifyMessage_ | A string object containing the complete NOTIFY SIP message received.                                    |
| _contentData_   | A byte array containing the content of the NOTIFY message body. This can be either text or binary data. |
| _contentLenght_ | The length of the `contentData` in bytes.                                                               |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSubscriptionFailure (Int32 subscribeId, Int32 statusCode)
```
{% endcode %}

This callback function is invoked when a subscription attempt fails. This occurs when the SDK sends a SUBSCRIBE message but receives an error response from the SIP server.

**Parameters**

| _subscribeId_ | The unique identifier for the subscription request that failed.        |
| ------------- | ---------------------------------------------------------------------- |
| _statusCode_  | A numerical code representing the reason for the subscription failure. |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSubscriptionTerminated (Int32 subscribeId)
```
{% endcode %}

This callback function is invoked when a subscription is terminated or expires. This can occur due to various reasons, such as a timeout, explicit termination by the server or client, or other factors.

**Parameters**

| _subscribeId_ | The unique identifier for the terminated or expired subscription. |
| ------------- | ----------------------------------------------------------------- |

### **Presence events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onPresenceRecvSubscribe (Int32 subscribeId, String fromDisplayName, String from, String subject)
```
{% endcode %}

This callback function is invoked when the SDK receives a SUBSCRIBE message from a contact, indicating that the contact wants to subscribe to your presence status.

**Parameters**

| _subscribeId_     | The unique identifier for the SUBSCRIBE request.                                                                                                         |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of the contact who sent the SUBSCRIBE request.                                                                                          |
| _from_            | The SIP URI of the contact who sent the SUBSCRIBE request.                                                                                               |
| _subject_         | The subject of the SUBSCRIBE request. This may indicate the specific type of presence information the contact is interested in (e.g., "online", "busy"). |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onPresenceOnline (String fromDisplayName, String from, String stateText)
```
{% endcode %}

This callback function is invoked when a contact's presence status changes to "online".

**Parameters**

| _fromDisplayName_ | The display name of the contact whose presence status has changed.                                |
| ----------------- | ------------------------------------------------------------------------------------------------- |
| _from_            | The SIP URI of the contact whose presence status has changed.                                     |
| _stateText_       | A human-readable description of the contact's presence status. In this case, it will be "online". |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onPresenceOffline (String fromDisplayName, String from)
```
{% endcode %}

This callback function is invoked when a contact's presence status changes to "offline".

**Parameters**

| _fromDisplayName_ | The display name of the contact whose presence status has changed. |
| ----------------- | ------------------------------------------------------------------ |
| _from_            | The SIP URI of the contact whose presence status has changed.      |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRecvMessage (Int32 sessionId, String mimeType, String subMimeType, byte[] messageData, Int32 messageDataLength)
```
{% endcode %}

This callback function is invoked when the SDK receives a MESSAGE SIP message during a call. A MESSAGE message is used to exchange text or binary data between parties in a call.

**Parameters**

| _sessionId_          | The unique identifier for the call.                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| _mimeType_           | The MIME type of the received message. This specifies the content type of the message, such as "text" or "image".      |
| _subMimeType_        | The sub-MIME type of the received message. This provides additional information about the content type, if applicable. |
| _messageData_        | A byte array containing the content of the received message. This can be text or binary data.                          |
| _messageDataLengt h_ | The length of the `messageData` in bytes.                                                                              |

<pre class="language-csharp" data-overflow="wrap"><code class="lang-csharp"><strong>Int32 PortSIP.SIPCallbackEvents.onRecvOutOfDialogMessage (String fromDisplayName, String from, String toDisplayName, String to, String mimeType, String subMimeType, byte[] messageData, Int32 messageDataLength)
</strong></code></pre>

This callback function is invoked when the SDK receives a MESSAGE SIP message that is not associated with an existing call (out-of-dialog message). This is typically used for messaging scenarios where a direct call is not established.

**Parameters**

| _fromDisplayName_    | The display name of the sender of the message.                                                                         |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| _from_               | The SIP URI of the sender of the message.                                                                              |
| _toDisplayName_      | The display name of the recipient of the message.                                                                      |
| _to_                 | The SIP URI of the recipient of the message.                                                                           |
| _mimeType_           | The MIME type of the received message. This specifies the content type of the message, such as "text" or "image".      |
| _subMimeType_        | The sub-MIME type of the received message. This provides additional information about the content type, if applicable. |
| _messageData_        | A byte array containing the content of the received message. This can be text or binary data.                          |
| _messageDataLengt h_ | The length of the `messageData` in bytes.                                                                              |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSendMessageSuccess (Int32 sessionId, Int32 messageId)
```
{% endcode %}

This callback function is invoked when a MESSAGE message is sent successfully during a call.

**Parameters**

| _sessionId_ | The unique identifier for the call.                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------- |
| _messageId_ | The unique identifier for the sent message. This is the same value that was returned by the `sendMessage` function. |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSendMessageFailure (Int32 sessionId, Int32 messageId, String reason, Int32 code)
```
{% endcode %}

This callback function is invoked when a MESSAGE message fails to be sent, either during a call or out-of-dialog.

**Parameters**

| _sessionId_ | The unique identifier for the call (if applicable).                 |
| ----------- | ------------------------------------------------------------------- |
| _messageId_ | The unique identifier for the message that failed to be sent.       |
| _reason_    | A human-readable description of the reason for the message failure. |
| _code_      | A numerical code representing the reason for the message failure.   |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSendOutOfDialogMessageSuccess (Int32 messageId, String fromDisplayName, String from, String toDisplayName, String to)
```
{% endcode %}

This callback function is invoked when a MESSAGE message is sent successfully out of dialog (not associated with an existing call).

**Parameters**

| _messageId_       | The unique identifier for the sent message. |
| ----------------- | ------------------------------------------- |
| _fromDisplayName_ | The display name of the message sender.     |
| _from_            | The SIP URI of the message sender.          |
| _toDisplayName_   | The display name of the message recipient.  |
| to                | The SIP URI of the message recipient.       |
|                   |                                             |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onSendOutOfDialogMessageFailure (Int32 messageId, String fromDisplayName, String from, String toDisplayName, String to, String reason, Int32 code)
```
{% endcode %}

This callback function is invoked when a MESSAGE message fails to be sent out of dialog.

**Parameters**

| _messageId_       | The unique identifier for the message that failed to be sent. This is the same value that was returned by the `SendOutOfDialogMessage` function when the message was originally sent. It allows you to track and reference the failed message. |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of the message sender.                                                                                                                                                                                                        |
| _from_            | The SIP URI of the message sender.                                                                                                                                                                                                             |
| _toDisplayName_   | The display name of the message recipient.                                                                                                                                                                                                     |
| _to_              | he SIP URI of the message recipient.                                                                                                                                                                                                           |
| _reason_          | A human-readable description of the reason for the message failure.                                                                                                                                                                            |
| _code_            | A numerical code representing the reason for the message failure.                                                                                                                                                                              |

### **Play audio and video files finished events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onPlayFileFinished (Int32 sessionId, String fileName)
```
{% endcode %}

This callback function is invoked when the playback of a file to the remote party has completed in a non-looping mode.

**Parameters**

| _sessionId_ | The unique identifier for the call.   |
| ----------- | ------------------------------------- |
| _fileName_  | The name of the file that was played. |

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onStatistics (Int32 sessionId, String stat)
```
{% endcode %}

This callback function is invoked when RTP statistics are received for a given session. This occurs after calling the `getStatistics` function.

**Parameters**

| _sessionId_ | The unique identifier for the call.                             |
| ----------- | --------------------------------------------------------------- |
| _stat_      | A JSON string representing the RTP statistics for the session.  |

### **RTP callback events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onRTPPacketCallback (IntPtr callbackObject, Int32 sessionId, Int32 mediaType, Int32 direction, byte[] RTPPacket, Int32 packetSize)
```
{% endcode %}

This callback function is invoked when an RTP packet is received or sent during a call. This callback is enabled by calling the `enableRtpCallback` function.

**Parameters**

| _sessionId_  | The unique identifier for the call.                                                                                                                                                                                                 |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _mediaType_  | <p></p><p>The type of media associated with the RTP packet:</p><ul><li>0: Audio</li><li>1: Video</li><li>2: Screen</li></ul>                                                                                                        |
| _direction_  | <p></p><p>The direction of the RTP stream:</p><ul><li><code>DIRECTION_SEND</code>: Callback for sending RTP streams for a channel.</li><li><code>DIRECTION_RECV</code>: Callback for receiving RTP streams for a channel.</li></ul> |
| _RTPPacket_  | A pointer to the memory containing the entire RTP packet.                                                                                                                                                                           |
| _packetSize_ | The size of the received RTP packet in bytes.                                                                                                                                                                                       |

**Note**

It is important to avoid calling SDK API functions directly within this callback function, as it may lead to performance issues or unexpected behavior. Instead, consider posting a message to another thread to execute time-consuming operations.

### **Audio and video stream callback events**

{% code overflow="wrap" %}
```csharp
Int32 PortSIP.SIPCallbackEvents.onAudioRawCallback (IntPtr callbackObject, Int32 sessionId, Int32 callbackType, byte[] data, Int32 dataLength, Int32 samplingFreqHz)
```
{% endcode %}

This callback function is invoked when audio packets are received or sent during a call. This callback is enabled by calling the `enableAudioStreamCallback` function.

**Parameters**

| _sessionId_          | The unique identifier for the call.                                                           |
| -------------------- | --------------------------------------------------------------------------------------------- |
| _audioCallbackMod e_ | The type of audio callback, as specified in the `enableAudioStreamCallback` function.         |
| _data_               | A byte array containing the raw audio data in PCM format.                                     |
| _dataLength_         | The size of the audio data in bytes.                                                          |
| _samplingFreqHz_     | The sampling frequency of the audio data in Hertz (Hz). Common values include 8000 and 16000. |

**Note**

It is important to avoid calling SDK API functions directly within this callback function, as it may lead to performance issues or unexpected behavior. Instead, consider posting a message to another thread to execute time-consuming operations.

```
Int32 PortSIP.SIPCallbackEvents.onVideoRawCallback (IntPtr callbackObject, 
                                Int32 sessionId, 
                                Int32 callbackType, 
                                Int32 width, 
                                Int32 height, 
                                byte[] data, 
                                Int32 dataLength)
```

This callback function is invoked when video packets are received or sent during a call. This callback is enabled by calling the `enableVideoStreamCallback` function.

**Parameters**

```C#
```

| _sessionId_          | The unique identifier for the call.                                                   |
| -------------------- | ------------------------------------------------------------------------------------- |
| _videoCallbackMod e_ | The type of video callback, as specified in the `enableVideoStreamCallback` function. |
| _width_              | The width of the video frame in pixels.                                               |
| _height_             | The height of the video frame in pixels.                                              |
| _data_               | A byte array containing the raw video data in YUV420 format (YV12).                   |
| _dataLength_         | The size of the video data in bytes.                                                  |

**Note**

It is important to avoid calling SDK API functions directly within this callback function, as it may lead to performance issues or unexpected behavior. Instead, consider posting a message to another thread to execute time-consuming operations.

