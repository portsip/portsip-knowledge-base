# 27 STIR/SHAKEN

Robocalls and telemarketing calls are currently the number one source of consumer complaints at the FCC. What was once a nuisance has become a plague to U.S consumers receiving an estimated “16.3 billion robo-calls in the first five months of 2018. In May 2018, U.S. people received about 4.1 billion robocalls or 12 calls for every person.”

The FCC has been encouraging service providers to offer call blocking solutions that give customers greater control over the types of calls they receive. Call blocking is one part of the robocall solution. Another part is identifying the bad actors who use robocalls to take advantage of unsuspecting consumers by using numbers assigned to others (spoofing). They use cheap and accessible technologies to spoof their caller identity and scam victims with threats from the IRS and offers of loans or free travel.

Although several providers and third parties offer call blocking and caller identification verification products, there is no ubiquitous solution that spans wireline and wireless communication networks. In order to address these concerns, the service providers are focusing their efforts across three key areas: source authentication as well as network and consumer blocking tools. The goal of these solutions is to protect consumers from unwanted calls and give them more control over the calls and texts they receive.

## STIR/SHAKEN Definitions

STIR (Secure Telephony Identity Revisited) is the proposed standard developed by IETF that defines a signature to verify the calling number, and specifies how it will be transported in SIP “on the wire.”

SHAKEN (Signature-based Handling of Asserted information using toKENs) is the framework document developed by the ATIS/SIP Forum IP-NNI task force to provide an implementation profile for service providers implementing STIR. STIR/SHAKEN will be the basis for verifying calls, classifying calls, and facilitating the ability to trust the caller ID information.

## Caller Authentication

The idea behind of STIR/SHAKEN is to mitigate unwanted robocalls and bad actors who use caller ID spoofing to increase the chances of speaking to a subscriber.

STIR is used to enhance the SIP protocol to provide a mechanism for service providers to verify that the originator of a SIP call is highly likely to be valid (i.e. not a spoofed/fraudulent calling party). The goal of these enhancements is to make it considerably more difficult for bad actors to spoof the identity of a call for malevolent or other purposes. Examples of such activities are spoofing voice messaging or credit card validation services to get access to the victim’s voice messages or credit, respectively; engaging in confidence schemes by masquerading as legitimate enterprises looking for information or cash (e.g., banks for personal identification or the IRS for swindling); or to get through blocked caller lists (e.g., robocalling).

## PortSIP Solution

We are pleased to announce that PortSIP has officially launched **PortSIP PBX v22.0** with a complete implementation of **STIR/SHAKEN** for two key reasons:

1. Our clients no longer need to pay for expensive STIR/SHAKEN subscription services for their business.
2. Our clients' data stays secure, without being routed through third-party service provider networks.

However, it is important to note that each of our partners must still obtain a **STIR/SHAKEN** **certificate**.

