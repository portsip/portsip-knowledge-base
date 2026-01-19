# User Manual for macOS

### FAQ

#### Where can I download the PortSIP VoIP SDK for testing?

You can download the **PortSIP VoIP SDK**, along with sample projects, from the [PortSIP Website](https://www.portsip.com).

***

#### What macOS versions are required for development?

Development using the **PortSIP VoIP/IMS SDK for macOS** requires:

* macOS Ventura 13.5 or later
* Xcode 15.0 or later

***

#### How can I create a new project with the PortSIP VoIP SDK?

1. **Download the Sample Project**\
   Download the sample project from the PortSIP Website and extract it to a desired directory.
2. **Create a New Project**\
   In **Xcode**, create a new **macOS Cocoa Application** project.
3. **Add the PortSIPVoIPSDK Framework**\
   Drag and drop `PortSIPVoIPSDK.framework` from Finder into your project’s **Frameworks** folder.
4. **Copy the Framework Files**\
   In the project editor, go to **Build Phases** and add a **Copy Files** build phase:
   * Set **Destination** to **Frameworks**
   * Add `PortSIPVoIPSDK.framework` to the list of files to be copied
5.  **Import the SDK**\
    In your header file, import the SDK:

    ```objective-c
    #import <PortSIPVoIPSDK/PortSIPVoIPSDK.h>
    ```

***

#### How can I process callback events?

1. **Implement the Delegate Protocol**\
   Your class must implement the `PortSIPEventDelegate` protocol to receive callback events.
2. **Handle Callback Methods**\
   Implement the required callback methods to process events, such as:
   * `onRegistrationState`
   * `onCallState`
   * Other signaling, media, or call-related callbacks as needed

***

#### How do I initialize the SDK?

1. **Create an SDK Instance**\
   Create an instance of the `PortSIPSDK` class.
2. **Set the Delegate**\
   Assign your class (which implements `PortSIPEventDelegate`) as the delegate of the SDK instance.

***

#### Is the SDK thread-safe?

Yes, the PortSIP SDK is **thread-safe**. You can safely call API functions from multiple threads without additional synchronization.

**Exceptions:**\
The following callbacks **must not** be invoked directly from other threads:

* `onAudioRawCallback`
* `onVideoRawCallback`
* `onRTPPacketCallback`

***

### Register events

```objc
- (void)onRegisterSuccess: (NSString *)statusText 
              statusCode: (int)statusCode 
              sipMessage: (NSString *)sipMessage
```

When successfully registered to server, this event will be triggered.

**Parameters**

| _statusText_ | The status text.          |
| ------------ | ------------------------- |
| _statusCode_ | The status code.          |
| _sipMessage_ | The SIP message received. |

```objc
- (void)onRegisterFailure: (NSString *)statusText 
              statusCode: (int)statusCode 
              sipMessage: (NSString *)sipMessage
```

If registration to SIP server fails, this event will be triggered.

**Parameters**

| _statusText_ | The status text.          |
| ------------ | ------------------------- |
| _statusCode_ | The status code.          |
| _sipMessage_ | The SIP message received. |

### Call events

```objc
- (void)onInviteIncoming: (long)sessionId 
       callerDisplayName: (NSString *)callerDisplayName 
                  caller: (NSString *)caller 
       calleeDisplayName: (NSString *)calleeDisplayName 
                  callee: (NSString *)callee 
             audioCodecs: (NSString *)audioCodecs 
             videoCodecs: (NSString *)videoCodecs 
             existsAudio: (BOOL)existsAudio 
             existsVideo: (BOOL)existsVideo 
              sipMessage: (NSString *)sipMessage

```

When the call is coming, this event will be triggered.

**Parameters**

| _sessionId_          | The session ID of the call.                                                        |
| -------------------- | ---------------------------------------------------------------------------------- |
| _callerDisplayNam e_ | The display name of caller                                                         |
| _caller_             | The caller.                                                                        |
| _calleeDisplayNam e_ | The display name of callee.                                                        |
| _callee_             | The callee.                                                                        |
| _audioCodecs_        | The matched audio codecs. It's separated by "#" if there are more than one codecs. |
| _videoCodecs_        | The matched video codecs. It's separated by "#" if there are more than one codecs. |
| _existsAudio_        | By setting to true, it indicates that this call includes the audio.                |
| _existsVideo_        | By setting to true, it indicates that this call includes the video.                |
| _sipMessage_         | The SIP message received.                                                          |

```objc
- (void)onInviteTrying: (long)sessionId
```

If the outgoing call is being processed, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onInviteSessionProgress:(long)sessionId
                    audioCodecs:(NSString*)audioCodecs
                    videoCodecs:(NSString*)videoCodecs
               existsEarlyMedia:(BOOL)existsEarlyMedia
                    existsAudio:(BOOL)existsAudio
                    existsVideo:(BOOL)existsVideo
                     sipMessage:(NSString*)sipMessage;
```

Once the caller received the "183 session in progress" message, this event will be triggered.

**Parameters**

| _sessionId_        | The session ID of the call.                                                        |
| ------------------ | ---------------------------------------------------------------------------------- |
| _audioCodecs_      | The matched audio codecs. It's separated by "#" if there are more than one codecs. |
| _videoCodecs_      | The matched video codecs. It's separated by "#" if there are more than one codecs. |
| _existsEarlyMedia_ | By setting to true, it indicates that the call has early media.                    |
| _existsAudio_      | By setting to true, it indicates that this call includes the audio.                |
| _existsVideo_      | By setting to true, it indicates that this call includes the video.                |
| _sipMessage_       | The SIP message received.                                                          |

```objc
- (void)onInviteRinging: (long)sessionId 
              statusText: (NSString *)statusText 
              statusCode: (int)statusCode 
              sipMessage: (NSString *)sipMessage 
```

If the outgoing call is ringing, this event will be triggered.

**Parameters**

| _sessionId_  | The session ID of the call. |
| ------------ | --------------------------- |
| _statusText_ | The status text.            |
| _statusCode_ | The status code.            |
| _sipMessage_ | The SIP message received.   |

```objc
- (void)onInviteAnswered: (long)sessionId 
       callerDisplayName: (NSString *)callerDisplayName 
                  caller: (NSString *)caller 
       calleeDisplayName: (NSString *)calleeDisplayName 
                  callee: (NSString *)callee 
             audioCodecs: (NSString *)audioCodecs 
             videoCodecs: (NSString *)videoCodecs 
             existsAudio: (BOOL)existsAudio 
             existsVideo: (BOOL)existsVideo 
              sipMessage: (NSString *)sipMessage
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
| _existsAudio_        | By setting to true, it indicates that this call includes the audio.                |
| _existsVideo_        | By setting to true, it indicates that this call includes the video.                |
| _sipMessage_         | The SIP message received.                                                          |

```objc
- (void)onInviteFailure: (long)sessionId 
      callerDisplayName: (NSString *)callerDisplayName 
                 caller: (NSString *)caller 
      calleeDisplayName: (NSString *)calleeDisplayName 
                 callee: (NSString *)callee 
                 reason: (NSString *)reason 
                   code: (int)code 
             sipMessage: (NSString *)sipMessage
```

If the outgoing/incoming call fails, this event will be triggered.

**Parameters**

| _sessionId_          | The session ID of the call. |
| -------------------- | --------------------------- |
| _callerDisplayNam e_ | The display name of caller  |
| _caller_             | The caller.                 |
| _calleeDisplayNam e_ | The display name of callee. |
| _callee_             | The callee.                 |
| _reason_             | The failure reason.         |
| _code_               | The failure code.           |
| _sipMessage_         | The SIP message received.   |

```objc
- (void)onInviteUpdated: (long)sessionId 
             audioCodecs: (NSString *)audioCodecs 
             videoCodecs: (NSString *)videoCodecs 
             screenCodecs: (NSString *)screenCodecs 
             existsAudio: (BOOL)existsAudio 
             existsVideo: (BOOL)existsVideo 
             existsScreen: (BOOL)existsScreen 
              sipMessage: (NSString *)sipMessage
```

This event will be triggered when remote party updates this call.

**Parameters**

| _sessionId_    | The session ID of the call.                                                         |
| -------------- | ----------------------------------------------------------------------------------- |
| _audioCodecs_  | The matched audio codecs. It's separated by "#" if there are more than one codecs.  |
| _videoCodecs_  | The matched video codecs. It's separated by "#" if there are more than one codecs.  |
| _screenCodecs_ | The matched screen codecs. It's separated by "#" if there are more than one codecs. |
| _existsAudio_  | By setting to true, it indicates that this call includes the audio.                 |
| _existsVideo_  | By setting to true, it indicates that this call includes the video.                 |
| _existsScreen_ | By setting to true, it indicates that this call includes the screen.                |
| _sipMessage_   | The SIP message received.                                                           |

```objc
- (void)onInviteConnected: (long)sessionId
```

This event will be triggered when UAC sent/UAS received ACK (the call is connected). Some functions (hold, updateCall etc...) can be called only after the call is connected, otherwise it will return error.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onInviteBeginingForward: (NSString *)forwardTo
```

If the enableCallForward method is called and a call is incoming, the call will be forwarded automatically and this event will be triggered.

**Parameters**

| _forwardTo_ | The target SIP URI for forwarding. |
| ----------- | ---------------------------------- |

```objc
- (void)onInviteClosed: (long)sessionId 
            sipMessage: (NSString *)sipMessage

```

This event will be triggered once remote side closes the call.

**Parameters**

| _sessionId_  | The session ID of the call. |
| ------------ | --------------------------- |
| _sipMessage_ | The SIP message received.   |

```objc
- (void)onDialogStateUpdated: (NSString *)BLFMonitoredUri 
              BLFDialogState: (NSString *)BLFDialogState 
                BLFDialogId: (NSString *)BLFDialogId 
          BLFDialogDirection: (NSString *)BLFDialogDirection
```

If a user subscribed and his dialog status monitored, when the monitored user is holding a call or being rang, this event will be triggered.

**Parameters**

| _BLFMonitoredUri_     | the monitored user's URI    |
| --------------------- | --------------------------- |
| _BLFDialogState_      | - the status of the call    |
| _BLFDialogId_         | - the id of the call        |
| _BLFDialogDirecti on_ | - the direction of the call |

```objc
- (void)onRemoteHold: (long)sessionId
```

If the remote side has placed the call on hold, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onRemoteUnHold: (long)sessionId 
            audioCodecs: (NSString *)audioCodecs 
            videoCodecs: (NSString *)videoCodecs 
            existsAudio: (BOOL)existsAudio 
            existsVideo: (BOOL)existsVideo
```

If the remote side un-holds the call, this event will be triggered.

**Parameters**

| _sessionId_   | The session ID of the call.                                                        |
| ------------- | ---------------------------------------------------------------------------------- |
| _audioCodecs_ | The matched audio codecs. It's separated by "#" if there are more than one codecs. |
| _videoCodecs_ | The matched video codecs. It's separated by "#" if there are more than one codecs. |
| _existsAudio_ | By setting to true, it indicates that this call includes the audio.                |
| _existsVideo_ | By setting to true, it indicates that this call includes the video.                |

### Refer events

```objc
- (void)onReceivedRefer:(long)sessionId
                referId:(long)referId
                     to:(NSString*)to
                   from:(NSString*)from
        referSipMessage:(NSString*)referSipMessage;
```

This event will be triggered once receiving a REFER message.

**Parameters**

| _sessionId_       | The session ID of the call.                                         |
| ----------------- | ------------------------------------------------------------------- |
| _referId_         | The ID of the REFER message. Pass it to acceptRefer or rejectRefer. |
| _to_              | The refer target.                                                   |
| _from_            | The sender of REFER message.                                        |
| _referSipMessage_ | The SIP message of "REFER". Pass it to "acceptRefer" function.      |

```objc
- (void)onReferAccepted:(long)sessionId;
```

This callback will be triggered once remote side calls "acceptRefer" to accept the REFER.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onReferRejected:(long)sessionId 
                 reason:(NSString*)reason
                   code:(int)code;
```

This callback will be triggered once remote side calls "rejectRefer" to reject the REFER.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _reason_    | Reason for rejecting.       |
| _code_      | Rejecting code.             |

```objc
- (void)onTransferTrying:(long)sessionId;
```

When the refer call is being processed, this event will be trigged.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onTransferRinging:(long)sessionId;
```

When the refer call rings, this event will be triggered.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onACTVTransferSuccess:(long)sessionId;
```

When the refer call succeeds, this event will be triggered. ACTV means Active. For example: A starts the call with B, and A transfers B to C. When C accepts the refered call, A will receive this event.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

```objc
- (void)onACTVTransferFailure:(long)sessionId 
                       reason:(NSString*)reason
                         code:(int)code;
```

When the refer call fails, this event will be triggered. ACTV means Active. For example: A starts the call with B, and A transfers B to C. When C rejects the refered call, A will receive this event.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _reason_    | The error reason.           |
| _code_      | The error code.             |

### Signaling events

```objc
- (void)onReceivedSignaling:(long)sessionId 
                    message:(NSString*)message; 
```

This event will be triggered when receiving an SIP message. This event is disabled by default. To enable, use enableCallbackSignaling.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _message_   | The SIP message received.   |

```objc
- (void)onSendingSignaling:(long)sessionId 
                   message:(NSString*)message; 
```

This event will be triggered when a SIP message is sent. This event is disabled by default. To enable, use enableCallbackSignaling.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _message_   | The SIP message sent.       |

### MWI events

```objc
- (void)onWaitingVoiceMessage:(NSString*)messageAccount
        urgentNewMessageCount:(int)urgentNewMessageCount
        urgentOldMessageCount:(int)urgentOldMessageCount
              newMessageCount:(int)newMessageCount
              oldMessageCount:(int)oldMessageCount;
```

If there are any waiting voice messages (MWI), this event will be triggered.

**Parameters**

| _messageAccount_         | Account for voice message.    |
| ------------------------ | ----------------------------- |
| _urgentNewMessag eCount_ | Count of new urgent messages. |
| _urgentOldMessage Count_ | Count of old urgent messages. |
| _newMessageCount_        | Count of new messages.        |
| _oldMessageCount_        | Count of old messages.        |

```objc
- (void)onWaitingFaxMessage:(NSString*)messageAccount
      urgentNewMessageCount:(int)urgentNewMessageCount
      urgentOldMessageCount:(int)urgentOldMessageCount
            newMessageCount:(int)newMessageCount
            oldMessageCount:(int)oldMessageCount;
```

If there are any waiting fax messages (MWI), this event will be triggered.

**Parameters**

| _messageAccount_         | Account for fax message.      |
| ------------------------ | ----------------------------- |
| _urgentNewMessag eCount_ | Count of new urgent messages. |
| _urgentOldMessage Count_ | Count of old urgent messages. |
| _newMessageCount_        | Count of new messages.        |
| _oldMessageCount_        | Count of old messages.        |

### DTMF events

```objc
- (void)onRecvDtmfTone:(long)sessionId 
                  tone:(int)tone;
```

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

### INFO/OPTIONS message events

```objc
- (void)onRecvOptions:(NSString*)optionsMessage;
```

This event will be triggered when receiving the OPTIONS message.

**Parameters**

| _optionsMessage_ | The whole received OPTIONS message in text format. |
| ---------------- | -------------------------------------------------- |

```objc
- (void)onRecvInfo:(NSString*)infoMessage;
```

This event will be triggered when receiving the INFO message.

**Parameters**

| _infoMessage_ | The whole received INFO message in text format. |
| ------------- | ----------------------------------------------- |

```objc
- (void)onRecvNotifyOfSubscription:(long)subscribeId
                     notifyMessage:(NSString*)notifyMessage
                       messageData:(unsigned char*)messageData
                 messageDataLength:(int)messageDataLength;
```

This event will be triggered when receiving a NOTIFY message of the subscription.

**Parameters**

| _subscribeId_        | The ID of SUBSCRIBE request.                                     |
| -------------------- | ---------------------------------------------------------------- |
| _notifyMessage_      | The received INFO message in text format.                        |
| _messageData_        | The received message body. It can be either text or binary data. |
| _messageDataLengt h_ | The length of "messageData".                                     |

### Presence events

```objc
- (void)onPresenceRecvSubscribe:(long)subscribeId
                fromDisplayName:(NSString*)fromDisplayName
                           from:(NSString*)from
                        subject:(NSString*)subject; 
```

This event will be triggered when receiving the SUBSCRIBE request from a contact.

**Parameters**

| _subscribeId_     | The ID of SUBSCRIBE request.                 |
| ----------------- | -------------------------------------------- |
| _fromDisplayName_ | The display name of contact.                 |
| _from_            | The contact who sends the SUBSCRIBE request. |
| _subject_         | The subject of the SUBSCRIBE request.        |

```objc
- (void)onPresenceOnline:(NSString*)fromDisplayName
                    from:(NSString*)from
               stateText:(NSString*)stateText;
```

This event will be triggered when the contact is online or changes presence status.

**Parameters**

| _fromDisplayName_ | The display name of contact.                 |
| ----------------- | -------------------------------------------- |
| _from_            | The contact who sends the SUBSCRIBE request. |
| _stateText_       | The presence status text.                    |

```objc
- (void)onPresenceOffline:(NSString*)fromDisplayName from:(NSString*)from;
```

When the contact status is changed to offline, this event will be triggered. **Parameters**

| _fromDisplayName_ | The display name of contact.                |
| ----------------- | ------------------------------------------- |
| _from_            | The contact who sends the SUBSCRIBE request |

### MESSAGE message events

```objc
- (void)onRecvMessage:(long)sessionId
             mimeType:(NSString*)mimeType
          subMimeType:(NSString*)subMimeType
          messageData:(unsigned char*)messageData
    messageDataLength:(int)messageDataLength;
```

This event will be triggered when receiving a MESSAGE message in dialog.

**Parameters**

| _sessionId_          | The session ID of the call.                                      |
| -------------------- | ---------------------------------------------------------------- |
| _mimeType_           | The message mime type.                                           |
| _subMimeType_        | The message sub mime type.                                       |
| _messageData_        | The received message body. It can be either text or binary data. |
| _messageDataLengt h_ | The length of "messageData".                                     |

```objc
- (void)onRecvOutOfDialogMessage:(NSString*)fromDisplayName
                            from:(NSString*)from
                   toDisplayName:(NSString*)toDisplayName
                              to:(NSString*)to
                        mimeType:(NSString*)mimeType
                     subMimeType:(NSString*)subMimeType
                     messageData:(unsigned char*)messageData
               messageDataLength:(int)messageDataLength
                      sipMessage:(NSString*)sipMessage;
```

This event will be triggered when receiving a MESSAGE message out of dialog. For example: pager message.

**Parameters**

| _fromDisplayName_    | The display name of sender.                               |
| -------------------- | --------------------------------------------------------- |
| _from_               | The message sender.                                       |
| _toDisplayName_      | The display name of receiver.                             |
| _to_                 | The recipient.                                            |
| _mimeType_           | The message mime type.                                    |
| _subMimeType_        | The message sub mime type.                                |
| _messageData_        | The received message body. It can be text or binary data. |
| _messageDataLengt h_ | The length of "messageData".                              |
| _sipMessage_         | The SIP message received.                                 |

```objc
- (void)onSendMessageSuccess:(long)sessionId
                   messageId:(long)messageId
                  sipMessage:(NSString*)sipMessage;
```

This event will be triggered when the message is sent successfully in dialog.

**Parameters**

| _sessionId_  | The session ID of the call.                                             |
| ------------ | ----------------------------------------------------------------------- |
| _messageId_  | The message ID. It's equal to the return value of sendMessage function. |
| _sipMessage_ | The SIP message received.                                               |

```objc
- (void)onSendMessageFailure:(long)sessionId
                   messageId:(long)messageId
                      reason:(NSString*)reason
                        code:(int)code
                  sipMessage:(NSString*)sipMessage;
```

This event will be triggered when the message fails to be sent out of dialog.

**Parameters**

| _sessionId_  | The session ID of the call.                                             |
| ------------ | ----------------------------------------------------------------------- |
| _messageId_  | The message ID. It's equal to the return value of sendMessage function. |
| _reason_     | The failure reason.                                                     |
| _code_       | Failure code.                                                           |
| _sipMessage_ | The SIP message received.                                               |

```objc
- (void)onSendOutOfDialogMessageSuccess:(long)messageId
                        fromDisplayName:(NSString*)fromDisplayName
                                   from:(NSString*)from
                          toDisplayName:(NSString*)toDisplayName
                                     to:(NSString*)to
                             sipMessage:(NSString*)sipMessage; 
```

This event will be triggered when the message is sent successfully out of dialog.

**Parameters**

| _messageId_       | The message ID. It's equal to the return value of SendOutOfDialogMessage function. |
| ----------------- | ---------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of message sender.                                                |
| _from_            | The message sender.                                                                |
| _toDisplayName_   | The display name of message receiver.                                              |
| _to_              | The message receiver.                                                              |
| _sipMessage_      | The SIP message received.                                                          |

```objc
- (void)onSendOutOfDialogMessageFailure:(long)messageId
                        fromDisplayName:(NSString*)fromDisplayName
                                   from:(NSString*)from
                          toDisplayName:(NSString*)toDisplayName
                                     to:(NSString*)to
                                 reason:(NSString*)reason
                                   code:(int)code
                             sipMessage:(NSString*)sipMessage;
```

This event will be triggered when the message fails to be sent out of dialog.

**Parameters**

| _messageId_       | The message ID. It's equal to the return value of SendOutOfDialogMessage function. |
| ----------------- | ---------------------------------------------------------------------------------- |
| _fromDisplayName_ | The display name of message sender                                                 |
| _from_            | The message sender.                                                                |
| _toDisplayName_   | The display name of message receiver.                                              |
| _to_              | The message recipient.                                                             |
| _reason_          | The failure reason.                                                                |
| _code_            | The failure code.                                                                  |
| _sipMessage_      | The SIP message received.                                                          |

```objc
- (void)onSubscriptionFailure:(long)subscribeId
                   statusCode:(int)statusCode;
```

This event will be triggered on sending SUBSCRIBE failure.

**Parameters**

| _subscribeId_ | The ID of SUBSCRIBE request. |
| ------------- | ---------------------------- |
| _statusCode_  | The status code.             |

```objc
- (void)onSubscriptionTerminated:(long)subscribeId;
```

This event will be triggered when a SUBSCRIPTION is terminated or expired.

**Parameters**

| _subscribeId_ | The ID of SUBSCRIBE request. |
| ------------- | ---------------------------- |

### Play audio and video file finished events

```objc
- (void)onPlayFileFinished:(long)sessionId fileName:(NSString*)fileName; 
```

If startPlayingFileToRemote function is called with no loop mode, this event will be triggered once the file play finished.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |
| _fileName_  | The play file name.         |

```objc
- (void)onStatistics:(long)sessionId stat:(NSString*)stat;
```

If getStatistics function is called, this event will be triggered once receiving a RTP statistics .

**Parameters**

| _sessionId_ | The session ID of the call.      |
| ----------- | -------------------------------- |
| _stat_      | RTP statistics of a json format. |

### RTP callback events

```objc
- (void)onRTPPacketCallback:(long)sessionId
                  mediaType:(int)mediaType
                  direction:(DIRECTION_MODE) direction
                  RTPPacket:(unsigned char *)RTPPacket
                 packetSize:(int)packetSize;
```

If enableRtpCallback function is called to enable the RTP callback, this event will be triggered once a RTP packet received.

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

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code, which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

### Audio and video stream callback events

```objc
- (void)onAudioRawCallback:(long)sessionId
         audioCallbackMode:(int)audioCallbackMode
                      data:(unsigned char *)data
                dataLength:(int)dataLength
            samplingFreqHz:(int)samplingFreqHz;
```

This event will be triggered once receiving the audio packets when enableAudioStreamCallback function is called.

**Parameters**

| _sessionId_          | The session ID of the call.                                            |
| -------------------- | ---------------------------------------------------------------------- |
| _audioCallbackMod e_ | The type that is passed in enableAudioStreamCallback function.         |
| _data_               | The memory of audio stream. It's in PCM format.                        |
| _dataLength_         | The data size.                                                         |
| _samplingFreqHz_     | The audio stream sample in HZ. For example, it could be 8000 or 16000. |

**Note**

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code, which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

```objc
- (int)onVideoRawCallback:(long)sessionId
        videoCallbackMode:(int)videoCallbackMode
                    width:(int)width
                   height:(int)height
                     data:(unsigned char *)data
               dataLength:(int)dataLength;
```

This event will be triggered once received the video packets if called enableVideoStreamCallback function.

**Parameters**

| _sessionId_          | The session ID of the call.                                      |
| -------------------- | ---------------------------------------------------------------- |
| _videoCallbackMod e_ | The type passed in enableVideoStreamCallback function.           |
| _width_              | The width of video image.                                        |
| _height_             | The height of video image.                                       |
| _data_               | The memory of video stream. It's in YUV420 format, such as YV12. |
| _dataLength_         | The data size.                                                   |

**Returns**

If you changed the sent video data, dataLength should be returned, otherwise 0.

**Note**

Don't call any SDK API functions in this event directly. If you want to call the API functions or other code, which is time-consuming, you should post a message to another thread and execute SDK API functions or other code in another thread.

## SDK functions

### Initialize and register functions

```objc
- (int) initialize:(TRANSPORT_TYPE)transport
           localIP:(NSString*)localIP
      localSIPPort:(int)localSIPPort
          loglevel:(PORTSIP_LOG_LEVEL)logLevel
           logPath:(NSString*)logFilePath
           maxLine:(int)maxCallLines
             agent:(NSString*)sipAgent
  audioDeviceLayer:(int)audioDeviceLayer
  videoDeviceLayer:(int)videoDeviceLayer
TLSCertificatesRootPath:(NSString*)TLSCertificatesRootPath
     TLSCipherList:(NSString*)TLSCipherList
verifyTLSCertificate:(BOOL)verifyTLSCertificate
        dnsServers:(NSString*)dnsServers;
```

Initialize the SDK.

**Parameters**

| _transport_                | Transport for SIP signaling. TRANSPORT\_PERS\_UDP/TRANSPORT\_PERS\_TCP is the PortSIP private transport for anti SIP blocking. It must be used with PERS Server.                                                                                                                                                                                                    |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _localIP_                  | <p>The local computer IP address to be bounded (for example: 192.168.1.108). It will be used for sending and receiving SIP messages and RTP packets. If this IP is transferred in IPv6 format, the SDK will use IPv6.</p><p>If you want the SDK to choose correct network interface (IP) automatically, please pass the "0.0.0.0"(for IPv4) or "::" (for IPv6).</p> |
| _localSIPPort_             | The SIP message transport listener port (for example: 5060).                                                                                                                                                                                                                                                                                                        |
| _logLevel_                 | Set the application log level. The SDK will generate "PortSIP\_Log\_datatime.log" file if the log enabled.                                                                                                                                                                                                                                                          |
| _logFilePath_              | The log file path. The path (folder) MUST be existent.                                                                                                                                                                                                                                                                                                              |
| _maxCallLines_             | Theoretically, unlimited lines are supported depending on the device capability. For SIP client recommended value ranges 1 - 100.                                                                                                                                                                                                                                   |
| _sipAgent_                 | The User-Agent header to be inserted in SIP messages.                                                                                                                                                                                                                                                                                                               |
| _audioDeviceLayer_         | 0 = Use OS default device 1 = Set to 1 to use the virtual audio device if the no sound device installed.                                                                                                                                                                                                                                                            |
| _videoDeviceLayer_         | 0 = Use OS default device 1 = Set to 1 to use the virtual video device if no camera installed.                                                                                                                                                                                                                                                                      |
| _TLSCertificatesRo otPath_ | Specify the TLS certificate path, from which the SDK will load the certificates automatically. Note: On Windows, this path will be ignored, and SDK will read the certificates from Windows certificates stored area instead.                                                                                                                                       |
| _TLSCipherList_            | Specify the TLS cipher list. This parameter is usually passed as empty so that the SDK will offer all available ciphers.                                                                                                                                                                                                                                            |
| _verifyTLSCertificat e_    | Indicate if SDK will verify the TLS certificate. By setting to false, the SDK will not verify the validity of TLS certificate.                                                                                                                                                                                                                                      |
| _dnsServers_               | Additional Nameservers DNS servers. Value null indicates system DNS Server. Multiple servers will be split by ";", e.g "8.8.8.8;8.8.4.4"                                                                                                                                                                                                                            |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setInstanceId:(NSString*)instanceId;
```

Set the instance Id, the outbound instanceId((RFC5626) ) used in contact headers.

**Parameters**

| _instanceId_ | The SIP instance ID. If this function is not called, the SDK will generate an instance ID automatically. The instance ID MUST be unique on the same device (device ID or IMEI ID is recommended). Recommend to call this function to set the ID on Android devices. |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void) unInitialize;
```

Un-initialize the SDK and release resources.

```objc
- (int) setUser:(NSString*)userName
    displayName:(NSString*)displayName
       authName:(NSString*)authName
       password:(NSString*)password
     userDomain:(NSString*)userDomain
      SIPServer:(NSString*)sipServer
  SIPServerPort:(int)sipServerPort
     STUNServer:(NSString*)stunServer
 STUNServerPort:(int)stunServerPort
 outboundServer:(NSString*)outboundServer
