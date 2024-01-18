# Summary of Changes

{% hint style="info" %}
Please follow the [guide ](upgrading-portsip-pbx-to-new-versions.md)to upgrading your PBX to the latest version.
{% endhint %}

## Changes for Release v16.2.0

Date: Jan 18, 2024

The following changes are included in this release:

* Supports SNOM M series and Yealink W series DECT phones.
* Adds support auto provisioning for Fanvil i504(W), i505(W), i506W, and i507W devices.
* Changes the SNOM phone configuration file to HTM format.
* Enables playing of call recording voice prompts for inbound calls.
* Allows setting of office hours and holidays for each IVR DTMF input.
* If an extension is registered to PBX from multiple devices, once one of the devices rejects the incoming call by **486**, the PBX will also hang up the call on other ringing devices.
* Adds an option for the trunk to allow or disallow the PBX to adjust the SDP direction when holding the call.
* Supports auto-provisioning of emergency numbers to the IP phones, allowing the IP phone to dial the emergency number even when the phone keys are locked.
* No longer displays the offline agent in the Contact Center Wallboard.
* Removes the Offline status in the BLF settings.
* Supports creating the transport on the port which is less than 1024 (limited permissions by some Linux).
* Displays the Feature Access Code in the WebRTC and Windows Client.
* Updates the apps (iOS, Android, Windows, WebRTC) to synchronize the status between the apps and IP Phones. The statuses include:
  * Online
  * Ringing
  * On call
  * Away
  * Do Not Disturb
  * Business Trip
* Supports synchronization of the Do Not Disturb (DND) status by pressing the DND button on the IP phone.
* Adjust the REST API rate limit to 5,000 per minute.
* Changed the WSI (Web Socket Interface) events so subscribers more easily watch the extension status.
* Resolved an issue that was preventing new voicemail alerts on SNOM phones.
* Fixed a bug that could prevent the Queue prompt voice from playing to the caller when the Queue servers are configured as a cluster.

## Changes for Release v16.1.0

Date: Nov 2, 2023

The following changes are included in this release:

* Improved security; now each extension's IP Phone configuration file is stored in a separate directory with a random name to prevent guessing even if the phone MAC address is leaked
* Integrated Microsoft 365 and enabled Single Sign-On (SSO)
* Implemented Contact Center Wallboards
* Introduced skill-based routing for ring groups
* Introduced skill-based routing for queues
* Added support for Last Called Agent Routing for the queue
* Supported setting agent status to Not Ready after the agent completes a Non-ACD call
* Introduced Queue Manager Role
* Changed the CTI menu name to Contact Center
* Implemented custom headers at tenant level, allowing the adding of custom SIP headers for a call and relaying of specified custom headers for a call
* Optimized High Availability (HA) for On-Premise and AWS deployments
* Enabled scaling of the Media server, Queue server, Virtual Receptionist Server, and Meeting server as a cluster
* Implemented Direct Inward System Access (DISA)
* Added support for meeting invitations and joining meetings via link
* Added support for Hotdesking
* Introduced the FAC `*70` and `*71` for Hotdesking
* Added theme support, allowing customers to adjust the themes of the WebRTC Client, PBX Web Portal, and SBC Web portal to fit their business style
* Added support for Italian and Spanish languages and voice prompts
* Added support for Speed Dial 8 and Speed Dial 100
* Added functionality to resend welcome emails to extensions
* Enabled voicemail PIN authorization by default
* Allowed contact's phone number to be empty
* Refactored the WSI (WebSocket Subscribe Interface)
* Enabled changing the user's presence status via BLF key
* Fixed a bug preventing SNOM phones from picking up calls via BLF key
* Introduced Advanced Routing for inbound rule that allows routing of inbound calls based on years, months, days, weekdays, and any time shifts
* Added a notification template for meeting invitations
* Implemented max concurrent call limits for trunks at both global and tenant levels
* Added functionality to send notification emails when max concurrent calls on a trunk are reached
* Introduced user personal contacts feature
* Implemented the User-Agent blacklist
* Added support for associating IP addresses with CIDR format for a trunk
* Optimized performance for WebRTC, outbound calls, and video meetings
* Introduced Recipients for Shared Voicemail that sends email notifications to them
* Allowed specifying outbound caller ID for the outbound rule
* Supported random routes for an outbound rule
* With the PortSIP PBX setup wizard, the web domain field is now mandatory to enter
* Adjusted the REST API rate to 1,000 requests per minute per IP address
* Added Virtual Group Park in the BLF keys, supporting Fanvil, Yealink, and Dinstar phones
* Added support for more new models of Fanvil IP Phones and intercom devices
* Added support for more new models of Yealink IP Phones
* Added support for more new models of SNOM IP Phones
* Allowed recharging with a negative amount to reduce the balance
* Made some other minor changes and improvements
* Fixed some minor bugs

## Changes for Release v16.0.4

**Date**: August 8, 17 2023

* Fixed a security issue.
* Fixed a bug that can't hear early media when launching the call by REST API.

## Changes for Release v16.0.3

**Date**: July 17 2023

The following changes are included in this release:

