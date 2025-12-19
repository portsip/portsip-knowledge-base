# Summary of Changes

### PortSIP PBX REST API Changes Summary

Version: v22.3.0

Date: December 19, 2025

#### New endpoints

* Added `GET/POST /api/admin/users` to create and list **system-wide users**.
* Added `GET/POST /api/admin/users/:id` to retrieve and update a **system-wide user** by ID.
* Added `POST /api/admin/users/:id/username` to change the username of a **system-wide user**.
* Added `POST /api/admin/users/:id/password` to change the password of a **system-wide user**.
* Added `POST /api/admin/users/:id/role` to update the role of a **system-wide user**.
* Added `POST /api/admin/users/:id/destroy` to remove a **system-wide user**.
* Added `GET/POST /api/admin/user` to retrieve and update the **currently logged-in system user**.
* Added `POST /api/admin/user/username` to change the username of the **currently logged-in system user**.
* Added `POST /api/admin/user/password` to change the password of the **currently logged-in system user**.
* Added `GET/POST /api/admin/ai_engine` to manage **system-wide AI transcription integration**.
* Added `POST /api/user/transcription` to create a **recording transcription** for the currently logged-in user.
* Added `GET/POST /api/user/transcription/:id` to retrieve and update a **user transcription** by ID.
* Added `POST /api/user/crm_contacts/:id/favorite` to mark a CRM contact as **favorite**.
* Added `POST /api/user/crm_contacts/:id/unfavorite` to remove **favorite** from a CRM contact.
* Added `POST /api/user/crm_contacts/sync_tokens` to create a **sync token** for CRM contacts.
* Added `GET /api/user/crm_contacts/sync_tokens/:token/diff` to fetch **CRM contact changes (diff)** using a sync token.
* Added `POST /api/users/create_many` to **bulk create extension users**.
* Added `POST /api/users/status/get_many` to **bulk query current extension user status**.
* Added `POST /api/transcription` to create **tenant-level recording transcriptions**.
* Added `GET/POST /api/transcription/:id` to retrieve and update a **tenant transcription** by ID.
* Added `POST /api/call_queues/:id/agents/status/get_many` to **bulk query call queue agent status**.
* Added `GET/POST /api/crm` to retrieve and update **tenant CRM integration configuration**.
* Added `GET /api/crm/providers` to retrieve metadata for **available CRM providers**.
* Added `POST /api/crm/test` to test **CRM provider configuration**.
* Added `GET/POST /api/crm/contacts` to create and list **CRM contacts**.
* Added `POST /api/crm/contacts/search` to search **CRM contacts**.
* Added `GET/POST /api/crm/contacts/:id` to retrieve and update a **CRM contact** by ID.
* Added `POST /api/crm/contacts/:id/destroy` to remove a **CRM contact** by ID.
* Added `GET/POST /api/crm/contacts/:id/notes` to create and list **notes** for a CRM contact.
* Added `GET/POST /api/crm/contacts/:id/notes/:note_id` to retrieve and update a **specific note**.
* Added `POST /api/crm/contacts/:id/notes/:note_id/destroy` to remove a **specific note**.
* Added `GET /api/crm/contacts/:id/calls` to list **call records** for a CRM contact.
* Added `GET/POST /api/crm/contacts/:id/calls/:call_id` to retrieve and update a **specific call record**.
* Added `GET/POST /api/not_ready_codes` to manage **Not Ready Codes**.
* Added `POST /api/dataflow/queues/summary` to retrieve **current queue KPI summary**.
* Added `POST /api/dataflow/queues/timeseries` to retrieve **queue call timeseries data** for the past N hours.
* Added `POST /api/dataflow/queues/agent/summary` to retrieve an **agent summary** for a specific queue and time range.
* Added `POST /api/dataflow/queues/agent/timeseries` to retrieve **agent timeseries (bucketed) data** for a specific queue and time range.
* Added `POST /api/dataflow/analytics/calls/timeseries` to retrieve **call volume timeseries analytics** for a specified range.
* Added `POST /api/dataflow/analytics/calls/history` to query **Call History** by conditions.
* Added `POST /api/dataflow/analytics/calls/user/history` to query the **current user’s Call History** by conditions.
* Added `POST /api/dataflow/analytics/calls/history/:session_id` to query **Call History details** for a session.
* Added `POST /api/dataflow/analytics/calls/user/history/:session_id` to query **current user’s Call History details** for a session.
* Added report job APIs:
  * Added `POST /api/dataflow/report_jobs` to create a **report job**.
  * Added `POST /api/dataflow/report_jobs/:id` to update a **report job**.
  * Added `GET /api/dataflow/report_jobs/:id` to retrieve a **report job**.
  * Added `POST /api/dataflow/report_jobs/:id/destroy` to delete a **report job**.
  * Added `GET /api/dataflow/report_jobs/:id/detail` to retrieve **report job details**.
  * Added `GET /api/dataflow/report_list` to retrieve the **completed report list**.
  * Added `POST /api/dataflow/report_list/:id/destroy` to delete a **report item**.