outboundServerPort:(int)outboundServerPort;
```

Set user account info.

**Parameters**

| _userName_            | Account (username) of the SIP. It's usually provided by an IP-Telephony service provider.                                          |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| _displayName_         | The display name of user. You can set it as your like, such as "James Kend". It's optional.                                        |
| _authName_            | Authorization user name (usually equal to the username).                                                                           |
| _password_            | The password of user. It's optional.                                                                                               |
| _userDomain_          | User domain. This parameter is optional. It allows to pass an empty string if you are not using domain.                            |
| _sipServer_           | SIP proxy server IP or domain. For example: xx.xxx.xx.x or sip.xxx.com.                                                            |
| _sipServerPort_       | Port of the SIP proxy server. For example: 5060.                                                                                   |
| _stunServer_          | Stun server, used for NAT traversal. It's optional and can pass an empty string to disable STUN.                                   |
| _stunServerPort_      | STUN server port. It will be ignored if the outboundServer is empty.                                                               |
| _outboundServer_      | Outbound proxy server. For example: sip.domain.com. It's optional and allows to pass an empty string if not using outbound server. |
| _outboundServerPo rt_ | Outbound proxy server port. It will be ignored if the outboundServer is empty.                                                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setDisplayName:(NSString*)displayName;
```

Set the display name of user.

