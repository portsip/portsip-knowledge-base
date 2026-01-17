# Using Call Parking Feature

### Park and Retrieve Calls with PortSIP

PortSIP PBX allows you to **park an active call** and retrieve it later from the same device or a different device, using simple **Feature Access Codes (FACs)**. This is useful when you need to move between devices, involve another colleague, or temporarily place a call on hold for retrieval.

#### Feature Access Codes (FACs)

*   **Park a call**\
    While on an active call, transfer the call to:

    ```
    *68
    ```

    This places the call into a parking state.
*   **Retrieve a parked call**\
    To retrieve the parked call, dial:

    ```
    *88
    ```
*   **Park a call at a specific extension**\
    To associate the parked call with a specific extension, transfer the call to:

    ```
    *68<extension>
    ```

    Example: `*68102`
*   **Retrieve a call parked at a specific extension**\
    To retrieve a call parked at a specific extension, dial:

    ```
    *88<extension>
    ```

    Example: `*88102`

***

### Scenario 1: Park a Call for Another User

Bob’s extension number is **101**, and he answers a client call on his desktop IP phone. He needs assistance from his colleague Alice, whose extension number is **102**.

1.  Bob transfers the active call to:

    ```
    *68102
    ```
2. Bob hangs up.
3. Alice’s IP phone lights up and displays a parked call alert.
4. Alice retrieves the call by:
   * Pressing the corresponding **soft key**, or
   * Dialing `*88`, or
   * Dialing `*88102`

Alice is now connected to the client.

> **Note:**\
> If Alice is not available, **any other colleague** can retrieve the parked call by dialing `*88102`.

***

### Scenario 2: Move a Call Between Devices (Same Extension)

Bob’s extension number is **101**, and he answers a client call on his desktop IP phone. He needs to walk to another office.

1.  Bob transfers the call to:

    ```
    *68
    ```
2. Bob hangs up on the desktop IP phone.
3. The caller hears music while the call is parked.
4. Bob opens the **PortSIP mobile app**, which is registered to the PBX using the **same extension (101)**.
5.  Bob retrieves the call by dialing:

    ```
    *88
    ```

Bob continues the conversation while walking to the other office.

***

### Scenario 3: Retrieve a Parked Call from Another IP Phone

Bob’s extension number is **101**, and he answers a client call on his desktop IP phone. He then needs to continue the call from a different office.

1.  Bob transfers the call to:

    ```
    *68
    ```
2. Bob hangs up on the original IP phone.
3. The caller hears music while the call is parked.
4. After arriving at the other office, Bob uses another IP phone.
5.  Bob retrieves the call by dialing:

    ```
    *88101
    ```

Bob is reconnected to the client and continues the call from the new location.

***

### Retrieve Parked Calls Using the PortSIP App, Fanvil, and Yealink IP Phones

The **PortSIP App**, **Fanvil**, and **Yealink** IP phones all support **one-touch retrieval** of parked calls. When a call is parked, supported devices automatically notify the user and allow the call to be retrieved with a **single button or key press**, without dialing any Feature Access Codes.

#### Scenario: One-touch call retrieval

Bob’s extension number is **101**, and he answers a client call (caller number `003386002678`) on his desktop IP phone. The call now needs to be handled by his colleague Alice, whose extension number is **102**.

1.  Bob transfers the active call to:

    ```
    *68102
    ```
2. Bob hangs up.
3.  Once the call is successfully parked:

    * Alice’s **PortSIP App** (Mobile App, Windows Desktop App, or WebRTC App), and
    * Alice’s **IP phone** (Fanvil, Yealink, or Dinstar)

    receive a **parked call notification**.
4. Alice’s device:
   * Lights up (IP phone),
   * Displays an on-screen message indicating that a call is parked on **extension 102**.
5. Alice retrieves the call by:
   * Pressing the **on-screen button** in the PortSIP App, or
   * Pressing the **dedicated key or soft key** on the IP phone.

No manual dialing (such as `*88` or `*88102`) is required.

<figure><img src="../../../.gitbook/assets/yealink_t31_park.png" alt="" width="375"><figcaption></figcaption></figure>

***

### Group Call Park

**Group Call Park** allows a user to park a call to a **Call Park Group**, making the call available for retrieval by any other member of that group. This is ideal for team-based call handling, where calls need to be shared and answered by the first available colleague.

***

### Feature Notes

Before configuring Group Call Park, be aware of the following:

