# User Manual for Android

## **FAQ**

### **Where can I download the PortSIP VoIP SDK for testing?**

You can download the PortSIP VoIP SDK and sample projects from the [PortSIP Website](https://www.portsip.com/download-portsip-voip-sdk/).

### **What Android API version is required?**

The PortSIP VoIP SDK requires Android API version 16 or later.

### **How can I create a new project with the PortSIP VoIP SDK?**

1. **Download the Sample Project and SDK:** Obtain the sample project and trial SDK from the [PortSIP Website](https://www.portsip.com/download-portsip-voip-sdk/) and extract them to a desired directory.
2. **Create a New Project:** In Android Studio, create a new Android Application project.
3. **Add the SDK Libraries:** Copy all files from the `libs` directory of the extracted SDK to the `libs` directory of your new application.
4.  **Import Necessary Classes:** Import the required classes from the SDK:

    ```java
    import com.portsip.OnPortSIPEvent;
    import com.portsip.PortSipSdk;
    ```

### **How can I process callback events?**

1. **Implement the OnPortSIPEvent Interface:** Your class should implement the `OnPortSIPEvent` interface to handle callback events.
2. **Override Callback Methods:** Override the necessary callback methods (e.g., `onRegistrationState`, `onCallState`) to handle specific events.

### **How do I initialize the SDK?**

1. **Create an Instance:** Create an instance of the `PortSipSdk` class.
2. **Set the Event Listener:** Set the event listener using `setOnPortSIPEvent`.
3. **Create a Call Manager:** Create a call manager using `CreateCallManager`.
4. **Initialize the SDK:** Call the `initialize` method with appropriate parameters.

### **Is the SDK thread-safe?**

Yes, the PortSIP SDK is thread-safe. You can call API functions from multiple threads without worrying about synchronization issues. However, there are exceptions: the `onAudioRawCallback`, `onVideoRawCallback`, and `onRTPPacketCallback` callbacks should not be called directly from other threads.

## SDK Callback events

### Register events

```java
void onRegisterSuccess(String reason, int code,String sipMessage);
```

When successfully registered to server, this event will be triggered.

**Parameters**

| _reason_     | The status text.          |
| ------------ | ------------------------- |
| _code_       | The status code.          |
| _sipMessage_ | The SIP message received. |

```java
void onRegisterFailure(String reason, int code,String sipMessage);
```

When failed to register to SIP server, this event will be triggered.

**Parameters**

| _reason_     | The status text.          |
| ------------ | ------------------------- |
| _code_       | The status code.          |
| _sipMessage_ | The SIP message received. |

**Call events**

```java
void onInviteIncoming(long sessionId,
                      String callerDisplayName,
                      String caller,
                      String calleeDisplayName,
                      String callee,
                      String audioCodecs,
                      String videoCodecs,
                      boolean existsAudio,
                      boolean existsVideo,
                      String sipMessage);
```

When a call is coming, this event will be triggered.

**Parameters**

| _sessionId_          | The session ID of the call.                                                        |
| -------------------- | ---------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | The display name of caller                                                         |
| _caller_             | The caller.                                                                        |
| _calleeDisplayNam e_ | The display name of callee.                                                        |
| _callee_             | The callee.                                                                        |
| _audioCodecs_        | The matched audio codecs. It's separated by "#" if there are more than one codecs. |
| _videoCodecs_        | The matched video codecs. It's separated by "#" if there are more than one codecs. |
| _existsAudio_        | By setting to true, it means that this call include the audio.                     |
| _existsVideo_        | By setting to true, it means that this call include the video.                     |
| _sipMessage_         | The SIP message received.                                                          |

```java
void onInviteTrying(long sessionId);
```

If the outgoing call is being processed, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onInviteSessionProgress(long sessionId,
                             String audioCodecs,
                             String videoCodecs,
                             boolean existsEarlyMedia,
                             boolean existsAudio,
                             boolean existsVideo,
                             String sipMessage);
```

Once the caller received the "183 session progress" message, this event would be triggered.

**Parameters**

| _sessionId_        | The session ID of the call.                                                        |
| ------------------ | ---------------------------------------------------------------------------------- |
| _audioCodecs_      | The matched audio codecs. It's separated by "#" if there are more than one codecs. |
| _videoCodecs_      | The matched video codecs. It's separated by "#" if there are more than one codecs. |
| _existsEarlyMedia_ | By setting to true it means the call has early media.                              |
| _existsAudio_      | By setting to true it means this call include the audio.                           |
| _existsVideo_      | By setting to true it means this call include the video.                           |
| _sipMessage_       | The SIP message received.                                                          |

```java
void onInviteRinging(long sessionId,
                                String statusText,
                                int statusCode,
                                String sipMessage);
```

If the outgoing call is ringing, this event will be triggered.

**Parameters**

| _sessionId_  | The session ID of the call. |
| ------------ | --------------------------- |
| _statusText_ | The status text.            |
| _statusCode_ | The status code.            |
| _sipMessage_ | The SIP message received.   |

```java
void onInviteAnswered(long sessionId,
                      String callerDisplayName,
                      String caller,
                      String calleeDisplayName,
                      String callee,
                      String audioCodecs,
                      String videoCodecs,
                      boolean existsAudio,
                      boolean existsVideo,
                      String sipMessage);
```

If the remote party answered the call, this event would be triggered.

**Parameters**

| _sessionId_          | The session ID of the call.                                                        |
| -------------------- | ---------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | The display name of caller                                                         |
| _caller_             | The caller.                                                                        |
| _calleeDisplayNam e_ | The display name of callee.                                                        |
| _callee_             | The callee.                                                                        |
| _audioCodecs_        | The matched audio codecs. It's separated by "#" if there are more than one codecs. |
| _videoCodecs_        | The matched video codecs. It's separated by "#" if there are more than one codecs. |
| _existsAudio_        | By setting to true, this call includes the audio.                                  |
| _existsVideo_        | By setting to true, this call includes the video.                                  |
| _sipMessage_         | The SIP message received.                                                          |

```java
void onInviteFailure(long sessionId, String callerDisplayName,
                                String caller,
                                String calleeDisplayName,
                                String callee,
                                String reason,
                                int code,
                                String sipMessage);
```

This event will be triggered if the outgoing or incoming call fails.

**Parameters**

| _sessionId_          | The session ID of the call.   |
| -------------------- | ----------------------------- |
| _callerDisplayNam e_ | The display name of caller l  |
| _caller_             | The caller. l                 |
| _calleeDisplayNam e_ | The display name of callee. l |
| _callee_             | The callee.                   |
| _reason_             | The failure reason.           |
| _code_               | The failure code.             |
| _sipMessage_         | The SIP message received.     |

```java
public void onInviteUpdated(long sessionId,
                            String audioCodecs,
                            String videoCodecs,
                            String screenCodecs,
                            boolean existsAudio,
                            boolean existsVideo,
                            boolean existsScreen,
                            String sipMessage);
```

This event will be triggered when remote party updates the call.

**Parameters**

| _sessionId_    | The session ID of the call.                                                         |
| -------------- | ----------------------------------------------------------------------------------- |
| _audioCodecs_  | The matched audio codecs. It's separated by "#" if there are more than one codecs.  |
| _videoCodecs_  | The matched video codecs. It's separated by "#" if there are more than one codecs.  |
| _screenCodecs_ | The matched screen codecs. It's separated by "#" if there are more than one codecs. |
| _existsAudio_  | By setting to true, this call includes the audio.                                   |
| _existsVideo_  | By setting to true, this call includes the video.                                   |
| _existsScreen_ | By setting to true, this call includes the screen shared.                           |
| _sipMessage_   | The SIP message received.                                                           |

```java
void onInviteConnected(long sessionId);
```

This event will be triggered when UAC sent/UAS received ACK (the call is connected). Some functions (hold, updateCall etc...) can be called only after the call connected, otherwise the functions will return error.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onInviteBeginingForward(String forwardTo);
```

If the enableCallForward method is called and a call is incoming, the call will be forwarded automatically and this event will be triggered.

**Parameters**

| _forwardTo_ | The target SIP URI of the call forwarding. |
| ----------- | ------------------------------------------ |

```java
void onInviteClosed(long sessionId, String sipMessage);
```

This event is triggered once remote side ends the call.

**Parameters**

| _sessionId_  | The session ID of the call. |
| ------------ | --------------------------- |
| _sipMessage_ | The SIP message received.   |

```java
void onDialogStateUpdated(String BLFMonitoredUri,
                          String BLFDialogState,
                          String BLFDialogId,
                          String BLFDialogDirection);
```

If a user subscribed and his dialog status monitored, when the monitored user is holding a call or is being rang, this event will be triggered.

**Parameters**

| _BLFMonitoredUri_     | the monitored user's URI    |
| --------------------- | --------------------------- |
| _BLFDialogState_      | - the status of the call    |
| _BLFDialogId_         | - the id of the call        |
| _BLFDialogDirecti on_ | - the direction of the call |

```java
void onRemoteHold(long sessionId);
```

If the remote side places the call on hold, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onRemoteUnHold(long sessionId,
                               String audioCodecs,
                               String videoCodecs,
                               boolean existsAudio,
                               boolean existsVideo);
```

If the remote side un-holds the call, this event will be triggered **Parameters**

| _sessionId_   | The session ID of the call.                                                       |
| ------------- | --------------------------------------------------------------------------------- |
| _audioCodecs_ | The matched audio codecs. It's separated by "#" if there are more than one codec. |
| _videoCodecs_ | The matched video codecs. It's separated by "#" if there are more than one codec. |
| _existsAudio_ | By setting to true, this call includes the audio.                                 |
| _existsVideo_ | By setting to true, this call includes the video.                                 |

### Refer events

```java
    public void onReceivedRefer(long sessionId,
                         long referId,
                         String to,
                         String from,
                         String referSipMessage);
```

This event will be triggered once received a REFER message.

**Parameters**

| _sessionId_       | The session ID of the call.                                        |
| ----------------- | ------------------------------------------------------------------ |
| _referId_         | The ID of the REFER message. Pass it to acceptRefer or rejectRefer |
| _to_              | The refer target.                                                  |
| _from_            | The sender of REFER message.                                       |
| _referSipMessage_ | The SIP message of "REFER". Pass it to "acceptRefer" function.     |

```java
void onReferAccepted(long sessionId);
```

This callback will be triggered once remote side calls "acceptRefer" to accept the REFER **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onReferRejected(long sessionId, String reason, int code);
```

This callback will be triggered once remote side calls "rejectRefer" to reject the REFER **Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _reason_    | Reject reason.              |
| _code_      | Reject code.                |

```java
void onTransferTrying(long sessionId);
```

When the refer call is being processed, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onTransferRinging(long sessionId);
```

When the refer call is ringing, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onACTVTransferSuccess(long sessionId);
```

When the refer call succeeds, this event will be triggered. The ACTV means Active. For example, A establishes the call with B, A transfers B to C, C accepts the refer call, and A will receive this event.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```java
void onACTVTransferFailure(long sessionId, String reason, int code);
```

When the refer call fails, this event will be triggered. The ACTV means Active. For example, A establish the call with B, A transfers B to C, C rejects this refer call, and A will receive this event.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _reason_    | The error reason.           |
| _code_      | The error code.             |

### Signaling events

```java
void onReceivedSignaling(long sessionId, String message);
```

This event will be triggered when receiving a SIP message. This event is disabled by default. To enable, use enableCallbackSignaling.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _message_   | The received SIP message.   |

```java
void onSendingSignaling (long sessionId, String  message)
```

This event will be triggered when sent a SIP message. This event is disabled by default. To enable, use enableCallbackSignaling.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _message_   | The sent SIP message.       |

### MWI events

```java
void onWaitingVoiceMessage(String messageAccount,
                           int urgentNewMessageCount,
                           int urgentOldMessageCount,
                           int newMessageCount,
                           int oldMessageCount);
```

If there is the waiting voice message (MWI), this event will be triggered.

**Parameters**

| _messageAccount_         | Voice message account            |
| ------------------------ | -------------------------------- |
| _urgentNewMessag eCount_ | Count of new urgent messages.    |
| _urgentOldMessage Count_ | Count of history urgent message. |
| _newMessageCount_        | Count of new messages.           |
| _oldMessageCount_        | Count of history messages.       |

```java
void onWaitingFaxMessage(String messageAccount,
                         int urgentNewMessageCount,
                         int urgentOldMessageCount,
                         int newMessageCount,
                         int oldMessageCount);
```

If there is waiting fax message (MWI), this event will be triggered.

**Parameters**

| _messageAccount_         | Fax message account               |
| ------------------------ | --------------------------------- |
| _urgentNewMessag eCount_ | Count of new urgent messages.     |
| _urgentOldMessage Count_ | Count of history urgent messages. |
| _newMessageCount_        | Count of new messages.            |
| _oldMessageCount_        | Count of old messages.            |

### DTMF events

```java
void onRecvDtmfTone(long sessionId, int tone);
```

This event will be triggered when receiving a DTMF tone from remote side.

**Parameters**

| _sessionId_ | Session ID of the call. |   |   |
| ----------- | ----------------------- | - | - |
| _tone_      |                         |   |   |
| code        | Description             |   |   |
| 0           | The DTMF tone 0.        |   |   |
| 1           | The DTMF tone 1.        |   |   |
| 2           | The DTMF tone 2.        |   |   |
| 3           | The DTMF tone 3.        |   |   |
| 4           | The DTMF tone 4.        |   |   |
| 5           | The DTMF tone 5.        |   |   |
| 6           | The DTMF tone 6.        |   |   |
| 7           | The DTMF tone 7.        |   |   |
| 8           | The DTMF tone 8.        |   |   |
| 9           | The DTMF tone 9.        |   |   |
| 10          | The DTMF tone \*.       |   |   |
| 11          | The DTMF tone #.        |   |   |
| 12          | The DTMF tone A.        |   |   |
| 13          | The DTMF tone B.        |   |   |
| 14          | The DTMF tone C.        |   |   |
| 15          | The DTMF tone D.        |   |   |
| 16          | The DTMF tone FLASH.    |   |   |

### INFO/OPTIONS message events

```java
void onRecvOptions(String optionsMessage);
```

This event will be triggered when receiving the OPTIONS message.

**Parameters**

| _optionsMessage_ | The received whole OPTIONS message in text format. |
| ---------------- | -------------------------------------------------- |

```java
void onRecvInfo(String infoMessage);
```

This event will be triggered when receiving the INFO message.

**Parameters**

| _infoMessage_ | The whole INFO message received in text format. |
| ------------- | ----------------------------------------------- |

```java
void onRecvNotifyOfSubscription(long subscribeId,String notifyMessage,
                                byte[] messageData,
                                int messageDataLength);
```

This event will be triggered when receiving a NOTIFY message of the subscription.

**Parameters**

| _subscribeId_        | The ID of SUBSCRIBE request.                                       |
| -------------------- | ------------------------------------------------------------------ |
| _notifyMessage_      | The received INFO message in text format.                          |
| _messageData_        | The received message body. It's can be either text or binary data. |
| _messageDataLengt h_ | The length of "messageData".                                       |

### Presence events

```java
void onPresenceRecvSubscribe(long subscribeId,
                             String fromDisplayName,
                             String from,
                             String subject);
```

This event will be triggered when receiving the SUBSCRIBE request from a contact.

**Parameters**

| _subscribeId_     | The ID of SUBSCRIBE request.                 |
| ----------------- | -------------------------------------------- |
| _fromDisplayName_ | The display name of contact.                 |
| _from_            | The contact who sends the SUBSCRIBE request. |
| _subject_         | The subject of the SUBSCRIBE request.        |

```java
void onPresenceOnline(String fromDisplayName,
                      String from,
                      String stateText);
```

When the contact is online or changes presence status, this event will be triggered.

**Parameters**

| _fromDisplayName_ | The display name of contact.                 |
| ----------------- | -------------------------------------------- |
| _from_            | The contact who sends the SUBSCRIBE request. |
| _stateText_       | The presence status text.                    |

```java
void onPresenceOffline(String fromDisplayName,
                       String from);
```

When the contact is offline, this event will be triggered.

**Parameters**

| _fromDisplayName_ | The display name of contact.                |
| ----------------- | ------------------------------------------- |
| _from_            | The contact who sends the SUBSCRIBE request |

```java
void onRecvMessage(long sessionId,
                   String mimeType,
                   String subMimeType,
                   byte[] messageData,
                   int messageDataLength);
```

This event will be triggered when receiving a MESSAGE message in dialog.

**Parameters**

| _sessionId_          | The session ID of the call.                                                                                                                                                                                                                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _mimeType_           | The message mime type.                                                                                                                                                                                                                                                                                                             |
| _subMimeType_        | The message sub mime type.                                                                                                                                                                                                                                                                                                         |
| _messageData_        | The received message body. It can be text or binary data. Use the mimeType and subMimeType to differentiate them. For example, if the mimeType is "text" and subMimeType is "plain", "messageData" is text message body. If the mimeType is "application" and subMimeType is "vnd.3gpp.sms", "messageData" is binary message body. |
| _messageDataLengt h_ | The length of "messageData".                                                                                                                                                                                                                                                                                                       |

```java
void onRecvOutOfDialogMessage(String fromDisplayName,
                              String from,
                              String toDisplayName,
                              String to,
                              String mimeType,
                              String subMimeType,
                              byte[] messageData,
                              int messageDataLength,
                              String sipMessage);
```

This event will be triggered when receiving a MESSAGE message out of dialog. For example pager message.

**Parameters**

| _fromDisplayName_    | The display name of sender.                                                                                                                                                                                                                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _from_               | The message sender.                                                                                                                                                                                                                                                                                                                |
| _toDisplayName_      | The display name of receiver.                                                                                                                                                                                                                                                                                                      |
| _to_                 | The receiver.                                                                                                                                                                                                                                                                                                                      |
| _mimeType_           | The message mime type.                                                                                                                                                                                                                                                                                                             |
| _subMimeType_        | The message sub mime type.                                                                                                                                                                                                                                                                                                         |
| _messageData_        | The received message body. It can be text or binary data. Use the mimeType and subMimeType to differentiate them. For example, if the mimeType is "text" and subMimeType is "plain", "messageData" is text message body. If the mimeType is "application" and subMimeType is "vnd.3gpp.sms", "messageData" is binary message body. |
| _messageDataLengt h_ | The length of "messageData".                                                                                                                                                                                                                                                                                                       |
| _sipMessage_         | The SIP message received.                                                                                                                                                                                                                                                                                                          |

```java
void onSendMessageSuccess(long sessionId, long messageId,String sipMessage);
```

If the message is sent successfully in dialog, this event will be triggered.

**Parameters**

| _sessionId_  | The session ID of the call.                                             |
| ------------ | ----------------------------------------------------------------------- |
| _messageId_  | The message ID. It's equal to the return value of sendMessage function. |
| _sipMessage_ | The SIP message received.                                               |

```java
void onSendMessageFailure(long sessionId,
                          long messageId,
                          String reason,
                          int code,
                          String sipMessage);
```

If the message is failed to be sent out of dialog, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

| _messageId_  | The message ID. It's equal to the return value of sendMessage function. |
| ------------ | ----------------------------------------------------------------------- |
| _reason_     | The failure reason.                                                     |
| _code_       | Failure code.                                                           |
| _sipMessage_ | The SIP message received.                                               |

```java
void onSendOutOfDialogMessageSuccess( long messageId,
                                      String fromDisplayName,
                                      String from,
                                      String toDisplayName,
                                      String to,
                                      String sipMessage);
```

If the message is sent successfully out of dialog, this event will be triggered.

**Parameters**

| _messageId_       | The message ID. It's equal to the return value of SendOutOfDialogMessage function. |
| ----------------- | ---------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of message sender.                                                |
| _from_            | The message sender.                                                                |
| _toDisplayName_   | The display name of message receiver.                                              |
| _to_              | The message receiver.                                                              |
| _sipMessage_      | The SIP message received.                                                          |

```java
void onSendOutOfDialogMessageFailure(long messageId,
                                     String fromDisplayName,
                                     String from,
                                     String toDisplayName,
                                     String to,
                                     String reason,
                                     int code,
                                     String sipMessage);

```

If the message failed to be sent out of dialog, this event would be triggered.

**Parameters**

| _messageId_       | The message ID. It's equal to the return value of SendOutOfDialogMessage function. |
| ----------------- | ---------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of message sender                                                 |
| _from_            | The message sender.                                                                |
| _toDisplayName_   | The display name of message receiver.                                              |
| _to_              | The message receiver.                                                              |
| _reason_          | The failure reason.                                                                |
| _code_            | The failure code.                                                                  |
| _sipMessage_      | The SIP message received.                                                          |

```java
void onSubscriptionFailure(long subscribeId, int statusCode);
```

This event will be triggered on sending SUBSCRIBE failure.

**Parameters**

| _subscribeId_ | The ID of SUBSCRIBE request. |
| ------------- | ---------------------------- |
| _statusCode_  | The status code.             |

```java
void onSubscriptionTerminated(long subscribeId);
```

This event will be triggered when a SUBSCRIPTION is terminated or expired.

**Parameters**

| _subscribeId_ | The ID of SUBSCRIBE request. |
| ------------- | ---------------------------- |

### audio device changed,Play audio and video file finished events

```java
void onPlayFileFinished(long sessionId, String fileName);
```

If called startPlayingFileToRemote function with no loop mode, this event will be triggered once the file play finished.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _fileName_  | The play file name.         |

```java
void onStatistics(long sessionId,String statistics);
```

If called getStatistics function, this event will be triggered once the statistics get finished.

**Parameters**

| _sessionId_  | The session ID of the call.  |
| ------------ | ---------------------------- |
| _statistics_ | The session call statistics. |

```java
void onAudioDeviceChanged(PortSipEnumDefine.AudioDevice audioDevice, Set<PortSipEnumDefine.AudioDevice> devices);
```

fired When available audio devices changed or audio device currently in use changed.

**Parameters**

| _audioDevice_ | device currently in use                                                                                                                                                                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _devices_     | devices useable. If a wired headset is connected, it should be the only possible option. When no wired headset connected, the devices set may contain speaker, earpiece, Bluetooth devices. can be set by PortSipSdk#setAudioDevice to switch to current device. |

```java
void onAudioFocusChange(int focusChange);
```

fired when the audio focus has been changed.

**Parameters**

| _focusChange_ | the type of focus change, one of AudioManager::AUDIOFOCUS\_GAIN, AudioManager::AUDIOFOCUS\_LOSS, AudioManager::AUDIOFOCUS\_LOSS\_TRANSIENT and AudioManager::AUDIOFOCUS\_LOSS\_TRANSIENT\_CAN\_DUCK. |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

### RTP callback events

```java
void onRTPPacketCallback(long sessionId,
                         int mediaType,
                         int enum_direction,
                         byte[] RTPPacket, 
                         int packetSize);
```

If enableRtpCallback function is called to enable the RTP callback, this event will be triggered once a RTP packet is received or sent.

**Parameters**

| _sessionId_       | The session ID of the call.                                        |
| ----------------- | ------------------------------------------------------------------ |
| _mediaType_       | RTP packet media type, 0 for audio, 1 for video , 2 for screen.    |
| _enum\_direction_ | RTP packet direction enum\_DIRECTION\_SEND, enum\_DIRECTION\_RECV. |
| _RTPPacket_       | The received or sent RTP packet.                                   |
| _packetSize_      | The size of the RTP packet in bytes.                               |

**Remarks**

Donot call any SDK API functions in this event directly. If you want to call the API functions or other code which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

```java
void onAudioRawCallback(long sessionId,
                        int enum_direction,
                        byte[] data, 
                        int dataLength,
                        int samplingFreqHz);
```

This event will be triggered once receiving the audio packets if called enableAudioStreamCallback function.

**Parameters**

| _sessionId_       | The session ID of the call.                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------- |
| _enum\_direction_ | The type passed in enableAudioStreamCallback function. Below types allowed: enum\_DIRECTION\_SEND, enum\_DIRECTION\_RECV. |
| _data_            | The memory of audio stream. It's in PCM format.                                                                           |
| _dataLength_      | The data size.                                                                                                            |
| _samplingFreqHz_  | The audio stream sample in HZ. For example, 8000 or 16000.                                                                |

**Remarks**

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

**See also**

PortSipSdk::enableAudioStreamCallback

```java
void onVideoRawCallback(long sessionId,
                        int enum_direction, 
                        int width, 
                        int height,
                        byte[] data, 
                        int dataLength);
```

This event will be triggered once receiving the video packets if enableVideoStreamCallback function is called.

**Parameters**

| _sessionId_       | The session ID of the call.                                                                                                        |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| _enum\_direction_ | The type which is passed in enableVideoStreamCallback function. Below types allowed: enum\_DIRECTION\_SEND, enum\_DIRECTION\_RECV. |
| _width_           | The width of video image.                                                                                                          |
| _height_          | The height of video image.                                                                                                         |
| _data_            | The memory of video stream. It's in YUV420 format, YV12.                                                                           |
| _dataLength_      | The data size.                                                                                                                     |

**See also**

PortSipSdk::enableVideoStreamCallback

## SDK functions\*

### Initialize and register functions

```java
int initialize(int enum_transport, 
						  String localIP, 
						  int localSIPPort, 
						  int enum_LogLevel,
						  String LogPath, 
						  int maxLines, 
						  String agent,
						  int audioDeviceLayer, 
						  int videoDeviceLayer, 
						  String TLSCertificatesRootPath,
						  String TLSCipherList, 
						  boolean verifyTLSCertificate, 
						  String dnsServers)
```

Initialize the SDK.

**Parameters**

| _enum\_transport_                                 | Transport for SIP signaling, which can be set as: ENUM\_TRANSPORT\_UDP, ENUM\_TRANSPORT\_TCP, ENUM\_TRANSPORT\_TLS,                                                                                                                                                                                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _localIP_                                         | <p>The local PC IP address (for example: 192.168.1.108). It will be used for sending and receiving SIP messages and RTP packets.</p><p>` `If the local IP is provided in IPv6 format, the SDK will use IPv6.</p><p>` `If you want the SDK to choose correct network interface (IP) automatically, please use "0.0.0.0" for IPv4, or "::" for IPv6.</p> |
| _localSIPPort_                                    | The listening port for SIP message transmission, for example 5060.                                                                                                                                                                                                                                                                                     |
| _enum\_LogLevel_                                  | Set the application log level. The SDK will generate "PortSIP\_Log\_datatime.log" file if the log is enabled. ENUM\_LOG\_LEVEL\_NONE ENUM\_LOG\_LEVEL\_DEBUG                                                                                                                                                                                           |
| ENUM\_LOG\_LEVEL\_ERROR ENUM\_LOG\_LEVEL\_WARNING |                                                                                                                                                                                                                                                                                                                                                        |
| ENUM\_LOG\_LEVEL\_INFO ENUM\_LOG\_LEVEL\_DEBUG    |                                                                                                                                                                                                                                                                                                                                                        |
| _LogPath_                                         | The path for storing log file. The path (folder) specified MUST be existent.                                                                                                                                                                                                                                                                           |
| _maxLines_                                        | Theoretically, unlimited count of lines are supported depending on the device capability. For SIP client, it is recommended to limit it as ranging 1 - 100.                                                                                                                                                                                            |
| _agent_                                           | The User-Agent header to be inserted in to SIP messages.                                                                                                                                                                                                                                                                                               |
| _audioDeviceLayer_                                | Specifies the audio device layer that should be using: 0 = Use the OS defaulted device.                                                                                                                                                                                                                                                                |

|                            | 1 = Virtual device, usually use this for the device that has no sound device installed.                                                                                                                                       |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _videoDeviceLayer_         | <p>Specifies the video device layer that should be using:</p><p>0 = Use the OS defaulted device.</p><p>1 = Use Virtual device, usually use this for the device that has no camera installed.</p>                              |
| _TLSCertificatesRo otPath_ | Specify the TLS certificate path, from which the SDK will load the certificates automatically. Note: On Windows, this path will be ignored, and SDK will read the certificates from Windows certificates stored area instead. |
| _TLSCipherList_            | Specify the TLS cipher list. This parameter is usually passed as empty so that the SDK will offer all available ciphers.                                                                                                      |
| _verifyTLSCertificate_     | Indicate if SDK will verify the TLS certificate or not. By setting to false, the SDK will not verify the validity of TLS certificate.                                                                                         |
| _dnsServers_               | Additional Nameservers DNS servers. Value null indicates system DNS Server. Multiple servers will be split by ";", e.g "8.8.8.8;8.8.4.4"                                                                                      |

**Returns**

If the function succeeds, it returns value 0. If the function fails, it will return a specific error code

```java
void unInitialize() 
```

Un-initialize the SDK and release resources.

```java
int setInstanceId(String instanceId)
```

Set the instance Id, the outbound instanceId((RFC5626) ) used in contact headers.

**Parameters**

| _instanceId_ | The SIP instance ID. If this function is not called, the SDK will generate an instance ID automatically. The instance ID MUST be unique on the same device (device ID or IMEI ID is recommended). Recommend to call this function to set the ID on Android devices. |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setUser(String userName, 
					   String displayName, 
					   String authName,
					   String password, 
					   String userDomain, 
					   String SIPServer, 
					   int SIPServerPort,
					   String STUNServer, 
					   int STUNServerPort, 
					   String outboundServer,
					   int outboundServerPort)
```

Set user account info.

**Parameters**

| _userName_           | Account (username) of the SIP, usually provided by an IP-Telephony service provider.                                                      |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| _displayName_        | The name displayed. You can set it as your like, such as "James Kend". It's optional.                                                     |
| _authName_           | Authorization user name (usually equal to the username).                                                                                  |
| _password_           | User's password. It's optional.                                                                                                           |
| _userDomain_         | User domain; this parameter is optional, which allows to transfer an empty string if you are not using the domain.                        |
| _SIPServer_          | SIP proxy server IP or domain, for example xx.xxx.xx.x or sip.xxx.com.                                                                    |
| _SIPServerPort_      | Port of the SIP proxy server, for example 5060.                                                                                           |
| _STUNServer_         | Stun server for NAT traversal. It's optional and can be used to transfer empty string to disable STUN.                                    |
| _STUNServerPort_     | STUN server port. It will be ignored if the outboundServer is empty.                                                                      |
| _outboundServer_     | Outbound proxy server, for example sip.domain.com. It's optional and allows to transfer an empty string if not using the outbound server. |
| _outboundServerPort_ | Outbound proxy server port, it will be ignored if the outboundServer is empty.                                                            |

**Returns**

If this function succeeds, it will return value 0. If it fails, it will return a specific error code.

```java
void removeUser() 
```

remove user account info.

```java
int registerServer(int expires, int retryTimes)
```

Register to SIP proxy server (login to server)

**Parameters**

| _expires_    | Time interval for registration refreshment, in seconds. The maximum of supported value is 3600. It will be inserted into SIP REGISTER message headers.                                            |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _retryTimes_ | The maximum of retry attempts if failed to refresh the registration. By setting to <= 0, the attempt will be disabled and onRegisterFailure callback will be triggered when facing retry failure. |

**Returns**

If this function succeeds, it will return value 0. If fails, it will return a specific error code.

If the registration to server succeeds, onRegisterSuccess will be triggered; otherwise onRegisterFailure will be triggered.

```java
int refreshRegistration(int expires) 
```

Refresh the registration manually after successfully registered.

**Parameters**

| _expires_ | Time interval for registration refreshment, in seconds. The maximum of supported value is 3600. It will be inserted into SIP REGISTER message headers. |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Returns**

If this function succeeds, it will return value 0. If fails, it will return a specific error code.

If the registration to server succeeds, onRegisterSuccess will be triggered; otherwise onRegisterFailure will be triggered.

```java
int unRegisterServer(int waitMS)
```

Un-register from the SIP proxy server.

**Parameters**

| _waitMS_ | Wait for the server to reply that the un-registration is successful, waitMS is the longest waiting milliseconds, 0 means not waiting. |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If this function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setDisplayName(String displayName)
```

Set the display name of user.

**Parameters**

| _displayName_ | That will appear in the From/To Header. |
| ------------- | --------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setLicenseKey (String  key)
```

Set the license key. It must be called before setUser function.

**Parameters**

| _key_ | The SDK license key. Please purchase from PortSIP. |
| ----- | -------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Audio and video codecs functions

```java
int addAudioCodec (int  enum_audiocodec)
```

Enable an audio codec, and it will be shown in SDP.

**Parameters**

| _enum\_audiocodec_                                                         |
| -------------------------------------------------------------------------- |
|                                                                            |
| ENUM\_AUDIOCODEC\_ISACSWB, ENUM\_AUDIOCODEC\_OPUS, ENUM\_AUDIOCODEC\_DTMF. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int addVideoCodec (int  enum_videocodec)
```

Enable a video codec, and it will be shown in SDP.

**Parameters**

| _enum\_videocodec_ | Video codec type. Supported types include enum\_VIDEOCODEC\_H264, enum\_VIDEOCODEC\_VP8. enum\_VIDEOCODEC\_VP9. |
| ------------------ | --------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
**boolean isAudioCodecEmpty ()
```

Detect if the audio codecs are enabled.

**Returns**

If no audio codec enabled, it will return value true; otherwise it returns false.

```java
**boolean isVideoCodecEmpty ()
```

Detect if the video codecs are enabled.

**Returns**

If no video codec enabled, it will return value true; otherwise it returns false.

```java
int setAudioCodecPayloadType (int  enum_audiocodec, int  payloadType)
```

Set the RTP payload type for dynamic audio codec.

**Parameters**

| _enum\_audiocodec_ | Audio codec type, which is defined in the PortSIPTypes file. |
| ------------------ | ------------------------------------------------------------ |
| _payloadType_      | The new RTP payload type that you want to set.               |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoCodecPayloadType (int  enum_videocodec, int  payloadType)
```

Set the RTP payload type for dynamic video codec.

**Parameters**

| _enum\_videocodec_ | Video codec type. Supported types include: enum\_VIDEOCODEC\_H264, enum\_VIDEOCODEC\_VP8. enum\_VIDEOCODEC\_VP9. |
| ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| _payloadType_      | The new RTP payload type that you want to set.                                                                   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void clearAudioCodec ()
```

Remove all the enabled audio codecs.

```java
void clearVideoCodec ()
```

Remove all the enabled video codecs.

```java
int setAudioCodecParameter (int  enum_audiocodec, String  sdpParameter)
```

Set the codec parameter for audio codec.

**Parameters**

| _enum\_audiocodec_ | Audio codec type, defined in the PortSIPTypes file. |
| ------------------ | --------------------------------------------------- |
| _sdpParameter_     | The parameter is in string format.                  |
| -                  | -                                                   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**See also**

PortSipEnumDefine

**Remarks**

Example:

setAudioCodecParameter(AUDIOCODEC\_AMR, "mode-set=0; octet-align=1; robust-sorting=0"!\[])

```java
int setVideoCodecParameter (int  enum_videocodec, String  sdpParameter)
```

Set the codec parameter for video codec.

**Parameters**

| _enum\_videocodec_ | Video codec types. Supported types include: enum\_VIDEOCODEC\_H264, enum\_VIDEOCODEC\_VP8. enum\_VIDEOCODEC\_VP9. |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- |
| _sdpParameter_     | The parameter is in string format.                                                                                |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Example:

`setVideoCodecParameter(PortSipEnumDefine.enum_VIDEOCODEC_H264, profile-level-id=420033; packetization-mode=0");`

### Additional settings functions

```java
String getVersion ()
```

Get the version number of the current SDK.

**Returns**

String with version description

```java
int enableRport (boolean  enable)
```

Enable/Disable rport(RFC3581).

**Parameters**

| _enable_ | enable Set to true to enable the SDK to support rport. By default it is enabled. |
| -------- | -------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int enableEarlyMedia (boolean  enable)
```

Enable/disable rport(RFC3581).

**Parameters**

| _enable_ | Set to true to enable the SDK to support rport. By default it is enabled. |
| -------- | ------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code. Enable/Disable Early Media.

**Parameters**

| _enable_ | Set to true to enable the SDK support Early Media. By default the Early Media is disabled. |
| -------- | ------------------------------------------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int enablePriorityIPv6Domain (boolean  enable)
```

Enable/disable which allows specifying the preferred protocol when a domain supports

both IPV4 and IPV6 simultaneously.

**Parameters**

| _enable_ | Set to true to enable priority IPv6 Domain. with the default priority being IPV4. |
| -------- | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setUriUserEncoding (String  character, boolean  enable)
```

Modifies the default URI user character needs to be escaped.

**Parameters**

| _character_ | The character to be modified, set one character at a time.                              |
| ----------- | --------------------------------------------------------------------------------------- |
| _enable_    | Whether escaping is required, true for allowing escaping, false for disabling escaping. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setReliableProvisional (int  mode)
```

Enable/Disable PRACK.

**Parameters**

| _mode_ | <p>Modes work as follows:<br>0 - Never, Disable PRACK,By default the PRACK is disabled.<br>1 - SupportedEssential, Only send reliable provisionals if sending a body and far end supports.<br>2 - Supported, Always send reliable provisionals if far end supports.<br>3 - Required Always send reliable provisionals.</p> |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int enable3GppTags (boolean  enable)
```

Enable/disable the 3Gpp tags, including "ims.icsi.mmtel" and "g.3gpp.smsip".

**Parameters**

| _enable_ | Set to true to enable 3Gpp tags for SDK. |
| -------- | ---------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void enableCallbackSignaling (boolean   enableSending, boolean  enableReceived)
```

Enable/disable the callback of the SIP messages.

**Parameters**

| _enableSending_  | Set as true to enable to callback the sent SIP messages, or false to disable. Once enabled, the "onSendingSignaling" event will be triggered when the SDK sends a SIP message.         |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _enableReceived_ | Set as true to enable to callback the received SIP messages, or false to disable. Once enabled, the "onReceivedSignaling" event will be triggered when the SDK receives a SIP message. |

```java
void setSrtpPolicy (int  enum_srtppolicy)
```

Set the SRTP policy.

**Parameters**

| _enum\_srtppolicy_ | <p>The SRTP policy.allow:<br>enum_SRTPPOLICY_NONE,<br>enum_SRTPPOLICY _FORCE,<br>enum_SRTPPOLICY_PREFER.</p> |
| ------------------ | ------------------------------------------------------------------------------------------------------------ |

```java
int setRtpPortRange (int  minimumRtpPort, int  maximumRtpPort)
```

This function allows to set the RTP port range for audio and video streaming.

**Parameters**

| _minimumRtpPort_ | The minimum RTP port. |
| ---------------- | --------------------- |
| _maximumRtpPort_ | The maximum RTP port. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The port range ((max - min) / maxCallLines) should be greater than 4.

```java
int enableCallForward (boolean  forBusyOnly, String   forwardTo)
```

Enable call forwarding.

**Parameters**

| _forBusyOnly_ | If this parameter is set to true, the SDK will forward incoming calls when the user is currently busy. If set it to false, SDK will forward all incoming calls. |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _forwardTo_   | The target to which the call will be forwarded. It must be in the format of sip[:xxxx@sip.portsip.com.](mailto:xxxx@sip.portsip.com)                            |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int disableCallForward ()
```

Disable the call forwarding. The SDK will not forward any incoming call when this function is called.

**Returns**

If the function succeeds, it will not return value 0. If the function fails, it will return a specific error code.

```java
int enableSessionTimer (int  timerSeconds)
```

This function allows to periodically refresh Session Initiation Protocol (SIP) sessions by

sending repeated INVITE requests.

**Parameters**

| _timerSeconds_ | The value of the refresh interval in seconds. A minimum of 90 seconds required. |
| -------------- | ------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The repeated INVITE requests, or re-INVITEs, are sent during an active call log to allow user agents (UA) or proxies to determine the status of a SIP session. Without this keep-alive mechanism, proxies that remember incoming and outgoing requests (stateful proxies) may continue to retain call state in vain. If a UA fails to send a BYE message at the end of a session, or if the BYE message is lost due to network problems, a stateful proxy will not know that the session has ended. The re-INVITES ensure that active sessions stay active and completed sessions are terminated.

```java
void disableSessionTimer ()
```

Disable the session timer.

```java
void setDoNotDisturb (boolean  state)
```

Enable/disable the "Do not disturb" status.

**Parameters**

| _state_ | If it is set to true, the SDK will reject all incoming calls. |
| ------- | ------------------------------------------------------------- |

```java
void enableAutoCheckMwi (boolean  state)
```

Enable/disable the "Auto Check MWI" status.

**Parameters**

| _state_ | If it is set to true, the SDK will check Mwi automatically. |
| ------- | ----------------------------------------------------------- |

```java
int setRtpKeepAlive (boolean  state, int  keepAlivePayloadType, int  deltaTransmitTimeMS)
```

Enable or disable to send RTP keep-alive packet when the call is ongoing.

**Parameters**

| _state_                 | When it's set to true, it's allowed to send the keep-alive packet during the conversation;               |
| ----------------------- | -------------------------------------------------------------------------------------------------------- |
| _keepAlivePayload Type_ | The payload type of the keep-alive RTP packet. It's usually set to 126.                                  |
| _deltaTransmitTime MS_  | The interval for sending keep-alive RTP packet, in millisecond. Recommended value ranges 15000 - 300000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setKeepAliveTime (int  keepAliveTime)
```

Enable or disable to send SIP keep-alive packet.

**Parameters**

| _keepAliveTime_ | This is the time interval for SIP keep-alive, in seconds. When it is set to 0, the SIP keep-alive will be disabled. Recommended value is 30 or 50. |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setAudioSamples (int  ptime, int  maxptime)
```

Set the audio capture sample, which will be present in the SDP of INVITE and 200 OK message as "ptime and "maxptime" attribute.

**Parameters**

| _ptime_    | It should be a multiple of 10 between 10 - 60 (included 10 and 60).                                                      |
| ---------- | ------------------------------------------------------------------------------------------------------------------------ |
| _maxptime_ | The "maxptime" attribute should be a multiple of 10 between 10 - 60 (included 10 and 60). It can't be less than "ptime". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int addSupportedMimeType (String  methodName, String  mimeType, String  subMimeType)
```

Set the SDK to receive SIP messages that include special mime type.

**Parameters**

| _methodName_  | Method name of the SIP message, such as INVITE, OPTION, INFO, MESSAGE, UPDATE, ACK etc. For more details please refer to RFC3261. |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| _mimeType_    | The mime type of SIP message.                                                                                                     |
| _subMimeType_ | The sub mime type of SIP message.                                                                                                 |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

In default, PortSIP VoIP SDK supports media types (mime types) included in the below incoming SIP messages:

"message/sipfrag" in NOTIFY message.

"application/simple-message-summary" in NOTIFY message.

"text/plain" in MESSAGE message. "application/dtmf-relay" in INFO message.\
"application/media\_control+xml" in INFO message.

The SDK allows to receive SIP messages that include above mime types. Now if remote side send an INFO SIP message with its "Content-Type" header value "text/plain", SDK will reject this INFO message, because "text/plain" of INFO message is not included in the default type list. How should we enable the SDK to receive SIP INFO messages that include "text/plain" mime type? The answer is addSupportedMimyType:

`addSupportedMimeType("INFO", "text", "plain");`

If the user wishes to receive the NOTIFY message with "application/media\_control+xml", it should be set as below:

addSupportedMimeType("NOTIFY", "application", "media\_control+xml");

For more details about the mime type, please visit: [http://www.iana.org/assignments/media-types/](http://www.iana.org/assignments/media-types/)

### Access SIP message header functions

```java
String getSipMessageHeaderValue (String  sipMessage, String  headerName)
```

Access the SIP header of SIP message.

**Parameters**

| _sipMessage_ | The SIP message.                                           |
| ------------ | ---------------------------------------------------------- |
| _headerName_ | The header of which user wishes to access the SIP message. |

**Returns**

String. The SIP header of SIP message.

```java
long addSipMessageHeader(long sessionId, 
									       String methodName, 
									       int msgType, 
									       String headerName,
									       String headerValue)
```

Add the SIP Message header into the specified outgoing SIP message.

**Parameters**

| _sessionId_   | Add the header to the SIP message with the specified session Id only. By setting to -1, it will be added to all messages.                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_  | Add the header to the SIP message with specified method name only. For example: "INVITE", "REGISTER", "INFO" etc. If "ALL" specified, it will add all SIP messages. |
| _msgType_     | 1 refers to apply to the request message, 2 refers to apply to the response message, 3 refers to apply to both request and response.                                |
| _headerName_  | The header name which will appear in SIP message.                                                                                                                   |
| _headerValue_ | The custom header value.                                                                                                                                            |

**Returns**

If the function succeeds, it will return the addedSipMessageId , which is greater than 0. If the function fails, it will return a specific error code.

```java
int removeAddedSipMessageHeader (long  addedSipMessageId)
```

Remove the headers (custom header) added by addSipMessageHeader.

**Parameters**

| _addedSipMessageI d_ | The addedSipMessageId return by addSipMessageHeader. |
| -------------------- | ---------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void clearAddedSipMessageHeaders ()
```

Clear the added extension headers (custom headers)

**Remarks**

For example, we have added two custom headers into every outgoing SIP message and want to have them removed.

```java
addSipMessageHeader(-1,"ALL",3,"Blling", "usd100.00");
addSipMessageHeader(-1,"ALL",3,"ServiceId", "8873456"); 
clearAddedSipMessageHeaders(); 
```

If this function is called, the added extension headers will no longer appear in outgoing SIP message.

```java
long modifySipMessageHeader(long sessionId, 
									          String methodName, 
									          int msgType,
									          String headerName, 
									          String headerValue)
```

Modify the special SIP header value for every outgoing SIP message.

**Parameters**

| _sessionId_   | The header to the SIP message with the specified session Id. By setting to -1, it will be added to all messages.                                                       |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_  | Modify the header to the SIP message with specified method name only. For example: "INVITE", "REGISTER", "INFO" etc. If "ALL" specified, it will add all SIP messages. |
| _msgType_     | 1 refers to apply to the request message, 2 refers to apply to the response message, 3 refers to apply to both request and response.                                   |
| _headerName_  | The SIP header name of which the value will be modified.                                                                                                               |
| _headerValue_ | The heaver value to be modified.                                                                                                                                       |

**Returns**

If the function succeeds, it will return modifiedSipMessageId, which is greater than 0. If the function fails, it will return a specific error code.

**Remarks**

Example: modify "Expires" header and "User-Agent" header value for every outgoing SIP message:

modifySipMessageHeader(-1,"ALL",3, "Expires", "1000"); modifySipMessageHeader(-1,"ALL",3, "User-Agent", "MyTest Softphone 1.0");

```java
int removeModifiedSipMessageHeader (long  modifiedSipMessageId)
```

Remove the headers (custom header) added by modifiedSipMessageId.

**Parameters**

| _modifiedSipMessa geId_ | The modifiedSipMessageId return by modifySipMessageHeader. |
| ----------------------- | ---------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void clearModifiedSipMessageHeaders ()
```

Clear the modify headers value. Once cleared, it will no longer modify every outgoing SIP message header values.

**Remarks** Example: modify two headers value for every outgoing SIP message and then clear it:

```java
  modifySipMessageHeader(-1,"ALL",3, "Expires", "1000"); 
  modifySipMessageHeader(-1,"ALL",3, "User-Agent", "MyTest Softphone 1.0");   
  cleaModifyHeaders();
```

**Parameters**

### Audio and video functions

```java
int setVideoDeviceId (int  deviceId)
```

Set the video device that will be used for video call. **Parameters**

| _deviceId_ | Device ID (index) for video device (camera). |
| ---------- | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoOrientation (int  rotation)
```

Setting the video Device Orientation.

**Parameters**

| _rotation_ | Device Orientation for video device (camera), e.g 0,90,180,270. |
| ---------- | --------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int enableVideoHardwareCodec (boolean  enableHWEncoder, boolean  enableHWDecoder)
```

Set enable/disable video Hardware codec.

| _enableHWEncoder_ | <p>If it is set to true, the SDK will use video hardware encoder when available.</p><p>By default it is true.</p> |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| _enableHWDecoder_ | <p>If it is set to true, the SDK will use video hardware decoder when available.</p><p>By default it is true.</p> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoResolution (int  width, int  height)
```

Set the video capturing resolution.

**Parameters**

| _width_  | Video resolution, width  |
| -------- | ------------------------ |
| _height_ | Video resolution, height |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setAudioBitrate (long  sessionId, int  enum_audiocodec, int  bitrateKbps)
```

Set the audio bitrate. **Parameters**

| _sessionId_        | The session ID of the call.                      |
| ------------------ | ------------------------------------------------ |
| _enum\_audiocodec_ | Audio codec type allowed: enum\_AUDIOCODEC\_OPUS |
| _bitrateKbps_      | The Audio bitrate in KBPS.                       |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoBitrate (long  sessionId, int  bitrateKbps)
```

Set the video bitrate.

**Parameters**

| _sessionId_   | The session ID of the call. |
| ------------- | --------------------------- |
| _bitrateKbps_ | The video bitrate in KBPS.  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoFrameRate (long  sessionId, int  frameRate)
```

Set the video frame rate. Usually you do not need to call this function to set the frame rate since the SDK uses default frame rate.

**Parameters**

| _sessionId_ | The session ID of the call.                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _frameRate_ | The frame rate value, with its minimum of 5, and maximum value of 30. The greater the value is, the better video quality enabled and more bandwidth required; |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int sendVideo (long  sessionId, boolean  send)
```

Send the video to remote side.

**Parameters**

| _sessionId_ | The session ID of the call.                              |
| ----------- | -------------------------------------------------------- |
| _send_      | Set to true to send the video, or false to stop sending. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setRemoteVideoWindow (long  sessionId, PortSIPVideoRenderer  renderer)
```

Set the window for a session that is used for displaying the received remote video image.

**Parameters**

| _sessionId_ | The session ID of the call.                                               |
| ----------- | ------------------------------------------------------------------------- |
| _renderer_  | SurfaceView a SurfaceView for displaying the received remote video image. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setRemoteScreenWindow (long  sessionId, PortSIPVideoRenderer  renderer)
```

Set the window for a session that is used for displaying the received remote screen image.

**Parameters**

| _sessionId_ | The session ID of the call.                                                |
| ----------- | -------------------------------------------------------------------------- |
| _renderer_  | SurfaceView a SurfaceView for displaying the received remote screen image. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void displayLocalVideo (boolean   state, boolean  mirror, PortSIPVideoRenderer  renderer)
```

Start/stop displaying the local video image.

**Parameters**

| _state_    | Set to true to display local video image.                               |
| ---------- | ----------------------------------------------------------------------- |
| _mirror_   | Set to true to display the mirror image of local video.                 |
| _renderer_ | SurfaceView a SurfaceView for displaying local video image from camera. |

```java
int setVideoNackStatus (boolean  state)
```

Enable/disable the NACK feature (rfc6642) which helps to improve the video quality.

**Parameters**

| _state_ | Set to true to enable. |
| ------- | ---------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setChannelOutputVolumeScaling (long  sessionId, int  scaling)
```

Set a volume |scaling| to be applied to the outgoing signal of a specific audio channel.

47 **Parameters**

| _sessionId_ | The session ID of the call.                    |
| ----------- | ---------------------------------------------- |
| _scaling_   | Valid scale ranges \[0, 1000]. Default is 100. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setChannelInputVolumeScaling (long  sessionId, int  scaling)
```

Set a volume |scaling| to be applied to the microphone signal of a specific audio channel.

**Parameters**

| _sessionId_ | The session ID of the call.                    |
| ----------- | ---------------------------------------------- |
| _scaling_   | Valid scale ranges \[0, 1000]. Default is 100. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void enableAudioManager (boolean   state)
```

enable/disable sdk audio manager,when enable sdk will auto manager audio device input/output. if the state is enabled, the onAudioDeviceChanged event will be triggered when available audio devices changed or audio device currently in use changed .

**Parameters**

| _state_ | @true enable sdk audio manager @false disable audio manager |
| ------- | ----------------------------------------------------------- |

```java
Set<PortSipEnumDefine.AudioDevice> getAudioDevices ()
```

Get current set of available/selectable audio devices.

**Returns**

Current set of available/selectable audio devices.

```java
int setAudioDevice (PortSipEnumDefine.AudioDevice  defaultDevice)
```

Set the audio device that will used for audio call. For Android and iOS, switch between earphone and Loudspeaker allowed.

**Parameters**

| _defaultDevice_ | Set to true the SDK use loudspeaker for audio call, this just available for mobile platform only. |
| --------------- | ------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Call functions

```java
long call (String  callee, boolean  sendSdp, boolean  videoCall)
```

Make a call

**Parameters**

| _callee_    | The callee. It can be a name only or full SIP URI, for example: user001 or sip[:user001@sip.iptel.org ](mailto:user001@sip.iptel.org)or sip[:user002@sip.yourdomain.com:](mailto:user002@sip.yourdomain.com)5068 |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _sendSdp_   | If it is set to false, the outgoing call will not include the SDP in INVITE message.                                                                                                                             |
| _videoCall_ | If it is set to true and at least one video codec was added, the outgoing call will include the video codec into SDP. Otherwise no video codec will be added into outgoing SDP.                                  |

**Returns**

If the function succeeds, it will return the session ID of the call, which is greater than 0. If the function fails, it will return a specific error code.

Note: the function success just means the outgoing call is processing, you need to detect the call final state in onInviteTrying, onInviteRinging, onInviteFailure callback events.

```java
int rejectCall (long  sessionId, int  code)
```

rejectCall Reject the incoming call.

**Parameters**

| _sessionId_ | The session ID of the call.             |
| ----------- | --------------------------------------- |
| _code_      | Reject code, for example, 486, 480 etc. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int hangUp (long  sessionId)
```

hangUp Hang up the call.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int answerCall (long  sessionId, boolean  videoCall)
```

answerCall Answer the incoming call.

**Parameters**

| _sessionId_ | The session ID of call.                                                                                                                                                                              |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _videoCall_ | <p>If the incoming call is a video call and the video codec is matched, set to true to answer the video call.</p><p>If set to false, the answer call does not include video codec answer anyway.</p> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int updateCall (long  sessionId, boolean  enableAudio, boolean  enableVideo)
```

updateCall Use the re-INVITE to update the established call.

**Parameters**

| _sessionId_   | The session ID of call.                                                                    |
| ------------- | ------------------------------------------------------------------------------------------ |
| _enableAudio_ | Set to true to allow the audio in updated call, or false to disable audio in updated call. |
| _enableVideo_ | Set to true to allow the video in update call, or false to disable video in updated call.  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return specific error code.

**Remarks**

Example usage:

Example 1: A called B with the audio only, and B answered A, there would be an audio conversation between A and B. Now A want to see B through video, A could use these functions to fulfill it.

clearVideoCodec();\
addVideoCodec(VIDEOCODEC\_H264); updateCall(sessionId, true, true);

Example 2: Remove video stream from the current conversation.\
updateCall(sessionId, true, false);

```java
int hold (long  sessionId)
```

To place a call on hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int unHold (long  sessionId)
```

Take off hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int muteSession(long sessionId, 
						    boolean muteIncomingAudio,
						    boolean muteOutgoingAudio, 
						    boolean muteIncomingVideo,
						    boolean muteOutgoingVideo) 
```

Mute the specified audio or video session.

**Parameters**

| _sessionId_          | The session ID of the call.                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| _muteIncomingAudi o_ | Set it to true to mute incoming audio stream. Once set, remote side audio cannot be heard.     |
| _muteOutgoingAudi o_ | Set it to true to mute outgoing audio stream. Once set, the remote side cannot hear the audio. |
| _muteIncomingVide o_ | Set it to true to mute incoming video stream. Once set, remote side video cannot be seen.      |
| _muteOutgoingVide o_ | Set it to true to mute outgoing video stream, the remote side cannot see the video.            |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int forwardCall (long  sessionId, String   forwardTo)
```

Forward call to another one when receiving the incoming call.

**Parameters**

| _sessionId_ | The session ID of the call.                                                     |
| ----------- | ------------------------------------------------------------------------------- |
| _forwardTo_ | Target of the forward. It can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

```java
long pickupBLFCall (String  replaceDialogId, boolean  videoCall)
```

This function will be used for picking up a call based on the BLF (Busy Lamp Field) status.

**Parameters**

| _replaceDialogId_ | The ID of the call which will be pickup. It comes with onDialogStateUpdated callback. |
| ----------------- | ------------------------------------------------------------------------------------- |
| _videoCall_       | Indicates pickup video call or audio call                                             |

If the function succeeds, it will return a session ID that is greater than 0 to the new call, otherwise returns a specific error code that is less than 0.

**Remarks**

The scenario is:

1. User 101 subscribed the user 100's call status: sendSubscription(mSipLib, "100", "dialog");
2. When 100 holds a call or 100 is ringing, onDialogStateUpdated callback will be triggered, and 101 will receive this callback. Now 101 can use pickupBLFCall function to pick the call rather than 100 to talk with caller.

```java
int sendDtmf (long  sessionId, 
              int  enum_dtmfMethod, 
              int  code, 
              int  dtmfDuration, 
              boolean  playDtmfTone)
```

Send DTMF tone.

**Parameters**

| _sessionId_        | The session ID of the call.                                                                           |   |   |
| ------------------ | ----------------------------------------------------------------------------------------------------- | - | - |
| _enum\_dtmfMethod_ | DTMF tone could be sent via two methods: DTMF\_RFC2833 or DTMF\_INFO. The DTMF\_RFC2833 is recommend. |   |   |
| _code_             | The DTMF tone. Values include:                                                                        |   |   |
| code               | Description                                                                                           |   |   |
| 0                  | The DTMF tone 0.                                                                                      |   |   |
| 1                  | The DTMF tone 1.                                                                                      |   |   |
| 2                  | The DTMF tone 2.                                                                                      |   |   |
| 3                  | The DTMF tone 3.                                                                                      |   |   |
| 4                  | The DTMF tone 4.                                                                                      |   |   |
| 5                  | The DTMF tone 5.                                                                                      |   |   |
| 6                  | The DTMF tone 6.                                                                                      |   |   |
| 7                  | The DTMF tone 7.                                                                                      |   |   |
| 8                  | The DTMF tone 8.                                                                                      |   |   |
| 9                  | The DTMF tone 9.                                                                                      |   |   |
| 10                 | The DTMF tone \*.                                                                                     |   |   |
| 11                 | The DTMF tone #.                                                                                      |   |   |
| 12                 | The DTMF tone A.                                                                                      |   |   |
| 13                 | The DTMF tone B.                                                                                      |   |   |
| 14                 | The DTMF tone C.                                                                                      |   |   |
| 15                 | The DTMF tone D.                                                                                      |   |   |
| 16                 | The DTMF tone FLASH.                                                                                  |   |   |

**Parameters**

| _dtmfDuration_ | The DTMF tone samples. Recommended value 160.                    |
| -------------- | ---------------------------------------------------------------- |
| _playDtmfTone_ | Set to true the SDK play local DTMF tone sound during send DTMF. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Refer functions

```java
int refer (long  sessionId, String  referTo)
```

Transfer the current call to another callee.

**Parameters**

| _sessionId_ | The session ID of the call.                                                             |
| ----------- | --------------------------------------------------------------------------------------- |
| _referTo_   | Target callee of the transfer. It can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

refer(sessionId, "sip:testuser12@sip.portsip.com");

You can refer to the video on Youtube at:

[https://www.youtube.com/watch?v=\_2w9EGgr3FY,](https://www.youtube.com/watch?v=\_2w9EGgr3FY) which will demonstrate how to complete the transfer.

```java
int attendedRefer (long  sessionId, long  replaceSessionId, String  referTo)
```

Make an attended refer. **Parameters**

| _sessionId_        | The session ID of the call.                                                          |
| ------------------ | ------------------------------------------------------------------------------------ |
| _replaceSessionId_ | Session ID of the replace call.                                                      |
| _referTo_          | Target callee of the refer. It can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Please read the sample project source code to get more details, or you can refer to the video on YouTube at:

[https://www.youtube.com/watch?v=\_2w9EGgr3FY](https://www.youtube.com/watch?v=\_2w9EGgr3FY)

Note: Please use Windows Media Player to play the AVI file, which demonstrates how to complete the transfer.

```java
int attendedRefer2(long sessionId, 
							     long replaceSessionId, 
							     String replaceMethod,
							     String target,
							     String referTo)
```

Make an attended refer.

**Parameters**

| _sessionId_        | The session ID of the call.                                                      |
| ------------------ | -------------------------------------------------------------------------------- |
| _replaceSessionId_ | The session ID of the session to be replaced.                                    |
| _replaceMethod_    | The SIP method name to be added in the "Refer-To" header, usually INVITE or BYE. |
| _target_           | The target to which the REFER message will be sent.                              |
| _referTo_          | The URI to be added into the "Refer-To" header.                                  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int outOfDialogRefer (long  replaceSessionId, String  replaceMethod, String  target, String  referTo)
```

Make an attended refer.

**Parameters**

| _replaceSessionId_ | The session ID of the session which will be replaced.                                    |
| ------------------ | ---------------------------------------------------------------------------------------- |
| _replaceMethod_    | The SIP method name which will be added in the "Refer-To" header, usually INVITE or BYE. |
| _target_           | The target to which the REFER message will be sent.                                      |
| _referTo_          | The URI which will be added into the "Refer-To" header.                                  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
long acceptRefer (long  referId, String  referSignaling)
```

By accepting the REFER request, a new call will be made if this function is called. The

function is usually called after onReceivedRefer callback event.

**Parameters**

| _referId_        | The ID of REFER request that comes from onReceivedRefer callback event.          |
| ---------------- | -------------------------------------------------------------------------------- |
| _referSignaling_ | The SIP message of REFER request that comes from onReceivedRefer callback event. |

**Returns**

If the function succeeds, it will return a session ID greater than 0 to the new call for REFER; otherwise it will return a specific error code less than 0;

```java
int rejectRefer (long  referId)
```

Reject the REFER request.

**Parameters**

| _referId_ | The ID of REFER request that comes from onReceivedRefer callback event. |
| --------- | ----------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Send audio and video stream functions

```java
int enableSendPcmStreamToRemote (long  sessionId, boolean  state, int  streamSamplesPerSec)
```

Enable the SDK send PCM stream data to remote side from another source instead of microphone. This function MUST be called first to send the PCM stream data to another

side.

**Parameters**

| _sessionId_            | The session ID of call.                                            |
| ---------------------- | ------------------------------------------------------------------ |
| _state_                | Set to true to enable the send stream, or false to disable.        |
| _streamSamplesPer Sec_ | The PCM stream data sample, in seconds. For example 8000 or 16000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int sendPcmStreamToRemote (long  sessionId, byte[]  data, int  dataLength)
```

Send the audio stream in PCM format from another source instead of audio device

capturing (microphone).

**Parameters**

| _sessionId_  | Session ID of the call conversation.               |
| ------------ | -------------------------------------------------- |
| _data_       | The PCM audio stream data. It must be 16bit, mono. |
| _dataLength_ | The size of data.                                  |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Usually we should use it like below:

```java
enableSendPcmStreamToRemote(sessionId, true, 16000); 
sendPcmStreamToRemote(sessionId, data, dataSize); 
```

You can't have too much audio data at one time as we have 100ms audio buffer only. Once you put too much, data will be lost. It is recommended to send 20ms audio data every 20ms.

```java
int enableSendVideoStreamToRemote (long  sessionId, boolean  state)
```

Enable the SDK to send video stream data to remote side from another source instead of camera.

This function MUST be called first to send the video stream data to another side.

**Parameters**

| _sessionId_ | The session ID of call.                                     |
| ----------- | ----------------------------------------------------------- |
| _state_     | Set to true to enable the send stream, or false to disable. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int sendVideoStreamToRemote (long  sessionId, byte[]  data, int  dataLength, int  width, int  height)
```

Send the video stream in i420 from another source instead of video device capturing (camera).

Before calling this function, you MUST call the enableSendVideoStreamToRemote function.

**Parameters**

| _sessionId_  | Session ID of the call conversation.              |
| ------------ | ------------------------------------------------- |
| _data_       | The video stream data. It must be in i420 format. |
| _dataLength_ | The size of data.                                 |
| _width_      | The width of the video image.                     |
| _height_     | The height of video image.                        |

**Returns**

If the function succeeds, it will return value is 0. If the function fails, it will return a specific error code.

### RTP packets, Audio stream and video stream callback

```java
long enableRtpCallback (long  sessionId, int  mediaType, int  directionMode)
```

Set the RTP callbacks to allow access to the sent and received RTP packets.

**Parameters**

| _sessionId_     | The session ID of the call.                                     |
| --------------- | --------------------------------------------------------------- |
| _mediaType_     | RTP packet media type, 0 for audio, 1 for video , 2 for screen. |
| _directionMode_ | RTP packet direction, 0 for sending, 1 for receiving.           |

**Returns**

If the function succeeds, it will return value is 0. If the function fails, it will return a specific error code.

```java
void enableAudioStreamCallback (long  sessionId, boolean  enable, int  enum_direction)
```

Enable/disable the audio stream callback. The onAudioRawCallback event will be triggered if the callback is enabled.

**Parameters**

| _sessionId_                                        | The session ID of call.                                                                               |
| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| _enable_                                           | Set to true to enable audio stream callback, or false to stop the callback.                           |
| _enum\_direction_                                  | The audio stream callback mode. Supported modes include ENUM\_DIRECTION\_NONE, ENUM\_DIRECTION\_SEND, |
| ENUM\_DIRECTION\_RECV ENUM\_DIRECTION\_SEND\_RECV. |                                                                                                       |

```java
void enableVideoStreamCallback (long  sessionId, int  enum_direction)
```

Enable/disable the video stream callback, the onVideoRawCallback event will be triggered if the callback is enabled.

**Parameters**

| _sessionId_                                         | The session ID of call.                                                                               |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| _enum\_direction_                                   | The video stream callback mode. Supported modes include ENUM\_DIRECTION\_NONE, ENUM\_DIRECTION\_SEND, |
| ENUM\_DIRECTION\_RECV, ENUM\_DIRECTION\_SEND\_RECV. |                                                                                                       |

**Record functions**

```java
int startRecord(long sessionId, 
						    String recordFilePath,
						    String recordFileName, 
						    boolean appendTimeStamp,
						    int audioChannels,
						    int enum_fileFormat,
						    int enum_audioRecordMode,
						    int enum_videoRecordMode)
```

Start recording the call.

**Parameters**

| _sessionId_              | The session ID of call conversation.                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _recordFilePath_         | The file path to save record file. It must be existent.                                                                                                                                                                                                                                                                                                                                                                                          |
| _recordFileName_         | The file name of record file. For example audiorecord.wav or videorecord.avi.                                                                                                                                                                                                                                                                                                                                                                    |
| _appendTimeStamp_        | Set to true to append the timestamp to the name of the recording file.                                                                                                                                                                                                                                                                                                                                                                           |
| _audioChannels_          | Set to record file audio channels, 1 - mono 2 - stereo.                                                                                                                                                                                                                                                                                                                                                                                          |
| _enum\_fileFormat_       | <p>The record file format, allow below values:</p><p>enum_FILE_FORMAT_WAVE = 1, ///&#x3C; The record audio file is WAVE format.</p><p>enum_FILE_FORMAT_AMR =2, ///&#x3C; The record audio file is in AMR format with all voice data compressed by AMR codec.</p><p>enum_FILE_FORMAT_MP3 = 3;///&#x3C; The record audio file is in MP3 format.</p><p>enum_FILE_FORMAT_MP4 = 4;///&#x3C; The record video file is in MP4(AAC and H264) format.</p> |
| _enum\_audioRecor dMode_ | <p>The audio record mode, allow below values:</p><p>enum_RECORD_MODE_NONE = 0, ///&#x3C; Not Record.</p><p>enum_RECORD_MODE_RECV = 1, ///&#x3C; Only record the received data.</p><p>enum_RECORD_MODE_SEND, ///&#x3C; Only record send data.</p><p>enum_RECORD_MODE_BOTH ///&#x3C; Record both received and sent data.</p>                                                                                                                       |
| _enum\_videoRecord Mode_ | Allow to set video record mode. Support to record received and/or sent video.                                                                                                                                                                                                                                                                                                                                                                    |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int stopRecord (long  sessionId)
```

Stop recording.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Play audio and video file and RTMP/RTSP stream functions

```java
int startPlayingFileToRemote (long  sessionId, String  fileUrl, boolean  loop, int  playAudio)
```

Play an local file or RTSP/RTMP stream to remote party.

**Parameters**

| _sessionId_ | Session ID of the call.                                                                  |
| ----------- | ---------------------------------------------------------------------------------------- |
| _fileUrl_   | The url or filepath, such as "/mnt/sdcard/test.avi".                                     |
| _loop_      | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _playAudio_ | 0 - Not play file audio. 1 - Play file audio. 2 - Play file audio mix with Mic.          |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int stopPlayingFileToRemote (long  sessionId)
```

Stop play file to remote side.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int startPlayingFileLocally (String  fileUrl, boolean  loop, PortSIPVideoRenderer  renderer)
```

Play an local file or RTSP/RTMP stream.

**Parameters**

| _fileUrl_  | The url or filepath, such as "/mnt/sdcard/test.avi".                                     |
| ---------- | ---------------------------------------------------------------------------------------- |
| _loop_     | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _renderer_ | SurfaceView a SurfaceView for displaying the play image.                                 |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int stopPlayingFileLocally ()
```

Stop play file locally.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void audioPlayLoopbackTest (boolean  enable)
```

Used for testing loopback for the audio device.

**Parameters**

| _enable_ | Set to true to start testing audio loopback test; or set to false to stop. |
| -------- | -------------------------------------------------------------------------- |

### Conference functions

```java
int createAudioConference ()
```

Create an audio conference. It will fail if the existing conference is not ended yet. **Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int createVideoConference(PortSIPVideoRenderer conferenceVideoWindow,
									 int videoWidth, 
									 int videoHeight, 
									 int layout)
```

Create a video conference. It will fail if the existing conference is not ended yet.

**Parameters**

| _conferenceVideoW indow_ | SurfaceView The window used for displaying the conference video.                                                                                                                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _videoWidth_             | Width of conference video resolution                                                                                                                                                                                                       |
| _videoHeight_            | Height of conference video resolution                                                                                                                                                                                                      |
| _layout_                 | <p>Conference Video layout, default is 0 - Adaptive.<br>0 - Adaptive(1,3,5,6)<br>1 - Only Local Video<br>2 - 2 video,PIP<br>3 - 2 video, Left and right<br>4 - 2 video, Up and Down<br>5 - 3 video<br>6 - 4 split video<br>7 - 5 video</p> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
void destroyConference ()
```

End the exist conference.

```java
int setConferenceVideoWindow (PortSIPVideoRenderer  conferenceVideoWindow)
```

Set the window for a conference that is used for displaying the received remote video

image.

**Parameters**

| _conferenceVideoWindow_ | SurfaceView The window which is used for displaying the conference vide |
| ----------------------- | ----------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int joinToConference (long  sessionId)
```

Join a session into existing conference. If the call is in hold, it will be un-hold

automatically.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int removeFromConference (long  sessionId)
```

Remove a session from an existing conference.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### RTP and RTCP QOS functions

```java
int setAudioRtcpBandwidth (long  sessionId, int  BitsRR, int  BitsRS, int  KBitsAS)
```

Set the audio RTCP bandwidth parameters as RFC3556.

**Parameters**

| _sessionId_ | Set the audio RTCP bandwidth parameters as RFC3556. |
| ----------- | --------------------------------------------------- |
| _BitsRR_    | The bits for the RR parameter.                      |
| _BitsRS_    | The bits for the RS parameter.                      |
| _KBitsAS_   | The Kbits for the AS parameter.                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoRtcpBandwidth (long  sessionId, int  BitsRR, int  BitsRS, int  KBitsAS)
```

Set the video RTCP bandwidth parameters as the RFC3556.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |
| _BitsRR_    | The bits for the RR parameter.       |
| _BitsRS_    | The bits for the RS parameter.       |
| _KBitsAS_   | The Kbits for the AS parameter.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int enableAudioQos (boolean  state)
```

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for audio channel.

**Parameters**

| _state_ | Set to YES to enable audio QoS and DSCP value will be 46; or NO to disable audio QoS and DSCP value will be 0. |
| ------- | -------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int enableVideoQos (boolean  state)
```

Set the DSCP(differentiated services code point) value of QoS(Quality of Service) for video channel.

**Parameters**

| _state_ | Set to YES to enable video QoS and DSCP value will be 34; or NO to disable video QoS and DSCP value will be 0. |
| ------- | -------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setVideoMTU (int  mtu)
```

Set the MTU size for video RTP packet.

**Parameters**

| _mtu_ | Set MTU value. Allow values range 512 - 65507. Default is 14000. |
| ----- | ---------------------------------------------------------------- |

**Returns**

If the function succeeds, the return value is 0. If the function fails, the return value is a specific error code.

## RTP statistics functions

```java
int getStatistics (long  sessionId)
```

Obtain the statistics of channel. the event onStatistics will be triggered.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

**Returns**

If the function succeeds, it will return value is 0. If the function fails, it will return a specific error code.

### Audio effect functions

```java
void enableVAD (boolean  state)
```

Enable/disable Voice Activity Detection(VAD).

**Parameters**

| _state_ | Set to true to enable VAD, or false to disable. |
| ------- | ----------------------------------------------- |

```java
void enableAEC (boolean  state)
```

Enable/disable AEC (Acoustic Echo Cancellation).

**Parameters**

| _state_ | Set to true to enable AEC, or false to disable. |
| ------- | ----------------------------------------------- |

```java
void enableCNG (boolean  state)
```

Enable/disable Comfort Noise Generator(CNG).

**Parameters**

| _state_ | Set to true to enable CNG, or false to disable. |
| ------- | ----------------------------------------------- |

```java
void enableAGC (boolean  state)
```

Enable/disable Automatic Gain Control(AGC).

**Parameters**

| _state_ | Set to true to enable AEC, or false to disable. |
| ------- | ----------------------------------------------- |

```java
void enableANS (boolean  state)
```

Enable/disable Audio Noise Suppression(ANS).

**Parameters**

| _state_ | Set to true to enable ANS, or false to disable. |
| ------- | ----------------------------------------------- |

### Send OPTIONS/INFO/MESSAGE functions

```java
int sendOptions (String  to, String  sdp)
```

Send OPTIONS message.

**Parameters**

| _to_  | The recipient of OPTIONS message.                                                                     |
| ----- | ----------------------------------------------------------------------------------------------------- |
| _sdp_ | The SDP of OPTIONS message. It's optional if user does not want to send the SDP with OPTIONS message. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

```java
int sendInfo (long  sessionId, String  mimeType, String  subMimeType, String  infoContents)
```

Send a INFO message to remote side in dialog.

**Parameters**

| _sessionId_    | The session ID of call.                           |
| -------------- | ------------------------------------------------- |
| _mimeType_     | The mime type of INFO message.                    |
| _subMimeType_  | The sub mime type of INFO message.                |
| _infoContents_ | The contents that will be sent with INFO message. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
long sendMessage(long sessionId, 
							   String mimeType,
							   String subMimeType, 
							   byte[] message, 
							   int messageLength)
```

Send a MESSAGE message to remote side in dialog.

**Parameters**

| _sessionId_     | The session ID of call.                                                   |
| --------------- | ------------------------------------------------------------------------- |
| _mimeType_      | The mime type of MESSAGE message.                                         |
| _subMimeType_   | The sub mime type of MESSAGE message.                                     |
| _message_       | The contents that will be sent with MESSAGE message. Binary data allowed. |
| _messageLength_ | The message size.                                                         |

**Returns**

If the function succeeds, it will return a message ID that allows to track the message sending state in onSendMessageSuccess and onSendMessageFailure. If the function fails, it will return a specific error code that is less than 0.

**Remarks**

Example 1: Send a plain text message. Note: to send other languages text, please use the UTF8 to encode the message before sending.

sendMessage(sessionId, "text", "plain", "hello",6);

Example 2: Send a binary message.

sendMessage(sessionId, "application", "vnd.3gpp.sms", binData, binDataSize);

```java
long sendOutOfDialogMessage (String  to, String  mimeType, String  subMimeType, boolean  isSMS, byte[]  message, int  messageLength)
```

Send a out of dialog MESSAGE message to remote side.

**Parameters**

| _to_            | The message receiver. Likes sip[:receiver@portsip.com](mailto:receiver@portsip.com) |
| --------------- | ----------------------------------------------------------------------------------- |
| _mimeType_      | The mime type of MESSAGE message.                                                   |
| _subMimeType_   | The sub mime type of MESSAGE message.                                               |
| _isSMS_         | Set to YES to specify "messagetype=SMS" in the To line, or NO to disable.           |
| _message_       | The contents that will be sent with MESSAGE message. Binary data allowed.           |
| _messageLength_ | The message size.                                                                   |

**Returns**

If the function succeeds, it will return a message ID that allows to track the message sending state in onSendOutOfMessageSuccess and onSendOutOfMessageFailure. If the function fails, it will return a specific error code that is less than 0.

**Remarks**

Example 1: Send a plain text message. Note: to send other languages text, please use the UTF8 to encode the message before sending.

sendOutOfDialogMessage("sip:user1@sip.portsip.com", "text", "plain", "hello", 6);

Example 2: Send a binary message.

sendOutOfDialogMessage("sip:user1@sip.portsip.com","application", "vnd.3gpp.sms", binData, binDataSize);

```java
long setPresenceMode (int  mode)
```

Indicate the SDK uses the P2P mode for presence or presence agent mode.

**Parameters**

| _mode_ | 0 - P2P mode; 1 - Presence Agent mode. Default is P2P mode. |
| ------ | ----------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Since presence agent mode requires the PBX/Server support the PUBLISH, please ensure you have your server and PortSIP PBX support this feature. For more details please visit: [https://www.portsip.com/portsip-pbx](https://www.portsip.com/portsip-pbx)

```java
long setDefaultSubscriptionTime (int  secs)
```

Set the default expiration time to be used when creating a subscription.

**Parameters**

| _secs_ | The default expiration time of subscription. |
| ------ | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
long setDefaultPublicationTime (int  secs)
```

Set the default expiration time to be used when creating a publication.

**Parameters**

| _secs_ | The default expiration time of publication. |
| ------ | ------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
long presenceSubscribe (String  contact, String  subject)
```

Send a SUBSCRIBE message for presence to a contact.

**Parameters**

| _contact_ | The target contact, it must be in the format of sip[:contact001@sip.portsip.com.](mailto:contact001@sip.portsip.com)                                                                |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _subject_ | <p>This subject text will be inserted into the SUBSCRIBE message. For example: "Hello, I'm Jason".</p><p>The subject maybe is in UTF8 format. You should use UTF8 to decode it.</p> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int presenceTerminateSubscribe (long   subscribeId)
```

Terminate the given presence subscription.

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int presenceAcceptSubscribe (long   subscribeId)
```

Accept the presence SUBSCRIBE request which received from contact.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event includes the subscription ID. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int presenceRejectSubscribe (long   subscribeId)
```

Reject a presence SUBSCRIBE request received from contact.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event inclues the subscription ID. |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
int setPresenceStatus (long   subscribeId, String  statusText)
```

Send a NOTIFY message to contact to notify that presence status is online/offline/changed.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe that includes the Subscription ID will be triggered. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _statusText_  | The state text of presence status. For example: "I'm here", offline must use "offline"                                                                   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```java
long sendSubscription (String  to, String  eventName)
```

Send a SUBSCRIBE message to subscribe an event.

**Parameters**

| _to_        | The user/extension will be subscribed. |
| ----------- | -------------------------------------- |
| _eventName_ | The event name to be subscribed.       |

**Returns**

If the function succeeds, it will return the ID of that SUBSCRIBE which is greater than 0. If the function fails, it will return a specific error code which is less than 0.

**Remarks**

Example 1, below code indicates that user/extension 101 is subscribed to MWI (Message Waiting notifications) for checking his voicemail: int32 mwiSubId = sendSubscription("sip:101@test.com", "message-summary");

Example 2, to monitor a user/extension call status, You can use code: sendSubscription(mSipLib, "100", "dialog"); Extension 100 refers to the user/extension to be monitored. Once being monitored, when extension 100 hold a call or is ringing, the onDialogStateUpdated callback will be triggered.

```java
int terminateSubscription (long   subscribeId)
```

Terminate the given subscription.

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

For example, if you want stop check the MWI, use below code:

terminateSubscription(mwiSubId);

## Class Documentation

### com.portsip.PortSipEnumDefine

**com.portsip.PortSipEnumDefine.AudioDevice Enum Reference**

**Public Attributes**

* **SPEAKER\_PHONE**
* **WIRED\_HEADSET**
* **EARPIECE**
* **BLUETOOTH**
* **NONE**

**Detailed Description**

AudioDevice list possible audio devices that we currently support.

**Static Public Attributes**

* static final int **enum\_AUDIOCODEC\_G729** = 18
* static final int **enum\_AUDIOCODEC\_PCMA** = 8
* static final int **enum\_AUDIOCODEC\_PCMU** = 0
* static final int **enum\_AUDIOCODEC\_GSM** = 3
* static final int **enum\_AUDIOCODEC\_G722** = 9
* static final int **enum\_AUDIOCODEC\_ILBC** = 97
* static final int **enum\_AUDIOCODEC\_AMR** = 98
* static final int **enum\_AUDIOCODEC\_AMRWB** = 99
* static final int **enum\_AUDIOCODEC\_SPEEX** = 100
* static final int **enum\_AUDIOCODEC\_SPEEXWB** =102
* static final int **enum\_AUDIOCODEC\_ISACWB** = 103
* static final int **enum\_AUDIOCODEC\_ISACSWB** =104
* static final int **enum\_AUDIOCODEC\_OPUS** =105
* static final int **enum\_AUDIOCODEC\_DTMF** = 101
* static final int enum\_VIDEOCODEC\_NONE = -1
* static final int enum\_VIDEOCODEC\_I420 = 133
* static final int **enum\_VIDEOCODEC\_H264** = 125
* static final int **enum\_VIDEOCODEC\_VP8** = 120
* static final int **enum\_VIDEOCODEC\_VP9** = 122
* static final int **enum\_SRTPPOLICY\_NONE** = 0
* static final int **enum\_SRTPPOLICY\_FORCE** = 1
* static final int **enum\_SRTPPOLICY\_PREFER** = 2
* static final int **enum\_TRANSPORT\_UDP** = 0
* static final int **enum\_TRANSPORT\_TLS** = 1
* static final int **enum\_TRANSPORT\_TCP** = 2
* static final int **enum\_LOG\_LEVEL\_NONE** = -1
* static final int **enum\_LOG\_LEVEL\_ERROR** = 1
* static final int **enum\_LOG\_LEVEL\_WARNING** = 2
* static final int **enum\_LOG\_LEVEL\_INFO** = 3
* static final int **enum\_LOG\_LEVEL\_DEBUG** = 4
* static final int **enum\_DTMF\_MOTHOD\_RFC2833** = 0
* static final int **enum\_DTMF\_MOTHOD\_INFO** = 1
* static final int **enum\_DIRECTION\_NONE** = 0
* static final int enum\_DIRECTION\_SEND\_RECV = 1
* static final int enum\_DIRECTION\_SEND = 2
* static final int enum\_DIRECTION\_RECV = 3
* static final int enum\_RECORD\_MODE\_NONE = 0
* static final int enum\_RECORD\_MODE\_RECV = 1
* static final int enum\_RECORD\_MODE\_SEND = 2
* static final int enum\_RECORD\_MODE\_BOTH = 3
*   static final int enum\_FILE\_FORMAT\_WAVE = 1

    _The record audio file is in WAVE format._
*   static final int enum\_FILE\_FORMAT\_AMR = 2

    _The record audio file is in AMR format - all voice data are compressed by AMR codec. Mono._
* static final int enum\_FILE\_FORMAT\_MP3 = 3 _The record audio file is in MP3 format._
*   static final int enum\_FILE\_FORMAT\_MP4 = 4

    _The record video file is in MP4(AAC and H264) format. !\[ref6]_

**Member Data Documentation**

**final int com.portsip.PortSipEnumDefine.enum\_VIDEOCODEC\_NONE = -1\[static]**

Used in startRecord only

**final int com.portsip.PortSipEnumDefine.enum\_VIDEOCODEC\_I420 = 133\[static]**

Used in startRecord only

**final int com.portsip.PortSipEnumDefine.enum\_DIRECTION\_SEND\_RECV = 1\[static]**

both received and sent

**final int com.portsip.PortSipEnumDefine.enum\_DIRECTION\_SEND = 2\[static]**

Only the sent

**final int com.portsip.PortSipEnumDefine.enum\_DIRECTION\_RECV = 3\[static]**

Only the received

**final int com.portsip.PortSipEnumDefine.enum\_RECORD\_MODE\_NONE = 0\[static]**

Not Recorded.

**final int com.portsip.PortSipEnumDefine.enum\_RECORD\_MODE\_RECV = 1\[static]**

Only record the received data.

**final int com.portsip.PortSipEnumDefine.enum\_RECORD\_MODE\_SEND = 2\[static]**

Only record the sent data.

**final int com.portsip.PortSipEnumDefine.enum\_RECORD\_MODE\_BOTH = 3\[static]**

**Static Public Attributes**

* static final int **ECoreErrorNone** = 0
* static final int **INVALID\_SESSION\_ID** = -1
* static final int **ECoreAlreadyInitialized** = -60000
* static final int **ECoreNotInitialized** = -60001
* static final int **ECoreSDKObjectNull** = -60002
* static final int **ECoreArgumentNull** = -60003
* static final int **ECoreInitializeWinsockFailure** = -60004
* static final int **ECoreUserNameAuthNameEmpty** = -60005
* static final int **ECoreInitiazeStackFailure** = -60006
* static final int **ECorePortOutOfRange** = -60007
* static final int **ECoreAddTcpTransportFailure** = -60008
* static final int **ECoreAddTlsTransportFailure** = -60009
* static final int **ECoreAddUdpTransportFailure** = -60010
* static final int **ECoreNotSupportMediaType** = -60011
* static final int **ECoreNotSupportDTMFValue** = -60012
* static final int **ECoreAlreadyRegistered** = -60021
* static final int **ECoreSIPServerEmpty** = -60022
* static final int **ECoreExpiresValueTooSmall** = -60023
* static final int **ECoreCallIdNotFound** = -60024
* static final int **ECoreNotRegistered** = -60025
* static final int **ECoreCalleeEmpty** = -60026
* static final int **ECoreInvalidUri** = -60027
* static final int **ECoreAudioVideoCodecEmpty** = -60028
* static final int **ECoreNoFreeDialogSession** = -60029
* static final int **ECoreCreateAudioChannelFailed** = -60030
* static final int **ECoreSessionTimerValueTooSmall** = -60040
* static final int **ECoreAudioHandleNull** = -60041
* static final int **ECoreVideoHandleNull** = -60042
* static final int **ECoreCallIsClosed** = -60043
* static final int **ECoreCallAlreadyHold** = -60044
* static final int **ECoreCallNotEstablished** = -60045
* static final int **ECoreCallNotHold** = -60050
* static final int **ECoreSipMessaegEmpty** = -60051
* static final int **ECoreSipHeaderNotExist** = -60052
* static final int **ECoreSipHeaderValueEmpty** = -60053
* static final int **ECoreSipHeaderBadFormed** = -60054
* static final int **ECoreBufferTooSmall** = -60055
* static final int **ECoreSipHeaderValueListEmpty** = -60056
* static final int **ECoreSipHeaderParserEmpty** = -60057
* static final int **ECoreSipHeaderValueListNull** = -60058
* static final int **ECoreSipHeaderNameEmpty** = -60059
* static final int **ECoreAudioSampleNotmultiple** = -60060
* static final int **ECoreAudioSampleOutOfRange** = -60061
* static final int **ECoreInviteSessionNotFound** = -60062
* static final int **ECoreStackException** = -60063
* static final int **ECoreMimeTypeUnknown** = -60064
* static final int **ECoreDataSizeTooLarge** = -60065
* static final int **ECoreSessionNumsOutOfRange** = -60066
* static final int **ECoreNotSupportCallbackMode** = -60067
* static final int **ECoreNotFoundSubscribeId** = -60068
* static final int **ECoreCodecNotSupport** = -60069
* static final int **ECoreCodecParameterNotSupport** = -60070
* static final int **ECorePayloadOutofRange** = -60071
* static final int **ECorePayloadHasExist** = -60072
* static final int **ECoreFixPayloadCantChange** = -60073
* static final int **ECoreCodecTypeInvalid** = -60074
* static final int **ECoreCodecWasExist** = -60075
* static final int **ECorePayloadTypeInvalid** = -60076
* static final int **ECoreArgumentTooLong** = -60077
* static final int **ECoreMiniRtpPortMustIsEvenNum** = -60078
* static final int **ECoreCallInHold** = -60079
* static final int **ECoreNotIncomingCall** = -60080
* static final int **ECoreCreateMediaEngineFailure** = -60081
* static final int **ECoreAudioCodecEmptyButAudioEnabled** = -60082
* static final int **ECoreVideoCodecEmptyButVideoEnabled** = -60083
* static final int **ECoreNetworkInterfaceUnavailable** = -60084
* static final int **ECoreWrongDTMFTone** = -60085
* static final int **ECoreWrongLicenseKey** = -60086
* static final int **ECoreTrialVersionLicenseKey** = -60087
* static final int **ECoreOutgoingAudioMuted** = -60088
* static final int **ECoreOutgoingVideoMuted** = -60089
* static final int **ECoreFailedCreateSdp** = -60090
* static final int **ECoreTrialVersionExpired** = -60091
* static final int **ECoreStackFailure** = -60092
* static final int **ECoreTransportExists** = -60093
* static final int **ECoreUnsupportTransport** = -60094
* static final int **ECoreAllowOnlyOneUser** = -60095
* static final int **ECoreUserNotFound** = -60096
* static final int **ECoreTransportsIncorrect** = -60097
* static final int **ECoreCreateTransportFailure** = -60098
* static final int **ECoreTransportNotSet** = -60099
* static final int **ECoreECreateSignalingFailure** = -60100
* static final int **ECoreArgumentIncorrect** = -60101
* static final int **ECoreIVRObjectNull** = -61001
* static final int **ECoreIVRIndexOutOfRange** = -61002
* static final int **ECoreIVRReferFailure** = -61003
* static final int **ECoreIVRWaitingTimeOut** = -61004
* static final int **EAudioFileNameEmpty** = -70000
* static final int **EAudioChannelNotFound** = -70001
* static final int **EAudioStartRecordFailure** = -70002
* static final int **EAudioRegisterRecodingFailure** = -70003
* static final int **EAudioRegisterPlaybackFailure** = -70004
* static final int **EAudioGetStatisticsFailure** = -70005
* static final int **EAudioPlayFileAlreadyEnable** = -70006
* static final int **EAudioPlayObjectNotExist** = -70007
* static final int **EAudioPlaySteamNotEnabled** = -70008
* static final int **EAudioRegisterCallbackFailure** = -70009
* static final int **EAudioCreateAudioConferenceFailure** = -70010
* static final int **EAudioOpenPlayFileFailure** = -70011
* static final int **EAudioPlayFileModeNotSupport** = -70012
* static final int **EAudioPlayFileFormatNotSupport** = -70013
* static final int **EAudioPlaySteamAlreadyEnabled** = -70014
* static final int **EAudioCreateRecordFileFailure** = -70015
* static final int **EAudioCodecNotSupport** = -70016
* static final int **EAudioPlayFileNotEnabled** = -70017
* static final int **EAudioPlayFileUnknowSeekOrigin** = -70018
* static final int **EAudioCantSetDeviceIdDuringCall** =-70019
* static final int **EAudioVolumeOutOfRange** =-70020
* static final int **EVideoFileNameEmpty** = -80000
* static final int **EVideoGetDeviceNameFailure** = -80001
* static final int **EVideoGetDeviceIdFailure** = -80002
* static final int **EVideoStartCaptureFailure** = -80003
* static final int **EVideoChannelNotFound** = -80004
* static final int **EVideoStartSendFailure** = -80005
* static final int **EVideoGetStatisticsFailure** = -80006
* static final int **EVideoStartPlayAviFailure** = -80007
* static final int **EVideoSendAviFileFailure** = -80008
* static final int **EVideoRecordUnknowCodec** = -80009
* static final int **EVideoCantSetDeviceIdDuringCall** = -80010
* static final int **EVideoUnsupportCaptureRotate** = -80011
* static final int **VideoUnsupportCaptureResolution** = -80012
* static final int **ECameraSwitchTooOften** = -80013
* static final int **EMTUOutOfRange** = -80014
* static final int **EDeviceGetDeviceNameFailure** = -90001