**Parameters**

| _displayName_ | that will appear in the From/To Header. |
| ------------- | --------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void) removeUser;
```

Remove user account info.

```objc
- (int)registerServer:(int)expires
           retryTimes:(int)retryTimes;
```

Register to SIP proxy server (login to server)

**Parameters**

| _expires_    | Registration refreshment interval in seconds. Maximum of 3600 allowed. It will be inserted into SIP REGISTER message headers.                                   |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _retryTimes_ | The retry times if failed to refresh the registration. Once set to <= 0, the retry will be disabled and onRegisterFailure callback triggered for retry failure. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code. If registration to server succeeds, onRegisterSuccess will be triggered, otherwise onRegisterFailure triggered.

```objc
- (int)refreshRegistration:(int)expires;
```

Refresh the registration manually after successfully registered.

**Parameters**

| _expires_ | Registration refreshment interval in seconds. Maximum of 3600 supported. It will be inserted into SIP REGISTER message header. If it's set to 0, default value will be used. |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code. If registration to server succeeds, onRegisterSuccess will be triggered, otherwise onRegisterFailure triggered.

```objc
- (int)unRegisterServer:(int)waitMS; 
```

Un-register from the SIP proxy server.

**Parameters**

| _waitMS_ | Wait for the server to reply that the un-registration is successful, waitMS is the longest waiting milliseconds, 0 means not waiting. |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setLicenseKey:(NSString*)key;
```

Set the license key. It must be called before setUser function.

**Parameters**

| _key_ | The SDK license key. Please purchase from PortSIP. |
| ----- | -------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Parameters**

### NIC and local IP functions

```objc
- (int)getNICNums;
```

Get the Network Interface Card numbers. **Returns**

If the function succeeds, it will return NIC numbers, which are greater than or equal to 0. If the function fails, it will return a specific error code.

```objc
- (NSString*)getLocalIpAddress:(int)index;
```

Get the local IP address by Network Interface Card index.

**Parameters**

| _index_ | The IP address index. For example, suppose the PC has two NICs. If we want to obtain the second NIC IP, please set this parameter as 1, and the first NIC IP index 0. |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

The buffer for receiving the IP.