* Group Call Park is a **tenant-level feature** included with the PortSIP PBX license. **No additional license or cost** is required.
* A user can belong to **only one** Call Park group.
* A Call Park group can include **only users from the same tenant**.
* A tenant can create **multiple Call Park groups**.
* **Call Park group names must be unique** within the tenant.

***

### Modifying Call Park Settings

1. Sign in to the **PortSIP PBX Web Portal**.
2. Go to **Advanced Services > Call Park**.

***

### Add or Delete a Call Park Group

#### Create a Call Park Group

1. Click **Add** to create a new Call Park group. The **Settings** screen appears.
2.  In **Group Name**, enter a unique and descriptive name.

    > This field is required and is used to identify the group throughout the system.
3. Configure the **Recall destination**, which defines where the call is routed if it is not retrieved within the recall time.

#### Recall destination options

Choose one of the following **Recall To** behaviors:

* **Alert parking user only**
  * If the parked call is not retrieved before the **Recall Timer** expires, the call is returned to the user who parked it.
  * If the parking user does not answer and the Recall Timer expires again, the system retries the parking user after **10 seconds**.
* **Alert parking user first, then Ring Group**
  * The call is first returned to the parking user when the Recall Timer expires.
  * If the parking user does not answer within the configured **Alert Ring Group Wait Time**, the call is forwarded to the selected **Ring Group**.
  * Once forwarded to the Ring Group, the call follows normal Ring Group routing and is **not reverted again**.
  * _Available only when a Ring Group is configured._
* **Alert Ring Group only**
  * If the parked call is not retrieved before the Recall Timer expires, it is forwarded directly to the selected **Ring Group**.
  * The call follows Ring Group routing and is **not reverted**.
  * _Available only when a Ring Group is configured._

4. **Ring Group**
   * Select a Ring Group as the recall destination.
   * This field is applicable only when **Alert parking user first, then Ring Group** or **Alert Ring Group only** is selected.

***

#### Assign users to the Call Park Group

1. Open the **GROUP MEMBERS** tab.
2. Click **Add**.
3. Select users from the **Available** list.
4. Click **OK** to save the group.

***

### Feature Operation

#### Parking a call to a group

*   To park an active call to a Call Park group, the user transfers the call to the Feature Access Code:

    ```
    *58
    ```
* The Group Call Park service automatically searches for the **first available member** of the Call Park group.
* The system always starts with the **first assigned group member**.
* Once an available member is found, the call is parked against that member.
* **All members of the Call Park group** receive parked-call notifications.

#### Retrieving a parked call

* While the call is parked, the caller hears **music on hold**.
*   Any group member can retrieve the parked call by:

    * Pressing the **parked call button / soft key** on supported devices, or
    *   Dialing:

        ```
        *88
        ```

    from other IP phones or third-party softphones.

#### Recall behavior

* If the parked call is **not retrieved** within the configured **Recall Timer**:
  * The call is recalled to the configured **parking user** or **Ring Group**, based on the Call Park group settings.
* Recall behavior is fully configurable per Call Park group.

***

### Example

Users **101**, **102**, and **103** are members of a Call Park group.

1.  User **101** parks an active call by transferring it to:

    ```
    *58
    ```
2. The caller hears music on hold.
3. Users **102** and **103** receive parked-call notifications on:
   * PortSIP Apps
   * Fanvil, Yealink, and Dinstar IP phones
4. Either user **102** or **103** retrieves the call by:
   * Pressing the parked-call button on their device, or
   * Dialing `*88` from another phone or app.

***

### Enhanced Call Park

On Fanvil, Yealink, Snom, Grandstream, and Dinstar IP phones, the Enhanced Call Park feature is supported, providing an improved user experience with visual notifications and one-touch call retrieval.

For detailed instructions on how to use Enhanced Call Park on supported devices, refer to the following articles:

* [Using Enhanced Call Park on Fanvil IP Phones](using-enhanced-call-park-on-fanvil-ip-phones.md)
* [Using Enhanced Call Park on Yealink IP Phones](using-enhanced-call-park-on-yealink-ip-phones.md)
* [Using Enhanced Call Park on Snom IP Phones](using-enhanced-call-park-on-snom-ip-phones.md)
* [Using Enhanced Call Park on Grandstream IP Phones](using-enhanced-call-park-on-grandstream-ip-phones.md)
* [Using Enhanced Call Park on Dinstar IP Phones](using-enhanced-call-park-on-dinstar-ip-phones.md)

