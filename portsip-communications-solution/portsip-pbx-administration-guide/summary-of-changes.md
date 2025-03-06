# Summary of Changes

{% hint style="info" %}
Please follow the [guide ](1-installation-of-the-portsip-pbx/installation-of-portsip-pbx-v22/upgrade-to-the-latest-version-within-v22.x-on-linux.md)to upgrading your PBX to the latest version.
{% endhint %}

## Changes for Release v22.1.7

Date: Feb 27, 2025

### New Features & Enhancements

* **OAuth Integration with Microsoft 365 and Google Workspace**\
  PBX system administrators and tenants can now authenticate email notifications using OAuth for Gmail and Microsoft 365 accounts.
* **Apply Mail Server Settings to All Tenants**\
  A new feature allows tenants to adopt the system administrator’s mail server settings for email notifications, ensuring consistent configuration across all tenants.
* **SMS and WhatsApp Message Records**\
  The system now supports listing and querying records for both SMS and WhatsApp messages, providing better tracking and management of communications.
* **Trunk Integration with VoIP Innovations, Bandwidth, and Flowroute**\
  Users can now easily configure trunks and integrate with the SMS API for VoIP Innovations, Bandwidth, and Flowroute, simplifying trunk setup and management.
* **New SIP Header – X-Info**\
  A new SIP header, _X-Info_, has been introduced to enhance the transmission of call information for improved troubleshooting and analytics.
* **Removal of X-Trunk-Name SIP Header**\
  The _X-Trunk-Name_ SIP header has been removed. Trunk-related information will now be transmitted via the _X-Info_ header for better standardization.
* **Azure Blob Storage Support**\
  Added support for storing call recordings and voicemail files in Azure Blob Storage, offering flexible and scalable storage options.
* **BLF Subscription for System Extensions**\
  System extensions can now subscribe to other system extensions' BLF status. Previously, only extensions could subscribe to the BLFs of other extensions.
* **Updated Feature Access Code (FAC) Format Rules**\
  The format rules for Feature Access Codes (FAC) have been updated to ensure better compatibility and user experience.
* **Optimized CDR Query Performance**\
  Performance improvements have been made to enhance the efficiency and speed of Call Detail Record (CDR) queries.
* **Increased REST API Rate Limit**\
  The REST API rate limit has been increased to 10,000 requests per minute, improving scalability and performance for high-traffic applications.
* **SMTP Authentication Mode – IP Authentication**\
  A new “None” option has been added to the SMTP Authentication Mode settings for use with SMTP servers that employ IP address-based authentication.
* **Chat Group Member Limit**\
  The maximum number of members allowed in a chat group has been increased to 200, providing greater flexibility for team communication.
* **Handset Language for SNOM DECT M100 Auto-Provisioning**\
  Support has been added for setting the handset language during auto-provisioning of SNOM DECT M100 devices, ensuring smoother user experiences.
* **User and Engineer Passwords for SNOM DECT Auto-Provisioning**\
  Fields for User and Engineer passwords have been added in the auto-provisioning setup for SNOM DECT M300, M400, M700, and M900 devices.
* **Web Portal Optimization**\
  The web portal has been optimized to enhance usability and provide a more intuitive user interface, improving the overall user experience.

### Bug Fixes

1. **Trunk ACK Delay Handling**\
   Fixed a bug where slow ACK responses from the trunk to the PBX prevented calls from being offered to queue agents.

### REST API Changes

* **New Endpoint:** `/api/external_messages` – Allows querying of SMS and WhatsApp message histories.
* **New Endpoint:** `/api/user/external_messages` – Allows querying of the current user’s SMS and WhatsApp message histories.
* **Endpoint Removal:** `/api/test_email` – This endpoint has been removed. System administrators can now use `/api/admin/notification/test_email`, and tenant administrators can use `/api/tenant/notification/test_email` as alternatives.
* **Updated Endpoint:** `/api/admin/notification` – Added the `enable_tenant_access` option, which allows tenants to use the system administrator’s mail server settings to send email notifications.
* **Updated Endpoint:** `/api/tenant/notification/test_email` – Added the `enable_system_email_server` option, which indicates whether the tenant has permission to use the system administrator’s mail server settings to send email notifications.