### Audio and video codecs functions

```objc
- (int)addAudioCodec:(AUDIOCODEC_TYPE)codecType;
```

Enable an audio codec. It will appear in SDP.

**Parameters**

| _codecType_ | Audio codec type. |
| ----------- | ----------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)addVideoCodec:(VIDEOCODEC_TYPE)codecType;
```

Enable a video codec. It will appear in SDP.

**Parameters**

| _codecType_ | Video codec type. |
| ----------- | ----------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (BOOL)isAudioCodecEmpty;
```

Detect if the enabled audio codecs is empty.

**Returns**

If no audio codec is enabled, it will return value true, otherwise false.

```objc
- (BOOL)isVideoCodecEmpty;
```

Detect if enabled video codecs is empty or not.

**Returns**

If no video codec is enabled, it will return value true, otherwise false.

```objc
- (int)setAudioCodecPayloadType:(AUDIOCODEC_TYPE)codecType
                    payloadType:(int) payloadType;
```

Set the RTP payload type for dynamic audio codec.

**Parameters**

| _codecType_   | Audio codec type defined in the PortSIPTypes file. |
| ------------- | -------------------------------------------------- |
| _payloadType_ | The new RTP payload type that you want to set.     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoCodecPayloadType:(VIDEOCODEC_TYPE)codecType
                    payloadType:(int) payloadType;
```

Set the RTP payload type for dynamic Video codec.

**Parameters**

| _codecType_   | Video codec type defined in the PortSIPTypes file. |
| ------------- | -------------------------------------------------- |
| _payloadType_ | The new RTP payload type that you want to set.     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setAudioCodecParameter:(AUDIOCODEC_TYPE)codecType
                     parameter:(NSString*)parameter;
 
```

Set the codec parameter for audio codec.

**Parameters**

| _codecType_ | Audio codec type defined in the PortSIPTypes file. |
| ----------- | -------------------------------------------------- |
| _parameter_ | The parameter in string format.                    |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Example:

`[myVoIPsdk setAudioCodecParameter:AUDIOCODEC\_AMR parameter:"mode-set=0; octet-align=1; robust-sorting=0"];`

```objc
- (int)setVideoCodecParameter:(VIDEOCODEC_TYPE) codecType
                     parameter:(NSString*)parameter;

```

Set the codec parameter for video codec.

**Parameters**

| _codecType_ | Video codec type defined in the PortSIPTypes file. |
| ----------- | -------------------------------------------------- |
| _parameter_ | The parameter in string format.                    |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

**Remarks**

Example:

`[myVoIPsdk setVideoCodecParameter:VIDEOCODEC\_H264 parameter:"profile-level-id=420033; packetization-mode=0"];`

```objc
- (void) clearAudioCodec;
```

Remove all enabled audio codecs.

```objc
- (void) clearVideoCodec;
```

Remove all enabled video codecs.

### Additional settings functions

```objc
- (NSString*)getVersion;
```

Get the current version number of the SDK.

**Returns**

Return a current version number MAJOR.MINOR.PATCH of the SDK.

```objc
- (int)enableRport:(BOOL)enable;
```

Enable/disable rport(RFC3581).

**Parameters**

| _enable_ | Set to true to enable the SDK to support rport. By default it is enabled. |
| -------- | ------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)enableEarlyMedia:(BOOL)enable;
```

Enable/disable Early Media.

| _enable_ | Set to true to enable the SDK to support Early Media. By default the Early Media is disabled. |
| -------- | --------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)enablePriorityIPv6Domain:(BOOL)enable;
```

Enable/disable which allows specifying the preferred protocol when a domain supports both IPV4 and IPV6 simultaneously.

**Parameters**

| _enable_ | Set to true to enable priority IPv6 Domain. with the default priority being IPV4. |
| -------- | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setUriUserEncoding:(NSString*)character enable:(BOOL)enable;
```

Modifies the default URI user character needs to be escaped.

**Parameters**

| _character_ | The character to be modified, set one character at a time.                              |
| ----------- | --------------------------------------------------------------------------------------- |
| _enable_    | Whether escaping is required, true for allowing escaping, false for disabling escaping. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
-(int)setReliableProvisional:(int)mode;
```

Enable/disable PRACK.

**Parameters**

| _mode_ | <p>Modes work as follows:<br>0 - Never, Disable PRACK,By default the PRACK is disabled.<br>1 - SupportedEssential, Only send reliable provisionals if sending a body and far end supports.<br>2 - Supported, Always send reliable provisionals if far end supports.<br>3 - Required Always send reliable provisionals.</p> |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)enable3GppTags:(BOOL)enable;
```

Enable/disable the 3Gpp tags, including "ims.icsi.mmtel" and "g.3gpp.smsip".

**Parameters**

| _enable_ | Set to true to enable the SDK to support 3Gpp tags. |
| -------- | --------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)enableCallbackSignaling:(BOOL)enableSending
                 enableReceived:(BOOL)enableReceived;
```

Enable/disable to callback the SIP messages.

**Parameters**

| _enableSending_  | Set as true to enable to callback the sent SIP messages, or false to disable. Once enabled, the "onSendingSignaling" event will be triggered when the SDK sends a SIP message.         |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _enableReceived_ | Set as true to enable to callback the received SIP messages, or false to disable. Once enabled, the "onReceivedSignaling" event will be triggered when the SDK receives a SIP message. |

```objc
- (int)setSrtpPolicy:(SRTP_POLICY)srtpPolicy;
```

**Parameters**

| _srtpPolicy_ | The SRTP policy. |
| ------------ | ---------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setRtpPortRange:(int) minimumRtpPort
        maximumRtpPort:(int) maximumRtpPort;
```

Set the RTP ports range for audio and video streaming.

**Parameters**

| _minimumRtpPort_ | The minimum RTP port. |
| ---------------- | --------------------- |
| _maximumRtpPort_ | The maximum RTP port. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The port range ((max - min) / maxCallLines) should be greater than 4.

```objc
- (int)enableCallForward:(BOOL)forBusyOnly forwardTo:(NSString*) forwardTo;
```

Enable call forwarding.

**Parameters**

| _forBusyOnly_ | If this parameter is set as true, the SDK will forward all incoming calls when currently it's busy. If it's set as false, the SDK forward all incoming calls anyway. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _forwardTo_   | The target of call forwarding. It must in the format of sip[:xxxx@sip.portsip.com.](mailto:xxxx@sip.portsip.com)                                                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)disableCallForward;
```

Disable the call forwarding. The SDK is not forwarding any incoming calls once this function is called.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)enableSessionTimer:(int) timerSeconds refreshMode:(SESSION_REFRESH_MODE)refreshMode;
```

Allows to periodically refresh Session Initiation Protocol (SIP) sessions by sending INVITE requests repeatedly.

**Parameters**

| _timerSeconds_ | The value of the refreshment interval in seconds. Minimum of 90 seconds required.                     |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| _refreshMode_  | Allow to set the session refreshment by UAC or UAS: SESSION\_REFERESH\_UAC or SESSION\_REFERESH\_UAS; |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The INVITE requests, or re-INVITEs, are sent repeatedly during an active call log to allow user agents (UA) or proxies to determine the status of a SIP session. Without this keep-alive mechanism, proxies for remembering incoming and outgoing requests (stateful proxies) may continue to retain call state needlessly. If a UA fails to send a BYE message at the end of a session or if the BYE message is lost because of network problems, a stateful proxy does not know that the session has ended. The re-INVITEs ensure that active sessions stay active and completed sessions are terminated.

```objc
- (int)disableSessionTimer;
```

Disable the session timer.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)setDoNotDisturb:(BOOL)state;
```

Enable the "Do not disturb" to enable/disable.

**Parameters**

| _state_ | If it is set to true, the SDK will reject all incoming calls anyway. |
| ------- | -------------------------------------------------------------------- |

```objc
- (void)enableAutoCheckMwi:(BOOL)state;
```

Enable the CheckMwi to enable/disable.

**Parameters**

| _state_ | If it is set to true, the SDK will check Mwi automatically. |
| ------- | ----------------------------------------------------------- |

```objc
- (int)setRtpKeepAlive:(BOOL)state
  keepAlivePayloadType:(int)keepAlivePayloadType
   deltaTransmitTimeMS:(int)deltaTransmitTimeMS;
```

Enable or disable to send RTP keep-alive packet when the call is established.

**Parameters**

| _state_                 | Set as true to allow to send the keep-alive packet during the conversation.                           |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| _keepAlivePayload Type_ | The payload type of the keep-alive RTP packet. It's usually set to 126.                               |
| _deltaTransmitTime MS_  | The keep-alive RTP packet sending interval, in milliseconds. Recommended value ranges 15000 - 300000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setKeepAliveTime:(int) keepAliveTime;
```

Enable or disable to send SIP keep-alive packet.

**Parameters**

| _keepAliveTime_ | This is the SIP keep-alive time interval in seconds. By setting to 0, the SIP keep-alive will be disabled. Recommended value is 30 or 50. |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setAudioSamples:(int) ptime
              maxPtime:(int) maxPtime;
```

Set the audio capturing sample.

**Parameters**

| _ptime_    | It should be a multiple of 10 between 10 - 60 (with 10 and 60 inclusive).                                                               |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| _maxPtime_ | For the "maxptime" attribute, it should be a multiple of 10 between 10 - 60 (with 10 and 60 inclusive). It cannot be less than "ptime". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

It will appear in the SDP of INVITE and 200 OK message as "ptime and "maxptime" attribute.

```objc
- (int)addSupportedMimeType:(NSString*) methodName
                   mimeType:(NSString*) mimeType
                subMimeType:(NSString*) subMimeType;
```

Set the SDK to receive the SIP message that includes special mime type.

**Parameters**

| _methodName_ | Method name of the SIP message, such as INVITE, OPTION, INFO, MESSAGE, UPDATE, ACK etc. For more details please refer to the RFC3261. |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------- |

|\*mimeType \*| The mime type of SIP message. |

|_subMimeType_| The sub mime type of SIP message. |

**Returns** If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

By default, PortSIP VoIP SDK supports the media types (mime types) listed in the below incoming SIP messages:

"message/sipfrag" in NOTIFY message. application/simple-message-summary" in NOTIFY message. "text/plain" in MESSAGE message. "application/dtmf-relay" in INFO message. "application/media\_control+xml" in INFO message.

The SDK allows to receive SIP messages that include above mime types. Now if remote side sends an INFO SIP message with its "Content-Type" header value "text/plain", SDK will reject this INFO message, for "text/plain" of INFO message is not included in the default supported list. How should we enable the SDK to receive the SIP INFO messages that include "text/plain" mime type? The answer is to use addSupportedMimyType:

`[myVoIPSdk addSupportedMimeType:@"INFO" mimeType:@"text" subMimeType:@"plain"];`

To receive the NOTIFY message with "application/media\_control+xml":

`[myVoIPSdk addSupportedMimeType:@"NOTIFY" mimeType:@"application" ]subMimeType:@"media\_control+xml"];`

