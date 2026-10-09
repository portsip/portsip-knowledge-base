# Summary of Changes

### v22.8.0

Date: September 29, 2026

#### New Endpoints

**Data Flow Analytics**

* Added `<GET> /api/dataflow/status`: Retrieve the Dataflow server status.
* Added `<POST> /api/dataflow/analytics/calls/direction/summary`: Retrieve cumulative call direction metrics (inbound / outbound / internal) for a given absolute time range.
* Added `<POST> /api/dataflow/analytics/calls/duration/summary`: Retrieve the average duration of completed calls in a given absolute time range.

**IM Service**

* Added `<GET> /api/im/status`: Retrieve the IM server status.

**User Groups**

* Added `<GET> /api/groups/{id}/administrators`: List the administrators of a user group.
* Added `<POST> /api/groups/{id}/administrators/{extension_number}`: Add an administrator to a user group.
* Added `<POST> /api/groups/{id}/administrators/{extension_number}/destroy`: Remove an administrator from a user group.

**Tenant Integrations**

* Added `<GET> /api/ms365/users/details/{id}`: Retrieve Microsoft 365 user details.

**Users and Devices**

* Added `<GET> /api/user/groups/administered`: List the user groups administered by the current user.

**Executive Assistant**

* Added `<GET | POST> /api/user/assistants`: List / update the assistants of the current extension.
* Added `<GET | POST> /api/user/executives`: List / update the executives the current extension assists.
* Added `<GET | POST> /api/user/assistant_filters`: List / create assistant call filtering rules for the current extension.
* Added `<GET | POST> /api/user/assistant_filters/{id}`: Retrieve / update an assistant call filtering rule of the current extension.
* Added `<POST> /api/user/assistant_filters/{id}/destroy`: Delete an assistant call filtering rule of the current extension.
* Added `<GET | POST> /api/users/{id}/assistants`: List / update the assistants of the specified extension.
* Added `<GET | POST> /api/users/{id}/executives`: List / update the executives the specified extension assists.
* Added `<GET | POST> /api/users/{id}/assistant_filters`: List / create assistant call filtering rules for the specified extension.
* Added `<GET | POST> /api/users/{id}/assistant_filters/{filter_id}`: Retrieve / update an assistant call filtering rule of the specified extension.
* Added `<POST> /api/users/{id}/assistant_filters/{filter_id}/destroy`: Delete an assistant call filtering rule of the specified extension.

#### Updated Endpoints

**System Administration**

* Updated `<GET> /api/admin/ai_engine`: Removed properties `engine_type`, `config`, `id`; added properties `provider`, `opts`.
* Updated `<POST> /api/admin/ai_engine`: Removed properties `engine_type`, `config`; added properties `provider`, `opts`. The added `provider` property gains the new values `openai` and `openai-compatible` (previously `aws`, `azure`, `deepgram`).
* Updated `<POST> /api/admin/google/authorization`: New permission requirement `Integration.FullAccess`.
* Updated `<GET> /api/admin/password_policy`: Permission requirement removed (no permission required).
* Updated `<POST> /api/admin/password_policy`: New permission requirement `SystemSettings.FullAccess`.
* Updated `<GET | POST> /api/admin/settings`: Added property `hide_not_deployed_server_warnings`.
* Updated `<GET> /api/admin/status`: Added property `license_usage`.
* Updated `<GET> /api/admin/users`: Added property `timezone`.
* Updated `<POST> /api/admin/users`: Added property `timezone`.
* Updated `<GET> /api/admin/users/{id}`: Added property `timezone`.
* Updated `<POST> /api/admin/users/{id}`: Added property `timezone`.

**Call Queues**

* Updated `<GET> /api/call_queues`: Added properties `allow_busy_agent_calls`, `prompt_playback_delay`.
* Updated `<POST> /api/call_queues`: Added properties `allow_busy_agent_calls`, `prompt_playback_delay`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET | POST> /api/call_queues/{id}`: Added properties `allow_busy_agent_calls`, `prompt_playback_delay`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/call_queues/{id}/agents`: New License module requirement `cc` (scope: `target-extension`). New response codes `403` (`LICENSE_MODULE_UNAVAILABLE`, `LICENSE_TENANT_MODULE_REQUIRED`, `LICENSE_TARGET_MODULE_REQUIRED`) and `503` (`LICENSE_SNAPSHOT_UNAVAILABLE`).

**Conference Rooms**

