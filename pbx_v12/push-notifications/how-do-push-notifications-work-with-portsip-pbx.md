# How do push notifications work with PortSIP PBX?

A Voice over Internet Protocol (VoIP) app lets users make and receive phone calls by using an Internet connection instead of the device’s cellular service. Since the VoIP app relies heavily on the network, it’s not surprising that making calls usually results in high power consumption.

A VoIP app has to maintain a persistent network connection with a server to receive incoming calls and other data. This means writing complex code that sends periodic messages back and forth between the app and server to keep the connection alive, even when the app is not in use. This technique resulted in frequent device wakes that wasted energy. It also means if a user exits the VoIP app, calls from the server could no longer be received.

With push notifications, a VoIP app doesn't require to be running in the foreground or background. Whenever a push notification is received, the VoIP app could display an alert, and provide an option to accept or reject the call.

There are many advantages to use push notifications:

* The device is woken only when push notification occurs, saving energy.
* Push notifications are regarded as high-priority notifications and are delivered without delay.
* Usually, VoIP pushes can include more data than what is provided with standard push notifications.
* Your app is automatically relaunched if it’s not running when a VoIP push is received.
* Your app will be given runtime to process a push message, even if your app is operating in the background or was forced out.

PortSIP PBX supports mobile push notifications for iOS and Android from V9.0. All PortSIP users will benefit from this new feature.



### **The Architecture of PortSIP PBX Push Notifications**

The below architecture demonstrates how PortSIP PBX provides the PUSH notifications.

![PortSIP PBX push notifications architecture](../../.gitbook/assets/pbx\_push\_arch.png)



### **The Call Flows for Send Push Notifications**

**The call flows for send PUSH notifications**

PortSIP uses [APNs](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) to send push notifications to the iOS device, and [Google FCM](https://firebase.google.com/docs/cloud-messaging/android/client) service to send push notifications to the Android device.

1. The App requests a device token from Apple Push Notification Service (APNS)/Google FCM.
2. The app receives the token, which functions as the address to send a push notification to.
3. The app registers to PortSIP PBX with an SIP **REGISTER** message. There must be an "**x-p-push"** header added into the **REGISTER** message, with the device token and other necessary information inclusive into this header to inform PortSIP PBX that the App would like to receive PUSH notifications for this extension, for example, extension 101.
4. Now the user forces terminate the app.
5. When someone calls 101, PortSIP PBX will send an **INVITE** message to the appropriate contact address if the 101 registration is not expired, and sends a push notification with a device token to the APNS/FCM.
6. Since the app is terminated, the **INVITE** message will not be received by the app.
7. APNS/FCM will send a push notification to the device.
8. The iOS/Android device receives that push notification and alerts the mobile device user the call is coming. the user accepts the call.
9. The app will be launched and registered to PortSIP PBX automatically.
10. After successful registration, PBX will send an **INVITE** message to the contact address which gets from the new registration.
11. Now the app receives an **INVITE** message and answers the call.

Same process for Instant Messaging.

The example of "**x-p-push**" header in the REGISTER message.

```
x-p-push:device-os=ios;device-uid=fe1b7bef1b4dc68dbfcd18143c8c06c122a25658c7f1d381c33fa626c9ed;allow-call-push=false;allow-message-push=false;app-id=com.portsip.portgo
```