For more details about the mime type, please visit: [http://www.iana.org/assignments/media-types/](http://www.iana.org/assignments/media-types/)

### Access SIP message header functions

```objc
-(NSString*)getSipMessageHeaderValue:(NSString*)sipMessage
                    headerName:(NSString*) headerName;
```

Access the SIP header of SIP message.

**Parameters**

| _sipMessage_ | The SIP message.                                 |
| ------------ | ------------------------------------------------ |
| _headerName_ | The header with which to access the SIP message. |

**Returns**

If the function succeeds, it will return value headerValue. If the function fails, it will return value null.

**Remarks**

When receiving an SIP message in the onReceivedSignaling callback event, and user wishes to get SIP message header value, use getExtensionHeaderValue:

`NSString* headerValue = [myVoIPSdk getSipMessageHeaderValue:message headerName:name];`

```objc
- (long)addSipMessageHeader:(long) sessionId
                methodName:(NSString*) methodName
                   msgType:(int) msgType
                headerName:(NSString*) headerName
               headerValue:(NSString*) headerValue;
```

Add the SIP Message header into the specified outgoing SIP message.

**Parameters**

| _sessionId_   | Add the header to the SIP message with the specified session Id only. By setting to -1, it will be added to all messages.                                           |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _methodName_  | Just add the header to the SIP message with specified method name. For example: "INVITE", "REGISTER", "INFO" etc. If "ALL" specified, it will add all SIP messages. |
| _msgType_     | 1 refers to apply to the request message, 2 refers to apply to the response message, 3 refers to apply to both request and response.                                |
| _headerName_  | The header name that will appear in SIP message.                                                                                                                    |
| _headerValue_ | The custom header value.                                                                                                                                            |

**Returns**

If the function succeeds, it will return addedSipMessageId, which is greater than 0. If the function fails, it will return a specific error code.

```objc
- (int)removeAddedSipMessageHeader:(long) addedSipMessageId;
```

Remove the headers (custom header) added by addSipMessageHeader.

**Parameters**

| _addedSipMessageI d_ | The addedSipMessageId return by addSipMessageHeader. |
| -------------------- | ---------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)clearAddedSipMessageHeaders;  
```

Clear the added extension headers (custom headers).

**Remarks**

For example, we have added two custom headers into every outgoing SIP message and wish to remove them.

```objc
  [myVoIPSdk addedSipMessageId:-1  methodName:@"ALL"  msgType:3 headerName:@"Blling" headerValue:@"usd100.00"]; 

  [myVoIPSdk addedSipMessageId:-1  methodName:@"ALL"  msgType:3 headerName:@"ServiceId" headerValue:@"8873456"]; 

  [myVoIPSdk clearAddedSipMessageHeaders]; 
```

```objc
- (long)modifySipMessageHeader:(long) sessionId
                methodName:(NSString*) methodName
                   msgType:(int) msgType
                headerName:(NSString*) headerName
               headerValue:(NSString*) headerValue;
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

```objc
- (int)removeModifiedSipMessageHeader:(long) modifiedSipMessageId;
```

Remove the extension header (custom header) from every outgoing SIP message.

**Parameters**

| _modifiedSipMessa geId_ | The modifiedSipMessageId is returned by modifySipMessageHeader. |
| ----------------------- | --------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)clearModifiedSipMessageHeaders;
```

Clear the modified headers value, and do not modify every outgoing SIP message header values any longer.

**Remarks**

For example, to modify two headers' value for every outging SIP message and wish to clear it:

```objc
  [myVoIPSdk removeModifiedSipMessageHeader:-1  methodName:@"ALL"  msgType:3 headerName:@"Expires" headerValue:@"1000"]; 

  [myVoIPSdk removeModifiedSipMessageHeader:-1  methodName:@"ALL"  msgType:3 headerName:@"User-Agent" headerValue:@"MyTest Softphone 1.0"]; 

  [myVoIPSdk clearModifiedSipMessageHeaders]; 
```

### Audio and video functions

```objc
- (void)clearModifiedSipMessageHeaders;
```

Set the video device that will be used for video call.

**Parameters**

| _deviceId_ | Device ID (index) for video device (camera). |
| ---------- | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoOrientation:(int) rotation;
```

Set the video Device Orientation.

**Parameters**

| _rotation_ | Device Orientation for video device (camera), e.g 0,90,180,270. |
| ---------- | --------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoResolution:(int)width
                   height:(int)height;
```

Set the video capturing resolution.

**Parameters**

| _width_  | Video width.  |
| -------- | ------------- |
| _height_ | Video height. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setAudioBitrate:(long) sessionId
             codecType:(AUDIOCODEC_TYPE)codecType
           bitrateKbps:(int)bitrateKbps;
```

Set the audio bit rate.

**Parameters**

| _sessionId_   | The session ID of the call. |
| ------------- | --------------------------- |
| _codecType_   | Audio codec type.           |
| _bitrateKbps_ | The Audio bit rate in KBPS. |
| -             | -                           |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoBitrate:(long) sessionId bitrateKbps:(int) bitrateKbps;
```

Set the video bitrate.

**Parameters**

| _sessionId_   | The session ID of the call. Set it to -1 for all calls. |
| ------------- | ------------------------------------------------------- |
| _bitrateKbps_ | The video bit rate in KBPS.                             |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoFrameRate:(long) sessionId frameRate:(int) frameRate;
```

Set the video frame rate.

**Parameters**

| _sessionId_ | The session ID of the call. Set it to -1 for all calls.                                                                                       |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| _frameRate_ | The frame rate value, with its minimum value 5, and maximum value 30. Greater value renders better video quality but requires more bandwidth. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Usually you do not need to call this function to set the frame rate, as the SDK uses default frame rate.

```objc
- (int)sendVideo:(long)sessionId
       sendState:(BOOL)sendState; 
```

Send the video to remote side.

**Parameters**

| _sessionId_ | The session ID of the call.                              |
| ----------- | -------------------------------------------------------- |
| _sendState_ | Set to true to send the video, or false to stop sending. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setRemoteVideoWindow:(long) sessionId
          remoteVideoWindow:(PortSIPVideoRenderView*) remoteVideoWindow;
```

Set the window for a session to display the received remote video image.

| _sessionId_          | The session ID of the call.                                            |
| -------------------- | ---------------------------------------------------------------------- |
| _remoteVideoWindo w_ | The PortSIPVideoRenderView for displaying received remote video image. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setRemoteScreenWindow:(long) sessionId
          remoteScreenWindow:(PortSIPVideoRenderView*) remoteScreenWindow;
```

Set the window for a session to display the received remote screen image.

**Parameters**

| _sessionId_           | The session ID of the call.                                             |
| --------------------- | ----------------------------------------------------------------------- |
| _remoteScreenWind ow_ | The PortSIPVideoRenderView for displaying received remote screen image. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)displayLocalVideo:(BOOL) state
                  mirror:(BOOL)mirror
        localVideoWindow:(PortSIPVideoRenderView*)localVideoWindow;
```

Start/stop displaying the local video image.

**Parameters**

| _state_            | Set to true to display local video image.                                |
| ------------------ | ------------------------------------------------------------------------ |
| _mirror_           | Set to true to display the mirror image of local video.                  |
| _localVideoWindow_ | The PortSIPVideoRenderView for displaying local video image from camera. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoNackStatus:(BOOL) state;
```

Enable/disable the NACK feature (RFC4585) to help to improve the video quality.

**Parameters**

| _state_ | Set to true to enable. |
| ------- | ---------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)muteMicrophone:(BOOL) mute;
```

Mute the device microphone. It's unavailable for Android and iOS.

| _mute_ | If the value is set to true, the microphone is muted, or set to false to be un-muted. |
| ------ | ------------------------------------------------------------------------------------- |

```objc
- (void)muteSpeaker:(BOOL) mute;
```

Mute the device speaker. It's unavailable for Android and iOS.

**Parameters**

| _mute_ | If the value is set to true, the speaker is muted, or set to false to be un-muted. |
| ------ | ---------------------------------------------------------------------------------- |

```objc
- (int)setAudioDeviceId:(int)inputDeviceId outputDeviceId:(int)outputDeviceId
```

Set the audio device that will be used for audio call.

**Parameters**

| _inputDeviceId_  | Device ID (index) for audio recording (Microphone). |
| ---------------- | --------------------------------------------------- |
| _outputDeviceId_ | evice ID (index) for audio playback (Speaker).      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Allow to switch between earphone and loudspeaker only.

```objc
- (int)setChannelOutputVolumeScaling:(long) sessionId
                              scaling:(int) scaling;
```

Set a volume |scaling| to be applied to the outgoing signal of a specific audio channel.

**Parameters**

| _sessionId_ | The session ID of the call.                    |
| ----------- | ---------------------------------------------- |
| _scaling_   | Valid scale ranges \[0, 1000]. Default is 100. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setChannelInputVolumeScaling:(long) sessionId
                             scaling:(int) scaling; 
```

Set a volume |scaling| to be applied to the microphone signal of a specific audio channel.

**Parameters**

| _sessionId_ | The session ID of the call.                    |
| ----------- | ---------------------------------------------- |
| _scaling_   | Valid scale ranges \[0, 1000]. Default is 100. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Call functions

```objc
- (long)call:(NSString*) callee
     sendSdp:(BOOL)sendSdp
   videoCall:(BOOL)videoCall;
```

Make a call.

**Parameters**

| _callee_    | The callee. It can be a name only or full SIP URI. For example, user001, sip[:user001@sip.iptel.org ](mailto:user001@sip.iptel.org)or sip[:user002@sip.yourdomain.com:](mailto:user002@sip.yourdomain.com)5068. |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _sendSdp_   | If set to false, the outgoing call will not include the SDP in INVITE message.                                                                                                                                  |
| _videoCall_ | If set to true and at least one video codec was added, the outgoing call will include the video codec into SDP.                                                                                                 |

**Returns**

If the function succeeds, it will return the session ID of the call, which is greater than 0. If the function fails, it will return a specific error code. Note: the function success just means the outgoing call is being processed, and you need to detect the final state of calling in onInviteTrying, onInviteRinging, onInviteFailure callback events.

```objc
- (int)rejectCall:(long)sessionId code:(int)code;  
```

rejectCall Reject the incoming call.

**Parameters**

| _sessionId_ | The session ID of the call.            |
| ----------- | -------------------------------------- |
| _code_      | Reject code. For example, 486 and 480. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)hangUp:(long)sessionId;
```

hangUp Hang up the call.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)answerCall:(long)sessionId videoCall:(BOOL)videoCall;
```

**Parameters**

| _sessionId_ | The session ID of the call.                                                                                                                                                                |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _videoCall_ | If the incoming call is a video call and the video codec is matched, set it to true to answer the video call. If set to false, the answer call will not include video codec answer anyway. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)updateCall:(long)sessionId
      enableAudio:(BOOL)enableAudio
      enableVideo:(BOOL)enableVideo;
