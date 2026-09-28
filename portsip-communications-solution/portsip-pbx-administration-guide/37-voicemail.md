# 37 Voicemail

PortSIP PBX voicemail lets callers leave a voice message when your calls are routed to your mailbox. You can call the voicemail service to listen to messages, remove messages, change your voicemail PIN, and record a personal greeting. You can also listen to and manage messages in the PortSIP ONE app.

> **Scope:** This guide covers an extension user's **personal voicemail**. Shared voicemail uses a different access code and mailbox number.

***

### Access Your Voicemail by Phone

1. From an IP phone or PortSIP ONE app registered to your extension, dial `*57`.
2. If prompted, enter your **voicemail PIN** and press `#`.
3. Listen to the welcome message and choose an option from the main menu.

`*57` is the default **Voicemail Access** feature access code. Your tenant administrator can change feature access codes, so contact your administrator if `*57` does not work. PIN entry depends on whether voicemail PIN authentication is enabled for your extension. The voicemail PIN is separate from your PBX Web Portal sign-in password and your extension's SIP registration password. [Feature Access Codes](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/23-feature-access-codes) · [Users](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/5-user-management/users)

> **Shared voicemail:** To access a shared mailbox, dial `*56` followed by its shared voicemail number and enter its PIN when prompted. See [Shared Voicemail](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/15-shared-voicemail).

***

### Phone Menu at a Glance

| Menu             | Key | Action                                                                                                 |
| ---------------- | :-: | ------------------------------------------------------------------------------------------------------ |
| Main menu        | `1` | Open **Play Messages**.                                                                                |
| Main menu        | `2` | Open **Set Up Mailbox**.                                                                               |
| Main menu        | `0` | Exit voicemail.                                                                                        |
| Play Messages    | `1` | Start playing messages and open the playback controls.                                                 |
| Play Messages    | `2` | Clear the mailbox. This removes the mailbox's messages; use it only when you intend to clear them all. |
| Play Messages    | `*` | Return to the main menu.                                                                               |
| Play Messages    | `0` | Exit voicemail.                                                                                        |
| Message playback | `1` | Play the current message again.                                                                        |
| Message playback | `2` | Play the next message.                                                                                 |
| Message playback | `3` | Play the previous message.                                                                             |
| Message playback | `4` | Delete the current message.                                                                            |
| Message playback | `*` | Return to **Play Messages**.                                                                           |
| Message playback | `0` | Exit voicemail.                                                                                        |
| Set Up Mailbox   | `1` | Change your voicemail PIN.                                                                             |
| Set Up Mailbox   | `2` | Record your voicemail greeting.                                                                        |
| Set Up Mailbox   | `*` | Return to the main menu.                                                                               |
| Set Up Mailbox   | `0` | Exit voicemail.                                                                                        |

`*` returns to the immediately preceding menu; `0` exits the voicemail service. Press `#` when a PIN or greeting prompt asks you to finish your entry or recording.

***

### Listen to Messages

1. Access your voicemail and press `1` at the main menu to open **Play Messages**.
2. Press `1` again to start playing messages.
3. During message playback, press `1` to replay, `2` for the next message, or `3` for the previous message. Press `4` to delete the current message.
4. Press `*` to return to **Play Messages**, or `0` to exit.

The service may announce how many **new** and **saved** messages you have and identify whom a message is **from**. When you reach the end of the available messages in either direction, the service announces that there are no more messages in that direction. If the mailbox has no messages, you will hear a no-message announcement.

> **Delete one or all:** Press `4` while playing a message to delete **only the current message**. Press `2` from the **Play Messages** menu to **clear the mailbox**. These are different actions.

***

### Clear Your Mailbox

1. Access your voicemail and press `1` at the main menu.
2. At the **Play Messages** menu, press `2` to clear the mailbox.
3. Follow any further spoken instructions. If the operation fails, the service announces that it could not delete the voice message.

To preserve messages you may need later, download them in PortSIP ONE before clearing the mailbox. A full mailbox announces **“Your mailbox is full.”** Clearing messages can free mailbox space.

***

### Change Your Voicemail PIN

1. Access your voicemail and press `2` at the main menu to open **Set Up Mailbox**.
2. Press `1` to change your PIN.
3. Enter the new PIN and press `#`.
4. Enter the same PIN again and press `#`.
5. Listen for confirmation that your password has been changed.

Use a numeric PIN that meets your organization's voicemail PIN policy. If the two entries do not match, the service announces the mismatch. If the change fails, it announces the failure. When the prompts say **“password,”** they mean your **voicemail PIN**, not your Web Portal password. [Bulk Importing Users and Auto Provisioning IP Phones](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/4-phone-device-management/bulk-importing-users-and-auto-provisioning-ip-phones)

### Record Your Voicemail Greeting

1. Access your voicemail and press `2` at the main menu.
2. Press `2` in **Set Up Mailbox**.
3. After the prompt, speak your greeting clearly.
4. Press `#` when you have finished. The service announces whether the greeting was recorded successfully.

Your greeting is what callers hear before they leave a message. If you have not recorded a personal greeting, the service may play the default greeting, which asks callers to leave their name and phone number. A recording failure produces an error announcement; try again later or contact your administrator if it continues.

**Example greeting:** “Hello, you've reached \[your name]. I can't take your call right now. Please leave your name, number, and a brief message, and I'll call you back.”

***

### Manage Messages in PortSIP ONE

In the PortSIP ONE desktop or WebRTC app, open the **Voicemails** tab. Select a message to view its details, then use **Play** to listen or **Download** to save its audio as a `.wav` file. To remove a message, open its **…** menu, choose **Delete**, and confirm. These app controls are separate from the phone menu described above. [Calls, Messages, and Voicemails](https://support.portsip.com/apps-guides/portsip-one-desktop-app/calls-messages-and-voicemails)

***

### If You Hear an Error

| Announcement                                 | What to do                                                                                                       |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Your input is invalid.**                   | Check the menu you are in and enter a listed key.                                                                |
| **The password entered is incorrect.**       | Re-enter your voicemail PIN followed by `#`, if prompted. Contact your administrator if you do not know the PIN. |
| **The passwords you entered did not match.** | Repeat the new PIN and enter the same digits both times.                                                         |
| **Failed to change the password.**           | Try the PIN change again; contact your administrator if it continues.                                            |
| **Failed to delete voice message.**          | Try again; contact your administrator if it continues.                                                           |
| **You currently have no voice message.**     | Your mailbox has no messages to play.                                                                            |
| **Your mailbox is full.**                    | Remove messages you no longer need, or contact your administrator if it remains full.                            |

***

### Related PortSIP Knowledge Base Articles

* [Feature Access Codes](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/23-feature-access-codes) — personal voicemail access and configurable codes.
* [Calls, Messages, and Voicemails](https://support.portsip.com/apps-guides/portsip-one-desktop-app/calls-messages-and-voicemails) — managing voicemail in PortSIP ONE.
* [Users](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/5-user-management/users) — administrator settings for a user's voicemail.
* [Shared Voicemail](https://support.portsip.com/portsip-communications-solution/portsip-pbx-administration-guide/15-shared-voicemail) — access to shared mailboxes.
