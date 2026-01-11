# Speed Dial 8

**Speed Dial 8** allows users to assign up to **eight frequently dialed numbers**—including long or hard-to-remember numbers—to **single-digit speed codes**. Users can then place calls by dialing the speed code instead of the full number.

* Speed codes use **single digits from 2 to 9**.
* Each speed code can be associated with:
  * Internal or external phone numbers
  * Emergency numbers (for example, **911**)
  * Service numbers (for example, **611** for customer support)

***

### Ways to Program Speed Dial 8

Users can configure Speed Dial 8 using either of the following methods:

* **PBX Web Portal**
* **Feature Access Codes (FAC)**

This section focuses on configuration through the Web Portal.

***

### Configuring Speed Dial 8 in the Web Portal

#### For Tenant Administrators

1. Sign in to the **PBX Web Portal** as a **Tenant Administrator**.
2. Navigate to **Call Manager > Users**.
3. Select the target extension and click the **Speed Dial 8** tab.
4. Assign phone numbers to the desired speed codes (2–9).

***

#### For Extension Users

1. Sign in to the **PBX Web Portal** as an **extension user**.
2. Open the **Profile** menu.
3. Click the **Speed Dial 8** tab.
4. Configure your personal speed dialing settings.

<figure><img src="../../../.gitbook/assets/speed-dial-8.png" alt=""><figcaption></figcaption></figure>

***

### Configuring Speed Dial 8 Using Feature Access Codes (FAC)

You can also configure **Speed Dial 8** by dialing **Feature Access Codes (FAC)** directly from a phone or client app. This method allows quick setup without accessing the Web Portal.

***

#### Set Up a Speed Dial Code

To program **speed dial code 5** for the phone number **12345678**:

1. From your phone or app, dial: **`*74512345678`**
2. Follow the voice prompts provided by the PBX.
3. The system confirms whether the configuration was successful.

After setup, you can place the call by simply dialing **`5`**, and the PBX will automatically dial **12345678**.

***

#### Modify an Existing Speed Dial Code

To change the number assigned to **speed dial code 5** to **0033125**:

1. Dial: **`*7450033125`**
2. The PBX replaces the previously stored number for code 5 with the new number.

***

#### Delete a Speed Dial Code

To delete the number assigned to **speed dial code 5**:

1. Dial: **`*745*`**
2. The PBX clears the stored number for code 5.