#### Updated endpoints / fields

* Updated `GET /api/auth/user`: added `domain`, `role_id`, `role_name`, and `capabilities`.
* Updated `/api/call_queues` and `/api/call_queues/:id`:
  * Added `enable_night_mode`, `play_periodic_music`, `play_periodic_music_interval`,\
    `enable_wrap_up`, `wrap_up_time`,\
    `enable_exit_forward_rule`, `exit_forward_rule`.
  * Updated `status`: removed optional values `BREAK` and `LUNCH`.
* Updated `/api/inbound_rules` and `/api/inbound_rules/:id`: added `usage`.
* Updated `/api/ivrs` and `/api/ivrs/:id`: added `enable_night_mode`, `exclude_list`.
* Updated `/api/providers` and `/api/providers/:id`: added `remove_plus_prefix`, `tel_uri_scheme_for_request_line`, `tel_uri_scheme_for_to_header`.
* Updated `/api/recordings`, `/api/recordings/:id`, `/api/user/recordings`, `/api/user/recordings/:id`:
  * Added AI transcription fields: `status`, `status_description`, `sentiment`.
* Updated `/api/ring_groups` and `/api/ring_groups/:id`: added `enable_night_mode`.
* Updated `/api/roles`: added `capabilities`.
* Updated role APIs to use `id` instead of `name` in the path:
  * Updated `GET /api/roles/:name` → path parameter changed from `name` to `id`; added `id`, `protected`, `capabilities`.
  * Updated `POST /api/roles/:name` → path parameter changed from `name` to `id`; added `capabilities`.
  * Updated `/api/roles/:name/destroy` → path parameter changed from `name` to `id`.
* Updated `GET /api/tenant`:
  * Added `ai_engine`, `limit_app_logins`, `allow_password_retrieval`, `contact_inbound_rule_append_type`, `crm_provider`.
  * Updated `password_policy`.
* Updated `POST /api/tenant`:
  * Added `contact_inbound_rule_append_type`, `ai_transcript`.
  * Updated `password_policy`.
* Updated `GET /api/tenants`: added `current_extensions`; updated `password_policy`.
* Updated `POST /api/tenants`:
  * Added `limit_app_logins`, `allow_password_retrieval`, `ai_engine`, `contact_inbound_rule_append_type`.
  * Updated `password_policy`.
