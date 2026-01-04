# Using Enhanced Call Park on Yealink IP Phones

This article explains how to use the PortSIP PBX’s uniquely enhanced call park feature with Yealink IP phones.

## Supported Yealink IP Phone Models

* T3x series, download the firmware:
  * [T31(T30,T30P,T31G,T31P,T33P,T33G)-124.86.0.163-muti.rom](https://yealink7-my.sharepoint.com/:u:/g/personal/jim_jia_yealink7_onmicrosoft_com/EW8MO9Pup05Mu3Fd8CIqpVUBKFAQ_15J0_EC-epILVQQlA?e=evSJJt)
  * [T31(T30,T30P,T31G,T31P,T33P,T33G)-124.86.0.118-muti.rom](https://dmfile.yealinkops.com/firmware/b842ee3ae2e94aa2b8450314b6867aba/1670571390494/T31\(T30,T30P,T31G,T31P,T33P,T33G\)-124.86.0.118.rom)&#x20;
* T4x series, download the firmware:&#x20;
  * [T46U(T43U,T46U,T41U,T48U,T42U,T44U,T44W)-108.86.0.164.rom](https://yealink7-my.sharepoint.com/:u:/g/personal/jim_jia_yealink7_onmicrosoft_com/EeK1bPv1h2VDnwzjGYwXY9cBD6g5sv8BUHRQg-9LWQppKQ?e=vkbFqs)
  * [T46U(T43U,T46U,T41U,T48U,T42U,T44U,T44W)-108.86.0.118.rom](https://dmfile.yealinkops.com/firmware/18facaab3c9c479e86e1707970ebe6fb/1670571333242/T46U\(T43U,T46U,T41U,T48U,T42U\)-108.86.0.118.rom)
* T5x series, download the firmware:&#x20;
  * [T54W(T57W,T53W,T53,T53C,T54,T57)-96.86.0.163.rom](https://yealink7-my.sharepoint.com/:u:/g/personal/jim_jia_yealink7_onmicrosoft_com/EevWwF0-xKNKgnwOB17mctUBLNnWQpQzuPfKehj8r0e4BQ?e=vj12dh)
  * [T54W(T57W,T53W,T53,T53C,T54,T57)-96.86.0.118.rom](https://dmfile.yealinkops.com/firmware/821074fc924f43a8ad2fe494a0a5cae9/1670571488097/T54W\(T57W,T53W,T53,T53C,T54,T57\)-96.86.0.118.rom)
* T7x, T8x serries, download the firmawre from yearlink website.

## Supported PortSIP PBX Version

From version 16.1.0.

## Application Scenarios

### **Enhanced Call Park**

Enhanced Call Park is a feature that improves the call park experience on the phone. It provides the park and retrieves soft keys instead of the call park FACs. And it provides the capability of call park notification and any other Enhanced features with PortSIP PBX.

### Call Park

The Call Park Service allows users to suspend a call for an extended period of time and then retrieve that call from any extension. For instance, you need to move to another place for some reason but you are answering a call. You can park this call in your number and retrieve the call you reach the place you want to go.

### Group Call Park

Provides a hunting mechanism so that when parking a call, the service hunts for an available user in a configured call park group as a place to park the call instead of only trying the parking user. For instance, your call will be parked in the line of your colleague if you are in the same call park group, your colleague can retrieve the call and then transfer it to you or just inform you after he/she retrieves the call.

### **Call Park Notification**

The Call Park Notification enables visual alerts when a user received a parked call. There will be a visual alert when receiving a parked call, the user just needs to press the button to retrieve that parked call.

### **Retrieve Park**

The user dials the Call Park Retrieve feature access code with the extension number where the call to be retrieved is parked. For instance, you can retrieve the parked call by yourself or your colleague if the call was parked on his/her line then transfer the call or just inform you.

### **Recall**

The User can configure the recall settings such as recall object and recall timer. The server will recall the number that parked the call or the specified number if no one retrieves the call in the limited time. For instance, if you set the recall timer to 30s, the server will recall your number if no one retrieves the call in 30s.

## Configuring Soft Key for Visual Park

When provisioning an IP phone, configure the  BLF keys for Visual Call Park and Visual Group Park.

<figure><img src="../../../.gitbook/assets/yealink-vpark.png" alt=""><figcaption></figcaption></figure>

After the IP phone has been successfully provisioned, the BLF keys will display the label "**Visual Park" and "Visual Group Park"**. As shown in the screenshot below, user James has extension number 101 and has configured the keys with **Park** and **Group Park**.

<figure><img src="../../../.gitbook/assets/yealink-vpark-1.jpg" alt=""><figcaption></figcaption></figure>

## Parking Call

If James wants to park a call to his colleague whose extension is 103, he just needs to press the configured Visual Park key and enter the number 103, then press the OK key. The IP Phone will then park the call at extension 103. In this way, James does not need to remember the FAC for park operations.

<figure><img src="../../../.gitbook/assets/yealink-vpark-3.jpg" alt=""><figcaption></figcaption></figure>

## Group Call Park

1. To configure a park group, sign in to the PBX web portal as the tenant administrator.
2. Select the menu **Advanced Services > Call Park**.&#x20;
3. Follow the guide to [configure a park group](./#adding-and-deleting-a-call-park-group). Assume that extensions 101, 102, 103, 104, and 105 are in the same park group.

James established a call with extension 102, James (extension number 101) wants to park that call to the park group, he just needs to press the **Group Park** key.&#x20;

<figure><img src="../../../.gitbook/assets/yealink-vpark-4.jpg" alt=""><figcaption></figcaption></figure>

As shown on the IP Phone screen in the above screenshot, press the "**Group Park**" key, the IP phone will then park this call to the group, and all members of the group will receive the park alert notification. In this way, James does not need to remember the FAC for the group park operation.

## Retrieve Call

Alice established the call with Bob, and Bob parked the call at James' extension number 101. On James' IP Phone, the IP Phone will display the Retrieve Label as shown below screenshot to alert James that a call has parked on him.

<figure><img src="../../../.gitbook/assets/yealink-vpark-2.jpg" alt=""><figcaption></figcaption></figure>

James simply presses the "**Retrieve"** soft key to retrieve the call.  In this way, James does not need to remember the FAC for the retrieve operation.

##
