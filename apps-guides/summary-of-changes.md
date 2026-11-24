# Summary of Changes

### Changes for Release v10.9.51

**New Features and Improvements**

* Restricted the anonymous calling feature to custom builds only. This feature is no longer available in standard releases.
* Improved the functionality for administrators to manage call queue agent statuses.
* Improved the desktop app's reconnection mechanism to prevent users from being signed out when token refresh fails due to network connectivity issues.
* Improved audio playback in IM conversations on the desktop app.
* Improved IM message search and search result presentation on the desktop app.
* Improved responsiveness on iOS when dialing from the dial pad and transitioning to the in-call screen.

**Bug Fixes**

* Fixed an issue where the desktop app could not launch on Windows when Smart App Control was enabled.

***

### Changes for Release v10.9.50

**New Features and Improvements**

* Improved system notification handling to prevent notifications for previous messages from appearing when an extension signs in for the first time.
* Added support for signing in through an Identity Provider (IdP) using OpenID Connect (OIDC).
* Added support for configuring the visibility and read-only access of client features and settings through configuration templates.
* Enhanced Screen Pop by adding an answering number parameter and support for launching external applications.
* Added support for restricting users from logging in to call queues and changing their agent status through configuration templates.
* Added the ability to manage agent statuses directly from the Wallboard.
* Improved group chat functionality and added a Clear Data option.
* Added a Make Anonymous Call option to the outbound caller ID drop-down list on the dial pad and in the settings.
* Updated holiday settings to default to the tenant's configured country when adding a new holiday.
* Added SRTP configuration options to the mobile apps.

**Bug Fixes**

* Fixed an issue on iOS where incoming calls received via push notifications could not be answered from the lock screen.
* Fixed an issue on Android where the selected theme colors were not applied to the in-call screen.
* Fixed an issue on the desktop app where the user list in the call transfer interface could fail to load completely in environments with a large number of users.
* Fixed a noise issue when decoding G.729 Annex B silence frames.
* Fixed an issue where active SRTP calls could be dropped when switching networks.
* Fixed an issue where voicemail notifications could no longer be received after switching networks and refreshing SIP registration.

***

### Changes in v10.9.42 for Android & iOS&#x20;

**Improvements**

* Refined translations and localization for a better user experience.
* Improved IM reconnection efficiency on iOS.
* Updated the Android in-call interface to automatically adapt to the selected theme.

**Bug Fixes**

* Fixed an issue on certain Android devices where ending a call could unexpectedly terminate an active local conference.
* Fixed an issue on Android where a second incoming call could be automatically rejected while another call was in progress.

***

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