* Updated `<POST> /api/conference_rooms`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET | POST> /api/conference_rooms/{id}`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.

**CRM**

* Updated `<GET | POST> /api/crm`: New License module requirement `cc` (scope: `tenant`).
* Updated `<GET | POST> /api/crm/contacts`: New License module requirement `cc` (scope: `tenant`).
* Updated `<POST> /api/crm/contacts/search`: New License module requirement `cc` (scope: `tenant`).
* Updated `<GET | POST> /api/crm/contacts/{id}`: New License module requirement `cc` (scope: `tenant`).
* Updated `<GET> /api/crm/contacts/{id}/calls`: New License module requirement `cc` (scope: `tenant`).
* Updated `<GET | POST> /api/crm/contacts/{id}/calls/{call_id}`: New License module requirement `cc` (scope: `tenant`).
* Updated `<GET | POST> /api/crm/contacts/{id}/notes`: New License module requirement `cc` (scope: `tenant`).
* Updated `<GET | POST> /api/crm/contacts/{id}/notes/{note_id}`: New License module requirement `cc` (scope: `tenant`).
* Updated `<POST> /api/crm/test`: New License module requirement `cc` (scope: `tenant`).

**Data Flow Analytics**

* Updated `<POST> /api/dataflow/queues/summary`: New License module requirement `cc` (scope: `actor-extension`).
* Updated `<POST> /api/dataflow/queues/timeseries`: New License module requirement `cc` (scope: `actor-extension`).
* Updated `<POST> /api/dataflow/queues/agent/summary`: New License module requirement `cc` (scope: `actor-extension`).
* Updated `<POST> /api/dataflow/queues/agent/timeseries`: New License module requirement `cc` (scope: `actor-extension`).

**Dealer**

* Updated `<GET> /api/dealer`: Added properties `max_cc_users`, `max_advanced_users`, `timezone`.
* Updated `<POST> /api/dealer`: Added property `timezone`.
* Updated `<GET> /api/dealer/status`: Added properties `license_type`, `license_usage`.
* Updated `<GET | POST> /api/dealers`: Added properties `max_cc_users`, `max_advanced_users`, `timezone`.
* Updated `<GET | POST> /api/dealers/{id}`: Added properties `max_cc_users`, `max_advanced_users`, `timezone`.
* Updated `<POST> /api/dealers/{id}/destroy`: New response code `409` (the Dealer still has child Dealers, `RESOURCE_IN_USE`).

**Tenant Integrations**

* Updated `<POST> /api/google/authorization`: New permission requirement `Integration.FullAccess`.
* Updated `<GET> /api/ms365/groups`: New permission requirement `Integration.ViewOnly`.
* Updated `<POST> /api/ms365/groups/get_many`: New permission requirement `Integration.ViewOnly`.

**User Groups**

* Updated `<GET> /api/groups`: Added properties `allow_admin_recordings`.
* Updated `<POST> /api/groups`: Added properties `allow_admin_recordings`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET | POST> /api/groups/{id}`: Added properties `allow_admin_recordings`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.

**Phone Provisioning and Hot Desking**

* Updated `<GET | POST> /api/hotdesking`: Added property `country_tone`.
* Updated `<GET | POST> /api/hotdesking/{id}`: Added property `country_tone`.
* Updated `<GET> /api/phone_models/{filename}`: Added property `country_tones`.

**Inbound Rules**

* Updated `<GET | POST> /api/inbound_rules`: Removed property `play_recording_disclaimer`.
* Updated `<GET | POST> /api/inbound_rules/{id}`: Removed property `play_recording_disclaimer`.

**System Information and Security**

* Updated `<GET> /api/info`: Added property `enable_im`.
* Updated `<GET> /api/sbc`: Permission requirement changed from `SystemSettings.FullAccess` to no permission required.
* Updated `<GET> /api/transports`: Permission requirement changed from `SystemSettings.ViewOnly` to no permission required.
* Updated `<GET> /api/transports/{id}`: Permission requirement changed from `SystemSettings.ViewOnly` to no permission required.
* Updated `<GET> /api/transports/{id}/status`: Permission requirement changed from `SystemSettings.ViewOnly` to no permission required.
* Updated `<GET> /api/audit_logs`: Enum value `MICROSOFT_INTEGRATIONS` of property `object_type` renamed to `MICROSOFT_INTEGRATION`.

**IVR**

* Updated `<POST> /api/ivrs`: Added properties `prompt_playback_delay`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET | POST> /api/ivrs/{id}`: Added properties `prompt_playback_delay`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.

**Licensing**

* Updated `<GET> /api/license`: Added response properties `max_online_users`, `license_type`, `license_status`, `base_max_users`, `cc_max_users`, `advanced_max_users`.
* Updated `<POST> /api/license`: New response code `5XX` (the Key was saved but the NATS notification failed).

**Mobile Push**

* Updated `<GET> /api/mobile_push`: Added response properties `ios_certificate`, `ios_private_key`.
* Updated `<POST> /api/mobile_push/{id}`: Removed request property `app_name`.

**Outbound Rules**

* Updated `<GET> /api/outbound_rules`: Added response property `extension_groups`; property `priority` gained a maximum value of `1000000`.
* Updated `<POST> /api/outbound_rules`: Property `priority` gained a maximum value of `1000000`.
* Updated `<GET> /api/outbound_rules/{id}`: Added response property `extension_groups`; property `priority` gained a maximum value of `1000000`.
* Updated `<POST> /api/outbound_rules/{id}`: Property `priority` gained a maximum value of `1000000`.

**SIP Trunk Providers**

* Updated `<GET | POST> /api/providers`: Added property `custom_headers`.
* Updated `<GET | POST> /api/providers/{id}`: Added property `custom_headers`.
* Updated `<GET> /api/providers/{id}/assignees`: Added property `tenant_name`.
* Updated `<GET> /api/providers/{id}/assignees/{tenant_id}`: Added properties `tenant_id`, `tenant_name`.
* Updated `<POST> /api/providers/{id}/assignees/{tenant_id}/destroy`: New response code `409` (`RESOURCE_IN_USE`).
* Updated `<POST> /api/providers/{id}/destroy`: New response code `409` (`RESOURCE_IN_USE`).

**Recordings**

* Updated `<GET> /api/recordings`: Added properties `caller_display_name`, `callee_display_name`; new query parameter `group_ids`; new response codes `400` (invalid `group_ids` value) and `403` (one or more requested groups are not authorized).
* Updated `<GET> /api/recordings/{id}`: Added properties `caller_display_name`, `callee_display_name`.

**Ring Groups**

* Updated `<POST> /api/ring_groups`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET | POST> /api/ring_groups/{id}`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.