* Fixed a bug that caused SNOM phones to fail to download configuration files when auto-provisioned using a custom template. The issue was due to the configuration file name being in lowercase instead of the phone’s MAC address. For more details, please read the article on [Custom IP Phone Templates.](https://support.portsip.com/portsip-pbx-administration-guide/4-phone-device-management/custom-ip-phone-template#snom-phone)
* Fixed a bug that caused mobile app push notifications to expire after 1.5 days. The expiration time has been changed to 60 days.
* Improved compatibility with 2N devices. 2N devices appended a non-standard server port to the TO and FROM headers in REGISTER and INVITE messages, causing calls to ring groups to return a 480 error.
* Fixed a bug that the video meeting recorded is not working.
* Fixed a bug that caused the media server to hang when video call recording was enabled in the PBX.
* Fixed a bug that screen sharing did not work during meetings when the PBX was running on a Linux server.
* Automatically determine if the server’s IPv6 is disabled, and if so, do not attempt to bind the service to the IPv6 interface.
* Added support for configuring the first BLF as a speed dial for Fanvil i series intercom devices via auto-provisioning.

## Changes for Release v16.0.2

**Date**: June 1, 2023

The following changes are included in this release:

* Updated the certificates for Apple push notifications. Users must upgrade the PBX to make the PortSIP app work with push notifications.
* Changed the default retain days for recording files, log files, temporary files, and call report files.
* Changed Max-Forwards from 20 to 70.
* Added support for visual park feature with Fanvil IP Phones.
* Improved voice mix in meetings.
* Fixed a bug that did not use new brand name for creating custom phone templates.
* Fixed bug that displayed incorrect presence status.
* Fixed bug that sent NOTIFY for new voicemails.
* Fixed billing bug that added one more billing round if billing time was multiple of 60 seconds.
* Fixed push status display incorrectly on the user list page.

## Changes for Release v16.0.1

**Date**: March 30, 2023

The following changes are included in this release:

* If the "Set agent to Ready automatically" option of the queue is disabled, an agent’s status will be restored to their previous status instead of "Wrap up" after they complete a non-queue call.
* The license key will only display the last part and mask other parts with \*.
* A new REST API `GET /api/sessions/directly` has been added which allows launching calls by URL.
* The REST API `POST /api/users/{id}/call` has been removed.
* When a caller presses a DTMF that is not predefined, a prompt will play.
* The MOH file of the queue is no longer mandatory. The queue will use the default MOH file if the user has not uploaded a MOH file for a queue.
* If the user is offline and push notifications are enabled, their status will be displayed as "**PushOnline"** in the Web portal.
* The Web portal will display the language as the browser language.
* The WebRTC client URL port has been changed to 10443. Don’t forget to create the firewall rule for port 10443 on TCP.
* Passwords will be auto-generated when creating a user.
* Custom menus can be used to link to external websites.
* The IP Address will be displayed when viewing the details of an online user.
* If a user signs in to the Web portal with an incorrect password or if the user does not exist, just return the message "**Authentication Error**" and no longer return the detailed reason.
* The issue where auto-provisioning for GrandStream IP Phones the URL is wrong has been fixed.
* The issue where emails failed to send with some SMTP servers has been fixed.
* The issue where there may be no voice after completing an attended transfer has been fixed.
* The issue where extension call forwarding is not affected if the call comes from Virtual Receptionist or Queue has been fixed.
* The issue where the outbound caller ID is set as starting with + and it won’t set to the INVITE message when making a call to the trunk has been fixed.
* The issue where if a user registers to PBX from multiple devices and launches a call from REST API with that user and the call actually answered but the CDR will display as not answered has been fixed.
* The issue where if you launch a call by REST API, one party may have no voice has been fixed.
* In the Webhook and WSI messages, the ID has been changed from in64\_t to string in order to be compatible with JS.
* The billing issue is that if the call duration is multiples of the billing circle, don’t plus more 1 billing round; in the previous version, if the duration is 10 seconds and the billing circle is 5 seconds, we will charge 3 rounds, now just charge 2 rounds.

## Changes for Release v16.0.0

**Date**: January 16, 2023

The following changes are included in this release.

* New Admin web portal
* New rebranding engine
* Rewritten all REST APIs
* Introduced dealer system, support distributor, sub-distributor, reseller
* Support call park and group call park
* Support group pickup
* Support shared voicemail
* Support automatic callback
* Support queue callback
* Support rich call reports
* Notifications, ability to configure the types of email notifications that the system sends to users
* Custom notifications template
* Custom IP Phone template
* Roles Based Permissions
* Custom Role
* Feature Access Codes (Dial Codes)
* Password policy
* Integrated SBC
* Support Microsoft Teams Direct Routing
* Billing on tenant
* Billing on user
* Online charging
* Offline charging
* Multiple office hours per day
* Route calls based on the DID number range
* Visual IVR Editor
* Voice Announcement
* Audit log viewer
* Trunk-Based Outbound Caller ID
* Outbound caller ID for tenant
* User Profile Picture
* RFC3323 Privacy Mechanism
* Priority for outbound rules

