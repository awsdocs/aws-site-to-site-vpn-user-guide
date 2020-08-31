# Tunnel options for your Site\-to\-Site VPN connection<a name="VPNTunnels"></a>

You use a Site\-to\-Site VPN connection to connect your remote network to a VPC\. Each Site\-to\-Site VPN connection has two tunnels, with each tunnel using a unique virtual private gateway public IP address\. It is important to configure both tunnels for redundancy\. When one tunnel becomes unavailable \(for example, down for maintenance\), network traffic is automatically routed to the available tunnel for that specific Site\-to\-Site VPN connection\.

The following diagram shows the two tunnels of the Site\-to\-Site VPN connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Multiple_VPN_Tunnels_diagram.png)

When you create a Site\-to\-Site VPN connection, you download a configuration file specific to your customer gateway device that contains information for configuring the device, including information for configuring each tunnel\. You can optionally specify some of the tunnel options yourself when you create the Site\-to\-Site VPN connection\. Otherwise, AWS provides default values\.

The following table describes the tunnel options that you can configure\.


| Item | Description | AWS\-provided default values | 
| --- | --- | --- | 
| Dead peer detection \(DPD\) timeout \(seconds\) |  The duration after which DPD timeout occurs\. You can specify 30 or higher\.  | 30 | 
|  DPD timeout action  |  The action to take after dead peer detection \(DPD\) timeout occurs\. You can specify the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/VPNTunnels.html) For more information, see [Site\-to\-Site VPN tunnel initiation options](initiate-vpn-tunnels.md)\.  |  Clear  | 
| IKE versions | The IKE versions that are permitted for the VPN tunnel\. You can specify one or more of the default values\. | ikev1, ikev2 | 
|  Inside tunnel IPv4 CIDR  |  The range of inside IPv4 addresses for the VPN tunnel\. You can specify a size /30 CIDR block from the `169.254.0.0/16` range\. The CIDR block must be unique across all Site\-to\-Site VPN connections that use the same virtual private gateway\. The following CIDR blocks are reserved and cannot be used:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/VPNTunnels.html)  |  A size /30 IPv4 CIDR block from the `169.254.0.0/16` range\.  | 
|  Inside tunnel IPv6 CIDR  |  \(IPv6 VPN connections only\) The range of inside IPv6 addresses for the VPN tunnel\. You can specify a size /126 CIDR block from the local `fd00::/8` range\. The CIDR block must be unique across all Site\-to\-Site VPN connections that use the same transit gateway\.  |  A size /126 IPv6 CIDR block from the local `fd00::/8` range\.  | 
| Phase 1 Diffie\-Hellman \(DH\) group numbers | The DH group numbers that are permitted for the VPN tunnel for phase 1 of the IKE negotiations\. You can specify one or more of the default values\. | 2, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24 | 
| Phase 2 Diffie\-Hellman \(DH\) group numbers | The DH group numbers that are permitted for the VPN tunnel for phase 2 of the IKE negotiations\. You can specify one or more of the default values\. | 2, 5, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24 | 
| Phase 1 encryption algorithms | The encryption algorithms that are permitted for the VPN tunnel for phase 1 of the IKE negotiations\. You can specify one or more of the default values\. | AES128, AES256, AES128\-GCM\-16, AES256\-GCM\-16 | 
| Phase 2 encryption algorithms | The encryption algorithms that are permitted for the VPN tunnel for phase 2 IKE negotiations\. You can specify one or more of the default values\. | AES128, AES256, AES128\-GCM\-16, AES256\-GCM\-16 | 
| Phase 1 integrity algorithms | The integrity algorithms that are permitted for the VPN tunnel for phase 1 of the IKE negotiations\. You can specify one or more of the default values\. | SHA\-1, SHA2\-256, SHA2\-384, SHA2\-512 | 
| Phase 2 integrity algorithms | The integrity algorithms that are permitted for the VPN tunnel for phase 2 of the IKE negotiations\. You can specify one or more of the default values\. | SHA\-1, SHA2\-256, SHA2\-384, SHA2\-512 | 
| Phase 1 lifetime \(seconds\) | The lifetime in seconds for phase 1 of the IKE negotiations\. You can specify a number between 900 and 28,800\. | 28,800 \(8 hours\) | 
| Phase 2 lifetime \(seconds\) | The lifetime in seconds for phase 2 of the IKE negotiations\. You can specify a number between 900 and 3,600\. The number that you specify must be less than the number of seconds for the phase 1 lifetime\. | 3,600 \(1 hour\) | 
|  Pre\-shared key \(PSK\)  |  The pre\-shared key \(PSK\) to establish the initial internet key exchange \(IKE\) security association between the virtual private gateway and customer gateway\.  The PSK must be between 8 and 64 characters in length and cannot start with zero \(0\)\. Allowed characters are alphanumeric characters, periods \(\.\), and underscores \(\_\)\.  |  A 32\-character alphanumeric string\.  | 
| Rekey fuzz \(percentage\) |  The percentage of the rekey window \(determined by the rekey margin time\) within which the rekey time is randomly selected\.  You can specify a value between 0 and 100\.  | 100 | 
| Rekey margin time \(seconds\) |  The margin time in seconds before the phase 2 lifetime expires, during which the AWS side of the VPN connection performs an IKE rekey\.  You can specify a number between 60 and half of the value of the phase 2 lifetime seconds\. The exact time of the rekey is randomly selected based on the value for rekey fuzz\.  | 540 \(9 minutes\) | 
| Replay window size packets |  The number of packets in an IKE replay window\.  You can specify a value between 64 and 2048\.  | 1024 | 
|  Startup action  |  The action to take when establishing the tunnel for a VPN connection\. You can specify the following:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/VPNTunnels.html) . Note: Both the Startup Action of Start and Add are only supported for Customer Gateway (CGW) with IP addresses. For more information, see [Site\-to\-Site VPN tunnel initiation options](initiate-vpn-tunnels.md)\.  |   Add  | 

You can specify the tunnel options when you create a Site\-to\-Site VPN connection, or you can modify the tunnel options for an existing VPN connection\. You cannot configure tunnel options for an AWS Classic VPN connection\. For more information, see the following topics:
+ [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)
+ [Modifying Site\-to\-Site VPN tunnel options](modify-vpn-tunnel-options.md)
