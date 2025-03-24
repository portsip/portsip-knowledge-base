# Manage SMS/WhatsApp Message Conversations

Short Message Service (SMS) and WhatsApp channels enable customers to reach out to agents by sending an SMS from anywhere and even when there is no data connectivity. Customers can contact businesses using a longcode, shortcode, or toll-free number. A new chat conversation is created in the SMS/WhatsApp list when a customer sends a message. Agents can then handle the chat and send a response to the customer.

## **Create Message Rule**

To route inbound SMS or WhatsApp messages to a ring group or call queue, you must first create an inbound rule.

For example, as shown in the screenshot below, if the customer sends an SMS to DID **16313808000**, the message will be routed to the queue with extension **8000**.

<figure><img src="../../../.gitbook/assets/sms-inbound-group-queue.png" alt=""><figcaption></figcaption></figure>

When SMS or WhatsApp messages are routed to a ring group or queue, **all agents** in the group can view the conversation. The PBX will automatically assign a suitable agent as the **designated recipient** for the customer. That agent will receive an **@ mention alert**, while other agents will not be alerted but can still view the message history.

As shown in the screenshot below, the **@ mention alert** in **red text** indicates that this agent has been assigned as the recipient and is being notified. The **blue text** indicates that another agent has been alerted in this conversation.

<figure><img src="../../../.gitbook/assets/sms-whatsapp-queue-2.png" alt=""><figcaption></figcaption></figure>

## Transferring a Message Conversation

If an agent is unable to resolve a customer’s inquiry and needs to escalate the conversation, they can transfer the chat to another agent.

> **Note:** Only the currently assigned (designated) agent can initiate a transfer.

**To transfer a conversation:**

1. Click the **⋯ (More Options)** icon in the message conversation window.
2. Select **Transfer Handler** from the menu.
3. In the pop-up window, choose the desired agent and click **CONFIRM**.

Once the transfer is completed, all future messages from this customer to the same group or queue will be automatically routed to the newly assigned agent as the designated recipient.

<figure><img src="../../../.gitbook/assets/sms-whatsapp-queue-1.png" alt=""><figcaption></figcaption></figure>

In the PortSIP ONE mobile app, the currently assigned agent can **tap on their own message**, and then select **Transfer** from the pop-up menu.&#x20;

<figure><img src="../../../.gitbook/assets/sms-whatsapp-queue-6.png" alt=""><figcaption></figcaption></figure>

From there, they can choose another agent to transfer the conversation to.



## Selecting the Destination Number and Outbound Caller ID

If the customer is a saved contact with multiple phone numbers, or if you want to present a different outbound caller ID when replying, you can easily make those changes.

Click the **down arrow** icon next to the phone number to select the desired **destination number** or **outbound caller ID**, as shown in the screenshot below.

<figure><img src="../../../.gitbook/assets/sms-whatsapp-queue-3.png" alt=""><figcaption></figcaption></figure>

With the PortSIP ONE mobile app, you can tap the **message icon** at the top of the conversation. This will open a window where you can select the **destination number** and **outbound caller ID**.

<figure><img src="../../../.gitbook/assets/sms-whatsapp-queue-4.png" alt="" width="375"><figcaption></figcaption></figure>

As shown in the screenshot below, you can tap the **dropdown list** to select the **destination number** or choose a different **outbound caller ID**.

<figure><img src="../../../.gitbook/assets/sms-whatsapp-queue-5.png" alt="" width="375"><figcaption></figcaption></figure>