```

Use the re-INVITE to update the established call.

**Parameters**

| _sessionId_   | The session ID of call.                                                                    |
| ------------- | ------------------------------------------------------------------------------------------ |
| _enableAudio_ | Set to true to allow the audio in updated call, or false to disable audio in updated call. |
| _enableVideo_ | Set to true to allow the video in updated call, or false to disable video in updated call. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Example usage:

Example 1: A called B with the audio only, and B answered A, then there would be an audio conversation between A and B. Now if A wants to see B visually, A could use these functions to fulfill it.

```objc
  [myVoIPSdk clearVideoCodec]; 
  [myVoIPSdk addVideoCodec:VIDEOCODEC\_H264]; 
  [myVoIPSdk updateCall:sessionId enableAudio:true enableVideo:true]; 
```

Example 2: Remove video stream from current conversation.

```objc
  [myVoIPSdk updateCall:sessionId enableAudio:true enableVideo:false]; 
```

```objc
- (int)hold:(long)sessionId;
```

Place a call on hold.

**Parameters**

| _sessionId_ | The session ID of the call. |
| ----------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)unHold:(long)sessionId;
```

Take off hold.

**Parameters**

| _sessionId_ | The session ID of call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)muteSession:(long)sessionId
 muteIncomingAudio:(BOOL)muteIncomingAudio
 muteOutgoingAudio:(BOOL)muteOutgoingAudio
 muteIncomingVideo:(BOOL)muteIncomingVideo
 muteOutgoingVideo:(BOOL)muteOutgoingVideo;
```

Mute the specified session audio or video.

**Parameters**

| _sessionId_          | The session ID of the call.                                                             |
| -------------------- | --------------------------------------------------------------------------------------- |
| _muteIncomingAudi o_ | Set it true to mute incoming audio stream, and user cannot hear from remote side audio. |
| _muteOutgoingAudi o_ | Set it true to mute outgoing audio stream, and the remote side cannot hear the audio.   |
| _muteIncomingVide o_ | Set it true to mute incoming video stream, and user cannot see remote side video.       |
| _muteOutgoingVide o_ | Set it true to mute outgoing video stream, and the remote side cannot see video.        |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)forwardCall:(long)sessionId forwardTo:(NSString*) forwardTo;
```

Forward the call to another user once received an incoming call.

**Parameters**

| _sessionId_ | The session ID of the call.                                                           |
| ----------- | ------------------------------------------------------------------------------------- |
| _forwardTo_ | Target of the call forwarding. It can be "sip:number@sipserver.com" or "number" only. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (long)pickupBLFCall:(const char * )replaceDialogId videoCall:(BOOL)videoCall;
```

This function will be used for picking up a call based on the BLF (Busy Lamp Field) status.

**Parameters**

| _replaceDialogId_ | The ID of the call to be picked up. It comes with onDialogStateUpdated callback. |
| ----------------- | -------------------------------------------------------------------------------- |
| _videoCall_       | Indicates if it is video call or audio call to be picked up.                     |

**Returns**

If the function succeeds, it will return a session ID that is greater than 0 to the new call, otherwise returns a specific error code that is less than 0.

**Remarks**

The scenario is:

1. User 101 subscribed the user 100's call status: sendSubscription("100", "dialog");
2. When 100 hold a call or 100 is ringing, onDialogStateUpdated callback will be triggered, and 101 will receive this callback. Now 101 can use pickupBLFCall function to pick the call rather than 100 to talk with caller.

```objc
- (int)sendDtmf:(long) sessionId
     dtmfMethod:(DTMF_METHOD) dtmfMethod
           code:(int) code
    dtmfDration:(int) dtmfDuration
   playDtmfTone:(BOOL) playDtmfTone;
```

Send DTMF tone.

**Parameters**

| _sessionId_  | The session ID of the call.                                                                                 |   |   |
| ------------ | ----------------------------------------------------------------------------------------------------------- | - | - |
| _dtmfMethod_ | Support sending DTMF tone with two methods: DTMF\_RFC2833 and DTMF\_INFO. The DTMF\_RFC2833 is recommended. |   |   |
| _code_       | The DTMF tone (0-16).                                                                                       |   |   |
| code         | Description                                                                                                 |   |   |
| 0            | The DTMF tone 0.                                                                                            |   |   |
| 1            | The DTMF tone 1.                                                                                            |   |   |
| 2            | The DTMF tone 2.                                                                                            |   |   |
| 3            | The DTMF tone 3.                                                                                            |   |   |
| 4            | The DTMF tone 4.                                                                                            |   |   |
| 5            | The DTMF tone 5.                                                                                            |   |   |
| 6            | The DTMF tone 6.                                                                                            |   |   |
| 7            | The DTMF tone 7.                                                                                            |   |   |
| 8            | The DTMF tone 8.                                                                                            |   |   |
| 9            | The DTMF tone 9.                                                                                            |   |   |
| 10           | The DTMF tone \*.                                                                                           |   |   |
| 11           | The DTMF tone #.                                                                                            |   |   |
| 12           | The DTMF tone A.                                                                                            |   |   |
| 13           | The DTMF tone B.                                                                                            |   |   |
| 14           | The DTMF tone C.                                                                                            |   |   |
| 15           | The DTMF tone D.                                                                                            |   |   |
| 16           | The DTMF tone FLASH.                                                                                        |   |   |

**Parameters**

| _dtmfDuration_ | The DTMF tone samples. Recommended value 160.                              |
| -------------- | -------------------------------------------------------------------------- |
| _playDtmfTone_ | By setting to true, the SDK plays local DTMF tone sound when sending DTMF. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Refer functions

```objc
- (int)refer:(long)sessionId referTo:(NSString*)referTo;
```

Refer the current call to another one.

**Parameters**

| _sessionId_ | The session ID of the call.                                                     |
| ----------- | ------------------------------------------------------------------------------- |
| _referTo_   | Target of the refer. It could be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

`[myVoIPSdk refer:sessionId referTo:@"sip:testuser12@sip.portsip.com"];`

You can watch the video on YouTube at[ https://www.youtube.com/watch?v=\_2w9EGgr3FY.](https://www.youtube.com/watch?v=_2w9EGgr3FY) It will demonstrate the transfer.

```objc
- (int)attendedRefer:(long)sessionId
    replaceSessionId:(long)replaceSessionId
             referTo:(NSString*)  referTo;
```

Make an attended refer.

**Parameters**

| _sessionId_        | The session ID of the call.                                                   |
| ------------------ | ----------------------------------------------------------------------------- |
| _replaceSessionId_ | Session ID of the replaced call.                                              |
| _referTo_          | Target of the refer. It can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Please read the sample project source code for more details, or you can watch the video on YouTube at[ https://www.youtube.com/watch?v=NezhIZW4lV4,](https://www.youtube.com/watch?v=NezhIZW4lV4) which will demonstrate the transfer.

```objc
- (int)attendedRefer2:(long)sessionId
     replaceSessionId:(long)replaceSessionId
        replaceMethod:(NSString*)replaceMethod
              target:(NSString*)target
        referTo:(NSString*)referTo;
```

Make an attended refer with specified request line and specified method embedded into the "Refer-To" header.

**Parameters**

| _sessionId_        | Session ID of the call.                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------------------------- |
| _replaceSessionId_ | Session ID of the replaced call.                                                                                    |
| _replaceMethod_    | The SIP method name which will be embedded in the "Refer-To" header, usually INVITE or BYE.                         |
| _target_           | The target to which the REFER message will be sent. It appears in the "Request Line" of REFER message.              |
| _referTo_          | Target of the refer that appears in the "Refer-To" header. It can be either "sip:number@sipserver.com" or "number". |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Please read the sample project source code for more details, or you can watch the video on YouTube at[ https://www.youtube.com/watch?v=NezhIZW4lV4,](https://www.youtube.com/watch?v=NezhIZW4lV4) which will demonstrate the transfer.

```objc
- (int)outOfDialogRefer:(long)replaceSessionId
          replaceMethod:(NSString*)replaceMethod
                 target:(NSString*)target
                referTo:(NSString*)referTo;
```

Send an out of dialog REFER to replace the specified call.

**Parameters**

| _replaceSessionId_ | The session ID of the session which will be replaced.                                    |
| ------------------ | ---------------------------------------------------------------------------------------- |
| _replaceMethod_    | The SIP method name which will be added in the "Refer-To" header, usually INVITE or BYE. |
| _target_           | The target to which the REFER message will be sent.                                      |
| _referTo_          | The URI to be added into the "Refer-To" header.                                          |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (long)acceptRefer:(long)referId referSignaling:(NSString*) referSignaling;
```

Once the REFER request accepted, a new call will be made if called this function. The function is usually called after onReceivedRefer callback event.

**Parameters**

| _referId_        | The ID of REFER request that comes from onReceivedRefer callback event.          |
| ---------------- | -------------------------------------------------------------------------------- |
| _referSignaling_ | The SIP message of REFER request that comes from onReceivedRefer callback event. |

**Returns**

If the function succeeds, it will return a session ID that is greater than 0 to the new call for REFER, otherwise returns a specific error code that is less than 0.

```objc
- (int)rejectRefer:(long)referId;
```

Reject the REFER request.

**Parameters**

| _referId_ | The ID of REFER request that comes from onReceivedRefer callback event. |
| --------- | ----------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Send audio and video stream functions

```objc
- (int)enableSendPcmStreamToRemote:(long)sessionId state:(BOOL)state streamSamplesPerSec:(int)streamSamplesPerSec;
```

Enable the SDK to send PCM stream data to remote side from another source instead of microphone.

**Parameters**

| _sessionId_            | The session ID of call.                                            |
| ---------------------- | ------------------------------------------------------------------ |
| _state_                | Set to true to enable the sending stream, or false to disable.     |
| _streamSamplesPer Sec_ | The PCM stream data sample in seconds. For example: 8000 or 16000. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

To send the PCM stream data to another side, this function MUST be called first.

```objc
- (int)sendPcmStreamToRemote:(long)sessionId data:(NSData*) data;
```

Send the audio stream in PCM format from another source instead of audio device capturing (microphone).

**Parameters**

| _sessionId_ | Session ID of the call conversation.                  |
| ----------- | ----------------------------------------------------- |
| _data_      | The PCM audio stream data. It must be in 16bit, mono. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Usually we should use it like below:

```objc
  [myVoIPSdk enableSendPcmStreamToRemote:sessionId state:YES streamSamplesPerSec:16000]; 
  [myVoIPSdk sendPcmStreamToRemote:sessionId data:data]; 
```

You can't have too much audio data at one time as we have 100ms audio buffer only. Once you put too much, data will be lost. It is recommended to send 20ms audio data every 20ms.

```objc
- (int)enableSendVideoStreamToRemote:(long)sessionId state:(BOOL)state;
```

Enable the SDK to send video stream data to remote side from another source instead of camera.

**Parameters**

