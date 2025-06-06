# Using Enhanced Call Park on Htek IP Phones

This article explains how to use the PortSIP PBX’s uniquely enhanced call park feature with Htek IP phones.

## Supported Htek IP Phone Models

### Models

* Models: UCxxx
* ROM: From V2.42.6.6.1R22

## Supported PortSIP PBX Version

From version 22.0.

## Application Scenarios

### **Enhanced Call Park**

Enhanced Call Park is a feature that improves the call park experience on the phone. It provides the park and retrieve key instead of the call park FACs. And it provides the capability of call park notification and any other enhanced features with PortSIP PBX.

### Call Park

The Call Park Service allows users to suspend a call for an extended period of time and then retrieve that call from any extension. For instance, you need to move to another place for some reason but you are answering a call. You can park this call in your number and retrieve the call you reach the place you want to go.

### Group Call Park

Provides a hunting mechanism so that when parking a call, the service hunts for an available user in a configured call park group as a place to park the call instead of only trying the parking user. For instance, your call will be parked in the line of your colleague if you are in the same call park group, your colleague can retrieve the call and then transfer it to you, or just inform you after he/she retrieves the call.

### **Call Park Notification**

The Call Park Notification enables a visual alert when a user receives a parked call. There will be a visual alert when receiving a parked call, the user just needs to press the button to retrieve that parked call.

### **Retrieve Park**

The user dials the Call Park Retrieve feature access code with the extension number where the call to be retrieved is parked. For instance, you can retrieve the parked call by yourself or your colleague if the call was parked on his/her line, then transfer the call or just inform you.

### **Recall**

The User can configure the recall settings, such as the recall object and recall timer. The server will recall the number that parked the call or the specified number if no one retrieves the call in the limited time. For instance, if you set the recall timer to 30 seconds, the server will recall your number if no one retrieves the call in 30s.

## Provisioning IP Phones

Once the IP phone has been successfully provisioned, the PortSIP PBX Enhanced Call Park feature is activated.

## Parking Call

If James wants to park a call to his colleague whose extension is **104**, he just needs to press the More button as the screenshot below shows.

<figure><img src="../../../.gitbook/assets/htek-direct-park.jpg" alt=""><figcaption></figcaption></figure>

On the next screen, press the **DPark** button.

<figure><img src="../../../.gitbook/assets/htek-direct-park-1.jpg" alt=""><figcaption></figcaption></figure>

Now enter the extension number 104 and press the DPark button again.

<figure><img src="../../../.gitbook/assets/htek-direct-park-2.jpg" alt=""><figcaption></figcaption></figure>

## Group Call Park

1. To configure a park group, sign in to the PBX web portal as the tenant administrator.
2. Select the menu **Advanced Services > Call Park**.&#x20;
3. Follow the guide to [configure a park group](./#adding-and-deleting-a-call-park-group). Assume that extensions 101, 102, 103, 104, and 105 are in the same park group.

If James (extension number 103) wants to park a call to this park group, he just needs to press the More button, then press the **GPark** button. The IP phone will then park this call to the group, and all members of the group will receive the park alert notification. In this way, James does not need to remember the FAC for the group park operation.

<figure><img src="../../../.gitbook/assets/htek-group-park.jpg" alt=""><figcaption></figcaption></figure>

## Retrieve Call

Once an IP Phone receives a park call/group park call notification, it will display the information like the screenshot below, simply by pressing the Retrieve button to retrieve that call.

<figure><img src="../../../.gitbook/assets/htek-park-retrieve.jpg" alt=""><figcaption></figcaption></figure>
