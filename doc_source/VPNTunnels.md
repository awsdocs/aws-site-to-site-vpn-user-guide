# Tunnel options for your Site\-to\-Site VPN connection<a name="VPNTunnels"></a>

You use a Site\-to\-Site VPN connection to connect your remote network to a VPC\. Each Site\-to\-Site VPN connection has two tunnels, with each tunnel using a unique virtual private gateway public IP address\. It is important to configure both tunnels for redundancy\. When one tunnel becomes unavailable \(for example, down for maintenance\), network traffic is automatically routed to the available tunnel for that specific Site\-to\-Site VPN connection\.

The following diagram shows the two tunnels of the Site\-to\-Site VPN connection\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Multiple_VPN_Tunnels_diagram.png)

When you create a Site\-to\-Site VPN connection, you download a configuration file specific to your customer gateway device that contains information for configuring the device, including information for configuring each tunnel\. You can optionally specify some of the tunnel options yourself when you create the Site\-to\-Site VPN connection\. Otherwise, AWS provides default values\.

The following are the tunnel options that you can configure\.

**Dead peer detection \(DPD\) timeout**  
The duration, in seconds, after which DPD timeout occurs\. You can specify 30 or higher\.  
Default: 30

**DPD timeout action**  
The action to take after dead peer detection \(DPD\) timeout occurs\. You can specify the following:  
+ `Clear`: End the IKE session when DPD timeout occurs \(stop the tunnel and clear the routes\)
+ `None`: Take no action when DPD timeout occurs
+ `Restart`: Restart the IKE session when DPD timeout occurs
For more information, see [Site\-to\-Site VPN tunnel initiation options](initiate-vpn-tunnels.md)\.  
Default: `Clear`

**IKE versions**  
The IKE versions that are permitted for the VPN tunnel\. You can specify one or more of the default values\.  
Default: `ikev1`, `ikev2`

**Inside tunnel IPv4 CIDR**  
The range of inside IPv4 addresses for the VPN tunnel\. You can specify a size /30 CIDR block from the `169.254.0.0/16` range\. The CIDR block must be unique across all Site\-to\-Site VPN connections that use the same virtual private gateway\.  
The following CIDR blocks are reserved and cannot be used:   
+ `169.254.0.0/30`
+ `169.254.1.0/30`
+ `169.254.2.0/30`
+ `169.254.3.0/30`
+ `169.254.4.0/30`
+ `169.254.5.0/30`
+ `169.254.169.252/30`
Default: A size /30 IPv4 CIDR block from the `169.254.0.0/16` range\.

**Inside tunnel IPv6 CIDR**  
\(IPv6 VPN connections only\) The range of inside IPv6 addresses for the VPN tunnel\. You can specify a size /126 CIDR block from the local `fd00::/8` range\. The CIDR block must be unique across all Site\-to\-Site VPN connections that use the same transit gateway\.  
Default: A size /126 IPv6 CIDR block from the local `fd00::/8` range\.

**Phase 1 Diffie\-Hellman \(DH\) group numbers**  
The DH group numbers that are permitted for the VPN tunnel for phase 1 of the IKE negotiations\. You can specify one or more of the default values\.  
Default: 2, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24

**Phase 2 Diffie\-Hellman \(DH\) group numbers**  
The DH group numbers that are permitted for the VPN tunnel for phase 2 of the IKE negotiations\. You can specify one or more of the default values\.  
Default: 2, 5, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24

**Phase 1 encryption algorithms**  
The encryption algorithms that are permitted for the VPN tunnel for phase 1 of the IKE negotiations\. You can specify one or more of the default values\.  
Default: AES128, AES256, AES128\-GCM\-16, AES256\-GCM\-16

**Phase 2 encryption algorithms**  
The encryption algorithms that are permitted for the VPN tunnel for phase 2 IKE negotiations\. You can specify one or more of the default values\.  
Default: AES128, AES256, AES128\-GCM\-16, AES256\-GCM\-16

**Phase 1 integrity algorithms**  
The integrity algorithms that are permitted for the VPN tunnel for phase 1 of the IKE negotiations\. You can specify one or more of the default values\.  
Default: SHA\-1, SHA2\-256, SHA2\-384, SHA2\-512

**Phase 2 integrity algorithms**  
The integrity algorithms that are permitted for the VPN tunnel for phase 2 of the IKE negotiations\. You can specify one or more of the default values\.  
Default: SHA\-1, SHA2\-256, SHA2\-384, SHA2\-512

**Phase 1 lifetime**  
The lifetime in seconds for phase 1 of the IKE negotiations\. You can specify a number between 900 and 28,800\.  
Default: 28,800 \(8 hours\)

**Phase 2 lifetime**  
The lifetime in seconds for phase 2 of the IKE negotiations\. You can specify a number between 900 and 3,600\. The number that you specify must be less than the number of seconds for the phase 1 lifetime\.  
Default: 3,600 \(1 hour\)

**Pre\-shared key \(PSK\)**  
The pre\-shared key \(PSK\) to establish the initial internet key exchange \(IKE\) security association between the virtual private gateway and customer gateway\.   
The PSK must be between 8 and 64 characters in length and cannot start with zero \(0\)\. Allowed characters are alphanumeric characters, periods \(\.\), and underscores \(\_\)\.  
Default: A 32\-character alphanumeric string\.

**Rekey fuzz**  
The percentage of the rekey window \(determined by the rekey margin time\) within which the rekey time is randomly selected\.   
You can specify a percentage value between 0 and 100\.  
Default: 100

**Rekey margin time**  
The margin time in seconds before the phase 2 lifetime expires, during which the AWS side of the VPN connection performs an IKE rekey\.   
You can specify a number between 60 and half of the value of the phase 2 lifetime seconds\.  
The exact time of the rekey is randomly selected based on the value for rekey fuzz\.  
Default: 540 \(9 minutes\)

**Replay window size packets**  
The number of packets in an IKE replay window\.   
You can specify a value between 64 and 2048\.  
Default: 1024

**Startup action**  
The action to take when establishing the tunnel for a VPN connection\. You can specify the following:   
+ `Start`: AWS initiates the IKE negotiation to bring the tunnel up\. Only supported if your customer gateway is configured with an IP address\.
+ `Add`: Your customer gateway device must initiate the IKE negotiation to bring the tunnel up\.
For more information, see [Site\-to\-Site VPN tunnel initiation options](initiate-vpn-tunnels.md)\.  
Default: `Add`

You can specify the tunnel options when you create a Site\-to\-Site VPN connection, or you can modify the tunnel options for an existing VPN connection\. You cannot configure tunnel options for an AWS Classic VPN connection\. For more information, see the following topics:
+ [Create a Site\-to\-Site VPN connection](SetUpVPNConnections.md#vpn-create-vpn-connection)
+ [Modifying Site\-to\-Site VPN tunnel options](modify-vpn-tunnel-options.md)