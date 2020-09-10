# Your customer gateway device<a name="your-cgw"></a>

A *customer gateway device* is a physical or software appliance that you own or manage in your on\-premises network \(on your side of a Site\-to\-Site VPN connection\)\. You or your network administrator must configure the device to work with the Site\-to\-Site VPN connection\. 

The following diagram shows your network, the customer gateway device, the VPN connection that goes to a virtual private gateway \(which is attached to your VPC\)\. The two lines between the customer gateway device and virtual private gateway represent the tunnels for the VPN connection\. If there's a device failure within AWS, your VPN connection automatically fails over to the second tunnel so that your access isn't interrupted\. From time to time, AWS also performs routine maintenance on the virtual private gateway, which might briefly disable one of the two tunnels of your VPN connection\. Your VPN connection automatically fails over to the second tunnel while this maintenance is performed\. When you configure your customer gateway device, it's therefore important that you configure both tunnels\.

![\[High-level customer gateway overview\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/cgw-high-level.png)

For the steps to set up a VPN connection, see [Getting started](SetUpVPNConnections.md)\. During this process, you create a customer gateway resource in AWS, which provides information to AWS about your device, for example, its public\-facing IP address\. For more information, see [Customer gateway options for your Site\-to\-Site VPN connection](cgw-options.md)\. The customer gateway resource in AWS does not configure or create the customer gateway device\. You must configure the device yourself\.

After you create the VPN connection, [download the configuration file](SetUpVPNConnections.md#vpn-download-config) from the Amazon VPC console, which contains information specific to your VPN connection\. Use this information to configure your customer gateway device\. In some cases, device\-specific configuration files are available for devices that we've tested\. Otherwise, you can download the generic configuration file\.

## Customer gateway devices that we've tested<a name="DevicesTested"></a>

Your customer gateway device can be a physical or software appliance\. We've tested and provided configuration information for the following devices:
+ Check Point Security Gateway running R77\.10 \(or later\) software
+ Cisco ASA running Cisco ASA 8\.2 \(or later\) software
+ Cisco IOS running Cisco IOS 12\.4 \(or later\) software
+ SonicWALL running SonicOS 5\.9 \(or later\) software
+ Fortinet Fortigate 40\+ Series running FortiOS 4\.0 \(or later\) software 
+ Juniper J\-Series running JunOS 9\.5 \(or later\) software
+ Juniper SRX running JunOS 11\.0 \(or later\) software
+ Juniper SSG running ScreenOS 6\.1, or 6\.2 \(or later\) software
+ Juniper ISG running ScreenOS 6\.1, or 6\.2 \(or later\) software
+ Netgate pfSense running OS 2\.2\.5 \(or later\) software\.
+ Palo Alto Networks PANOS 4\.1\.2 \(or later\) software
+ Yamaha RT107e, RTX1200, RTX1210, RTX1500, RTX3000 and SRT100 routers
+ Microsoft Windows Server 2008 R2 \(or later\) software
+ Microsoft Windows Server 2012 R2 \(or later\) software 
+ Zyxel Zywall Series 4\.20 \(or later\) software for statically routed VPN connections, or 4\.30 \(or later\) software for dynamically routed VPN connections

If you have one of these devices, but configure it for IPsec in a different way than presented in the configuration file, you can change our suggested configuration to match your needs\.

## Requirements for your customer gateway device<a name="CGRequirements"></a>

If you have a device that isn't in the preceding list of tested devices, this section describes the requirements that the device must meet for you to use it to establish a Site\-to\-Site VPN connection\.

There are four main parts to the configuration of your customer gateway device\. The following symbols represent each part of the configuration\.


|  |  | 
| --- |--- |
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/IKE.png)  |  Internet key exchange \(IKE\) security association\. This is required to exchange keys used to establish the IPsec security association\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/IPsec.png)  |  IPsec security association\. This handles the tunnel's encryption, authentication, and so on\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Tunnel.png)  |  Tunnel interface\. This receives traffic going to and from the tunnel\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/BGP.png)  |  \(Optional\) Border Gateway Protocol \(BGP\) peering\. For devices that use BGP, this exchanges routes between the customer gateway device and the virtual private gateway\.  | 

