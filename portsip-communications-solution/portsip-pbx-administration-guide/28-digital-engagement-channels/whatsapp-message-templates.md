# WhatsApp Message Templates

### 1. Feature Overview

#### 1.1 What Are WhatsApp Message Templates?

WhatsApp message templates are pre-approved message formats required by Meta. They allow a business to proactively send messages to WhatsApp users in specific scenarios, including:

* **First contact with a new customer:** A business account cannot directly send free-form text as the first message. The first outbound message must be sent by using an approved template.
* **After the 24-hour customer service window has expired:** If more than 24 hours have passed since the user’s last inbound message, from WhatsApp to PortSIP PBX, the business can only re-engage the user by sending an approved template message.
* **Marketing broadcasts:** Templates can be used for campaign promotions, holiday offers, and similar marketing scenarios.

This guide does not cover creating, editing, or submitting WhatsApp templates for review directly from the PortSIP PBX Web Portal. Template creation and approval must still be completed through Meta’s official channels, such as WhatsApp Manager.

> **Important**\
> WhatsApp message templates must be created and approved through Meta’s official tools before they can be used in PortSIP PBX. For detailed instructions, refer to the official Meta guide: [Create message templates for your WhatsApp Business account](https://www.facebook.com/business/help/2055875911147364?id=2129163877102343).

***

### 2. Client App Operations

#### 2.1 Automatic Conversation Status Detection

When the client app opens the conversation details page for a WhatsApp contact, it automatically checks the timestamp of the contact’s most recent inbound message to determine the conversation status.

> **Note**\
> The original document contains content that is only supported in Feishu Docs. Replace this placeholder with the appropriate screenshot, diagram, or conversation status description when publishing the guide.

#### 2.2 Selecting and Sending a Template Message

When the conversation is **outside the 24-hour customer service window**, follow these steps to send a WhatsApp message template.

#### Step 1: Open the template selection page

1. Click **Select Template**.
2. The system retrieves the list of available templates for the current WhatsApp channel.

\[Image]

#### Step 2: Filter templates

1. Use the **Language** filter to quickly find the required template.

\[Image]

#### Step 3: Preview the template and enter variables

1. Select a template.
2. The system displays a full preview of the selected template.
3. Template variables, such as `{{a}}` and `{{b}}`, are automatically rendered as input fields.
4. Enter the required values for each variable.

\[Image]

#### Step 4: Send the template message

1. After all required variables are completed, click **Send**.
2. The system sends the `template_id`, language, and variable values to the WhatsApp user.

**Expected result:** The selected WhatsApp message template is sent to the user through the configured WhatsApp channel.
