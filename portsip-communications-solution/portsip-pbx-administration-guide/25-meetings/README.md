# 25 Meetings

After the PortSIP PBX is successfully installed, you can create and manage meetings directly from the Web Portal.

#### Prerequisites

* You are signed in as a **Tenant Admin**
* PortSIP PBX installation is complete

***

#### Steps to Create a Meeting

1. **Open the Meeting Configuration Page**
   * Navigate to **Advanced Services > Meeting**.
   * Click **Add**.
2. **Basic Meeting Information**
   * **Topic**\
     Enter a meaningful meeting topic to help participants identify the purpose of the meeting.
   *   **Meeting Extension**\
       Enter a unique meeting extension number that participants will dial to join the meeting.

       > This extension must not conflict with any existing extension.
   * **Meeting Mode**\
     Select the desired meeting mode from the **Meeting Mode** drop-down list.
3. **Security Settings**
   * **Meeting PIN (Optional)**\
     If set, participants must enter this PIN to join the meeting.
   * **Admin PIN**\
     Enter the PIN for the meeting host.\
     Users who join using this PIN are identified as **meeting administrators** and can manage the meeting.
4. **Participant and Video Settings**
   * **Maximum Participants**\
     Specify the maximum number of participants allowed to join the meeting.
   * **Grids for Video Meeting**\
     Specify how many video streams are displayed simultaneously.\
     Supported values: `auto`, `1`, `2`, `3`, `4`, `6`, `9`
   * **Bitrate (kbps)**\
     Set the video bandwidth usage.
     * Range: **128 kbps – 8196 kbps**
     * Higher values provide better video quality.
   * **Frame Rate**\
     Select a frame rate between **5 and 30 fps**.\
     Higher values provide smoother video playback.
   * **Video Resolution**\
     Configure video height and width:
     * **720p**: 720 × 1280
     * **1080p**: 1080 × 1920
5. **Language and Time Settings**
   * **Prompt Language**\
     Select the language used for voice prompts when participants enter the meeting.
   *   **Time Zone**\
       Choose the meeting time zone.

       > The tenant’s time zone is used by default.
   * **Start Time / End Time**\
     Set the scheduled start and end times for the meeting.
6. **PSTN Dial-In (Optional)**
   *   **Dial from Landline / Mobile Phone**\
       Specify one or more DID numbers that participants can dial from the PSTN to join the meeting.\
       Example formats:

       * `+44 1494 523 500` (UK)
       * `+33 2022 123 123` (FR)

       > Ensure inbound rules are configured to route these DID numbers to the meeting.
7. **Inviting Participants**
   *   **Invitees (Email)**\
       Enter participant email addresses in the **Invitees** field.\
       The PBX sends meeting invitations containing a join link.

       > For more information, see [Joining a Meeting with the Invite Link](joining-a-meeting-with-the-invite-link.md).
8. **Optional Features**
   * **Record Meeting Automatically**\
     Enable this option to start recording as soon as the meeting begins.
   * **Play Welcome Message When Joining Meeting**\
     Plays an audio welcome message when participants join.
   *   **Outbound Caller ID**\
       When inviting external participants, the PBX places outbound calls through a configured SIP trunk using this caller ID.\
       This caller ID can replace specific SIP fields.

       > For details, see [Configurable SIP Fields](../7-trunk-management/handle-outbound-calls-through-sip-trunk.md#configurable-sip-fields).
9. **Save the Meeting**
   * Click **OK** or **Save** to create the meeting.

***

### Managing Meetings

Once a meeting has been created, tenant administrators and meeting hosts can join, invite participants, and manage the meeting and its participants from the PortSIP PBX Web Portal.

***

### Joining a Meeting

After a meeting is created, share the **Meeting Extension** with participants.

* Participants can join the meeting by dialing the meeting extension from any SIP client.
* Example:\
  If the meeting extension is set to **8008**, participants can join the meeting by dialing **8008**.

***

### Inviting Participants

In addition to dialing the meeting extension, participants can also be invited directly by the meeting host or administrator.

* Extensions within the PBX can be invited directly.
* External participants using mobile phones or landline phones can also be invited, provided:
  * A SIP trunk is configured
  * Appropriate outbound routing rules are in place

See **Managing Meeting Participants** below for details.

***

### Managing Meetings

To manage existing meetings:

1. Navigate to **Advanced Services > Meeting**.
2. The meeting list displays all available meetings.
3. Select a meeting to perform one of the following actions:

#### Available Actions

* **Edit**\
  Modify the meeting configuration, such as topic, PINs, participant limits, or video settings.
* **Manage**\
  Manage the meeting in real time, including participants and meeting controls.
* **Delete**\
  End and permanently delete the meeting.

***

### Managing Meeting Participants

To manage participants in a meeting:

1. Select a meeting from the meeting list.
2. Click **Manage** to open the meeting control panel.

<figure><img src="../../../.gitbook/assets/meeting_participants.png" alt=""><figcaption></figcaption></figure>

#### Meeting-Level Controls

* **Invite Participant**
  * Click **Invite** to select an extension from the list or enter an extension number manually.
  * The PBX places a call to the specified extension.
  * Once answered, the participant is automatically joined to the meeting.
  * Mobile or PSTN numbers can also be entered, provided outbound calling is properly configured.
* **Lock**\
  Locks the meeting to prevent additional participants from joining.
* **Record**\
  Start or stop meeting recording.
* **Mute**\
  Mute or unmute all participants in the meeting.
* **Recording Files**\
  View and download the meeting’s recording files.
* **Refresh**\
  Refresh the meeting status and participant information.

***

### Managing Individual Participants

For each participant listed in the meeting, the **Action** column provides controls to manage that participant:

* **Host**\
  Set the participant’s video as the main video displayed in the meeting.
* **Mute / Unmute**\
  Click the microphone icon to mute or unmute the selected participant.
* **Hang Up**\
  Remove the participant from the meeting



