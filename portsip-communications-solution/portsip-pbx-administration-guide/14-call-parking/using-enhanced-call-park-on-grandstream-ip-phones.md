# Using Enhanced Call Park on Grandstream IP Phones

This article explains how to use the PortSIP PBX’s uniquely enhanced call park feature with Grandstream IP phones.

## Supported Grandstream IP Phone Models

### Grandstream

* GRP 260x: 2604, 2603, 2602, 2601
* ROM: From 1.0.5.68

## Supported PortSIP PBX Version

From version 22.0.0.

## Application Scenarios

### **Enhanced Call Park**

Enhanced Call Park is a feature that improves the call park experience on the phone. It provides the park and retrieves soft keys instead of the call park FACs. And it provides the capability of call park notification and any other enhanced features with PortSIP PBX.

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

## Parking Call

If James wants to park a call to his colleague whose extension is 103, he just needs to press the **CallPark** key.&#x20;

<figure><img src="../../../.gitbook/assets/grandstream-park-2.png" alt=""><figcaption></figcaption></figure>

Now James can enter the number 103, then press the **Park** key. The IP Phone will then park the call at extension 103. In this way, James does not need to remember the FAC for park operations.

<figure><img src="../../../.gitbook/assets/grandstream-park-1.png" alt=""><figcaption></figcaption></figure>

## Group Call Park

1. To configure a park group, sign in to the PBX web portal as the tenant administrator.
2. Select the menu **Advanced Services > Call Park**.&#x20;
3. Follow the guide to [configure a park group](./#adding-and-deleting-a-call-park-group). Assume that extensions 101, 102, 103, 104, and 105 are in the same park group.

James established a call with extension 1004, James (extension number 1003) wants to park that call to the park group, he needs to hit the **Callpark** key first, then the **GPark** key.

As shown on the IP Phone screen in the below screenshot, press the **GPark** key, the IP phone will then park this call to the group, and all members of the group will receive the park alert notification. In this way, James does not need to remember the FAC for the group park operation.

<figure><img src="../../../.gitbook/assets/grandstream-park-3.png" alt=""><figcaption></figcaption></figure>

## Retrieve Call

Alice established the call with Bob, and Bob parked the call at James' extension number 101. On James' IP Phone, the IP Phone will display the Retrieve Label as shown below screenshot to alert James that a call has parked on him.

James simply presses the "**Retrievepark"** soft key to retrieve the call. In this way, James does not need to remember the FAC for the retrieve operation.

<figure><img src="../../../.gitbook/assets/grandstream-park-4.png" alt=""><figcaption></figcaption></figure>



