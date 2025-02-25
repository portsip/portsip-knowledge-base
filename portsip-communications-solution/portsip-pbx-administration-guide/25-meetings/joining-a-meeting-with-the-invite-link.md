# Joining a Meeting with the Invite Link

This article explains how to create a meeting in PortSIP PBX and send the meeting invitation to the invitees.

To join a PortSIP meeting, you may receive an invite link through email, message, or directly from the meeting host. The invite link is a web address that includes the meeting ID and can often include an embedded passcode, allowing participants to join more quickly. When you click the invite link, your web browser will display a dialog box to allow you to join the meeting.

## Prerequisites

* Ensure that PortSIP SBC is installed and configured for WebRTC. Please refer to this article: [Configuring SBC for WebRTC](../9-configuring-portsip-sbc/).
* If you want to send the meeting invitation via email, ensure that the SMTP server is configured as described in this article: [Configuring Email Notifications](../31-configuring-email-notifications.md).

## Create Meeting

To create a meeting in PortSIP PBX and send the invitation to the invitees, follow these steps:

1. Sign in as the tenant admin.
2. Select **Advanced Services** > **Meeting**.
3. Click **Add**.
4. Enter the necessary information for the meeting.

<figure><img src="../../../.gitbook/assets/create_meeting.png" alt="" width="375"><figcaption></figcaption></figure>

After clicking the OK button, a dialog box containing meeting details will appear, as shown in the screenshot below:

<figure><img src="../../../.gitbook/assets/meeting_invitation.png" alt=""><figcaption></figcaption></figure>

You can copy the meeting information by clicking the **Copy Invitation** button and send it directly to invitees. If the SMP server is configured, the PBX will automatically send a meeting invitation to invitees via email after you click the **OK** button.

## Joining the Meeting by Click the Link

To join the meeting, the invitee clicks the link in the invitation. The browser will automatically open the link and prompt you to choose your devices, enter your display name, select the correct speaker, microphone, and camera, and then click the **Join** button.

<figure><img src="../../../.gitbook/assets/link_meeting.png" alt="" width="375"><figcaption></figcaption></figure>

## Join the Meeting by Dialing the Meeting Number

As shown in the example above, to join the meeting, invitees can dial the number **110200800** from an IP phone or PortSIP app (mobile, Windows, WebRTC client).

## Join the Meeting by Dialing Number from PSTN

To join the meeting, invitees can dial the number **+44 1494 523 500** from a mobile phone or landline in the UK, or dial the number **+33 2022 123 123** from a mobile phone or landline in France.

To enable this feature, the PBX must configure the SIP Trunk and create inbound rules to route the DID numbers **44 1494 523 500** and **33 2022 123** **123** to the meeting number **110200800**. Once someone dials either of these numbers, the PBX will route the call to the meeting.

Alternatively, you can create an inbound rule to route these numbers to a virtual receptionist. When someone dials either **44 1494 523 500** or **33 2022 123** **123**, they will hear voice prompts instructing them to press DTMF to enter the meeting number. Once the caller presses the **110200800**, the PBX will route this call to the meeting **110200800**.