## Changes for Release v22.0.42

Date: Jan 16, 2025

* Fixed an issue where the client app’s username was displayed incorrectly when the app was offline but push notifications were enabled.&#x20;
* Resolved an issue where inbound calls to a queue via a trunk were not routed to an agent if the trunk’s ACK response was delayed.&#x20;
* Corrected a bug where the outbound caller ID specified in a REST API call was not properly recorded in the CDR.

### REST API Changes

* Added `/api/calllogs`to query the CDR logs.

## Changes for Release v22.0.39

Date: Jan 2, 2025

* Fixed an issue where inbound rules were not being applied correctly when a holiday was configured.&#x20;
* Resolved a bug with Advanced Routing, where the “all” option was not properly matching year/month parameters.&#x20;
* Corrected an issue where exception forwarding rules failed to match the caller number under certain scenarios.&#x20;
* Fixed a display issue where the caller’s display name was incorrect when calling into a ring group or queue.

## Changes for Release v22.0.38

Date: Dec 12, 2024

* Optimized performance: 2 cores, 4GB memory for up to 1,000 online users, supports \~500 simultaneous calls
* PortSIP ONE app: Available for WebRTC, Windows, iOS, and Android (macOS support coming soon)
* SSO login: Support for Microsoft 365 accounts across WebRTC, Windows, iOS, and Android apps
* Messaging features: Group chat, offline messaging, sync messages across devices, support for SMS and WhatsApp messaging
* Call management: Optimized for blind and attended call transfers, Call Flip and Call Park features within the app, Call Park notifications for easy retrieval, Visual voicemail in the app
* Customization options: Themes and emoji support, customizable caller ID for calls and SMS, manage personal and company contacts, easy import of contacts
* Synchronization across devices and apps: Sync presence and custom status, sync DND status across apps and IP phones, auto-sync extension users and CDR across apps
* VoIP trunk and SMS API integrations: Pre-configured trunks for Vonage, QuestBlue, VoIP.ms, Voxtelesys, Wavix, Twilio, Telnyx, Aire Networks, VoiceMeUp
* Phone support: Support for FANVIL DECT Phones (MODE and V66 models), SNOM phones, Yealink W73B DECT Phones, auto-provisioning for Grandstream GXP2604, HTEK phones
* Security and routing enhancements: STIR/SHAKEN support for enhanced call security, call routing based on extension presence status
* Administrative features: Tenant admins can manage Speed Dial 8 and Speed Dial 100 settings
* Operating system support: Linux (Debian 11/12, Ubuntu 22.04/24.04), Windows (10 1903/19H1 or higher, Windows Server 2022 or higher)
* Language support: Japanese language support
* WSI Pub/Sub integration: Provides **global\_\*** event notifications for system integration

### REST API Changes

