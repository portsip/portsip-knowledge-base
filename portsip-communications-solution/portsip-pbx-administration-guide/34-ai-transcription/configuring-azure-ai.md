# Configuring Azure AI

### Overview

This guide describes how to configure **Azure AI Transcription** in PortSIP PBX.

Once configured, PortSIP PBX integrates with Microsoft Azure Cognitive Services to provide high-quality Speech-to-Text (STT) transcription and optional sentiment analysis for calls and voicemails. These AI capabilities enhance call analytics, compliance recording, quality management, and overall user experience.

This guide follows VoIP, UCaaS, and CCaaS best practices and assumes basic familiarity with PortSIP PBX administration and Microsoft Azure services.

***

### Prerequisites

Before you begin, ensure the following requirements are met:

* **System Administrator** privileges in PortSIP PBX
* An active **Microsoft Azure account**
* Azure **Speech** service enabled
* _(Optional)_ Azure **Text Analytics** service enabled for sentiment analysis
* Azure service credentials obtained from the Azure Portal:
  * Account name
  * API key
  * Region

***

### Configuring the PortSIP PBX AI Engine

#### Step 1: Log in to PortSIP PBX

1. Sign in to the **PortSIP PBX Web Portal** using a **System Administrator** account.

***

#### Step 2: Select Azure as the AI Engine

1. Navigate to: **Integrations > AI Engine**
2. From the **AI Engine** drop-down list, select **Azure**.

<figure><img src="../../../.gitbook/assets/Azure_AI_Engine.png" alt=""><figcaption></figcaption></figure>

This configures PortSIP PBX to use **Microsoft Azure Cognitive Services** as the backend for AI transcription and language analytics.

***

### Step 3: Configure Azure Credentials and Service Settings

Use the information obtained from the Azure Portal to configure the following settings.\
These settings allow PortSIP PBX to store audio data and invoke Azure AI Foundry services for speech transcription and sentiment analysis.

***

#### Azure Storage Account Configuration

These settings are used to access the **Azure Storage Account** that stores call audio recordings and intermediate processing data.

**Account Name**

The name of your **Azure Storage Account**.

This value identifies the storage account where call audio files and related artifacts are stored for further processing by Azure AI Foundry services.

**Account Key**

The access key generated for the Azure Storage Account.

This key allows PortSIP PBX to authenticate and access the storage account.\
Ensure the key has appropriate permissions and is stored securely.

> **Security Best Practice**\
> Use a dedicated storage account and rotate access keys periodically.

***

**Container Name**

The name of the **Blob Container** within the Azure Storage Account.

This field is **mandatory** and must match an existing container name in the selected storage account.\
The container must have **read and write permissions** enabled.

For permission configuration details, see: [Azure trusted services security mechanism](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/batch-transcription-audio-data?tabs=portal#trusted-azure-services-security-mechanism)

#### Azure AI Foundry Configuration

These settings are used to invoke Azure AI Foundry services for Speech-to-Text and Sentiment Analysis.

**API Key**

The API key generated for your **Azure AI Foundry** resource.

This key is used by PortSIP PBX to authenticate API requests sent to Azure AI Foundry services.

> **Security Best Practice**\
> Protect this key carefully and restrict access using Azure role-based access control (RBAC) where possible.

**Region**

The Azure region where the **Azure AI Foundry** resource is deployed.

Examples include:\
`eastus`, `westus2`, `westeurope`, `japaneast`

> **Important**\
> The region must **exactly match** the region of the Azure AI Foundry resource; otherwise, API requests will fail.

**Locale**

Specifies the language and regional format used for **speech recognition**.

Examples include:\
`en-US`, `en-GB`, `ja-JP`, `zh-CN`, `vi-VN`

***

#### Azure Platform API Request Limits

These settings define the API request limits enforced when accessing Azure AI Foundry services.\
Proper configuration helps control usage and prevent throttling.

**Speech-to-Text Requests**

Specifies the maximum number of Speech-to-Text API requests that PortSIP PBX is allowed to send to the Azure platform.

**Sentiment Analysis Requests**

Specifies the maximum number of Sentiment Analysis API requests that PortSIP PBX is allowed to send to the Azure platform.

This limit is applied when analyzing transcription text to determine caller sentiment or call quality metrics.

***

#### Step 4: Enable AI Transcription for a Tenant

1. Log in to **PortSIP PBX** as a **System Administrator**.
2. Navigate to **Tenants**, select the target tenant, and click **Edit**.
3. Open the **Features** tab and enable **AI Transcription**.
4. Navigate to **General** and enable **Enable AI Transcription**.
5. Configure **Daily File Quota** to limit the tenant’s daily AI transcription usage and control Azure costs.

***

### Managing AI Transcription Within a Tenant

Once AI transcription is enabled, **Tenant Administrators** can manage and use AI transcription features as follows:

1. Log in to the **Tenant Administration Portal**.
2. Navigate to **Company > General** and configure:
   * **Automatically Transcribe Recorded Calls**
   * **Automatically Transcribe Voicemails**

<figure><img src="../../../.gitbook/assets/ai_transcription_options.png" alt=""><figcaption></figcaption></figure>

3. Navigate to **Data Analytics > Call Recordings** to view transcription results.

<figure><img src="../../../.gitbook/assets/ai_transcription_call_recordings.png" alt=""><figcaption></figcaption></figure>

4. Calls that have already been transcribed display a **sentiment indicator**.

<figure><img src="../../../.gitbook/assets/ai_transcription_result.png" alt=""><figcaption></figcaption></figure>



5. For calls that have not yet been transcribed, click the **transcription icon** to manually trigger transcription.

***

### Managing AI Transcription at the User Level

Once AI transcription is enabled for the tenant, **Tenant Administrators** can manage and use AI transcription features for individual users as follows:

1. Log in to the PortSIP PBX Web Portal as a **Tenant Administrator**.
2. Navigate to **Call Manager > Users**, then double-click the target user.
3. Select the **Extension** tab and enable or disable the following option:
   * **Automatically Transcribe Recorded Calls**
4. Navigate to **Data Analytics > Call Recordings** to view transcription results.
5. Calls that have already been transcribed display a **sentiment indicator**.
6. For calls that have not yet been transcribed, click the **transcription icon** to manually trigger transcription.

***

### Operational Notes and Best Practices

* **Call recording must be enabled** for transcription to function.
* Only recorded calls and voicemails are eligible for transcription.
* Transcription is processed asynchronously and may be queued during peak usage.
* Use **Daily File Quota** to control AI usage and manage Azure costs.
* For regulated environments, review **Azure data residency and retention policies**.



