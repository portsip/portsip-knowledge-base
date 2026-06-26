# Summary of Changes

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