**SMS**

* Updated `<POST> /api/sms`: New License module requirement `advanced` (scope: `tenant`).
* Updated `<POST> /api/sms/{id}`: New License module requirement `advanced` (scope: `tenant`).

**Tenant Management**

* Updated `<GET> /api/tenant` with the following new properties:
  * `dealer_id`, `dealer_name`, `dealer_display_name`, `dealer_level`, `max_cc_users`.
  * `max_advanced_users`, `enable_pre_call_announcement`, `enable_last_call_routing`, `last_call_routing_duration`, `ai_transcript`.
  * Added nested property `ai_engine.transcription_used_quota`; property `limit_app_logins.enable` renamed to `limit_app_logins.enabled`; added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/tenant`: Added properties `enable_last_call_routing`, `enable_pre_call_announcement`, `last_call_routing_duration`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET> /api/tenant/status`: Added properties `license_type`, `license_usage`.
* Updated `<GET> /api/tenants`: Added properties `dealer_id`, `dealer_name`, `dealer_display_name`, `dealer_level`, `max_cc_users`, `max_advanced_users`, `enable_last_call_routing`, `last_call_routing_duration`.
* Updated `<POST> /api/tenants`: Added properties `enable_pre_call_announcement`, `max_cc_users`, `max_advanced_users`, `enable_last_call_routing`, `last_call_routing_duration`; added nested property `ai_engine.transcription_used_quota`; property `limit_app_logins.enable` renamed to `limit_app_logins.enabled`.
* Updated `<GET> /api/tenants/{id}` with the following new properties:
  * `enable_pre_call_announcement`, `max_cc_users`, `max_advanced_users`, `enable_last_call_routing`, `last_call_routing_duration`.
  * `dealer_id`, `dealer_name`, `dealer_display_name`, `dealer_level`, `outbound_caller_ids[].provider_name`.
  * `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
  * Added nested property `ai_engine.transcription_used_quota`; property `limit_app_logins.enable` renamed to `limit_app_logins.enabled`.
* Updated `<POST> /api/tenants/{id}` with the following new properties:
  * `enable_pre_call_announcement`, `max_cc_users`, `max_advanced_users`, `enable_last_call_routing`, `last_call_routing_duration`.
  * `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
  * Added nested property `ai_engine.transcription_used_quota`; property `limit_app_logins.enable` renamed to `limit_app_logins.enabled`.
* Updated `<GET> /api/tenants/{id}/dealers`: Removed property `enabled`.
* Updated `<POST> /api/tenants/switch`: New permission requirement `SystemTenant.FullAccess`.

**Transcription**

* Updated `<POST> /api/transcription`: New License module requirement `advanced` (scope: `actor-extension`).

**Users and Devices**

