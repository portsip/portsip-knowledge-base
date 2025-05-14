# 14 Call Parking

Call parking is a feature that allows callers to be put on hold in a communal parking spot so that any extensions can retrieve the call on a different phone or extension. Today, every PBX offers this feature to its users.

Typically, most PBXs implement the Call Parking feature by creating a dozen park spots, which are actually PBX extensions, and these spots are registered to the PBX. Users will need to subscribe to the park spots for the Dialog Event package and maintain the subscription by sending SUBSCRIBE messages periodically.

Once someone parks a call at a spot, subscribers who have subscribed to this spot will receive a NOTIFY message for the Dialog Event, and the BLF key will flash. Then the user can press the BLF key to retrieve the call.

This article includes the following topics:

* [PortSIP Call Parking Feature](portsip-call-parking-feature.md)
* [Using Call Parking Feature](using-call-parking-feature.md)
* [Using Enhanced Call Park on Fanvil IP Phones](using-enhanced-call-park-on-fanvil-ip-phones.md)
* [Using Enhanced Call Park on Yealink IP Phones](using-enhanced-call-park-on-yealink-ip-phones.md)
* [Using Enhanced Call Park on GrandStream IP Phones](using-enhanced-call-park-on-grandstream-ip-phones.md)
* [Using Enhanced Call Park on Snom IP Phones](using-enhanced-call-park-on-snom-ip-phones.md)
* [Using Enhanced Call Park on Dinstar IP Phones](using-enhanced-call-park-on-dinstar-ip-phones.md)
* [Using Enhanced Call Park on HTEK IP Phones](using-enhanced-call-park-on-htek-ip-phones.md)

