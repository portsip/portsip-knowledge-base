# What Is an SBC?

”SBC” is an acronym for Session Border Controller. An SBC is a network device/software, typically deployed at the border of an administrative domain of a network to control certain types of network traffic. An SBC is particularly concerned with network traffic when implementing session-oriented multimedia communications such as Voice over IP (VoIP). A Session Border Controller, to put it simply, sits at the edge of a network domain and controls the flow of session-oriented traffic.

SBCs perform many essential functions aimed at enabling and enhancing multimedia communications. These functions include security, signaling protocol repair and interworking (such as IPv4 to IPv6 interworking), media transcoding, lawful intercept, and more.

The security functions of an SBC concerning VoIP traffic conceptually parallel those of a firewall for IP traffic. More specifically, the SBC’s behavior is comparable to that of a proxy firewall – it terminates certain network protocol sessions. It then propagates the information carried therein onto corresponding sessions on the other side. In an SBC, the term Back to Back User Agent (B2BUA) is used to describe this structure, as illustrated below. This fundamental approach underlies many of the security functions of the SBC by imposing a “permit what is understood and block the rest” architecture on the device.

<figure><img src="../.gitbook/assets/sbc_arch.png" alt=""><figcaption></figcaption></figure>

In addition to controlled relaying (or blocking) of the signaling and media information, an SBC may adjust the content, such as filtering SIP headers or transcoding the audio. Some SBCs can also provide more sophisticated services, such as call routing.&#x20;

The SBC B2BUA behavior happens at layer 5 of the protocol stack. From an IP perspective, an SBC is a network host; SBCs do not forward IP datagrams. Only the content of specifically