* Removed `/api/login/by_extension`.
* Removed `/api/tenants/:id/dealer`.
* Removed `/api/tokens`.
* Removed `/api/tokens/by_extension`.
* Removed `/api/tokens/refresh`.
* Removed `/api/tokens/destroy`.
* Removed `/api/user/chats/sessions`.
* Removed `/api/user/chats/sessions/:id/messages`.
* Removed `/api/user/chats/sessions/:id/messages/set_read`.
* Removed `/api/user/contacts/version`.
* Removed `/api/call_queues/:id/agents/:agent_number/login`.
* Removed `/api/call_queues/:id/agents/:agent_number/logout`.
* Removed `/api/call_queue_blacklist_prompts/:level`.
* Removed `/api/contacts/version`.
* Removed `/api/contact_groups`.
* Removed `/api/contact_groups/:id`.
* Removed `/api/contact_groups/:id/destroy`.
* Removed `/api/contact_groups/:id/contacts`.
* Removed `/api/contact_groups/:id/contacts/:contact_id`.
* Removed `/api/contact_groups/:id/contacts/:contact_id/destroy`.
* Removed `/api/files/:id/metadata`.
* Removed `/api/files/:id/data`.
* Removed `/api/files/uploads`.
* Removed `/api/files/uploads/:id/append`.
* Removed `/api/files/uploads/:id/complete`.
* Removed `/api/files/uploads/:id/status`.
* Removed `/api/files/uploads/:id/destroy`.
* Added `/api/user/presence` to manage the presence status of extension users.
* Added `/api/tenants/:id/dealers`, `/api/tenants/:id/dealers/:dealer_id`, `/api/tenants/:id/dealers/:dealer_id/destroy` to manage tenant-dealer relationships.
* Added `/api/im`, `/api/im/token`, `/api/im/token/destroy` to manage IM service-related features.
* Added `/api/sms`, `/api/sms/:id`, `/api/sms/:id/destroy` to manage SMS service-related features.
* Added `/api/whatsapp`, `/api/whatsapp/:id`, `/api/whatsapp/:id/destroy` to manage WhatsApp service-related features.
* Added `/api/user/cdrs/sync_tokens`, `/api/user/cdrs/sync_tokens/:token/diff` to sync extension user CDRs.
* Added `/api/user/meetings/:id/status`, `/api/user/meetings/:id/start`, `/api/user/meetings/:id/stop` to manage extension user meetings.
* Added `/api/user/contacts/:id/favorite`, `/api/user/contacts/:id/unfavorite` to manage personal contacts' favorites for extension users.
* Added `/api/user/contacts/sync_tokens`, `/user/contacts/sync_tokens/:token/diff` to sync personal contacts for extension users.
* Added `/api/user/business_contacts/:id/favorite`, `/api/user/business_contacts/:id/unfavorite` to manage business contacts' favorites for extension users.
* Added `/api/user/business_contacts/sync_tokens`, `/user/business_contacts/sync_tokens/:token/diff` to sync business contacts for extension users.
* Added `/api/user/extension_contacts/:id/favorite`, `/user/extension_contacts/:id/unfavorite` to manage extension contacts' favorites for extension users.
* Added `/api/user/extension_contacts/sync_tokens`, `/user/extension_contacts/sync_tokens/:token/diff` to sync extension contacts for extension users.
* Added `/api/user/outbound_caller_ids` to retrieve all outbound caller IDs for extension users.
* Added `/api/user/ring_groups` to retrieve all ring groups associated with extension users.
* Added `/api/users/:id/ms365_binding`, `/users/:id/ms365_binding/destroy` to bind and unbind extension users to Microsoft 365 integration.
* Added `/api/users/:id/speed_dial_8`, `/users/:id/speed_dial_8/:dial_id`, `/users/:id/speed_dial_8/:dial_id/destroy` to manage Speed Dial 8 for specific extension users.
* Added `/api/users/:id/speed_dial_100`, `/users/:id/speed_dial_100/:dial_id`, `/users/:id/speed_dial_100/:dial_id/destroy` to manage Speed Dial 100 for specific extension users.
* Renamed `/api/user/meetings/:id/members` to `/user/meetings/:id/participants`.
* Renamed `/api/user/meetings/:id/members/layout` to `/user/meetings/:id/participants/layout`.
* Renamed `/api/user/meetings/:id/members/:extension_number` to `/user/meetings/:id/participants/:participant_id`.
* Renamed `/api/user/meetings/:id/members/:extension_number/invite` to `/user/meetings/:id/participants/invite`.
* Renamed `/api/user/meetings/:id/members/:extension_number/mute` to `/user/meetings/:id/participants/:participant_id/mute`.
* Renamed `/api/user/meetings/:id/members/:extension_number/unmute` to `/user/meetings/:id/participants/:participant_id/unmute`.
* Renamed `/api/user/meetings/:id/members/:extension_number/chairman` to `/user/meetings/:id/participants/:participant_id/chairman`.
* Renamed `/api/user/meetings/:id/members/:extension_number/order` to `/user/meetings/:id/participants/:participant_id/position`.
* Renamed `/api/user/meetings/:id/members/:extension_number/destroy` to `/user/meetings/:id/participants/:participant_id/destroy`.
* Renamed `/api/conference_rooms/:id/members` to `/api/conference_rooms/:id/participants`.
* Renamed `/api/conference_rooms/:id/members/layout` to `/api/conference_rooms/:id/participants/layout`.
* Renamed `/api/conference_rooms/:id/members/:extension_number` to `/api/conference_rooms/:id/participants/:participant_id`.
* Renamed `/api/conference_rooms/:id/members/:extension_number/invite` to `/api/conference_rooms/:id/participants/invite`.
* Renamed `/api/conference_rooms/:id/members/:extension_number/mute` to `/api/conference_rooms/:id/participants/:participant_id/mute`.
* Renamed `/api/conference_rooms/:id/members/:extension_number/unmute` to `/api/conference_rooms/:id/participants/:participant_id/unmute`.
* Renamed `/api/conference_rooms/:id/members/:extension_number/chairman` to `/api/conference_rooms/:id/participants/:participant_id/chairman`.
* Renamed `/api/conference_rooms/:id/members/:extension_number/order` to `/api/conference_rooms/:id/participants/:participant_id/position`.
* Renamed `/api/conference_rooms/:id/members/:extension_number/destroy` to `/api/conference_rooms/:id/participants/:participant_id/destroy`.
* Renamed `/api/tariffs` to `/call_rates`.
* Renamed `/api/tariffs/:id` to `/call_rates/:id`.
* Renamed `/api/tariffs/:id/destroy` to `/call_rates/:id/destroy`.
* Renamed `/api/tariffs/export` to `/call_rates/export`.
* Modified `/api/admin/notification`: Added `auth`, `enable_starttls_auto` to configure SMTP authentication methods.
* Modified `/api/admin/settings`: Added properties `user_equal_required_for_auth_name`, `stir_shaken_cert`, `stir_shaken_key`; Modified property: `session_timer_duration` (default value changed to 3600).
* Modified `/api/call_park` (GET method): Removed property `prompt_file_id`.
* Modified `call_queue_blacklist_prompts` (GET method): Removed properties `level1_prompt_file_id`, `level2_prompt_file_id`.
* Modified `/api/call_queues`: Added properties `enable_paid`, `enable_prid`, `extension_number_as_to_header`.
* Modified `/api/call_queues/:id` (GET method): Removed properties `moh_prompt_file_id`, `intro_prompt_file_id`.
* Modified `/api/cdrs`: Added properties `service_number`, `user_data`.
* Modified `/api/cdrs/:id`: Added property `service_number`.
* Modified `completed_call_reports`: Removed property `file_id`.
* Modified `completed_call_reports`: Added property `file_url`.
* Modified `/api/conference_rooms`: Added properties `internal_invitees`, `external_invitees`.
* Modified `/conference_rooms/:id/recordings`: Removed property `file_id`.
* Modified `/conference_rooms/:id/recordings`: Added property `duration`.
* Modified `/api/contacts`: Removed property `pager`; Added properties `title`, `notes`.
* Modified `/api/dealers`: Added property `tenant_full_access`.
* Modified `/api/dect_phones`: Added property `region`.
* Modified `/api/hotdesking`: Added properties `external_ringtone`, `serial_number`.
* Modified `/api/media_servers`: Added property `custom_options`.
* Modified `/api/moh_server/musics`: Removed property `file_id`.
* Modified `/api/ms365` (GET method): Added property `sbc_redirect_uri`.
* Modified `/api/providers`: Added properties `enabled`, `brand`, `registration`, `stir_shaken_signature_required`; Modified properties: `inbound_parameters` (Added sub-properties: `enable_stir_shaken_validation`, `pai_header_parameter_name`, `drop_calls_with_verification_status`, `pass_api_header_to_uad`), `outbound_variable_user` (Added value: `OUTBOUND_CALLER_ID_AND_ORIGINATOR_CALLER_ID`), `outbound_variable_host` (Added value: `SIP_DOMAIN`), `status` (Removed values: `REGISTERED`, `UNREGISTERED`; Added values: `ONLINE`, `OFFLINE`).
* Modified `/api/ring_groups`: Added properties `enable_paid`, `enable_prid`, `extension_number_as_to_header`.
* Modified `/api/shared_voicemails/:id/greetings` (GET method): Removed properties `file_id`, `filename`.
* Modified `/api/shared_voicemails/:id/voicemails`: Added property `duration`.
* Modified `/api/tenants`: Removed properties `enable_concurrent_login`, `enable_queue_blacklist_first_level`, `enable_queue_blacklist_second_level`, `avatar`, `avatar_file_id`; Added properties `enable_billing`, `enable_feature_billing`, `enable_feature_call_statistics`, `enable_feature_contact_center`, `enable_feature_message_channels`, `enable_feature_microsoft_teams`, `enable_feature_trunks`, `enable_feature_whats_app`, `im_disk_quota`, `extension_im_disk_quota`, `stir_shaken_cert`, `stir_shaken_key`; Modified properties: `contact_match_type` (default value changed to `MATCH_EXACTLY`), `contact_update_interval` (default value changed to 720).
* Modified `/api/tenant@notification`: Added properties `auth`, `enable_starttls_auto`.
* Modified `/api/tenants`: Removed properties `enable_concurrent_login`, `enable_queue_blacklist_first_level`, `enable_queue_blacklist_second_level`, `deleted_at`, `avatar`, `avatar_file_id`; Added properties `enable_billing`, `enable_feature_billing`, `enable_feature_call_statistics`, `enable_feature_contact_center`, `enable_feature_message_channels`, `enable_feature_microsoft_teams`, `enable_feature_trunks`, `enable_feature_whats_app`, `im_disk_quota`, `extension_im_disk_quota`, `stir_shaken_cert`, `stir_shaken_key`; Modified properties: `contact_match_type` (default value changed to `MATCH_EXACTLY`), `contact_update_interval` (default value changed to 720).
* Modified `/api/tenants/switch`: When status is 200 OK, no longer returns a new token.
* Modified `/api/test_email`: Added properties `auth`, `enable_starttls_auto`.
* Modified `/api/user`: Removed properties `twitter`, `facebook`, `linkedin`, `instagram`, `avatar`, `avatar_file_id`, `online_no_answer_forward_rule`, `online_busy_forward_rule`; Added properties `address`, `department`, `sms`, `available_office_hours_forward_rule`, `available_non_office_hours_forward_rule`, `available_no_answer_forward_rule`, `busy_office_hours_forward_rule`, `busy_non_office_hours_forward_rule`, `busy_no_answer_forward_rule`, `dnd_office_hours_forward_rule`, `dnd_non_office_hours_forward_rule`, `away_office_hours_forward_rule`, `away_non_office_hours_forward_rule`, `lunch_office_hours_forward_rule`, `lunch_non_office_hours_forward_rule`, `trip_office_hours_forward_rule`, `trip_non_office_hours_forward_rule`.
* Modified `/api/user/cdrs`: Added properties `service_number`, `user_data`.
* Modified `/api/user/cdrs/:id`: Added property `service_number`.
* Modified `/api/user/contacts`: Removed property `pager`; Added properties `title`, `notes`.
* Modified `/api/user/greetings`: Removed properties `file_id`, `filename`.
* Modified `/api/user/meetings` (POST method): Removed properties `extension_number`, `capacity`, `close_on_chairman_exit`, `close_on_endtime`; Added properties `internal_invitees`, `external_invitees`; Modified property: `timezone` (required).
* Modified `/api/user/phones`: Added properties `external_ringtone`, `serial_number`, `door_password1`, `door_password2`.
* Modified `/api/user/recordings`: Removed properties `file_id`, `filename`; Added property `duration`.
* Modified `/api/users`: Removed properties `twitter`, `facebook`, `linkedin`, `instagram`, `avatar`, `avatar_file_id`, `online_no_answer_forward_rule`, `online_busy_forward_rule`; Added properties `address`, `department`, `sms`, `available_office_hours_forward_rule`, `available_non_office_hours_forward_rule`, `available_no_answer_forward_rule`, `busy_office_hours_forward_rule`, `busy_non_office_hours_forward_rule`, `busy_no_answer_forward_rule`, `dnd_office_hours_forward_rule`, `dnd_non_office_hours_forward_rule`, `away_office_hours_forward_rule`, `away_non_office_hours_forward_rule`, `lunch_office_hours_forward_rule`, `lunch_non_office_hours_forward_rule`, `trip_office_hours_forward_rule`, `trip_non_office_hours_forward_rule`; Modified property: `display_name` (required).
* Modified `/api/users/:id/greetings`: Removed properties `file_id`, `filename`.
* Modified `/api/users/:id/phones`: Added properties `external_ringtone`, `serial_number`, `door_password1`, `door_password2`.
* Modified `/api/voicemails`: Removed properties `file_id`, `filename`; Added property `duration`.
* Modified `/api/default_email_templates`: Modified property `name` (Added `TRUNK_CONNECTED`, `TRUNK_DISCONNECTED`).
* Modified `/api/custom_email_templates`: Modified property `name` (Added `TRUNK_CONNECTED`, `TRUNK_DISCONNECTED`).
* Modified `/api/feature_access_codes`: Modified property `code` (Added `CALL_FLIP`, `CALL_TRANSFER`, `CLEAR_PUSH`, `RESET_CALLS`).
* Modified password property: Minimum length restriction changed to 6 (previously 8).
* Modified outbound\_caller\_ids property: Added sub-property `description`.

