# Speed Dial 100

**Speed Dial 100** allows users to assign frequently dialed numbers to **two-digit speed codes** ranging from **00 to 99**. Once configured, users can place calls by dialing the two-digit code instead of the full phone number.

Speed Dial 100 supports a larger set of speed codes than Speed Dial 8 and is ideal for users who need quick access to many contacts.

***

### Ways to Program Speed Dial 100

You can configure Speed Dial 100 using either of the following methods:

* **PBX Web Portal**
* **Feature Access Codes (FAC)** from an IP phone or softphone

The Web Portal is generally easier to use and allows you to review and confirm all configured speed codes, but both methods are supported.

***

### Configuring Speed Dial 100 in the Web Portal

#### For Tenant Administrators

1. Sign in to the **PBX Web Portal** as a **Tenant Administrator**.
2. Navigate to **Call Manager > Users**.
3. Select the target extension.
4. Click the **Speed Dial 100** tab.
5. Assign phone numbers to the desired two-digit speed codes (**00–99**).

***

#### For Extension Users

1. Sign in to the **PBX Web Portal** as an **extension user**.
2. Open the **Profile** menu.
3. Click the **Speed Dial 100** tab.
4. Configure your personal speed dialing entries.

<figure><img src="../../../.gitbook/assets/speed-dial-100.png" alt=""><figcaption></figcaption></figure>

***

### Configuring Speed Dial 100 Using Feature Access Codes (FAC)

You can configure **Speed Dial 100** by dialing **Feature Access Codes (FAC)** directly from an IP phone or client app. This method allows quick setup without using the Web Portal.

***

#### Set a Speed Dial Code

To program **speed dial code 06** for the phone number **0015620671**:

1. From your phone or app, dial: **`*75060015620671`**
2. Listen to the voice prompt from the PBX, which confirms whether the configuration was successful.

After configuration, you can place the call by dialing **`06`**, and the PBX will automatically dial **0015620671**.

***

#### Modify an Existing Speed Dial Code

To change the number assigned to **speed dial code 06** to **0033125**:

1. Dial: **`*75060033125`**
2. The PBX replaces the existing number for code 06 with the new number.

***

#### Delete a Speed Dial Code

To delete the number assigned to **speed dial code 06**:

1. Dial: **`*7506*`**
2. The PBX removes the speed dial configuration for code 06.







