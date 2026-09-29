# Summary of Changes

### Changes for Release v10.9.4

**Release Date:** Sep 28, 2026

#### New Features and Improvements

* Redesigned the desktop contact view with two layout options: Compact and Spacious.
* Added hotkey dialing on desktop.
* Added support for sending documents through WhatsApp and MMS.
* Hidden IM and Data Flow features when their respective services are not installed.
* Added extension presence indicators when transferring calls on desktop.
* Updated the Android incoming call ringtone to follow the system ringtone settings.
* Hidden features that are not included in the current license.
* Added simultaneous display of the caller’s name and number on incoming call and active call screens.
* Added the option to set the mobile app as the system’s default dialer.
* Added headset control support on Windows.
* Improved performance when retrieving call history.
* Improved desktop network connection status handling.

#### Bug Fixes

* Fixed an issue that prevented new topics from loading when a user had too many topics.
* Fixed an issue where changes to WebRTC codec settings did not take effect after being saved.
* Fixed an issue where the desktop app did not check the messaging channel associated with the outbound caller ID before sending an MMS message.
* Fixed an issue where topic subscriptions were not filtered by the signed-in extension.
* Fixed an issue on Samsung phones where the other party could not hear the user when another app was opened during a call.
* Fixed an issue where an incoming SIM call automatically placed an active call on hold in the mobile app.
* Fixed an issue where the desktop app could not answer incoming calls after the computer had been asleep for an extended period.

***

### Changes for Release v10.9.2

**Release Date:** July 1, 2026

#### Fixes

* Fixed an issue where shared file URLs in MMS and WhatsApp messages could be generated incorrectly when PortSIP PBX was deployed without PortSIP SBC.
* Fixed missing language text for the Selective Call Rejection and Selective Call Acceptance Feature Access Codes.

***

### Changes for Release v10.9.1

**Release Date:** June 26, 2026

#### New Features and Improvements

**Ring Groups and Call Queues**

* Improved missed call reporting for Ring Groups and Call Queues in simultaneous ringing scenarios.
* Improved call park prompts to provide a clearer user experience.

**Contacts and Caller Identification**

* Improved contact matching logic.
* Enabled P-Asserted-Identity (PAI) by default and gave it higher priority for contact matching.
* Added a setting to configure the display priority for matched incoming call contacts.
* Improved mobile contact matching by ignoring special characters in contact phone numbers.

**PortSIP ONE Client**

* Added voicemail greeting management in the client.
* Added an iOS setting to control whether call history is synchronized with the native phone call log.
* Added support for mobile devices that use virtual navigation buttons.

**SMS, MMS, and WhatsApp**

* Added support for WhatsApp message templates.
* Added detection for the WhatsApp 24-hour messaging session status.
* Added support for sending media files through SMS, MMS, and WhatsApp.

#### Fixes

* Fixed an issue where the desktop client could display a blank white screen after a laptop resumed from extended sleep or hibernation.
* Fixed an issue where the transfer button could occasionally become unresponsive after a high volume of calls and call transfers.
* Fixed an issue where user names could be displayed incorrectly for both parties after an attended transfer.
* Fixed an issue where the desktop client might not receive incoming call notifications after being minimized.
* Fixed an issue where deleting a one-to-one IM conversation on mobile did not clear the previous chat history.
* Fixed push notification issues on some Android devices.