## Changes for Release v16.4.4

Date: Nov 25, 2024

* Fixed an issue where the call direction was incorrect when calls were initiated via the REST API.
* Resolved a bug where blacklisted numbers set to "permanent" were not properly affected.
* Addressed an issue causing no voice when a call launched via the REST API failed on the first trunk route but succeeded on the second.
* Fixed a bug where outbound caller IDs outside the trunk DID pool were still used during calls.
* Resolved an issue where outbound caller IDs were not recorded in CDRs for calls initiated via the REST API to trunks.
* Removed trunk information from CDRs for calls between internal extensions.
* Fixed a queue bug where the last agent did not receive calls in queue "ring simultaneously" mode when all agents were available.
* Resolved an issue in version 16.4.3 where the PBX used an incorrect IP address in the call SDP when making calls to trunks over the internet in a High Availability (HA) setup.

## Changes for Release v16.4.3

Date: Sep 10, 2024

* Users can now efficiently locate IVRs within the IVR list page by utilizing the search functionality.
* Fixed a crash issue that occurred when using a domain for the trace server host.
* Fixed a bug affecting calls using the G.723.1 codec.

## Changes for Release v16.4.2

Date: Jul 24, 2024

* Fixed a crash bug in the IVR Server.
* Fixed a bug where video might lag behind audio in a video call recording file.&#x20;
* Fixed a crash bug that occurred when the RTP port was already in use by another application.&#x20;
* Fixed a bug that could cause calls to hang up automatically.&#x20;
* Fixed a bug where system notification emails were incorrectly using the tenant’s template.&#x20;
* Fixed an issue where the MOH music file might be incorrectly used by another tenant.&#x20;
* Fixed a bug where making a large number of calls could result in a new call having only one voice channel.&#x20;
* Fixed a bug where deploying the queue server as a cluster might cause the REST API `/api/call_queues/{id}/waiting` to return empty callers.
* &#x20;Fixed a bug where incorrect codec usage in SNOM M400 and M900 could result in no voice.

