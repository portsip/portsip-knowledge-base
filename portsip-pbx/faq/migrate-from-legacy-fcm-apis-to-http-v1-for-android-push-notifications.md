# Migrate from legacy FCM APIs to HTTP v1 for Android Push Notifications

As announced by Google, [support for the legacy push method will cease in June 2024](https://firebase.google.com/docs/cloud-messaging/migrate-v1).&#x20;

PortSIP released the v16.4. version to support the new push method for Android notifications. All PortSIP PBX v16.x installations need to upgrade to v16.4 and change settings to ensure the new push notifications work correctly.

{% hint style="warning" %}
If you installed the PortSIP PBX v12.x, please reference this guide: [Upgrade PortSIP PBX v12.x to v12.8.7](../../pbx\_v12/tutorials/upgrade-portsip-pbx-v12.x-to-the-v12.8.7.md).
{% endhint %}

Please follow the below steps to update the push settings with PortSIP PBX v16.4.0.

## **Upgrading PortSIP PBX**

Please upgrade your current PBX v16.x to v16.4 per this guide: [Upgrading PortSIP PBX to New Versions](../portsip-pbx-administration-guide/upgrading-portsip-pbx-to-new-versions/upgrading-portsip-pbx-to-v16.x.md).

## **Changing Push Settings for Android**&#x20;

If you’re using the PortSIP softphone app, no action is required on your part. The upgraded PBX v16.4.0 will seamlessly integrate with the PortSIP softphone app.

### **Obtaining a New Push Profile JSON File**&#x20;

* If you’re using an Android app that PortSIP has rebranded for you, please [contact us](https://portsip.atlassian.net/servicedesk) to receive the new push setting JSON file.&#x20;
* If you’re using an Android app that you’ve developed yourself, follow the steps below to obtain the new push setting JSON file. Refer to the[ Android Push Notifications guide and follow steps 1 through 3](https://docs.apppresser.com/article/301-android-push-notifications) (no need to follow the other steps) to download the push setting JSON file.

For the purposes of this guide, let’s assume the push setting JSON file is: `prie-ffae0-c58bc735da.json`.

### **Adjusting Settings**&#x20;

* Sign in to the PortSIP PBX v16.4.0 Web portal, select the menu **Advanced > Mobile Push**, and double-click your app to change the settings.&#x20;
* Open the Push Profile JSON file `prie-ffae0-c58bc735da.json` by the text editor for example Windows Notepad, copy all of the contents and paste to the **FCM Service account key JSON** file and save your changes.

Please reference the below screenshot.

<figure><img src="../../.gitbook/assets/android_push_json_file (1).png" alt=""><figcaption></figcaption></figure>