The following table lists the requirements for the customer gateway device, the related RFC \(for reference\), and comments about the requirements\.

Each VPN connection consists of two separate tunnels\. Each tunnel contains an IKE security association, an IPsec security association, and a BGP peering\. You are limited to one unique security association \(SA\) pair per tunnel \(one inbound and one outbound\), and therefore two unique SA pairs in total for two tunnels \(four SAs\)\. Some devices use a policy\-based VPN and create as many SAs as ACL entries\. Therefore, you might need to consolidate your rules and then filter so that you don't permit unwanted traffic\.

By default, the VPN tunnel comes up when traffic is generated and the IKE negotiation is initiated from your side of the VPN connection\. You can configure the VPN connection to initiate the IKE negotiation from the AWS side of the connection instead\. For more information, see [Site\-to\-Site VPN tunnel initiation options](initiate-vpn-tunnels.md)\. 

VPN endpoints support rekey and can start renegotiations when phase 1 is about to expire if the customer gateway device hasn't sent any renegotiation traffic\.


|  Requirement  |  RFC |  Comments | 
| --- | --- | --- | 
|  Establish IKE security association   ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/IKE.png)   |  [RFC 2409](http://tools.ietf.org/html/rfc2409)  [RFC 7296](https://tools.ietf.org/html/rfc7296)  |  The IKE security association is established first between the virtual private gateway and the customer gateway device using a pre\-shared key or a private certificate that uses AWS Certificate Manager Private Certificate Authority as the authenticator\. When established, IKE negotiates an ephemeral key to secure future IKE messages\. There must be complete agreement among the parameters, including encryption and authentication parameters\. When you create a VPN connection in AWS, you can specify your own pre\-shared key for each tunnel, or you can let AWS generate one for you\. Alternatively, you can specify the private certificate using AWS Certificate Manager Private Certificate Authority to use for your customer gateway device\. For more information, about configuring VPN tunnels see [Tunnel options for your Site\-to\-Site VPN connection](VPNTunnels.md)\. The following versions are supported: IKEv1 and IKEv2\. We support Main mode only with IKEv1\. The Site\-to\-Site VPN service is a route\-based solution\. If you are using a policy\-based configuration, you must limit your configuration to a single security association \(SA\)\.  | 
|  Establish IPsec security associations in Tunnel mode  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/IPsec.png)   |   [RFC 4301](http://tools.ietf.org/html/rfc4301)   |  Using the IKE ephemeral key, keys are established between the virtual private gateway and the customer gateway device to form an IPsec security association \(SA\)\. Traffic between gateways is encrypted and decrypted using this SA\. The ephemeral keys used to encrypt traffic within the IPsec SA are automatically rotated by IKE on a regular basis to ensure confidentiality of communications\.  | 
|  Use the AES 128\-bit encryption or AES 256\-bit encryption function  |   [RFC 3602](http://tools.ietf.org/html/rfc3602)   |  The encryption function is used to ensure privacy for both IKE and IPsec security associations\.  | 
|  Use the SHA\-1 or SHA\-2 \(256\) hashing function  |   [RFC 2404](http://tools.ietf.org/html/rfc2404)   |  This hashing function is used to authenticate both IKE and IPsec security associations\.  | 
|  Use Diffie\-Hellman Perfect Forward Secrecy\.  |   [RFC 2409](http://tools.ietf.org/html/rfc2409)   |  IKE uses Diffie\-Hellman to establish ephemeral keys to secure all communication between customer gateway devices and virtual private gateways\.  The following groups are supported: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html)  | 
|  Fragment IP packets before encryption  |   [RFC 4459](http://tools.ietf.org/html/rfc4459)   |  When packets are too large to be transmitted, they must be fragmented\. We do not reassemble fragmented encrypted packets\. Therefore, your VPN device must fragment packets *before* encapsulating with the VPN headers\. The fragments are individually transmitted to the remote host, which reassembles them\.  | 
|  \(Dynamically\-routed VPN connections\) Use IPsec Dead Peer Detection  |   [RFC 3706](http://tools.ietf.org/html/rfc3706)   |  Dead Peer Detection enables the VPN devices to rapidly identify when a network condition prevents delivery of packets across the internet\. When this occurs, the gateways delete the security associations and attempt to create new associations\. During this process, the alternate IPsec tunnel is used if possible\.  | 
|  \(Dynamically\-routed VPN connections\) Bind tunnel to logical interface \(route\-based VPN\)  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/Tunnel.png)   |   None   |  Your device must be able to bind the IPsec tunnel to a logical interface\. The logical interface contains an IP address that is used to establish BGP peering to the virtual private gateway\. This logical interface should perform no additional encapsulation \(for example, GRE or IP in IP\)\. Your interface should be set to a 1399 byte Maximum Transmission Unit \(MTU\)\.   | 
|  \(Dynamically\-routed VPN connections\) Establish BGP peerings  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/vpn/latest/s2svpn/images/BGP.png)   |   [RFC 4271](http://tools.ietf.org/html/rfc4271)   |  BGP is used to exchange routes between the customer gateway device and the virtual private gateway for devices that use BGP\. All BGP traffic is encrypted and transmitted via the IPsec Security Association\. BGP is required for both gateways to exchange the IP prefixes that are reachable through the IPsec SA\.  | 

Because the connection encapsulates packets with additional network headers \(including IPsec\), the amount of data that can be transmitted in a single packet is reduced\. We recommend that you use the techniques listed in the following table to help you to minimize problems related to the amount of data that can be transmitted through the IPsec tunnel\.


|  Technique  |  RFC |  Comments | 
| --- | --- | --- | 
|  Adjust the maximum segment size \(MSS\) of TCP packets entering the VPN tunnel  |   [RFC 4459](http://tools.ietf.org/html/rfc4459)   |  TCP packets are often the most common type of packet across IPsec tunnels\. Some gateways can change the TCP maximum segment size \(MSS\) parameter\. This causes the TCP endpoints \(clients, servers\) to reduce the amount of data sent with each packet\. This is an ideal approach, as the packets arriving at the VPN devices are small enough to be encapsulated and transmitted\. We recommend setting the MSS on your customer gateway device to 1359 when using the SHA2\-384 or SHA2\-512 hashing algorithms\. This is necessary to accommodate for the larger header\.  | 
|  Reset the "Don't Fragment" flag on packets  |   [RFC 791](http://tools.ietf.org/html/rfc791)   |  Some packets carry a flag, known as the Don't Fragment \(DF\) flag, which indicates that the packet should not be fragmented\. If the packets carry the flag, the gateways generate an ICMP Path MTU Exceeded message\. In some cases, applications do not contain adequate mechanisms for processing these ICMP messages and for reducing the amount of data transmitted in each packet\. Some VPN devices can override the DF flag and fragment packets unconditionally as required\. If your customer gateway device has this ability, we recommend that you use it as appropriate\.   | 

An AWS VPN connection does not support Path MTU Discovery \([RFC 1191](https://tools.ietf.org/html/rfc1191)\)\.

If you have a firewall between your customer gateway device and the internet, see [Configuring a firewall between the internet and your customer gateway device](#FirewallRules)\.

## Configuring a firewall between the internet and your customer gateway device<a name="FirewallRules"></a>

You must have an internet\-routable IP address to use as the endpoint for the IPsec tunnels that connect your customer gateway device to the virtual private gateway\. If a firewall is in place between the internet and your gateway, the rules in the following tables must be in place to establish the IPsec tunnels\. The virtual private gateway addresses are in the configuration file\.


**Inbound \(from the internet\)**  

| 
| 
|  Input rule I1  | 
| --- |
|  Source IP  |  Virtual Private Gateway 1  | 
|  Dest IP  |  Customer Gateway  | 
|  Protocol  |  UDP  | 
|  Source port  |  500  | 
|  Destination  |  500  | 
|  Input rule I2  | 
| --- |
|  Source IP  |  Virtual Private Gateway 2  | 
|  Dest IP  |  Customer Gateway  | 
|  Protocol  |  UDP  | 
|  Source port  |  500  | 
|  Destination port  |  500  | 
|  Input rule I3  | 
| --- |
|  Source IP  |  Virtual Private Gateway 1  | 
|  Dest IP  |  Customer Gateway  | 
|  Protocol  |  IP 50 \(ESP\)  | 
|  Input rule I4  | 
| --- |
|  Source IP  |  Virtual Private Gateway 2  | 
|  Dest IP  |  Customer Gateway  | 
|  Protocol  |  IP 50 \(ESP\)  | 


**Outbound \(to the internet\)**  

| 
| 
|  Output rule O1  | 
| --- |
|  Source IP  |  Customer Gateway  | 
|  Dest IP  |  Virtual Private Gateway 1  | 
|  Protocol  |  UDP  | 
|  Source port  |  500  | 
|  Destination port  |  500  | 
|  Output rule O2  | 
| --- |
|  Source IP  |  Customer Gateway  | 
|  Dest IP  |  Virtual Private Gateway 2  | 
|  Protocol  |  UDP  | 
|  Source port  |  500  | 
|  Destination port  |  500  | 
|  Output rule O3  | 
| --- |
|  Source IP  |  Customer Gateway  | 
|  Dest IP  |  Virtual Private Gateway 1  | 
|  Protocol  |  IP 50 \(ESP\)   | 
|  Output rule O4  | 
| --- |
|  Source IP  |  Customer Gateway  | 
|  Dest IP  |  Virtual Private Gateway 2  | 
|  Protocol  |  IP 50 \(ESP\)  | 

Rules I1, I2, O1, and O2 enable the transmission of IKE packets\. Rules I3, I4, O3, and O4 enable the transmission of IPsec packets that contain the encrypted network traffic\.

If you are using NAT traversal \(NAT\-T\) on your device, you must include rules that allow UDP access over port 4500\. Check if your device is advertising NAT\-T\.

## Multiple VPN connection scenarios<a name="your-cgw-multiple-connection"></a>

The following are scenarios in which you might create multiple VPN connections with one or more customer gateway devices\.

**Multiple VPN connections using the same customer gateway device**  
You can create additional VPN connections from your on\-premises location to other VPCs using the same customer gateway device\. You can reuse the same customer gateway IP address for each of those VPN connections\.

**Redundant VPN connection using a second customer gateway device**  
To protect against a loss of connectivity if your customer gateway device becomes unavailable, you can set up a second VPN connection using a second customer gateway device\. For more information, see [Using redundant Site\-to\-Site VPN connections to provide failover](vpn-redundant-connection.md)\. When you establish redundant customer gateway devices at a single location, both devices should advertise the same IP ranges\.

**Multiple customer gateway devices to a single virtual private gateway \(AWS VPN CloudHub\)**  
You can establish multiple VPN connections to a single virtual private gateway from multiple customer gateway devices\. This enables you to have multiple locations connected to the AWS VPN CloudHub\. For more information, see [Providing secure communication between sites using VPN CloudHub](VPN_CloudHub.md)\. When you have customer gateway devices at multiple geographic locations, each device should advertise a unique set of IP ranges specific to the location\. 

## Routing for your customer gateway device<a name="cgw-routing-info"></a>

AWS recommends advertising specific BGP routes to influence routing decisions in the virtual private gateway\. Check your vendor documentation for the commands that are specific to your device\.

When you create multiple VPN connections, the virtual private gateway sends network traffic to the appropriate VPN connection using statically assigned routes or BGP route advertisements\. Which route depends on how the VPN connection was configured\. Statically assigned routes are preferred over BGP advertised routes in cases where identical routes exist in the virtual private gateway\. If you select the option to use BGP advertisement, then you cannot specify static routes\.

For more information about route priority, see [Route tables and VPN route priority](VPNRoutingTypes.md#vpn-route-priority)\.