## Changes for Release v16.4.1

Date: Jun 12, 2024

The following changes are included in this release:

* Support the Microsoft Teams direct routing refer request.
* Upgraded the DB to PostgreSQL 14.12.
* Fix an issue when provisioning the ALE H2P phone via RPS.
* Fix a bug that if an extension registers to PBX from IPv4 and IPv6 devices simultaneously.
* Allow set the Queue and ring group number for the BLF key.

## Changes for Release v16.4.0

Date: May 23, 2024

The following changes are included in this release:

* Now the PortSIP SBC supports transcoding.
* Google will stop supporting the legacy FCM APIs for push notifications in June 2024, all PortSIP PBX v16.x installations need to be upgraded to v16.4.0 to make the push notifications work correctly, please reference the article: [Migrate from legacy FCM APIs to HTTP v1 for Android Push Notifications](../faq/migrate-from-legacy-fcm-apis-to-http-v1-for-android-push-notifications.md).
* From v16.4, when a trunk is added, the outbound call over this trunk will apply the outbound caller ID to the FROM header by default, no longer needing to make the changes for the outbound parameters manually.
* Limited the file name size to a maximum of 128 characters when uploading the voice prompt files.
* Support French and Russian languages.

### REST API Changes

1. Updated `/api/mobile_push` series API, removed `android_server_key`, `android_sender_id`, added `android_service_account` to specify the the contents of **Firebase Cloud Messaging service account JSON file**.