* Updated `<GET> /api/user` with the following new properties:
  * `enable_cc`, `enable_advanced`, `executive_assistant`, `enable_assistant_opt_in_out`, `enable_assistant_call_filtering`.
  * `assistant_alerting_mode`, `assistant_timeout`, `assistant_timeout_action`, `assistant_timeout_number`, `enable_assistant_forwarding_filtered_calls`.
  * `assistant_forwarding_filtered_calls_to`, `assistant_caller_id_name_mode`, `assistant_caller_id_name_value`, `assistant_caller_id_number_mode`, `assistant_caller_id_number_value`.
  * `available_use_separate_fwd_rules_for_external`, `busy_use_separate_fwd_rules_for_external`, `dnd_use_separate_fwd_rules_for_external`, `away_use_separate_fwd_rules_for_external`, `lunch_use_separate_fwd_rules_for_external`.
  * `trip_use_separate_fwd_rules_for_external`, `offline_use_separate_fwd_rules_for_external`, `ext_available_office_hours_forward_rule`, `ext_available_non_office_hours_forward_rule`, `ext_available_no_answer_forward_rule`.
  * `ext_busy_office_hours_forward_rule`, `ext_busy_non_office_hours_forward_rule`, `ext_busy_no_answer_forward_rule`, `ext_dnd_office_hours_forward_rule`, `ext_dnd_non_office_hours_forward_rule`.
  * `ext_away_office_hours_forward_rule`, `ext_away_non_office_hours_forward_rule`, `ext_lunch_office_hours_forward_rule`, `ext_lunch_non_office_hours_forward_rule`, `ext_trip_office_hours_forward_rule`.
  * `ext_trip_non_office_hours_forward_rule`, `ext_offline_office_hours_forward_rule`, `ext_offline_non_office_hours_forward_rule`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`.
  * `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/user` with the following new properties:
  * `enable_cc`, `enable_advanced`, `executive_assistant`, `enable_assistant_opt_in_out`, `enable_assistant_call_filtering`.
  * `assistant_alerting_mode`, `assistant_timeout`, `assistant_timeout_action`, `assistant_timeout_number`, `enable_assistant_forwarding_filtered_calls`.
  * `assistant_forwarding_filtered_calls_to`, `assistant_caller_id_name_mode`, `assistant_caller_id_name_value`, `assistant_caller_id_number_mode`, `assistant_caller_id_number_value`.
  * `available_use_separate_fwd_rules_for_external`, `busy_use_separate_fwd_rules_for_external`, `dnd_use_separate_fwd_rules_for_external`, `away_use_separate_fwd_rules_for_external`, `lunch_use_separate_fwd_rules_for_external`.
  * `trip_use_separate_fwd_rules_for_external`, `offline_use_separate_fwd_rules_for_external`, `ext_available_office_hours_forward_rule`, `ext_available_non_office_hours_forward_rule`, `ext_available_no_answer_forward_rule`.
  * `ext_busy_office_hours_forward_rule`, `ext_busy_non_office_hours_forward_rule`, `ext_busy_no_answer_forward_rule`, `ext_dnd_office_hours_forward_rule`, `ext_dnd_non_office_hours_forward_rule`.
  * `ext_away_office_hours_forward_rule`, `ext_away_non_office_hours_forward_rule`, `ext_lunch_office_hours_forward_rule`, `ext_lunch_non_office_hours_forward_rule`, `ext_trip_office_hours_forward_rule`.
  * `ext_trip_non_office_hours_forward_rule`, `ext_offline_office_hours_forward_rule`, `ext_offline_non_office_hours_forward_rule`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`.
  * `outbound_caller_ids[].provider_did_numbers`.
  * New response code `403` (the current user lacks `User.FullAccess`, or the module quota is insufficient).
* Updated `<GET> /api/user/outbound_caller_ids`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET> /api/user/phones`: Added property `country_tone`.
* Updated `<POST> /api/user/phones`: Added property `country_tone`; new response code `409` (the phone MAC address is already bound to a Hot Desking device, `RESOURCE_IN_USE`).
* Updated `<GET | POST> /api/user/phones/{id}`: Added property `country_tone`.
* Updated `<GET> /api/user/recordings`: Added properties `caller_display_name`, `callee_display_name`.
* Updated `<GET> /api/user/recordings/{id}`: Added properties `caller_display_name`, `callee_display_name`.
* Updated `<POST> /api/user/transcription`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<GET> /api/user/meetings`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/user/meetings`: New License module requirement `advanced` (scope: `actor-extension`); added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET> /api/user/meetings/{id}`: Added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/user/meetings/{id}`: New License module requirement `advanced` (scope: `actor-extension`); added properties `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/user/meetings/{id}/lock`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/unlock`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/mute`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/unmute`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/start`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/start_recording`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/participants/invite`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/participants/layout`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/participants/{participant_id}/chairman`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/participants/{participant_id}/mute`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/participants/{participant_id}/unmute`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<POST> /api/user/meetings/{id}/participants/{participant_id}/position`: New License module requirement `advanced` (scope: `actor-extension`).
* Updated `<GET> /api/users`: Added properties `enable_cc`, `enable_advanced`.
* Updated `<POST> /api/users` with the following new properties:
  * `enable_cc`, `enable_advanced`, `executive_assistant`, `enable_assistant_opt_in_out`, `enable_assistant_call_filtering`.
  * `assistant_alerting_mode`, `assistant_timeout`, `assistant_timeout_action`, `assistant_timeout_number`, `enable_assistant_forwarding_filtered_calls`.
  * `assistant_forwarding_filtered_calls_to`, `assistant_caller_id_name_mode`, `assistant_caller_id_name_value`, `assistant_caller_id_number_mode`, `assistant_caller_id_number_value`.
  * `available_use_separate_fwd_rules_for_external`, `busy_use_separate_fwd_rules_for_external`, `dnd_use_separate_fwd_rules_for_external`, `away_use_separate_fwd_rules_for_external`, `lunch_use_separate_fwd_rules_for_external`.
  * `trip_use_separate_fwd_rules_for_external`, `offline_use_separate_fwd_rules_for_external`, `ext_available_office_hours_forward_rule`, `ext_available_non_office_hours_forward_rule`, `ext_available_no_answer_forward_rule`.
  * `ext_busy_office_hours_forward_rule`, `ext_busy_non_office_hours_forward_rule`, `ext_busy_no_answer_forward_rule`, `ext_dnd_office_hours_forward_rule`, `ext_dnd_non_office_hours_forward_rule`.
  * `ext_away_office_hours_forward_rule`, `ext_away_non_office_hours_forward_rule`, `ext_lunch_office_hours_forward_rule`, `ext_lunch_non_office_hours_forward_rule`, `ext_trip_office_hours_forward_rule`.
  * `ext_trip_non_office_hours_forward_rule`, `ext_offline_office_hours_forward_rule`, `ext_offline_non_office_hours_forward_rule`, `phones[].country_tone`, `outbound_caller_ids[].provider_name`.
  * `outbound_caller_ids[].provider_brand`, `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/users/create_many` with the following new properties:
  * `enable_cc`, `enable_advanced`, `executive_assistant`, `enable_assistant_opt_in_out`, `enable_assistant_call_filtering`.
  * `assistant_alerting_mode`, `assistant_timeout`, `assistant_timeout_action`, `assistant_timeout_number`, `enable_assistant_forwarding_filtered_calls`.
  * `assistant_forwarding_filtered_calls_to`, `assistant_caller_id_name_mode`, `assistant_caller_id_name_value`, `assistant_caller_id_number_mode`, `assistant_caller_id_number_value`.
  * `available_use_separate_fwd_rules_for_external`, `busy_use_separate_fwd_rules_for_external`, `dnd_use_separate_fwd_rules_for_external`, `away_use_separate_fwd_rules_for_external`, `lunch_use_separate_fwd_rules_for_external`.
  * `trip_use_separate_fwd_rules_for_external`, `offline_use_separate_fwd_rules_for_external`, `ext_available_office_hours_forward_rule`, `ext_available_non_office_hours_forward_rule`, `ext_available_no_answer_forward_rule`.
  * `ext_busy_office_hours_forward_rule`, `ext_busy_non_office_hours_forward_rule`, `ext_busy_no_answer_forward_rule`, `ext_dnd_office_hours_forward_rule`, `ext_dnd_non_office_hours_forward_rule`.
  * `ext_away_office_hours_forward_rule`, `ext_away_non_office_hours_forward_rule`, `ext_lunch_office_hours_forward_rule`, `ext_lunch_non_office_hours_forward_rule`, `ext_trip_office_hours_forward_rule`.
  * `ext_trip_non_office_hours_forward_rule`, `ext_offline_office_hours_forward_rule`, `ext_offline_non_office_hours_forward_rule`, `items[].phones[].country_tone`, `items[].outbound_caller_ids[].provider_name`.
  * `items[].outbound_caller_ids[].provider_brand`, `items[].outbound_caller_ids[].provider_did_numbers`.
