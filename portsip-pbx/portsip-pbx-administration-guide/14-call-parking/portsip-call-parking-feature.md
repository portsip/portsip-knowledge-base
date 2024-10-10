# PortSIP Call Parking Feature

## What's the Problem of the Normal Call Parking?

As described above, the call parking implementation works fine for traditional scenarios with single-tenant PBX.&#x20;

However, today is the cloud age, and most scenarios are for the cloud PBX. For multi-tenant usage, a service provider hosts the cloud PBX and creates many tenants. Each tenant has a virtual PBX but shares the same server resources. A cloud PBX instance may handle 1k or 10k tenants.

If each tenant creates 10 park spots, that means for a cloud PBX with 1k tenants, there will need to be 10K spots created for the tenants. As mentioned above, these spots are actually PBX extensions that need to register to the PBX and maintain registration by sending REGISTER messages periodically. Users need to subscribe to the spots for the Dialog Event, these will consume massive CPU, memory, and bandwidth resources and reduce cloud PBX performance. This is unacceptable for service providers. It’s hard to imagine if tenants need to create more park spots!

## PortSIP Solution

PortSIP PBX is designed for cloud PBX and service provider needs. It provides a real multi-tenant PBX where a service provider just needs to set up one PBX instance, then it has the ability to create thousands of tenants, each tenant is isolated and transparent to each other.

PortSIP implements the call parking feature in a unique way in order to avoid traditional parking problems. It does not require creating park spots and there is no need to subscribe to the Dialog Event package.

Here are the details of the PortSIP Call Parking implementation:

* There is no need to create a parking spot. Just use the extension number as the parking spot. For example, if the user wants to park a call to the extension who has extension number 103, just transfer the call to `*68103`. The `*68` is the FAC for call park.
* Once a call has parked at an extension, this extension device (IP Phone or softphone app) will receive an out-of-dialog NOTIFY message with the `park-info` event. The NOTIFY message includes the parker, parked, and retrieve information.
* The extension device can parse parked call details and retrieve information from the NOTIFY message and alert the extension that there is a call parked.
* The user can retrieve the call by pressing the button/soft key.

## Advantages

As listed above, the PortSIP solution has the following advantages:

* No need to create parking spots (extensions), and no need to register massive spots to the PBX and maintain the registration.
* No need to subscribe to the Dialog Event Package for the parking spots, and no need to maintain the subscription.
* The NOTIFY message for the parked call includes detailed information that the IP Phone and softphone app can use to alert the user with park call information and retrieve it with one click.
* For large tenants in the PBX, the call parking feature will not consume massive hardware resources and will not reduce performance.

