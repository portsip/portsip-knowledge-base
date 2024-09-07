# How Do Push Notifications Work with PortSIP PBX?

A Voice over Internet Protocol (VoIP) app lets users make and receive phone calls by using an Internet connection instead of the device’s cellular service. Since the VoIP app relies heavily on the network, it’s not surprising that making calls usually results in high power consumption.

A VoIP app has to maintain a persistent network connection with a server to receive incoming calls and other data. This means writing complex code that sends periodic messages back and forth between the app and server to keep the connection alive, even when the app is not in use. This technique resulted in frequent device wakes that wasted energy. It also means if a user exits the VoIP app, calls from the server could no longer be received.

With push notifications, a VoIP app doesn't require to be running in the foreground or background. Whenever a push notification is received, the VoIP app could display an alert, and provide an option to accept or reject the call.

There are many advantages to using push notifications:

* The device is woken only when a push notification occurs, saving energy.
* Push notifications are regarded as high-priority notifications and are delivered without delay.
* Usually, VoIP pushes can include more data than what is provided with standard push notifications.
* Your app is automatically relaunched if it’s not running when a VoIP push is received.
* Your app will be given runtime to process a push message, even if your app is operating in the background or was forced out.

PortSIP PBX supports mobile push notifications for iOS and Android from V9.0. All PortSIP users will benefit from this new feature.

## **The Architecture of PortSIP PBX Push Notifications**

The below architecture demonstrates how PortSIP PBX provides push notifications service.

<figure><img src="../../.gitbook/assets/pbx_16_push_arch.png" alt=""><figcaption></figcaption></figure>

## **Call Flows for Sending Push Notifications**

PortSIP uses Apple Push Notification Service (APNs) to send push notifications to iOS devices and Google Firebase Cloud Messaging (FCM) to send push notifications to Android devices.

1. **Requesting Device Token**: The app requests a device token from APNs/Google FCM.
2. **Receiving Device Token**: The app receives the token, which functions as the address to send a push notification to.
3. **Registering with PortSIP PBX**: The app registers with PortSIP PBX using a SIP REGISTER message. An “X-Push” header must be added to the REGISTER message, including the device token and other necessary information to inform PortSIP PBX that the app would like to receive push notifications for this extension (e.g., extension 101).
4. **App Termination**: The user force terminates the app.
5. **Incoming Call**: When someone calls extension 101, PortSIP PBX will send an INVITE message to the appropriate contact address if the 101 registration is not expired and sends a push notification with the device token to APNs/FCM.
6. **Push Notification**: Since the app is terminated, the INVITE message will not be received by the app. APNs/FCM will send a push notification to the device.
7. **Receiving Notification**: The iOS/Android device receives the push notification and alerts the user that a call is coming. The user accepts the call.
8. **App Launch and Registration**: The app will be launched and registered to PortSIP PBX automatically.
9. **Call Handling**: After successful registration, PBX will send an INVITE message to the contact address obtained from the new registration. The app receives the INVITE message and answers the call.

The same process applies to Instant Messaging.

The example of the "**X-Push**" header in the REGISTER message.

```
X-Push:device-os=ios;device-uid=fe1b7bef1b4dc68dbfcd18143c8c06c122a25658c7f1d381c33fa626c9ed;allow-call-push=false;allow-message-push=false;app-id=com.portsip.portgo
```