* Updated `<GET> /api/users/{id}` with the following new properties:
  * `enable_cc`, `enable_advanced`, `executive_assistant`, `enable_assistant_opt_in_out`, `enable_assistant_call_filtering`.
  * `assistant_alerting_mode`, `assistant_timeout`, `assistant_timeout_action`, `assistant_timeout_number`, `enable_assistant_forwarding_filtered_calls`.
  * `assistant_forwarding_filtered_calls_to`, `assistant_caller_id_name_mode`, `assistant_caller_id_name_value`, `assistant_caller_id_number_mode`, `assistant_caller_id_number_value`.
  * `available_use_separate_fwd_rules_for_external`, `busy_use_separate_fwd_rules_for_external`, `dnd_use_separate_fwd_rules_for_external`, `away_use_separate_fwd_rules_for_external`, `lunch_use_separate_fwd_rules_for_external`.
  * `trip_use_separate_fwd_rules_for_external`, `offline_use_separate_fwd_rules_for_external`, `ext_available_office_hours_forward_rule`, `ext_available_non_office_hours_forward_rule`, `ext_available_no_answer_forward_rule`.
  * `ext_busy_office_hours_forward_rule`, `ext_busy_non_office_hours_forward_rule`, `ext_busy_no_answer_forward_rule`, `ext_dnd_office_hours_forward_rule`, `ext_dnd_non_office_hours_forward_rule`.
  * `ext_away_office_hours_forward_rule`, `ext_away_non_office_hours_forward_rule`, `ext_lunch_office_hours_forward_rule`, `ext_lunch_non_office_hours_forward_rule`, `ext_trip_office_hours_forward_rule`.
  * `ext_trip_non_office_hours_forward_rule`, `ext_offline_office_hours_forward_rule`, `ext_offline_non_office_hours_forward_rule`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`.
  * `outbound_caller_ids[].provider_did_numbers`.
* Updated `<POST> /api/users/{id}` with the following new properties:
  * `enable_cc`, `enable_advanced`, `executive_assistant`, `enable_assistant_opt_in_out`, `enable_assistant_call_filtering`.
  * `assistant_alerting_mode`, `assistant_timeout`, `assistant_timeout_action`, `assistant_timeout_number`, `enable_assistant_forwarding_filtered_calls`.
  * `assistant_forwarding_filtered_calls_to`, `assistant_caller_id_name_mode`, `assistant_caller_id_name_value`, `assistant_caller_id_number_mode`, `assistant_caller_id_number_value`.
  * `available_use_separate_fwd_rules_for_external`, `busy_use_separate_fwd_rules_for_external`, `dnd_use_separate_fwd_rules_for_external`, `away_use_separate_fwd_rules_for_external`, `lunch_use_separate_fwd_rules_for_external`.
  * `trip_use_separate_fwd_rules_for_external`, `offline_use_separate_fwd_rules_for_external`, `ext_available_office_hours_forward_rule`, `ext_available_non_office_hours_forward_rule`, `ext_available_no_answer_forward_rule`.
  * `ext_busy_office_hours_forward_rule`, `ext_busy_non_office_hours_forward_rule`, `ext_busy_no_answer_forward_rule`, `ext_dnd_office_hours_forward_rule`, `ext_dnd_non_office_hours_forward_rule`.
  * `ext_away_office_hours_forward_rule`, `ext_away_non_office_hours_forward_rule`, `ext_lunch_office_hours_forward_rule`, `ext_lunch_non_office_hours_forward_rule`, `ext_trip_office_hours_forward_rule`.
  * `ext_trip_non_office_hours_forward_rule`, `ext_offline_office_hours_forward_rule`, `ext_offline_non_office_hours_forward_rule`, `outbound_caller_ids[].provider_name`, `outbound_caller_ids[].provider_brand`.
  * `outbound_caller_ids[].provider_did_numbers`.
  * New response code `409` (`LICENSE_EXTENSION_IN_USE`, License allocation conflict when Contact Center cannot be disabled).
* Updated `<GET> /api/users/{id}/phones`: Added property `country_tone`.
* Updated `<POST> /api/users/{id}/phones`: Added property `country_tone`; new response code `409` (the phone MAC address is already bound to a Hot Desking device, `RESOURCE_IN_USE`).
* Updated `<GET | POST> /api/users/{id}/phones/{phone_id}`: Added property `country_tone`.

**WhatsApp**

* Updated `<POST> /api/whatsapp`: New License module requirement `advanced` (scope: `tenant`).
* Updated `<POST> /api/whatsapp/{id}`: New License module requirement `advanced` (scope: `tenant`).
* Updated `<POST> /api/whatsapp/{id}/templates/refresh`: New License module requirement `advanced` (scope: `tenant`); permission requirement changed from `Trunk.ViewOnly` to `Trunk.FullAccess`.

#### Global Error Codes

* Added `RESOURCE_IN_USE` for operations that conflict with an existing resource reference or binding.

#### Removed Endpoints

* Removed `<GET> /api/google/directory`.
* Removed `<GET> /api/google/directory/{id}`.
* Removed `<POST> /api/google/directory/get_many`.

***

### v22.6.0

Date: June 30, 2026

#### New Endpoints

#### System Administration

* Added `<GET | POST> /api/admin/google/destroy` to stop the system-level Google application integration.
* Added `<POST> /api/admin/ms365/destroy` to stop the system-level Microsoft 365 application integration.
* Added `<GET | POST> /api/admin/password_policy` to manage password policies for system users.
* Added `<GET> /api/admin/email_templates` and `<GET | POST> /api/admin/email_templates/:name` to manage system-level email notification templates.

#### Dealer

* Added `<GET> /api/dealer/password_policy` to retrieve the password policy for the currently signed-in Dealer.

#### WhatsApp

* Added `<GET> /api/whatsapp/:id/templates`, `<GET> /api/whatsapp/:id/templates/:template_id`, and `<POST> /api/whatsapp/:id/templates/refresh` to manage WhatsApp Templates.

#### Tenant Integrations

* Added `<POST> /api/ms365/destroy` to stop the tenant-level Microsoft 365 application integration.
* Added `<POST> /api/google/destroy` to stop the tenant-level Google application integration.

#### Tenant Management

* Added `<POST> /api/tenants/summaries/query` to retrieve tenant statistics in bulk.

#### CRM

* Added `<GET> /api/crm/providers/:provider` to retrieve property mappings for CRM application integrations.

#### Data Flow Analytics

* Added `<POST> /api/dataflow/analytics/activity/users` and `<POST> /api/dataflow/analytics/activity/user` to retrieve user activity records from Data Flow for a specified time range. These APIs support querying activity for specified users or for the currently signed-in user.

#### Updated Endpoints

#### System Administration and Security

* Updated `<GET | POST> /api/admin/notification`:
  * Renamed `notify_service_disconnected` to `notify_service_status_changed`.
  * Added `notify_trunk_status_changed`.
* Updated `<GET | POST> /api/admin/settings` with the following new properties:
  * `enable_two_factor_authentication`
  * `enable_verify_phone_auto_provisioning`
  * `verify_phone_auto_provisioning_exclude`
  * `enable_sip603_reason_header`
  * `hide_pbx_private_ip`
  * `sdp_codecs_scope`
  * `sdp_codecs`
* Updated `<POST> /api/auth/forget_password`:
  * The `domain` property is now optional when the API is called by a system user.
* Updated `<POST> /api/admin/users`:
  * `email` is now required.
  * `display_name` is now required.
  * The maximum length of `display_name` is 128 characters.
* Updated `<POST> /api/dealers`:
  * `email` is now required.
  * `display_name` is now required.

#### Call Parking

* Updated `<GET | POST> /api/call_park`:
  * Added `send_notification_to_parker`.

#### Call Queues

* Updated `<GET | POST> /api/call_queues`:
  * Added `enable_audio_recording`.
* Updated `<POST> /api/call_queues`:
  * Added `outbound_caller_ids[].display_name`.
* Updated `<GET | POST> /api/call_queues/:id`:
  * Added `enable_audio_recording`.
  * Added `outbound_caller_ids[].display_name`.

#### Conference Rooms and Meetings

* Updated `<POST> /api/conference_rooms` and `<POST> /api/conference_rooms/:id`:
  * Added `prompt_file_id`.
  * Added `outbound_caller_ids[].display_name`.
* Updated `<GET> /api/conference_rooms/:id`:
  * Added `prompt_file_name`.
  * Added `prompt_file_size`.
  * Added `prompt_file_url`.
  * Added `outbound_caller_ids[].display_name`.
* Updated `<POST> /api/user/meetings` and `<GET | POST> /api/user/meetings/:id`:
  * Added `outbound_caller_ids[].display_name`.

#### CRM

* Updated `<GET> /crm/contacts` and `<GET> /crm/contacts/:id`:
  * Added `type`.
  * Added `url`.
* Updated `<POST> /api/user/crm_contacts/sync_tokens/:token/diff`:
  * Added `did_number`.

#### DECT Phones and Hot Desking

* Updated `<POST> /api/dect_phones` and `<POST> /api/dect_phones/:id`:
  * Added `provisioning_pin`.
* Updated `<GET> /api/dect_phones` and `<GET> /api/dect_phones/:id`:
  * Added `provisioning_pin`.
  * Added `firmware`.
  * Added `status`.
* Updated `<GET | POST> /api/hotdesking` and `<GET | POST> /api/hotdesking/:id`:
  * Added `provisioning_pin`.
  * `display_name` is now required.
  * The maximum length of `display_name` is 128 characters.

#### Feature Access Codes

* Updated `<GET | POST> /api/feature_access_codes`:
  * Removed the following `feature` values:
    * `ANONYMOUS_CALL_REJECTION_ACTIVATION`
    * `ANONYMOUS_CALL_REJECTION_DEACTIVATION`
  * Added the following `feature` values:
    * `SELECTIVE_CALL_ACCEPTANCE_ACTIVATION`
    * `SELECTIVE_CALL_ACCEPTANCE_DEACTIVATION`
    * `SELECTIVE_CALL_REJECTION_ACTIVATION`
    * `SELECTIVE_CALL_REJECTION_DEACTIVATION`

#### Inbound Rules, Outbound Rules, Virtual Receptionists, Ring Groups, and Groups

* Updated `<GET | POST> /api/inbound_rules` and `<GET | POST> /api/inbound_rules/:id`:
  * Added `enable_audio_recording`.
* Updated `<GET | POST> /api/ivrs` and `<GET | POST> /api/ivrs/:id`:
  * Added `enable_audio_recording`.
* Updated `<POST> /api/ivrs` and `<GET | POST> /api/ivrs/:id`:
  * Added `outbound_caller_ids[].display_name`.
* Updated `<GET | POST> /api/outbound_rules` and `<GET | POST> /api/outbound_rules/:id`:
  * Added `enable_audio_recording`.
* Updated `<GET | POST> /api/ring_groups` and `<GET | POST> /api/ring_groups/:id`:
  * Added `enable_audio_recording`.
* Updated `<POST> /api/ring_groups` and `<GET | POST> /api/ring_groups/:id`:
  * Added `outbound_caller_ids[].display_name`.
* Updated `<POST> /api/groups` and `<GET | POST> /api/groups/:id`:
  * Added `outbound_caller_ids[].display_name`.

#### Microsoft 365 and Google Integrations

* Updated `<GET | POST> /api/ms365`:
  * Removed `sign_in_as_administrator`.

#### Network

* Updated `<GET> /api/network`:
  * Added `sbc_domain`.

#### Phones and Provisioning

* Updated `<GET> /api/phones` and `<GET> /api/phones/:mac`:
  * Added `status`.
* Updated `<GET | POST> /api/users/:id/phones` and `<GET | POST> /api/users/:id/phones/:phone_id`:
  * Added `dhcp`.

#### Providers and SDP Codec Configuration

* Updated `<GET | POST> /api/providers` and `<GET | POST> /api/providers/:id`:
  * Added `enable_sdp_codecs`.
  * Added `sdp_codecs`.

#### SMS and WhatsApp

* Updated `<GET | POST> /api/sms` and `<GET | POST> /api/sms/:id`:
  * Added `channel_type`.
* Updated `<GET | POST> /api/whatsapp` and `<GET | POST> /api/whatsapp/:id`:
  * Added `business_account_id`.

#### Tenant APIs

* Updated `<GET | POST> /api/tenant` with the following new properties:
  * `enable_call_queue_audio_recording`
  * `enable_ring_group_audio_recording`
  * `enable_trunk_audio_recording`
  * `enable_virtual_receptionist_audio_recording`
  * `enable_verify_phone_auto_provisioning`
  * `crm_contact_type`
  * `recording_access_mode`
  * `outbound_caller_ids[].display_name`
* Updated `<GET | POST> /api/tenant/notification` with the following new properties:
  * `notify_emergency_call`
  * `notify_license_limited`
  * `notify_trunk_call_limited`
* Updated `<POST> /api/tenants` with the following new properties:
  * `enable_call_queue_audio_recording`
  * `enable_ring_group_audio_recording`
  * `enable_trunk_audio_recording`
  * `enable_virtual_receptionist_audio_recording`
  * `enable_verify_phone_auto_provisioning`
  * `recording_access_mode`
* Updated `<POST> /api/tenants`:
  * The `website` property must now be between 1 and 128 characters.
* Updated `<GET | POST> /api/tenants/:id` with the following new properties:
  * `enable_call_queue_audio_recording`
  * `enable_ring_group_audio_recording`
  * `enable_trunk_audio_recording`
  * `enable_virtual_receptionist_audio_recording`
  * `enable_verify_phone_auto_provisioning`
  * `recording_access_mode`
  * `outbound_caller_ids[].display_name`

#### User and Extension APIs

* Updated `<GET | POST> /api/user` with the following new properties:
  * `selective_call_acceptations`
  * `selective_call_rejections`
  * `timezone`
  * `enable_selective_call_acceptation`
  * `enable_selective_call_rejection`
* Updated `<POST> /api/users` and `<POST> /api/users/create_many` with the following new properties:
  * `selective_call_acceptations`
  * `selective_call_rejections`
  * `timezone`
  * `enable_selective_call_acceptation`
  * `enable_selective_call_rejection`
  * `outbound_caller_ids[].display_name`
* Updated `<POST> /api/users` and `<POST> /api/users/create_many`:
  * `display_name` is now required.
  * The maximum length of `display_name` is 128 characters.
* Updated `<GET> /api/users`:
  * Added `timezone`.
* Updated `<GET | POST> /api/users/:id` with the following new properties:
  * `selective_call_acceptations`
  * `selective_call_rejections`
  * `timezone`
  * `enable_selective_call_acceptation`
  * `enable_selective_call_rejection`
  * `outbound_caller_ids[].display_name`
* Updated `<GET | POST> /api/users/:id`:
  * `display_name` is now required.
  * The maximum length of `display_name` is 128 characters.
* Updated `<GET> /api/user/outbound_caller_ids`:
  * Added `items[].display_name`.

#### Voicemail and Shared Voicemail

* Updated `<GET> /api/voicemails` and `<GET> /api/voicemails/:id`:
  * Added `sender_name`.
  * Added `sender_number`.
  * Marked `sender` as deprecated.
* Updated `<GET> /api/shared_voicemails/:id/voicemails` and `<GET> /api/shared_voicemails/:id/voicemails/:mail_id`:
  * Added `sender_name`.
  * Added `sender_number`.
  * Marked `sender` as deprecated.

#### Compatibility Notes

* The `sender` property in voicemail and shared voicemail APIs is deprecated. Use `sender_name` and `sender_number` instead.
* The `sign_in_as_administrator` property has been removed from `<GET | POST> /api/ms365`.
* The Feature Access Code values `ANONYMOUS_CALL_REJECTION_ACTIVATION` and `ANONYMOUS_CALL_REJECTION_DEACTIVATION` have been removed.
* Several APIs now require `display_name`, with a maximum length of 128 characters.
* The tenant `website` property is now required and must be between 1 and 128 characters.

***

### Version: v22.5.0

Date: March 27, 2026

#### Updated endpoints/fields

* Modify `api/call_queues/:id/agents/:agent_number` to add the `reason` field in the response.
* Modify `api/user/call_queues/{queue_id}/agent` to add the `reason` field in the response.

***

#### v22.4.0

Date: February 10, 2026

#### **New Endpoints:**

1. `/api/templates/soft_phone GET`
2. `/api/templates/soft_phone POST`
3. `/api/templates/soft_phone/{filename} POST`
4. `/api/templates/soft_phone/{filename}/destroy POST`
5. `/api/templates/soft_phone/{file_name} GET`

#### **Updated Endpoints/Fields:**

1. **`/api/admin/ai_engine GET`**
   * Added Deepgram type.
2. **`/api/tenant GET`**
   * Added `soft_phone_template` field.
3. **`/api/tenant POST`**
   * Added `soft_phone_template` field.
4. **`/api/tenants/{id} GET`**
   * Added `soft_phone_template` field.
5. **`/api/tenants POST`**
   * Added `soft_phone_template` field.
6. **`/api/tenants/{id} POST`**
   * Added `soft_phone_template` field.
7. **`/api/users/{id} POST`**
   * Added `soft_phone_template` field.
8. **`/api/users POST`**
   * Added `soft_phone_template` field.
9. **`/api/users/{id} GET`**
   * Added `soft_phone_template` field.
10. **`/api/crm`** (related interfaces)
    * Added support for Odoo provider.
11. **`/api/mobile_push GET`**
    * Added `ios_cert_expire_at`, `cert_remaining_days` fields.
12. **`/api/mobile_push/{id} GET`**
    * Added `ios_cert_expire_at` field.
13. **`/api/providers POST`**
    * Added `force_outbound_proxy` field.
14. **`/api/providers/{id} POST`**
    * Added `force_outbound_proxy` field.
15. **`/api/providers/{id} GET`**
    * Added `force_outbound_proxy` field.
16. **`/api/dataflow/analytics/calls/user/history GET`**
    * Changed the `direction` field to `party` and added `direction` field.
17. **`/api/dataflow/analytics/calls/history GET`**
    * Added `direction` field.

***

### v22.3.0

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

#### Updated endpoints/fields

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