| _sessionId_ | The session ID of call.                                        |
| ----------- | -------------------------------------------------------------- |
| _state_     | Set to true to enable the sending stream, or false to disable. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)sendVideoStreamToRemote:(long)sessionId data:(NSData*) data width:(int)width height:(int)height;
```

Send the video stream to remote side.

**Parameters**

| _sessionId_ | Session ID of the call conversation.              |
| ----------- | ------------------------------------------------- |
| _data_      | The video stream data. It must be in i420 format. |
| _width_     | The video image width.                            |
| _height_    | The video image height.                           |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Send the video stream in i420 from another source instead of video device capturing (camera).

Before calling this function, you MUST call the enableSendVideoStreamToRemote function. Usually we should use it like below:

```objc
  [myVoIPSdk enableSendVideoStreamToRemote:sessionId state:YES]; 
  [myVoIPSdk sendVideoStreamToRemote:sessionId data:data width:352 height:288];
```

### RTP packets, audio stream and video stream callback functions

```objc
- (int)enableRtpCallback:(long) sessionId
               mediaType:(int)mediaType
                    mode:(DIRECTION_MODE)mode;
```

Set the RTP callbacks to allow to access the sent and received RTP packets.

**Parameters**

| _sessionId_           | The session ID of call.                                                        |   |   |
| --------------------- | ------------------------------------------------------------------------------ | - | - |
| _mediaType_           | 0 -audo 1-video 2-screen.                                                      |   |   |
| _mode_                | The RTP stream callback mode.                                                  |   |   |
| Type                  | Description                                                                    |   |   |
| DIRECTION\_SEND       | Callback the send RTP stream for one channel based on the given sessionId.     |   |   |
| DIRECTION\_RECV       | Callback the received RTP stream for one channel based on the given sessionId. |   |   |
| DIRECTION\_SEND\_RECV | Callback both local and remote RTP stream on the given sessionId.              |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)enableAudioStreamCallback:(long) sessionId  enable:(BOOL)enable callbackMode:(DIRECTION_MODE)callbackMode;
```

Enable/disable the audio stream callback.

**Parameters**

| _sessionId_           | The session ID of call.                                                                 |   |   |
| --------------------- | --------------------------------------------------------------------------------------- | - | - |
| _enable_              | Set to true to enable audio stream callback, or false to stop the callback.             |   |   |
| _callbackMode_        | The audio stream callback mode.                                                         |   |   |
| Type                  | Description                                                                             |   |   |
| DIRECTION\_SEND       | Callback the audio stream from microphone for one channel based on the given sessionId. |   |   |
| DIRECTION\_RECV       | Callback the received audio stream for one channel based on the given sessionId.        |   |   |
| DIRECTION\_SEND\_RECV | Callback both local and remote audio stream on the given sessionId.                     |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

onAudioRawCallback event will be triggered if the callback is enabled.

```objc
- (int)enableVideoStreamCallback:(long) sessionId callbackMode:(DIRECTION_MODE) callbackMode;
```

Enable/disable the video stream callback.

**Parameters**

| _sessionId_           | The session ID of call.                         |   |   |
| --------------------- | ----------------------------------------------- | - | - |
| _callbackMode_        | The video stream callback mode.                 |   |   |
| Mode                  | Description                                     |   |   |
| DIRECTION\_INACTIVE   | Disable video stream callback.                  |   |   |
| DIRECTION\_SEND       | Local video stream callback.                    |   |   |
| DIRECTION\_RECV       | Remote video stream callback.                   |   |   |
| DIRECTION\_SEND\_RECV | Both of local and remote video stream callback. |   |   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

The onVideoRawCallback event will be triggered if the callback is enabled.

### Record functions

```objc
- (int)startRecord:(long)sessionId
    recordFilePath:(NSString*) recordFilePath
    recordFileName:(NSString*) recordFileName
   appendTimeStamp:(BOOL)appendTimeStamp
          channels:(int)channels
        fileFormat:(FILE_FORMAT) recordFileFormat
   audioRecordMode:(RECORD_MODE) audioRecordMode
   videoRecordMode:(RECORD_MODE) videoRecordMode;
```

Start recording the call.

**Parameters**

| _sessionId_        | The session ID of call conversation.                                             |
| ------------------ | -------------------------------------------------------------------------------- |
| _recordFilePath_   | The filepath to which the recording will be saved. It must be existent.          |
| _recordFileName_   | The filename of the recording. For example audiorecord.wav or videorecord.avi.   |
| _appendTimeStamp_  | Set to true to append the timestamp to the filename of the recording.            |
| _channels_         | Set to record file audio channels, 1 - mono 2 - stereo.                          |
| _recordFileFormat_ | The file format for the recording.                                               |
| _audioRecordMode_  | Allow to set audio recording mode. Support to record received and/or sent audio. |
| _videoRecordMode_  | Allow to set video recording mode. Support to record received and/or sent video. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)stopRecord:(long)sessionId;
```

Stop recording.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Play audio and video files to remote party

```objc
- (int) startPlayingFileToRemote:(long)sessionId fileUrl:(NSString*) fileUrl loop:(BOOL)loop playAudio:(int)playAudio;
```

Play a file to remote party.

**Parameters**

| _sessionId_ | Session ID of the call.                                                                        |
| ----------- | ---------------------------------------------------------------------------------------------- |
| _fileUrl_   | url or file name, such as "/test.mp4","/test.wav",.                                            |
| _loop_      | Set to false to stop playing video file when it is ended, or true to play it repeatedly.       |
| _playAudio_ | If it is set to 0 - Not play file audio. 1 - Play file audio, 2 - Play file audio mix with Mic |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)stopPlayingFileToRemote:(long)sessionId;
```

Stop playing file to remote party.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int) startPlayingFileLocally:(NSString*) fileUrl
                           loop:(BOOL)loop
                playVideoWindow:(PortSIPVideoRenderView*) playVideoWindow;
```

Play a file to locally.

**Parameters**

| _fileUrl_         | url or file name, such as "/test.mp4","/test.wav",.                                      |
| ----------------- | ---------------------------------------------------------------------------------------- |
| _loop_            | Set to false to stop playing video file when it is ended, or true to play it repeatedly. |
| _playVideoWindow_ | The PortSIPVideoRenderView used for displaying the video.                                |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)stopPlayingFileLocally;
```

Stop playing file to locally. **Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)audioPlayLoopbackTest:(BOOL)enable;
```

Used for the loop back testing against audio device.

**Parameters**

| _enable_ | Set to true to start audio look back test, or false to stop. |
| -------- | ------------------------------------------------------------ |

### Conference functions

```objc
- (int)createAudioConference;
```

Create an audio conference.

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)createVideoConference:(PortSIPVideoRenderView*) conferenceVideoWindow
                  videoWidth:(int) videoWidth
                 videoHeight:(int) videoHeight
                      layout:(int) layout;
```

Create a video conference.

**Parameters**

| _conferenceVideoW indow_ | The PortSIPVideoRenderView used for displaying the conference video.                                                                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _videoWidth_             | The conference video width.                                                                                                                                                                                                                |
| _videoHeight_            | The conference video height.                                                                                                                                                                                                               |
| -                        | -                                                                                                                                                                                                                                          |
| _layout_                 | <p>Conference Video layout, default is 0 - Adaptive.<br>0 - Adaptive(1,3,5,6)<br>1 - Only Local Video<br>2 - 2 video,PIP<br>3 - 2 video, Left and right<br>4 - 2 video, Up and Down<br>5 - 3 video<br>6 - 4 split video<br>7 - 5 video</p> |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (void)destroyConference;
```

Destroy the existent conference.

```objc
- (int)setConferenceVideoWindow:(PortSIPVideoRenderView*) conferenceVideoWindow;
```

Set the window for a conference that is used to display the received remote video image.

**Parameters**

| _conferenceVideoW indow_ | The PortSIPVideoRenderView used to display the conference video. |
| ------------------------ | ---------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)joinToConference:(long)sessionId;
```

Join a session into existent conference. If the call is in hold, please un-hold first.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)removeFromConference:(long)sessionId;
```

Remove a session from an existent conference.

**Parameters**

| _sessionId_ | Session ID of the call. |
| ----------- | ----------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### RTP and RTCP QOS functions

```objc
- (int)setAudioRtcpBandwidth:(long)sessionId
                      BitsRR:(int)BitsRR
                      BitsRS:(int)BitsRS
                     KBitsAS:(int)KBitsAS;
```

Set the audio RTCP bandwidth parameters as the RFC3556.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |
| _BitsRR_    | The bits for the RR parameter.       |
| _BitsRS_    | The bits for the RS parameter.       |
| _KBitsAS_   | The Kbits for the AS parameter.      |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoRtcpBandwidth:(long)sessionId
                      BitsRR:(int)BitsRR
                      BitsRS:(int)BitsRS
                     KBitsAS:(int)KBitsAS;
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

```objc
- (int)enableAudioQos:(BOOL)state;
```

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for audio channel.

**Parameters**

| _state_ | Set to YES to enable audio QoS, and DSCP value will be 46; or NO to disable audio QoS, and DSCP value will be 0. |
| ------- | ---------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)enableVideoQos:(BOOL)state;
```

Set the DSCP (differentiated services code point) value of QoS (Quality of Service) for video channel.

**Parameters**

| _state_ | Set to YES to enable video QoS and DSCP value will be 34; or NO to disable video QoS, and DSCP value will be 0. |
| ------- | --------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setVideoMTU:(int)mtu;
```

Set the MTU size for video RTP packet.

**Parameters**

| _mtu_ | Set MTU value. Allowed value ranges 512-65507. Other values will be automatically changed to the default 1400. |
| ----- | -------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Media statistics functions

```objc
- (int)getStatistics:(long)sessionId;
```

Obtain the statistics of channel. the event onStatistics will be triggered.

**Parameters**

| _sessionId_ | The session ID of call conversation. |
| ----------- | ------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

### Audio effect functions

```objc
- (void)enableVAD:(BOOL)state;
```

Enable/disable Voice Activity Detection (VAD).

**Parameters**

| _state_ | Set to true to enable VAD, or false to disable it. |
| ------- | -------------------------------------------------- |

```objc
- (void)enableCNG:(BOOL)state;
```

Enable/disable Comfort Noise Generator (CNG).

**Parameters**

| _state_ | Set to true to enable CNG, or false to disable. |
| ------- | ----------------------------------------------- |

### Send OPTIONS/INFO/MESSAGE functions

```objc
- (int)sendOptions:(NSString*)to sdp:(NSString*) sdp;
```

Send OPTIONS message.

**Parameters**

| _to_  | The recipient of OPTIONS message.                                                                     |
| ----- | ----------------------------------------------------------------------------------------------------- |
| _sdp_ | The SDP of OPTIONS message. It's optional if user does not wish to send the SDP with OPTIONS message. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)sendInfo:(long)sessionId
       mimeType:(NSString*) mimeType
    subMimeType:(NSString*) subMimeType
   infoContents:(NSString*) infoContents; 
```

Send an INFO message to remote side in dialog.

**Parameters**

| _sessionId_    | The session ID of call.                    |
| -------------- | ------------------------------------------ |
| _mimeType_     | The mime type of INFO message.             |
| _subMimeType_  | The sub mime type of INFO message.         |
| _infoContents_ | The contents to be sent with INFO message. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (long)sendMessage:(long)sessionId
           mimeType:(NSString*) mimeType
        subMimeType:(NSString*)subMimeType
            message:(NSData*) message
      messageLength:(int)messageLength;
