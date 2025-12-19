# Configuring AWS AI

### Overview

This guide explains how to configure AWS AI Transcription in PortSIP PBX.

Once configured, PortSIP PBX integrates with **Amazon Web Services (AWS) AI services** to provide enterprise-grade **Speech-to-Text (STT)** transcription and optional **sentiment analysis** for calls and voicemails. These capabilities enhance call analytics, quality management, compliance recording, and overall customer experience.

This guide follows VoIP, UCaaS, and CCaaS best practices and assumes basic familiarity with PortSIP PBX administration and AWS services.

***

### AWS Services Used

PortSIP PBX leverages the following AWS AI services:

* **Amazon Transcribe**\
  Provides real-time or post-call **Speech-to-Text (STT)** transcription.
* **Amazon Comprehend**\
  Provides **sentiment analysis** and natural language processing (NLP) for transcribed text.

***

### Prerequisites

Before you begin, ensure the following requirements are met:

* **System Administrator** privileges in PortSIP PBX
* An active **AWS account**
* An AWS **IAM user or role** with permissions for:
  * `AmazonTranscribe`
  * `AmazonComprehend` (required only if sentiment analysis is enabled)
* AWS **Access Key ID** and **Secret Access Key**
* A selected **AWS region** where AI services are available
* _(Optional)_ An **Amazon S3 bucket** for storing transcription output, if required by your deployment model

***

### Configuring the PortSIP PBX AI Engine

#### Step 1: Log in to PortSIP PBX

1. Sign in to the **PortSIP PBX Web Portal** using a **System Administrator** account.

***

#### Step 2: Select AWS as the AI Engine

1. Navigate to menu: **Integrations > AI Engine**
2. From the **AI Engine** drop-down list, select **AWS**.

<figure><img src="../../../.gitbook/assets/AWS_AI_Engine.png" alt=""><figcaption></figcaption></figure>

This configures PortSIP PBX to use **Amazon AI services** as the backend for transcription and language analytics.

***

#### Step 3: Configure AWS Credentials and Settings

Using information from the **AWS Management Console**, configure the following settings.

***

**AWS Authentication**

* **Access Key**\
  The AWS IAM access key used to authenticate API requests.
* **Secret Key**\
  The corresponding secret key for the IAM user or role.

> **Security Best Practice**\
> Use a **dedicated IAM user or role** with least-privilege permissions.\
> Never use AWS root account credentials.

***

**AWS Service Configuration**

* **Region**\
  The AWS region where **Amazon Transcribe** and **Amazon Comprehend** are enabled\
  (for example: `us-east-1`, `us-west-2`, `eu-central-1`).
* **Bucket Name**\
  Specifies the S3 bucket used to store transcription output, if required by your workflow.
* **Language Code**\
  Defines the language used for speech recognition\
  (for example: `en-US`, `en-GB`, `ja-JP`).

> **Best Practice**\
> Select the AWS region geographically closest to your PBX deployment or primary user base to reduce latency and improve transcription performance.

***

**AI Request Limits**

* **Max Speech-to-Text Queue Size**\
  Defines the maximum number of speech-to-text transcription tasks that can be queued when the AI service is busy or temporarily rate-limited.
* **Sentiment Analysis Requests (per minute)**\
  Defines the maximum number of sentiment analysis requests processed per minute by **Amazon Comprehend**.

> **Recommendation**\
> For contact centers or high-volume call recording environments, monitor usage via **AWS CloudWatch** and adjust AWS service quotas or limits to avoid throttling.

***

### Assigning AI Capabilities to Tenants

> **Important**\
> **Tenant Administrators** cannot configure the AI engine themselves.\
> The **PBX System Administrator** must configure AWS AI at the system level and then explicitly assign AI capabilities to individual tenants.

***

#### Step 4: Enable AI Transcription for a Tenant

1. Log in to **PortSIP PBX** web portal as a **System Administrator**.
2. Navigate to **Tenants**, select the target tenant, and click **Edit**.
3. Select the **Features** tab and enable **AI Transcription**.
4. Navigate to **General** and enable **Enable AI Transcription**.
5. Configure the **Daily File Quota** to limit the tenant’s daily AI transcription usage and control AWS costs.

***

### Managing AI Transcription Within a Tenant

Once AI transcription is enabled, **Tenant Administrators** can manage and use AI features as follows:

1. Log in to the PortSIP PBX web portal as a tenant administrator.
2. Navigate to **Company > General** and configure:
   * **Automatically Transcribe Recorded Calls**
   * **Automatically Transcribe Voicemails**

<figure><img src="../../../.gitbook/assets/ai_transcription_options (1).png" alt=""><figcaption></figcaption></figure>

4. Navigate to **Data Analytics > Call Recordings** to view transcription results.

<figure><img src="../../../.gitbook/assets/ai_transcription_call_recordings (1).png" alt=""><figcaption></figcaption></figure>

5. Calls that have already been transcribed display a **sentiment indicator**.

<figure><img src="../../../.gitbook/assets/ai_transcription_result (1).png" alt=""><figcaption></figcaption></figure>

6. For calls that have not yet been transcribed, click the transcription icon to manually trigger transcription.

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

* Call recording must be enabled for transcription to function.
* Only recorded calls and voicemails are eligible for transcription.
* Transcription workloads are processed asynchronously and may be queued during peak usage.
* Use **Daily File Quotas** to manage AWS costs and prevent unexpected usage spikes.
* For regulated environments, review AWS data residency and retention policies.