* Updated `GET /api/tenants/:id`: added `limit_app_logins`, `ai_engine`, `allow_password_retrieval`, `contact_inbound_rule_append_type`.
* Updated `POST /api/tenants/:id`: added `ai_engine`, `limit_app_logins`, `allow_password_retrieval`, `contact_inbound_rule_append_type`.
* Updated `GET /api/user`: added `role_id`, `role_name`, `enable_ai_transcript`, `enable_uc_app`, `enable_teams_phone_app`.
* Updated `POST /api/user`: added `enable_ai_transcript`, `enable_uc_app`, `enable_teams_phone_app`.
* Updated `POST /api/user/call_queues/:id/agent`: added `reason`.
* Updated `GET /api/users`: added `role_id`, `role_name`.
* Updated `POST /api/users`: added `enable_ai_transcript`, `enable_uc_app`, `enable_teams_phone_app`, `phones`.
* Updated `GET /api/users/:id`: added `role_id`, `role_name`, `enable_ai_transcript`, `enable_uc_app`, `enable_teams_phone_app`, `password`, `extension_password`.
* Updated `POST /api/users/:id`: added `enable_ai_transcript`, `enable_uc_app`, `enable_teams_phone_app`.
* Updated `/api/users/:id/phones` and `/api/users/:id/phones/:phone_id`: added `all_codecs`.
* Updated `/api/cdrs`, `/api/cdrs/:id`, `/api/user/cdrs`, `/api/user/cdrs/:id`, `/api/calllogs`, `/api/calllogs/:id`:
  * Expanded `reroute_reason` enum values to include:\
    `CAPACITY_BLOCKED`, `QUEUE_EXIT`, `QUEUE_ABANDONED`, `REDIRECT`,\
    `FORWARDING_UNCONDITIONAL`, `FORWARDING_BUSY`, `FORWARDING_NO_ANSWER`,\
    `FORWARDING_NOT_REACHABLE`, `FORWARDING_EXCEPTION`,\
    `FORWARDING_LUNCH`, `FORWARDING_BUSINESS_TRIP`, `FORWARDING_AWAY`, `FORWARDING_NIGHT_MODE`.
* Updated `/api/ivrs`, `/api/ivrs/:id`, `/api/shared_voicemails`, `/api/shared_voicemails/:id`:
  * `pin` length is now determined by the **tenant password policy**.
* Updated `/api/user`, `/api/users`, `/api/users/:id`:
  * `pin` and `voicemail_pin` length is now determined by the **tenant password policy**.
* Updated `/api/users/:id/status` and `/api/user/status`:
  * Updated `status`: removed `ON_CALL`, added `OTHER_CALL`.
  * Updated `presence`: removed `AVAILABLE` and `ON_CALL`; added `ONLINE`, `OTHER_CALL`, `RINGING`.
* Updated `/api/users/export`: permission requirement changed to `User.FullAccess`.
* Updated `/api/providers/export`: permission requirement changed to `Trunk.FullAccess`.
* Updated `/api/ip_filters/export`: permission requirement changed to `SystemSettings.FullAccess`.
* Updated `/api/exclusive_numbers`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/vip_numbers`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/call_queue_blacklisted_numbers`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/contacts`: permission requirement changed to `CompanyContact.FullAccess`.
* Updated `/api/inbound_rules`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/outbound_rules`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/disallowed_codes`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/blacklisted_numbers`: permission requirement changed to `PhoneSystem.FullAccess`.
* Updated `/api/call_rates`: permission requirement changed to `Billing.FullAccess`.
* Updated `/api/users/:id` restrictions:
  * Only **system-wide users** and **tenant-scope Admin-role users** may update an Admin user’s:\
    enable/disable flag, user password, SIP password, and PIN.
* Updated `/api/users/:id/destroy` restrictions:
  * Only **system-wide users** may delete Admin-role users.
* Updated `/api/voicemails`: added AI transcription fields `transcription_status`, `transcription_status_desc`, `transcription_sentiment`.
* Updated `/api/recordings/:id`: added AI transcription fields `status`, `status_description`, `sentiment`.
* Updated `/api/sms`: added `prepend_add`.

#### Removed endpoints

* Removed `/api/admin/username`.
* Removed `/api/admin/password`.