```

Send a MESSAGE message to remote side in dialog.

**Parameters**

| _sessionId_     | The session ID of the call.                                        |
| --------------- | ------------------------------------------------------------------ |
| _mimeType_      | The mime type of MESSAGE message.                                  |
| _subMimeType_   | The sub mime type of MESSAGE message.                              |
| _message_       | The contents to be sent with MESSAGE message. Binary data allowed. |
| _messageLength_ | The message size.                                                  |

**Returns**

If the function succeeds, it will return a message ID that allows to track the message sending state in onSendMessageSuccess and onSendMessageFailure. If the function fails, it will return a specific error code less than 0.

**Remarks**

Example 1: Send a plain text message. Note: to send other languages text, please use the UTF-8 to encode the message before sending.

`[myVoIPsdk sendMessage:sessionId mimeType:@"text" subMimeType:@"plain" message:data messageLength:dataLen];`

Example 2: Send a binary message.

`[myVoIPsdk sendMessage:sessionId mimeType:@"application" subMimeType:@"vnd.3gpp.sms" message:data messageLength:dataLen];`

```objc
- (long)sendOutOfDialogMessage:(NSString*) to
                      mimeType:(NSString*) mimeType
                   subMimeType:(NSString*)subMimeType
                         isSMS:(BOOL)isSMS
                       message:(NSData*) message
                 messageLength:(int)messageLength;
```

Send an out of dialog MESSAGE message to remote side.

**Parameters**

| _to_            | The message recipient, such as sip[:receiver@portsip.com.](mailto:receiver@portsip.com)                                      |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| _mimeType_      | The mime type of MESSAGE message.                                                                                            |
| _subMimeType_   | The sub mime type of MESSAGE message. @isSMS isSMS Set to YES to specify "messagetype=SMS" in the To line, or NO to disable. |
| _message_       | The contents sent with MESSAGE message. Binary data allowed.                                                                 |
| _messageLength_ | The message size.                                                                                                            |

**Returns**

If the function succeeds, it will return a message ID that allows to track the message sending state in onSendOutOfMessageSuccess and onSendOutOfMessageFailure. If the function fails, it will return a specific error code less than 0.

**Remarks**

Example 1: Send a plain text message. Note: to send text in other languages, please use UTF-8 to encode the message before sending.

`[myVoIPsdk sendOutOfDialogMessage:@"sip:user1@sip.portsip.com" mimeType:@"text" subMimeType:@"plain" message:data messageLength:dataLen];`

Example 2: Send a binary message.

`[myVoIPsdk sendOutOfDialogMessage:@"sip:user1@sip.portsip.com" mimeType:@"application" subMimeType:@"vnd.3gpp.sms" isSMS:NO message:data messageLength:dataLen];`

**Presence functions**

```objc
- (int)setPresenceMode:(PRESENCE_MODES) mode;
```

Indicate that SDK uses the P2P mode for presence or presence agent mode.

**Parameters**

| _mode_ | 0 - P2P mode; 1 - Presence Agent mode, default is P2P mode. |
| ------ | ----------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Since presence agent mode requires the PBX/Server support the PUBLISH, please ensure you have your server and PortSIP PBX support this feature. For more details please visit: [https://www.portsip.com/portsip-pbx](https://www.portsip.com/portsip-pbx)

```objc
 (int)setDefaultSubscriptionTime:(int) secs;
```

Set the default expiration time to be used when creating a subscription.

**Parameters**

| _secs_ | The default expiration time of subscription. |
| ------ | -------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setDefaultPublicationTime:(int) secs;
```

Set the default expiration time to be used when creating a publication.

**Parameters**

| _secs_ | The default expiration time of publication. |
| ------ | ------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (long)presenceSubscribe:(NSString*) contact
                  subject:(NSString*) subject;
```

Send a SUBSCRIBE message for subscribing the contact's presence status.

**Parameters**

| _contact_ | The target contact. It must be like sip[:contact001@sip.portsip.com.](mailto:contact001@sip.portsip.com)                                                                           |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _subject_ | <p>This subject text will be inserted into the SUBSCRIBE message. For example: "Hello, I'm Jason".</p><p>The subject maybe in UTF-8 format. You should use UTF-8 to decode it.</p> |

**Returns**

If the function succeeds, it will return subscribeId. If the function fails, it will return a specific error code.

```objc
- (int)presenceTerminateSubscribe:(long) subscribeId;
```

Terminate the given presence subscription.

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)presenceAcceptSubscribe:(long)subscribeId;
```

Accept the presence SUBSCRIBE request which is received from contact.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event will include the subscription ID. |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

If the P2P presence mode is enabled, when someone subscribes your presence status, you will receive the subscription request in the callback, and you can use this function to reject it.

```objc
- (int)presenceRejectSubscribe:(long)subscribeId;
```

Reject a presence SUBSCRIBE request which is received from contact.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe will be triggered. The event includes the subscription ID. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

If the P2P presence mode is enabled, when someone subscribe your presence status, you will receive the subscribe request in the callback, and you can use this function to accept it.

```objc
- (int)setPresenceStatus:(long)subscribeId
              statusText:(NSString*) statusText;
```

Send a NOTIFY message to contact to notify that presence status is online/offline/changed.

**Parameters**

| _subscribeId_ | Subscription ID. When receiving a SUBSCRIBE request from contact, the event onPresenceRecvSubscribe that includes the Subscription ID will be triggered. |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _statusText_  | The state text of presence status. For example: "I'm here", offline must use "offline"                                                                   |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return value a specific error code.

```objc
- (long)sendSubscription:(NSString*) to
               eventName:(NSString*) eventName;
```

Send a SUBSCRIBE message to remote side.

**Parameters**

| _to_        | The subscribe user.              |
| ----------- | -------------------------------- |
| _eventName_ | The event name to be subscribed. |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

Example 1, to subscribe the MWI (Message Waiting notifications), You can use this code: long mwiSubId = sendSubscription("sip:101@test.com", "message-summary");

Example 2, to monitor a user/extension call status, You can use code: sendSubscription("100", "dialog"); Extension 100 is the one to be monitored. Once being monitored, when extension 100 hold a call or is ringing, the onDialogStateUpdated callback will be triggered.

```objc
- (int)terminateSubscription:(long)subscribeId;
```

Terminate the given subscription.

**Parameters**

| _subscribeId_ | The ID of the subscription. |
| ------------- | --------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

**Remarks**

For example, if you want stop check the MWI, use below code:

'terminateSubscription(mwiSubId); '

### Device Manage functions.

```objc
- (int)getNumOfVideoCaptureDevices;
```

Gets the number of available capturing devices. **Returns**

The return value is the count of video capturing devices. If fails, it will return a specific error code less than 0.

```objc
- (int)getVideoCaptureDeviceName:(int) index
                        uniqueId:(NSString**)uniqueIdUTF8
                      deviceName:(NSString**) deviceNameUTF8;
```

Gets the name of a specific video capturing device given by an index. **Parameters**

| _index_          | Device index (0, 1, 2, ..., N-1), of which N is given by getNumOfVideoCaptureDevices (). Also -1 is a valid value and will return the name of the default capturing device. |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _uniqueIdUTF8_   | Unique identifier of the capturing device.                                                                                                                                  |
| _deviceNameUTF8_ | A character buffer to which the device name will be copied as a null-terminated string in UTF-8 format.                                                                     |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)getNumOfRecordingDevices;
```

Gets the number of audio devices available for audio recording. **Returns**

The return value is the count of recording devices. If the function fails, it will return a specific error code less than 0.

```objc
- (int)getNumOfPlayoutDevices;
```

Gets the number of audio devices available for audio playout.

**Returns**

The return value is the count of playout devices. If the function fails, it will return a specific error code less than 0.

```objc
- (NSString*)getRecordingDeviceName:(int) index;
```

Get the name of a specific recording device given by an index. **Parameters**

| _index_ | Device index (0, 1, 2, ..., N-1), of which N is given by getNumOfRecordingDevices (). Also -1 is a valid value and will return the name of the default recording device. |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

**Returns**

A NSString to which the device name will be copied as a null-terminated string in UTF-8 format.

```objc
- (NSString*)getPlayoutDeviceName:(int) index;
```

Get the name of a specific playout device given by an index.

**Parameters**

| _index_ | Device index (0, 1, 2, ..., N-1), of which N is given by getNumOfPlayoutDevices (). Also -1 is a valid value and will return the name of the default playout device. |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

A NSString to which the device name will be copied as a null-terminated string in UTF8 format.

```objc
- (int)setSpeakerVolume:(int) volume;
```

Set the speaker volume level.

**Parameters**

| _volume_ | Volume of speaker. Valid value ranges 0 - 255. |
| -------- | ---------------------------------------------- |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)getSpeakerVolume;
```

Gets the speaker volume.

**Returns**

If the function succeeds, it will return the value of speaker volume that ranges 0 - 255. If the function fails, it will return a specific error code.

```objc
- (int)setMicVolume:(int) volume;
```

Sets the microphone volume level.

**Parameters**

| _volume_ | The microphone volume. The valid value ranges 0 - 255. |
| -------- | ------------------------------------------------------ |

**Returns**

If the function succeeds, it will return value 0. If the function fails, it will return a specific error code.

```objc
- (int)getMicVolume;
```

Retrieves the current microphone volume.

**Returns**

If the function succeeds, it will return the value of microphone volume. If the function fails, it will return a specific error code.

```objc
- (int)getScreenSourceCount;
```

Retrieves the current number of screen.

**Returns**

If the function succeeds, it will return the screen number. If the function fails, it will return a specific error code.

```objc
- (NSString*)getScreenSourceTitle:(int) index;
```

Retrieves the current screen title .

**Parameters**

| _index_ | Device index (0, 1, 2, ..., N-1), of which N is given by getScreenSourceCount (). |
| ------- | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

```objc
- (int)selectScreenSource:(int) index;
```

Sets the Screen to share .

**Parameters**

| _index_ | Device index (0, 1, 2, ..., N-1), of which N is given by getScreenSourceCount (). |
| ------- | --------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

```objc
- (int)setScreenFrameRate:(int) frameRate;
```

Sets the Screen video framerate .

**Parameters**

| _frameRate_ | The frame rate value, with its minimum value 5, and maximum value 30. Greater value renders better video quality but requires more bandwidth. |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------- |

**Returns**

If the function succeeds, return value 0. If the function fails, it will return a specific error code.

### PortSIPVideoRenderView

```objc
- (void) updateVideoRenderFrame: (CGRect)  frameRect
```

Change the Video Render size.

**Remarks**

Example:

```objc
NSRect rect = videoRenderView.frame; 
rect.size.width += 20; 
rect.size.height += 20;

videoRenderView.frame = rect; 
[videoRenderView setNeedsDisplay:YES]; 

NSRect renderRect = [videoRenderView bounds]; 
[videoRenderView updateVideoRenderFrame:renderRect]; 
```