## Changes for Release v16.3.0

Date: Mar 11, 2024

The following changes are included in this release:

* We’ve moved the Microsoft 365 settings from the global scope to the tenant level. Now, each tenant administrator can configure Microsoft 365 integration independently, without requiring assistance from the System Administrator.
* We’ve redesigned the login page for the PBX Web Portal, Windows app, and WebRTC app.
* New outbound rules will now use their own office hours by default, instead of using the tenant’s office hours.
* We’ve made changes to the username format limitations.
* System Administrators and Dealers are now allowed to change their usernames.
* We’ve updated the SMTP username format validation to be compatible with AWS SES.
* We’ve fixed a bug where, if a call was answered on the IP Phone or Desktop app, the mobile app would continue ringing for a while.
* Tenant administrators can now subscribe to the CDR event for the entire tenant scope.
* We’ve added an option for the trunk to remove the SRTP line, resolving compatibility issues with some trunks.
* We’ve introduced a new permission to control access to company contacts.
* Tenant administrators can now clear device registration and app push information on the Web Portal for an extension.
* We’ve added a new feature that allows re-provisioning of all phones for a tenant.
* Fix a bug count the concurrent calls are incorrect on a trunk for some special scenarios.
* Support the English UK language.

### REST API Changes

* &#x20; Remove `GET /api/token`.
* &#x20; Remove `POST /api/token/refresh`.
* &#x20; Remove `POST /api/token/destroy`.
* &#x20; Add POST `/api/login/by_microsoft` for Microsoft 365 integrated login.
* &#x20; Add GET `/api/login` for users to get their current login status.
* &#x20;Modify `GET /api/info` to accept an optional query parameter `domain`. When the `domain` parameter is specified in the request, the server will additionally return the public properties `name`, `domain`, `website`, `avatar_url`, `enable_ms365_integration`, `ms365_authorization_endpoint` of the corresponding tenant.
* &#x20; Add GET `/api/users/:id/status/:instance_id/destroy_status` to clear the login information of a specified user.
* &#x20; Rename `POST /api/phones/{mac}/reprovsion` to `POST /api/phones/{mac}/reprovision`.
* &#x20; Add `POST /phones/reprovision` to reconfigure all phones.
* &#x20; Add a configurable property `remove_srtp_info` for the Trunk.
* &#x20; Modify the username format validation rules to allow a sequence of 1-64 characters that can include uppercase and lowercase letters, numbers, underscores, hyphens, periods, and single quotes. However, periods are not allowed at the beginning or end.
* &#x20; Add `POST /api/admin/username` for users to change the system administrator's username.
* &#x20; Add `POST /api/dealer/username` to change the dealer's username.
* &#x20; Add `GET /api/user/call_queues` to get the list of call queues that the currently logged-in user belongs to.
* &#x20; Add `GET /api/user/call_queues/:id/agent` to get the agent status of the specified call queue that the currently logged-in user belongs to.
* &#x20; Add `POST /api/user/call_queues/:id/agent` to set the agent status of the specified call queue that the currently logged-in user belongs to.
* &#x20; Add `GET /api/users/:id/call_queues` to get the list of call queues that a specified user belongs to.
* &#x20; Add `GET /api/users/:id/call_queues/:queue_id/agent` to get the agent status of the specified call queue that a specified user belongs to.
* &#x20; Add `POST /api/users/:id/call_queues/:queue_id/agent` to configure the agent status of the specified call queue that a specified user belongs to.

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

**Date**: August 8, 2023

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

